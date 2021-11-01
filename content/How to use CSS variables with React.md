# How to use CSS variables with React
This is a _controversial opinion_, but I rather like CSS-in-JS. 😬🚨

But! I also really like CSS. And I don't believe that using CSS-in-JS absolves you from needing to learn it. You're writing CSS either way! It's just packaged a little bit differently.

No matter where you put your CSS, it behooves you to develop a mastery of the language. Becoming better at CSS will make you a more effective front-end developer.

In this tutorial, we're going to see how to take advantage of one of the most exciting newer developments in CSS: , AKA Custom Properties. We'll see how we can use them in our React apps to improve our workflows and do some pretty fancy stuff.

As a React developer, you might be thinking that you don't need variables in CSS. You have an entire JavaScript engine at your disposal!

There are two reasons to switch to CSS variables in your React app:

1.  The ergonomics are nice.
2.  It unlocks new possibilities! You can do things with CSS variables that are **not possible** with JS.

Let's look at how to use them, and then we'll see what doors get unlocked!

Here's what CSS variables look like:

The funky thing is that they look just like properties; in fact, the reason that they're officially called"CSS Custom Properties" is because they _are_ properties, just like `position` or `color`. We can define our own properties now.

We can attach them to any selector, and they'll be inherited by all descendants, just like `color` or `font-size`. By hanging our custom properties on the root `html` tag, we ensure that they can be accessed anywhere in our app.

Here are some other things to know about CSS variables:

-   You can summon the variable's value with the `var` keyword. Think of it as a getter function. `var(--color-primary)`, in this case, becomes `rebeccapurple`.
-   Custom properties **need** to start with two dashes. This is what differentiates them from traditional CSS properties.
-   They can hold any type of value, not just colors and pixels.
-   You can attach them to any selector, not just the root `html` tag!
-   You can specify a default value if the CSS variable isn't defined: `var(--primary-color, pink)` will fall back to `pink` if necessary.

Let's see what this looks like in React. This tutorial uses [styled-components](https://styled-components.com/), but the instructions should be relatively similar regardless of the CSS-in-JS library.

First, I'm going to assume that you have a file that holds all of your design tokens, something like this:

In a React app, you might import them directly into the components that need them:

Or, you might use a theme:

Here's the same code, but set up using CSS variables

We've created some variables, hung them on the root node, and now we can access them in our components:

This is a nice little win, in my opinion. Being able to access theme values without an import or an inline function is a breath of fresh air. You do lose some static typing benefits—more on this later—but it's a very happy tradeoff in my eyes.

This is a relatively minor difference, though. Let's look at something more interesting…

