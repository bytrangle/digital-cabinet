# Towards an animation smoothness metric
Learn about measuring animations, how to think about animation frames, and overall page smoothness.

You've probably experienced pages that"stutter"or"freeze" during scrolling or animations. We like to say that these experiences are not _smooth_. To address these types of issues, the Chrome team has been working on adding more support to our lab tooling for animation detection, as well as making steady improvements to the rendering pipeline diagnostics within Chromium.

We'd like to share some recent progress, offer concrete tooling guidance, and discuss ideas for future animation smoothness metrics. As always, we would love to hear your [feedback](#feedback).

This post will cover three main topics:

-   A quick look at animations and animation frames.
-   Our current thoughts on measuring overall animation smoothness.
-   A few practical suggestions for you to leverage in lab tooling today.

## What are animations? [#](#what-are-animations)

Animations bring content to life! By making content move, especially in response to user interactions, animations can make an experience feel more natural, understandable, and fun.

But poorly implemented animations, or just adding too many animations, can degrade the experience and make it decidedly not fun at all. We've probably all interacted with an interface which just added too many"helpful" transition effects, which actually become hostile to experience when they perform poorly. Some users therefore actually might [prefer reduced motion](https://web.dev/prefers-reduced-motion/), a user preference that you should honor.

### How do animations work? [#](#how-do-animations-work)

