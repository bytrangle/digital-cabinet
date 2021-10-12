# What the heck, z-index??
In CSS, we're given a tool to explicitly control the stacking order of HTML elements: `z-index`. Elements with a higher value will appear on top:

Because `.first.box` has a larger z-index than `.second.box`, it stacks in front. If we remove that z-index declaration, it falls to the back. The code above is editable—give it a shot!

Things aren't always so simple, however. Sometimes, the larger z-index value _doesn't_ win.

Check out what's going on here:

`.tooltip` has a **much** larger z-index than `header`! So why on earth is the header on top?

To unravel this mystery, we'll need to learn about _stacking contexts_, an obscure-yet-fundamental CSS mechanism. In this article, we'll explore what they are, how they work, and how we can use them to our advantage.

If you've ever used image-editing software like Photoshop or Figma, you're probably familiar with the concept of layers:

![](https://www.joshwcomeau.com/_next/image?url=%2Fimages%2Fstacking-contexts%2Fphotoshop-layers.png&w=1080&q=75)

Our image has 3 separate canvases, stacked like pancakes. The bottom layer is a cat photo, with 2 layers on top that add silly details. By flattening these layers, we wind up with a final composition:

![](https://www.joshwcomeau.com/_next/image?url=%2Fimages%2Fstacking-contexts%2Fcat-layers.jpg&w=1080&q=75)

In these programs, we can also _group layers_:

![](https://www.joshwcomeau.com/_next/image?url=%2Fimages%2Fstacking-contexts%2Fphotoshop-groups.png&w=1080&q=75)

Like files in a folder, a group allows us to segment our layers. In terms of stacking order, layers aren't allowed to “intermingle” between groups: All of `dog`'s layers will appear on top of all of `cat`'s layers.

When we export the composition, we don't see the cat at all, since it's behind the dog:

![](https://www.joshwcomeau.com/_next/image?url=%2Fimages%2Fstacking-contexts%2Fdog-layers.jpg&w=1080&q=75)

When it comes to CSS, things work in a similar way: elements are grouped into **stacking contexts**. When we give an element a z-index, that value is only compared _against other elements in the same context_. z-index values are not global.

By default, a plain HTML document will have a single stacking context that encompasses all nodes. But we can create additional contexts!

There are many ways to create stacking contexts, but here's the most common:

By combining these two declarations, a secret mechanism is triggered: a stacking context is created, forming a group around this element and all of its children.

Let's take another look at our problem from above:

We can map out the stacking contexts being created in this snippet:

-   The root context

    -   `<header>`
    -   `<main>`

        -   `<div class="tooltip">`

Our `.tooltip` element has a z-index of 999999, but that value is only relevant within the `<main>` stacking context. It controls whether the tooltip shows up above or below the adjacent `<p>` tag, nothing more.

Meanwhile, in the parent context, `<header>` and `<main>` are compared. Because `<main>` has a smaller z-index, it shows up underneath `<header>`. **All of its children come along for the ride.**

How do we solve our tooltip problem? Well, in this case, we don't actually need to create a stacking context on our `<main>`:

Without a z-index, `<main>` won't create a stacking context. Our hierarchy, then, looks like this:

-   The root context

    -   `<header>`
    -   `<div class="tooltip">`

Because the header and our tooltip are now in the same context, their z-index values face off, and the tooltip emerges as the victor.

**An important distinction:** we're not talking about parent/child relationships here. It doesn't matter that the tooltip is more deeply nested than the header. The browser only cares about stacking contexts.

\[

Link to this heading

## ](#creating-stacking-contexts)Creating stacking contexts

We've seen how we can create a stacking context by combining relative or absolute positioning with `z-index`, but it's not the only way! Here are some others:

-   Setting `opacity` to a value less than `1`
-   Setting `position` to `fixed` or `sticky` (No z-index needed for these values!)
-   Applying a `mix-blend-mode` other than `normal`
-   Adding a `z-index` to a child inside a `display: flex` or `display: grid` container
-   Using `transform`, `filter`, `clip-path`, or `perspective`
-   Using `will-change` with a value like `opacity` or `transform`
-   Explicitly creating a context with `isolation: isolate` (More on this soon!)

There are a few other ways as well. You can find [the full list on MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Positioning/Understanding_z_index/The_stacking_context#the_stacking_context).

**This can lead to some surprising situations.** Check out what's happening here:

`main` doesn't set a z-index anymore, but it uses `will-change`, a property that can create a stacking context all on its own.

\[

Link to this heading

## ](#a-common-misconception-about-z-index)A common misconception about z-index

In order for z-index to work, we need to set `position` to something like `relative` or `absolute`, right?

Not quite. Check out what's happening here:

The second box is lifted above its siblings using `z-index`. There are no `position` declarations anywhere in the snippet, though!

In general, `z-index` only works with "positioned" elements (elements that set `position` to something other than the default “static”). But the Flexbox specification adds an exception: flex children can use `z-index` even if they're statically-positioned.

An earlier version of this post said that all elements that create a stacking context can use `z-index`, but that was incorrect. 😬

There's a Weird Thing here, and I think it's worth pondering about for a minute or two.

In our Photoshop analogy, there is a clear distinction between groups and layers. All of the visual elements are layers, and groups can be conjured as structural helpers to contain them. They are distinct ideas.

On the web, however, the distinction is a bit less clear. Every element that uses z-index must _also_ create a stacking context.

When we decide to give an element a z-index, our goal is typically to lift or lower that element above/below some other element in the parent stacking context. _We aren't intending to produce a stacking context on that element!_ But it's important that we consider it.

When a stacking context is created, it “flattens” all of its descendants. Those children can still be rearranged internally, but we've essentially locked those children in.

Let's take another look at the markup from earlier:

By default, HTML elements will be stacked according to their DOM order. Without any CSS interference, `main` will render on top of `header`.

We can lift `header` to the front by giving it a z-index, but not without flattening all of its children. This mechanism is what led to the bug we discussed earlier.

We shouldn't think of `z-index` purely as a way to change an element's order. We should _also_ think of it as a way to form a group around that element's children. z-index won't work unless a group is formed.

\[

Link to this heading

## ](#airtight-abstractions-with-isolation)Airtight abstractions with “isolation”

One of my favourite CSS properties is also one of the most obscure. I'd like to introduce you to the `isolation` property, a hidden gem in the language.

Here's how you'd use it:

When we apply this declaration to an element, it does precisely 1 thing: it creates a new stacking context.

With so many different ways to create a stacking context, why do we need another one? Well, with every other method, stacking contexts are created implicitly, as the result of some other change. `isolation` creates a stacking context in the purest way possible:

-   No need to prescribe a z-index value
-   Can be used on statically-positioned elements
-   Doesn't affect the child's rendering in any way

This is **incredibly useful**, since it lets us "seal off" an element's children.

Let's look at an example. Recently, I built this neat envelope component. **Hover or focus** to see it open:

It consists of several layers:

I packaged this effect up in a React component, `<Envelope>`. It looks something like this (inline styles used for brevity):

(If you're wondering why `Flap` has a dynamic z-index, it's because it needs to shift behind the letter when the envelope is open.)

A good React component is sealed off from its environment, like a spacesuit. _This_ spacesuit, however, has sprung a leak. Check out what happens when I use it near a `<header>` with `z-index: 3`:

Our `<Envelope>` component wraps the 4 layers in a div, but it doesn't create a stacking context. As a result, those layers can become “intertwined” with other components, like the world's most boring game of Twister.

By using `isolation: isolate` on the top-level element within `<Envelope>`, **we guarantee that it'll be positioned as a group**:

Why not create a stacking context the old-fashioned way, with `position: relative; z-index: 1`? Well, React components are meant to be reusable; is `1` really the right z-index value for this component in _all_ circumstances? The beauty of `isolation` is that it keeps our components unopinionated and flexible.

More and more, **I'm starting to believe that z-index is an escape hatch**, similar to `!important`. This is one trick that allows us to control stacking order without pulling the big red z-index lever.

I'm working on a follow-up tutorial where we look at some other tricks to keep z-index inflation down. Watch this space!

\[

Link to this heading

## ](#debugging-stacking-context-issues)Debugging stacking context issues

Unfortunately, I haven't found much tooling to help debug stacking-context issues.

Microsoft Edge has an interesting “[3D view](https://docs.microsoft.com/en-us/microsoft-edge/devtools-guide-chromium/3d-view/)” that allows us to view stacking contexts:

This is an ambitious idea, but honestly I find it pretty overwhelming. It's hard to locate a specific element in this view, and I don't really feel like it helps me understand the stacking contexts in my app.

There's one other neat trick you can use sometimes: `offsetParent`.

`offsetParent` returns the closest ancestor rendered with a `position` value other than `static`. It crawls up the tree looking for relative / absolute / fixed / sticky ancestors.

This is not a perfect solution. Not all stacking contexts use positioned layout, and not all positioned elements create a stacking context! That said, in practice, there does tend to be a pretty strong correlation between these two things. At the very least, it's a starting point.

If you know of any tooling that can help here (or if you create one!), [let me know on Twitter](https://twitter.com/JoshWComeau)

**Update:** Felix Becker reached out to share a [VSCode extension that highlights when stacking contexts are created](https://marketplace.visualstudio.com/items?itemName=felixfbecker.css-stacking-contexts):

![](https://www.joshwcomeau.com/images/vscode-stacking-contexts.gif)

This extension works on .css and .scss files (no CSS-in-JS support).

**Update 2:** Giuseppe Gurgone reached out to let me know about this [Chrome extension](https://chrome.google.com/webstore/detail/z-context/jigamimbjojkdgnlldajknogfgncplbh) which adds a new “z-index” pane to the devtools.

**Update 3:** Andrea Dragotta created an _incredible_ browser extension that adds a bunch of super-important information about z-index and stacking contexts:

![](https://www.joshwcomeau.com/images/chrome-stacking-context.png)

This is an **awesome** tool, and I've been using it regularly. Install CSS Stacking Context Inspector:

Stacking contexts are a good example of how CSS is built on "hidden mechanisms". You can spend years building interfaces with CSS without knowing that they exist.

Unless you explicitly take the time to learn about these mechanisms, your mental model will always be missing pieces. And if your mental model is even _slightly_ misaligned, it's only a matter of time until that discrepancy causes problems.

CSS doesn't have warnings or error messages. When something surprising happens, there's no clear "next step" to figure out what went wrong. These disruptions take us out of flow state and shake our confidence. I think this is why so many front-end developers don't enjoy writing CSS.

Once you build up an intuition for the language, though, CSS becomes an absolute joy. I _love_ writing CSS nowadays.

My goal for 2021 is to help other developers discover this joy. I'm creating a comprehensive self-paced online course that explains how CSS works at a deeper level, and teaches the practical skills I use every day to build all kinds of user interfaces.

It's called [“CSS for JavaScript Developers”](https://css-for-js.dev/), and it's just been released to the public. 😄

### Last Updated

September 28th, 2021 
 [https://www.joshwcomeau.com/css/stacking-contexts/](https://www.joshwcomeau.com/css/stacking-contexts/) 
 [https://www.joshwcomeau.com/css/stacking-contexts/](https://www.joshwcomeau.com/css/stacking-contexts/)