\[

Link to this heading

## ](#changing-values-not-variables)Changing values, not variables

So let's say we have a Button component.

It looks pretty, but we get feedback that the click target is too small on mobile devices: industry guidelines are that interactive elements should be between [44px and 48px tall](https://medium.com/@zacdicko/size-matters-accessibility-and-touch-targets-56e942adc0cc). We need to bump up the size to make it easier to tap on phones.

Let's walk through a possible solution, _not_ using CSS variables.

We ship this change, and we sleep a little bit better knowing that we've improved the usability of our app.

We quickly learn that our work isn't done, however. Buttons are not the only tappable elements in our apps! There's also text inputs, among others.

Let's update our `TextInput` component as well. To keep things DRY, we'll store our sizes on our theme:

We use those values in both of our components:

This is a significant chunk of CSS to be lugging around to any tappable element!

It turns out, CSS variables offer a very compelling solution to this problem, but it requires a mental model shift.

Instead of imperatively specifying how each component should respond at different breakpoints, what if we passed it a _reactive variable_ that tracked that for us?

With this magic CSS variable, our responsive components get so much simpler:

Inside our components, `height` always points to the same variable, regardless of viewport size. The difference is that _the variable_ changes its value when the window width changes.

This is a pretty wild idea, and it requires looking at responsive design through a different lens, but it's really compelling!

-   By consolidating the breakpoint stuff in a single place, we now have a single source of truth. Before, it was possible for a wayward developer to accidentally delete one of the breakpoints. Now it's packaged into a resilient variable.
-   It lets us be more explicit about why we're doing this. We're giving it a name—`min-tap-target-height`—which communicates _why_ we need to set a `height` value in the first place.
-   It's more declarative! Instead of specifying _how_ each component should change at every specific window size, we're just giving it a value to use, and it'll resize automatically if that value changes.

The ["Principle of Least Knowledge"](http://www.ericfeminella.com/blog/2008/02/02/principle-of-least-knowledge/) is the idea that code should only have access to stuff directly adjacent to it, and not "reach over" into a totally different part of the codebase. I feel like if we squint a little, that same idea applies here.

Another quick example: we can do the same trick for font sizes, so that each viewport has its own scale.

Semantically, this feels nice. Our components pick a size from the scale, and we _swap out the scale_ depending on the viewport size.

\[

Link to this heading

## ](#other-new-possibilities)Other new possibilities

CSS variables open at least 2 other doors:

### \[

Link to this heading

](#animate-any-property)Animate any property

There are some CSS properties that simply can't be animated. If you've ever tried to animate a linear or radial gradient, for example, you've realized pretty quickly that it doesn't work.

With CSS variables, you can animate _any property_, because you aren't applying the transition to the property, you're applying the transition **to the value**.

For example, here's a fun gradient animation, made possible with CSS variables:

Read more about this button in my tutorial, ["Magical Rainbow Gradients"](https://www.joshwcomeau.com/react/rainbow-button/).

### \[

Link to this heading

](#dark-mode-flash-fix)“Dark Mode” flash fix

If you've tried to implement a"Dark mode"variant, you've probably been bitten by this tricky situation: for a brief moment, the wrong colors flash:

'Flash of light-mode' when the page loads.

"Dark Mode" is surprisingly tricky, _especially_ in a server-rendered context (like with Gatsby or Next.js). The problem is that the HTML is generated long before it reaches the user's device, so there's no way to know which color theme the user prefers.

This is a surprisingly tricky problem to solve, and it deserves its own blog post. I'll be publishing something in the next couple weeks about this—be sure to [subscribe](https://www.joshwcomeau.com/subscribe/) so you don't miss it!

In the example above, we hardcode our theme values in a `GlobalStyles` component:

There may be times where you need to access these values in JavaScript.

If you'd like, you can keep storing them in a `constants.js` file. They'll be used to instantiate the CSS variables, but then also imported wherever you need the raw values in JS:

Another idea, though, is to use CSS as the source of truth. You can access the values with a bit of JS:

You can set those values from within JS as well:

Probably the biggest downside to using CSS variables for themes is that there's no way to statically type them (via Typescript or Flow).

In my mind, this isn't a huge deal; I've been on both sides of this river. Having a typed theme object is nice, but I can't say that it's saved me a ton of time. Generally, it's obvious when you mistype the name of a CSS variable, and it's a quick fix.

I think it's important to run compile-time checks on your site, but I think tools like [Chromatic](https://www.chromaticqa.com/) are a much more reliable check. They run in CI and capture any differences in the rendered visual output.

**All of that said:** If type-safety is a must-have, you don't have to give it up! You'd just need to keep your styles in a JS object, and interpolate them in. [This tweet](https://twitter.com/pveyes/status/1249729653591293952) from Fatih Kalifa shows how he set up his types for CSS variables.

CSS variables enjoy healthy browser support amongst the 4 leading browsers, but it's missing IE support:

When using styled-components, you can put variables wherever you want, including within media queries:

CSS variables can't be used anywhere within media queries. There is chatter around letting users describe their own _environment_ variables with `env()`, which would allow for this… But it has yet to materialize.

Despite these drawbacks, CSS variables open a lot of doors. I've used them on this very blog, and overall I've really enjoyed the experience of working with them!

A lot of popular tooling like [Theme UI](https://theme-ui.com/) is built on top of CSS variables. Library authors are recognizing the value they provide.

Whether or not you decide to use CSS variables in your next project, it's worth knowing how to use them. You never know when you'll run into a problem that CSS variables can solve! It's one more tool for the toolbox.

### Last Updated

September 21st, 2021 
 [https://www.joshwcomeau.com/css/css-variables-for-react-devs/](https://www.joshwcomeau.com/css/css-variables-for-react-devs/) 
 [https://www.joshwcomeau.com/css/css-variables-for-react-devs/](https://www.joshwcomeau.com/css/css-variables-for-react-devs/)