As a quick recap, the [rendering pipeline](https://web.dev/animations-overview/#pipeline) consists of a few, **sequential** stages:

1.  **Style:** Calculate the styles that apply to the elements.
2.  **Layout:** Generate the geometry and position for each element.
3.  **Paint:** Fill out the pixels for each element into layers.
4.  **Composite:** Draw the layers to the screen.

While there are many ways to define animations, they all fundamentally work via one of the following:

-   Adjusting **layout** properties.
-   Adjusting **paint** properties.
-   Adjusting **composite** properties.

Because these stages are sequential, it is important to define animations in terms of properties that are further down the pipeline. The earlier the update happens in the process, the greater are the costs and it's less likely to be smooth. (See [Rendering performance](https://developers.google.com/web/fundamentals/performance/rendering) for more details.)

While it can be convenient to animate layout properties, there are costs to doing so, even if those costs aren't immediately apparent. Animations should be defined in terms of composite property changes wherever possible.

Defining [declarative CSS animations or using Web Animations](https://developers.google.com/web/fundamentals/design-and-ux/animations/css-vs-javascript), and ensuring you [animate composite properties](https://developer.chrome.com/blog/hardware-accelerated-animations/), is a great first step to ensuring smooth and efficient animations. But still, this alone does not guarantee smoothness because even efficient web animations have performance limits. That's why it is always important to measure!

## What are animation frames? [#](#what-are-animation-frames)

Updates to the visual representation of a page take time to appear. A visual change will lead to a new animation frame, which is eventually rendered on the user's display.

Displays update on some interval, so visual updates are batched. Many displays update on a fixed interval of time, such as 60 times a second (that is 60??Hz). Some more modern displays can offer higher refresh rates (90???120??Hz are becoming common). Often these displays can actively adapt between refresh rates as needed, or even offer fully variable frame rates.

The goal for any application, like a game or a browser, is to process all these batched visual updates and produce a visually complete animation frame within the deadline, every time. Note that this goal is entirely distinct from other important browser tasks such as loading content from the network quickly or executing JavaScript tasks efficiently.

At some point, it can become too difficult to complete all visual updates within the allotted deadline assigned by the display. When this happens, the browser **drops a frame**. Your screen doesn't go black, it just repeats itself. You see the same visual update for a bit longer???the same animation frame that was presented at the previous frame opportunity.

This actually happens often! It is not necessarily even perceptible, especially for static or document-like content, which is common on the web platform in particular. Dropped frames only become apparent when there are important visual updates, such as animations, for which we need a steady stream of animation updates to show smooth motion.

## What impacts animation frames? [#](#what-impacts-animation-frames)

Web developers can greatly impact the ability of a browser to quickly and efficiently render and present visual updates!

Some examples:

-   Using content that is too large or resource-intensive to decode quickly on the target device.
-   [Using too many layers](https://developers.google.com/web/fundamentals/performance/rendering/stick-to-compositor-only-properties-and-manage-layer-count) requiring too much GPU memory.
-   Defining overly complex CSS styles or web animations.
-   Using design anti-patterns that disable fast rendering optimizations.
-   Too much JS work on the main thread, leading to long tasks that block visual updates.

But how can you know when an animation frame has missed its deadline and caused a dropped frame?

One possible method is using [`requestAnimationFrame()`](https://developer.mozilla.org/docs/Web/API/window/requestAnimationFrame) polling, however it has several downsides. `requestAnimationFrame()`, or "rAF", tells the browser that you wish to perform an animation and asks for an opportunity to do so before the next paint stage of the rendering pipeline. If your callback function isn't called at the time you expect it, that means a paint wasn't executed, and one or more frames were skipped. By polling and counting how often rAF is called, you can compute a sort of "frames per second" (FPS) metric.

**Warning**: The following code is an anti-pattern and is strongly discouraged!

    let frameTimes = [];  
    function pollFramesPerSecond(now) {  
      frameTimes = [...frameTimes.filter(t => t > now - 1000), now];  
      requestAnimationFrame(pollFramesPerSecond);  
      console.log('Frames per second:', frameTimes.length);  
    }  
    requestAnimationFrame(pollFramesPerSecond);

Using `requestAnimationFrame()` polling is not a good idea for several reasons:

-   Every script has to set up its own polling loop.
-   It can block the critical path.
-   Even if the rAF polling is fast, it can prevent [`requestIdleCallback()`](https://developer.mozilla.org/docs/Web/API/window/requestIdleCallback) from being able to schedule long idle blocks when used continuously (blocks that exceed a single frame).
-   Similarly, lack of long idle blocks prevents the browser from scheduling other long-running tasks (such as longer garbage collection and other background or speculative work).
-   If polling is toggled on and off, then you'll miss cases where frame budget has been exceeded.
-   Polling will report false-positives in cases where the browser is using variable update frequency (for example, due to power or visibility status).
-   And most importantly, it doesn't actually capture all types of animation updates!

Too much work on the main thread can impact the ability to see animation frames. Check out the [Jank Sample](https://googlechrome.github.io/devtools-samples/jank/) to see how a rAF-driven animation, once there is too much work on the main thread (such as layout), will lead to dropped frames and fewer rAF callbacks, and lower FPS.

When the main thread becomes bogged down, visual updates begin to stutter. That's jank!

Many measurement tools have focused extensively on the ability for the main thread to yield in a timely manner, and for animation frames to run smoothly. But this is not the whole story! Consider the following example:

The video above shows a page that periodically injects long tasks onto the main thread. These long tasks completely ruin the ability of the page to provide certain types of visual updates, and you can see in the top-left corner a corresponding drop of `requestAnimationFrame()` reported FPS to 0.

And yet, despite these long tasks, the page continues to scroll smoothly. This is because on modern browsers, [scrolling is often threaded](https://blogs.windows.com/msedgedev/2017/03/08/scrolling-on-the-web/#:~:text=The%20multithreaded%20web), driven entirely by the compositor.

This is an example that simultaneously contains many dropped frames on the main thread, yet still has many successfully-delivered frames of scrolling on the compositor thread. Once the long task is complete, the main thread paint update has no visual change to offer anyway. rAF polling suggested a frame drop to 0, but _visually_, a user wouldn't be able to notice a difference!

For animation frames, the story is not that simple.

There are many reasons why long tasks are bad???and these are captured with [dedicated performance APIs](https://web.dev/custom-metrics/), such as the Long Tasks API or the Event Timing API. Yet, there are also features, such as [`isInputPending()`](https://web.dev/isinputpending/), where long tasks during idle periods may be added entirely by design and be a good thing.

## Animation frames: Updates that matter [#](#animation-frames:-updates-that-matter)

The above example showcases that there is more to the story than just `requestAnimationFrame()`.

So when do animation updates and animation frames matter? Here are some criteria we're thinking about and would love to get feedback on:

-   Main and compositor thread updates
-   Missing paint updates
-   Detecting animations
-   Quality versus quantity

### Main and compositor thread updates [#](#main-and-compositor-thread-updates)

Animation frame updates are not boolean. It is not the case that frames may only be fully dropped or fully presented. There are many reasons why an animation frame may be _partially_ _presented_. In other words, it can simultaneously have _some stale content_ while also having _some new visual updates_ which are presented.

The most common example of this is when the browser is unable to produce a new main thread update within frame deadline but does have a new compositor thread update (such as the threaded scrolling example from earlier).

One important reason why using declarative animations to animate composite properties is recommended is that doing so enables an animation to be driven entirely by the compositor thread even when the main thread is busy. These types of animations can continue to produce visual updates efficiently and in parallel.

On the other hand, there may be cases where a main thread update finally becomes available for presentation, but only after missing several frame deadlines. Here the browser will have _some_ new update, but it may not be _the very latest_.

Broadly speaking, we consider frames that contain some new visual updates, without all new visual updates, as a _partial frame_. Partial frames are fairly common. Ideally, partial updates would at least include the most important visual updates, like animations, but that can only happen if animations are driven by the compositor thread.

### Missing paint updates [#](#missing-paint-updates)

Another type of partial update is when media like images have not finished decoding and rasterizing in time for frame presentation.

Or, even if a page is perfectly static, browsers may still fall behind rendering visual updates during rapid scrolling. That is because the pixel renditions of content beyond the visible viewport may be discarded to save GPU memory. It takes time to render pixels, and it may take longer than a single frame to render everything after a large scroll, like a finger fling. This is commonly known as _checkerboarding_.

With each frame rendering opportunity, it's possible to track how much of the latest visual updates actually got to the screen. Measuring the ability to do so over many frames (or time) is broadly known as _frame throughput_.

If the GPU is really bogged down, the browser (or platform) may even begin to throttle the rate at which it attempts visual updates and thus decreases effective frame rates. While technically that can reduce the number of dropped frame updates, visually it will still appear as a lower frame throughput.

Yet, not all types of low frame throughput are bad. If the page is mostly idle and there are no active animations, a low frame rate is just as visually appealing as a high frame rate (and, it can save battery!).

So when does frame throughput matter?

### Detecting animations [#](#detecting-animations)

High frame throughput matters especially during periods with important animations. Different animation types will depend on visual updates from a specific thread (main, compositor, or a worker), so its visual update is dependent on that thread providing its update within the deadline. We say that a given thread _affects smoothness_ whenever there is an active animation that depends on that thread update.

Some types of animations are easier to define and detect than others. Declarative animations, or user-input-driven animations, are clearer to define than JavaScript-driven animations implemented as periodic updates to animatable style properties.

Even with `requestAnimationFrame()` you cannot always assume that every rAF call is necessarily producing a visual update or animation. For example, using rAF polling just to track frame rate (as shown above) should not itself affect smoothness measurements since there is no visual update.

Animation detection in tooling, as well as the specifics of animation implementations in browsers, continually evolves and improves. Chrome recently moved [background-color animations](https://developer.chrome.com/blog/hardware-accelerated-animations/#whats-coming-next) from the main thread to the compositor!

### Quality versus quantity [#](#quality-versus-quantity)

Finally, detecting animations and animation frame updates is still only part of the story because it only captures the quantity of animation updates, not the quality.

For example, you may see a steady framerate of of 60??fps while watching a video. Technically, this is perfectly smooth, but the video itself may have a low bit rate, or issues with network buffering. This isn't captured by animation smoothness metrics directly, yet may still be jarring to the user.

Or, a game which leverages `<canvas>` (perhaps even using techniques like [offscreen canvas](https://developers.google.com/web/updates/2018/08/offscreen-canvas) to ensure a steady frame rate) may technically be perfectly smooth in terms of animation frames, all while failing to load high quality game assets into the scene or exhibiting rendering artifacts.

And of course, a site can just have some really lame animations ????

![](https://upload.wikimedia.org/wikipedia/commons/1/1c/Under-Construction-Bulldozer.gif)

I mean, I guess they were pretty cool for their time!

## States of a single animation frame [#](#states-of-a-single-animation-frame)

Because frames may be partially presented, or dropped frames may happen in ways that do not affect smoothness, we have begun to think of each frame as having a completeness or smoothness score.

Here is the spectrum of ways in which we interpret the state of a single animation frame, ordered from best to worst case:

\| **No Update Desired** | Idle time, repeat of the previous frame. |
\| **Fully presented** | The main thread update was either committed within deadline, or no main thread update was desired. |
\| **Partially presented** | Compositor only; the delayed main thread update had no visual change. |
\| **Partially presented** | Compositor only; the main thread had a visual update, but that update did not include an animation that affects smoothness. |
\| **Partially presented** | Compositor only; the main thread had a visual update that affects smoothness, but a previously stale frame arrived and was used instead. |
\| **Partially presented** | Compositor only; without the desired main update, and the compositor update has an animation that affects smoothness. |
\| **Partially presented** | Compositor only but the compositor update does not have an animation that affects smoothness. |
\| **Dropped frame** | No update. There was no compositor update desired, and main was delayed. |
\| **Dropped frame** | A compositor update was desired, but it was delayed. |
\| **Stale frame** | An update was desired, it was produced by the renderer, but the GPU still did not present it before the vsync deadline. |

It's possible to turn these states into somewhat of a score. And perhaps one way to interpret this score is to consider it a _probability_ of being observable by the user. A single dropped frame may not be very observable, but a sequence of many dropped frames affecting smoothness in a row sure is!

## Putting it all together: A Percent Dropped Frames metric [#](#putting-it-all-together:-a-percent-dropped-frames-metric)

While it can sometimes be necessary to dive deep into the state of each animation frame, it's also useful to just assign a quick"at a glance" score for an experience.

Because frames may be _partially presented,_ and because even fully skipped frame updates may not actually _affect smoothness_, we want to focus less on just counting frames, and more on the _extent_ to which the browser is unable to provide _visually complete_ updates _when it mattered_.

The mental model should move from:

1.  _Frames Per Second_, to
2.  Detecting missing and important animation updates, to
3.  _Percentage Dropped_ over a given time period.

What matters is: _the proportion of time waiting for important updates_. We think this matches the natural way users experience the smoothness of web content in practice. So far, we have been using the following as an initial set of metrics:

-   **Average Percent Dropped:** for all non-idle animation frames throughout the whole timeline
-   **Worst Case of Percent Dropped Frames:** as measured over 1 second sliding windows of time.
-   **95th percentile of Percent Dropped Frames:** as measured over 1 second sliding windows of time.

You can find these scores in some Chrome developer tools today. While these metrics focus only on overall frame throughput, we are also evaluating other factors, such as frame latency.

These names are somewhat historical. The "Percent Dropped" naming no longer matches any literal "was a frame dropped" concept. As the previous section covered, each animation frame is a complicated arrangement of conditions leading to an overall probability. Expect these names to evolve over time.

### Performance HUD [#](#performance-hud)

Chromium has a neat Performance HUD hidden behind a flag (`chrome://flags/#show-performance-metrics-hud`). In it, you can find live scores for things like Core Web Vitals and also a few experimental definitions for animation smoothness based on _Percent Dropped Frames_ over time.

![](https://web-dev.imgix.net/image/eqprBhZUGfb8WYnumQ9ljAxRrA72/hblBG5oYxWsUOua0tW3C.png?auto=format)

### Frame Rendering Stats [#](#frame-rendering-stats)

[Enable "Frame Rendering Stats"](https://developer.chrome.com/docs/devtools/evaluate-performance/reference/#fps-meter) in DevTools via Rendering settings to see a live view of new animation frames, which are color-coded to differentiate partial updates from fully-dropped frame updates. The reported fps is for fully-presented frames only.

![](https://web-dev.imgix.net/image/eqprBhZUGfb8WYnumQ9ljAxRrA72/xazCKFNCWwjviBGS9JO6.png?auto=format)

### Frame Viewer in DevTools performance profile recordings [#](#frame-viewer-in-devtools-performance-profile-recordings)

The [DevTools Performance panel](https://developer.chrome.com/docs/devtools/evaluate-performance/) has long had a [Frames viewer](https://developer.chrome.com/docs/devtools/evaluate-performance/reference/#frames). However, it had grown a bit out of sync with how the modern rendering pipeline actually works. There has been plenty of recent improvement, even in the latest Chrome Canary, which we think will greatly ease debugging animation issues.

Today you will find that frames in the frames viewer are better aligned with vsync boundaries, and color-coded based on status. There is still not full visualization for all the nuances outlined above, but we're planning to add more in the near future.

![](https://web-dev.imgix.net/image/nM10iAStORWg0YlDbPc2czmpRRr2/ou89sgoFeg2481KVUWmF.png?auto=format)

### Chrome tracing [#](#chrome-tracing)

Finally, with Chrome Tracing, the tool of choice for diving deep into details, you can record a "Web content rendering" trace via the new [Perfetto UI](https://ui.perfetto.dev/) (or `about:tracing`), and dig deep into Chrome's graphics??pipeline. It can be a daunting task, but there are a few things recently added to Chromium to make it easier. You can get an overview of what is available in the [Life of a Frame](https://chromium.googlesource.com/chromium/src/+/HEAD/docs/life_of_a_frame.md) document.

Through trace events you can definitively determine:

-   Which animations are running (using events named `TrackerValidation`).
-   Getting the exact timeline of animation frames (using events named `PipelineReporter`).
-   For janky animation updates, figure out exactly what is blocking your animation from running faster (using the event breakdowns within `PipelineReporter` events).
-   For input-driven animations, see how long it takes to get a visual update (using events named `EventLatency`).

![](https://web-dev.imgix.net/image/eqprBhZUGfb8WYnumQ9ljAxRrA72/O0vDMhhaob9bllU0xRVy.png?auto=format)

## What's next [#](#what's-next)

The [Web Vitals](https://web.dev/vitals/) initiative aims to provide metric and guidance for building great user experiences on the web. [Lab-based](https://web.dev/user-centric-performance-metrics/#in-the-lab) metrics like [Total Blocking Time (TBT)](https://web.dev/tbt/) and [Time to Interactive (TTI)](https://web.dev/tti/) are vital for catching and diagnosing potential interactivity issues. We are planning to design a similar lab-based metric for animation smoothness.

We'll keep you posted as we continue to work through ideas for designing a comprehensive metric based on individual animation frame data.

In the future, we'd also like to design APIs that make it possible to measure animation smoothness performantly for real users in the [field](https://web.dev/user-centric-performance-metrics/#in-the-field) as well as in the lab. Stay tuned for updates there as well!

## Feedback [#](#feedback)

We're excited about all the recent improvements and developer tools that have shipped in Chrome to measure animation smoothness. Please try these tools out, benchmark your animations, and let us know where it leads!

You can send your comments to the [web-vitals-feedback](https://groups.google.com/g/web-vitals-feedback) Google group with "\[Smoothness Metrics]"in the subject line. We're really looking forward to hearing what you think!

Last updated: Nov 3, 2021 [Improve article](https://github.com/GoogleChrome/web.dev/blob/main/src/site/content/en/blog/smoothness/index.md) 
 [https://web.dev/smoothness/](https://web.dev/smoothness/) 
 [https://web.dev/smoothness/](https://web.dev/smoothness/)
