# The King vs. Pawn Game of UI Design – A List Apart
If you want to improve your UI design skills, have you tried looking at chess? I know it sounds contrived, but hear me out. I’m going to take a concept from chess and use it to build a toolkit of UI design strategies. By the end, we’ll have covered color, typography, lighting and shadows, and more.

Article Continues Below

But it all starts with rooks and pawns.

I want you to think back to the first time you ever played chess (if you’ve never played chess, humor me for a second—and no biggie; you will still understand this article). If your experience was anything like mine, your friend set up the board like this:

![](https://alistapart.com/wp-content/uploads/2018/01/fig1-1.png?w=423&resize=423%2C424)

And you got your explanation of all the pieces. _This one’s a pawn and it moves like this, and this one is a rook and it moves like this, but the knight goes like this or this—still with me?—and the bishop moves diagonally, and the king can only do this, but the queen is your best piece, like a combo of the rook and the bishop. OK, want to play?_

This is probably the most common way of explaining chess, and it’s enough to make me hate board games forever. I don’t want to sit through an arbitrary lecture. I want to _play_.

One particular chess player happens to agree with me. His name is Josh Waitzkin, and he’s actually pretty good. Not only at chess (where he’s a grandmaster), but also at Tai Chi Push Hands (he’s a world champion) and Brazilian Jiu Jitsu (he’s the first black belt under 5x world champion Marcelo Garcia). Now he trains financiers to go from the top 1% to the top .01% in their profession.

Point is: _this dude knows a lot about getting good at stuff._

Now here’s the crazy part. When Josh teaches you chess, the board looks like _this_:

![](https://alistapart.com/wp-content/uploads/2018/01/fig2-1.png?w=368&resize=368%2C367)

King vs. King and Pawn

Whoa.

Compared to what we saw above, this is _stupidly simple_.

And, if you know how to play chess, it’s even more mind-blowing that someone would start teaching with this board. In the actual game of chess, you _never_ see a board like this. Someone would have won _long_ ago. This is the chess equivalent of a street fight where both guys break every bone in their body, dislocate both their arms, can hardly see out of their swollen eyes, yet continue to fight for another half-hour.

What gives?

Here’s Josh’s thinking: _when you strip the game down to its core, everything you learn is a universal principle_.

That sounds pretty lofty, but I think it makes sense when you consider it. There are lots of things to distract a beginning chess player by a fully-loaded board, but _everything you start learning_ in a king-pawn situation is fundamentally important to chess:

-   using two pieces to apply pressure together;
-   which spaces are “hot”;
-   and the difference between driving for a _checkmate_ and a _draw_.

Are you wondering if I’m ever going to start talking about design? Glad you asked.

## The simplest possible scenario[#section2](#section2)

What if, instead of trying to design an _entire page with dozens of elements_ (nav, text, input controls, a logo, etc.), we consciously started by designing the _simplest thing possible_? We deliberately limit the playing field to _one, tiny thing_ and see what we learn? Let’s try.

What is the simplest possible element? I vote that it’s a button.

![](https://alistapart.com/wp-content/uploads/2018/01/fig3.png?w=960)

This is the most basic, default button I could muster. It’s Helvetica (default font) with a 16px font size (pretty default) on a plain, Sketch-default-blue rectangle. It’s 40px tall (nice, round number) and has 20px of horizontal padding on each side.

So yeah, I’ve already made a bunch of design decisions, but can we agree I basically just used default values instead of making decisions for principled, design-related reasons?

Now let’s start playing with this button. What properties are modifiable here?

-   the font (and text styling)
-   the color
-   the border radius
-   the border
-   the shadows

These are just the first things that come to my mind. There are even more, of course.

## Typography[#section3](#section3)

Playing with the font is a pretty easy place to start.

![](https://alistapart.com/wp-content/uploads/2018/01/fig4.png?w=960)

Blown up to show font detail.

Now I’ve changed the font to Moon ([available for free on Behance for personal use](https://www.behance.net/gallery/23468357/Moon-Free-Font?isa0=1)). It’s rounded and soft, unlike Helvetica, which felt a little more squared-off—or at least not as overtly friendly.

The funny thing is: do you see how the perfectly square edges now look a tad awkward with the rounded font?

Let’s round the corners a bit.

![](https://alistapart.com/wp-content/uploads/2018/01/fig5.png?w=960)

Bam. Nice. That’s a 3px border radius.

But that’s kind of weird, isn’t it? We adjusted the border radius of a button because of the _shape of the letterforms_ in our font. I wouldn’t want you thinking fonts are just loosey-goosey works of art that only work when you say the right incantations.

No, _fonts are shapes_. Shapes have connotations. It’s not rocket science.

Here’s another popular font, DIN.

![](https://alistapart.com/wp-content/uploads/2018/01/fig6.png?w=960)

With its squared edges, DIN is a clean and solid workhorse font.

Specifically, this is a version called DIN 2014 ([available for cheap on Typekit](https://typekit.com/fonts/din-2014)). It’s the epitome of a squared-off-but-still-readable font. A bit harsh and no-nonsense, but in a bureaucratic way.

It’s the official font of the German government, and it looks the part.

So let’s test our working hypothesis with DIN.

![](https://alistapart.com/wp-content/uploads/2018/01/fig7.png?w=960)

How does DIN look with those rounded corners?

Well, we need to compare it to square corners now, don’t we?

![](https://alistapart.com/wp-content/uploads/2018/01/fig8.png?w=960)

Ahhh, the squared-off corners are better here. It’s a much more consistent feel.

Now look at our two buttons with their separate fonts. Which is more readable? I think Moon has a slight advantage here. DIN’s letters just look too cramped by comparison. Let’s add a bit of letter-spacing.

![](https://alistapart.com/wp-content/uploads/2018/01/fig9.png?w=960)

When we add some letter-spacing, it’s far more relaxed.

This is a key law of typography: _always letter-space your uppercase text_. Why? Because unless a font _doesn’t_ have lowercase characters, it was designed for sentence-case reading, and characters in uppercase words will ALWAYS appear too cramped. (Moon is the special exception here—it only has uppercase characters, and notice how the letter-spacing is _built into_ the font.)

We’ll review later, but so far we’ve noticed two things that apply not just to buttons, but to all elements:

-   Rounded fonts go better with rounded shapes; squared-off fonts with squared-off shapes.
-   Fonts designed for sentence case should be letter-spaced when used in words that are all uppercase.

Let’s keep moving for now.

## Color[#section4](#section4)

Seeing the plain default Sketch blue is annoying me. It’s _begging_ to be changed into something that matches the typefaces we’re using.

How can a _color_ match a _font_? Well, I’ll hand it to you. This one _is_ a bit more loosey-goosey.

For our Moon button, we want something a bit more friendly. To me, a staid blue says _default, unstyled, trustworthy, takes-no-risks, design-by-committee_. How do you inject some fun into it?

Well, like all problems of [modifying color](https://medium.com/@erikdkennedy/color-in-ui-design-a-practical-framework-e18cacd97f9e), it helps to think in the [HSB color system](https://learnui.design/blog/the-hsb-color-system-practicioners-primer.html) (hue, saturation, and brightness). When we boil color down to three intuitive numbers, we give ourselves levers to pull.

For instance, let’s look at hue. We have two directions we can push hue: down to aqua or up to indigo. Which sounds more in line with Moon? To me, aqua does. A bit less staid, a bit more Caribbean sea. Let’s try it. We’ll move the hue to 180° or so.

![](https://alistapart.com/wp-content/uploads/2018/01/fig10.png?w=960)

Ah, Moon Button, now you’ve got a beach vibe going on. You’re a vibrant sea foam!

This is a critical lesson about color. “Blue” is not a monolith; it’s a starting point. I’ve [taught hundreds of students UI design](http://learnui.design/), and this comes up again and again: just because blue was one color in kindergarten doesn’t mean that we can’t find interesting variations around it as designers.

![](https://alistapart.com/wp-content/uploads/2018/01/fig11.png?w=960)

“Blue” is not a monolith. Variations are listed in HSB, with CSS color names given below each swatch.

Aqua is a great variation with a much cooler feel, but it’s also much harder to read that white text. So now we have another problem to fix.

“Hard to read” is actually a numerically-specific property. The World Wide Web Consortium has published [guidelines for contrast between text and background](https://www.w3.org/TR/WCAG21/#contrast-enhanced), and if we use a tool to test those, we find we’re lacking in a couple departments.

![](https://alistapart.com/wp-content/uploads/2018/01/fig12.png?w=960)

White text on an aqua button doesn’t provide enough contrast, failing to pass either AA or AAA WCAG recommendations.

According to [Stark](http://www.getstark.co/) (which is my preferred Sketch plugin for checking contrast—check out Lea Verou’s [Contrast Ratio](https://leaverou.github.io/contrast-ratio/) for a similar web-based tool), we’ve failed our contrast guidelines across the board!

How do you make the white text more legible against the aqua button? Let’s think of our HSB properties again.

-   _Brightness._ Let’s decrease it. That much should be obvious.
-   _Saturation._ We’re going to increase it. Why? Because we’re contrasting with white text, and white has a saturation of zero. So a higher saturation will naturally stand out more.
-   _Hue._ We’ll leave this alone since we like its vibe. But if the contrast continued to be too low, you could lower the aqua’s luminosity by [shifting its hue up toward blue](https://medium.com/@erikdkennedy/color-in-ui-design-a-practical-framework-e18cacd97f9e).

So now, we’ve got a teal button:

![](https://alistapart.com/wp-content/uploads/2018/01/fig13.png?w=960)

Much better?

![](https://alistapart.com/wp-content/uploads/2018/01/fig14.png?w=960)

Much better.

For what it’s worth, I’m not particularly concerned about missing the AAA standard here. [WCAG developed the levels](https://www.w3.org/TR/WCAG21/#contrast-enhanced) as relative descriptors of how much contrast there is, not as an absolute benchmark of, say, some particular percentage of people to being able to read the text. The gold standard is—as always—to test with real people. AAA is best to hit, but at times, AA may be as good as you’re going to get with the colors you have to work with.

Some of the ideas we’ve used to make a button’s blue a bit more fun _and_ legible against white are actually deeper lessons about color that apply to almost everything else you design:

-   Think in HSB, as it gives you intuitive levers to pull when modifying color.
-   If you like the general feel of a color, shifting the hue in either direction can be a baseline for getting interesting variations on it (e.g., we wanted to spice up the default blue, but not by, say, changing it to red).
-   Modify saturation and brightness at the same time (but always in opposite directions) to increase or decrease contrast.

OK, now let’s switch over to our DIN button. What color goes with its harsh edges and squared-off feel?

The first thing that comes to mind is black.

![](https://alistapart.com/wp-content/uploads/2018/01/fig15.png?w=960)

But let’s keep brainstorming. Maybe a stark red would also work.

![](https://alistapart.com/wp-content/uploads/2018/01/fig16.png?w=960)

Or even a construction-grade orange.

![](https://alistapart.com/wp-content/uploads/2018/01/fig17.png?w=960)

(But not the red and orange together. Yikes! In general, two adjacent hues with high saturations will not look great next to each other.)

Now, ignoring that the text of this is “Learn More” and a button like this probably doesn’t need to be _blaze_ orange, I want you to pay attention to the colors I’m picking. We’re trying to maintain consistency with the official-y, squared-off DIN. So the colors we go to naturally have some of the same connotations: _engineered, decisive, no funny business_.

Sure, this match-a-color-and-a-font business is _more_ subjective, but there’s something solid to it: note that the words I used to describe the colors (“stark” and “construction-grade”) apply equally well to DIN—a fact I am only noticing now, not something done intentionally.

Want to match a color with a font? This is another lesson applicable to all of branding. It’s best to _start with adjectives/emotions_, then match everything to those. Practically by accident, we’ve uncovered something fundamental in the branding design process.

## Shadows[#section5](#section5)

Let’s shift gears to work with _shadows_ for a bit.

There are a couple directions we could go with shadows, but the two main categories are (for lack of better terms):

-   realistic shadows;
-   and cartoon-y shadows.

Here’s an example of each:

![](https://alistapart.com/wp-content/uploads/2018/01/fig18.png?w=960)

The top button’s shadow is more _photorealistic_. It behaves like a shadow in the real world.

The bottom button’s shadow is a bit lower-fidelity. It shows that the button is raised up, but it’s a cartoon version, with a slightly unrealistic, idealized bottom edge—and without a normal shadow, which would be present in the real world.

The bottom works better for the button we’re crafting. The smoothness, the friendliness, the cartoon fidelity—it all goes together.

As for our DIN button?

![](https://alistapart.com/wp-content/uploads/2018/01/fig20.png?w=960)

I’m more ambivalent here. Maybe the shadow is for a hover state, à la [Material Design](https://material.io/guidelines/components/buttons.html#buttons-raised-buttons)?

In any case, with a black background, a darkened bottom edge is impossible—you can’t get any darker than black.

By the way, you may not have noticed it above, but the black button has a much stronger shadow. Compare:

![](https://alistapart.com/wp-content/uploads/2018/01/fig21.jpg?w=960)

The teal button’s shadow is 30%-opacity black, shifted 1 pixel down on the y-axis, with a 2-pixel blur (0 1px 2px). The black button’s is 50%-opacity black, shifted 2 pixels down on the y-axis, with a 4-pixel blur (0 2px 4px). What’s more, the stronger shadow looks _awful_ on the teal button.

![](https://alistapart.com/wp-content/uploads/2018/01/fig22.png?w=960)

Why is that? The answer, like so many questions that involve color, is in _luminosity_. When we put the button’s background in luminosity blend mode, converting it to a gray of equal natural lightness, we see something interesting.

![](https://alistapart.com/wp-content/uploads/2018/01/fig23.png?w=960)

The shadow, at its darkest, is basically as dark as the button itself. Or, at least, the rate of change of luminosity is steady between each row of pixels.

![](https://alistapart.com/wp-content/uploads/2018/01/fig24.png?w=960)

The top row is the button itself, not shadow.

Shadows that are too close to the luminosity of their element’s backgrounds will appear too strong. And while this may sound like an overly specific lesson, it’s actually broadly applicable across elements. You know where else you see it?

## Borders[#section6](#section6)

Let’s put a border on our teal button.

![](https://alistapart.com/wp-content/uploads/2018/01/fig25.png?w=960)

Now the way I’ve added this border is something that a bunch of people have thought of: make the border translucent black so that it works _on any background color_. In this case, I’ve used a single-pixel-wide border of 20%-opacity black.

However, if I switch the background color to a more standard blue, which is naturally a lot less luminous, that border all but disappears.

![](https://alistapart.com/wp-content/uploads/2018/01/fig26.png?w=960)

In fact, to see it on blue just as much as you can see it on teal, you’ve got to crank up black’s opacity to something like 50%.

![](https://alistapart.com/wp-content/uploads/2018/01/fig27.png?w=960)

This is a generalizable rule: when you want to _layer black on another color_, it needs to be a more opaque black to show up the same amount on less luminous background colors. Where else would you apply this idea?

![](https://alistapart.com/wp-content/uploads/2018/01/fig28.png?w=960)

Spoiler alert: _shadows_!

Each of these buttons has the same shadow (0 2px 3px) except for different opacities. The top two buttons’ shadows have opacity 20%, and the bottom two have opacity 40%. Note how what’s fine on a white background (top left) is hardly noticeable on a dark background (top right). And what’s too dark on a white background (lower left) works fine on a dark background (lower right).

## Icons[#section7](#section7)

I want to change gears one more time and talk about icons.

![](https://alistapart.com/wp-content/uploads/2018/01/fig29.png?w=960)

Here’s the download icon from Font Awesome, my least favorite icon set of all time.

![](https://alistapart.com/wp-content/uploads/2018/01/fig30.png?w=960)

I dislike it, not only because it’s completely overused, but also because the icons are really _bubbly and soft_. Yet most of the time, they’re used in _clean, crisp websites_. They just don’t fit.

You can see it works better with a soft, rounded font. I’m less opposed to _this_ sort of thing.

![](https://alistapart.com/wp-content/uploads/2018/01/fig31.png?w=960)

But there’s still a problem: _the icon has these insanely small details_! The dots are never going to show up at size, and even the space between the arrow and the disk is a fraction of a pixel in practice. Compared to the letterforms, it doesn’t look like quite the same style.

But what good is my complaining if I don’t offer a solution?

Let’s create a new take on the “download” icon, but with some different guiding principles:

-   We’ll use a _stroke weight_ that’s equivalent (or basically equivalent) to the _text weight_.
-   We’ll use corner radii that are similar to the corner radii of our font: squared off for DIN, rounded for Moon.
-   We’ll use a simpler icon shape so the differences are easy to see.

Let’s see how it looks:

![](https://alistapart.com/wp-content/uploads/2018/01/fig32.png?w=960)

I call this “drawing with the same pen.” Each of these icons looks like it could basically be a character in the font used in the button. And _that’s_ the point here. I’m not saying all icons will appear this way, but for an icon that appears inline with text like this, it’s a fantastic rule of thumb.

## Wrapping it up[#section8](#section8)

Now this is just the beginning. Buttons can take _all kinds_ of styles.

![](https://alistapart.com/wp-content/uploads/2018/01/fig33.png?w=960)

But we’ve got a good start here considering we _designed just two buttons_. In doing so, we covered a bunch of the things that are focal points of my day-to-day work as a UI designer:

-   lighting and shadows;
-   color;
-   typography;
-   consistency;
-   and brand.

And the lessons we’ve learned in those areas are fundamental to the _entirety_ of UI design, not just one element. Recall:

-   Letterforms are shapes. You can analyze fonts as sets of shapes, not simply as works of art.
-   You should letter-space uppercase text, since most fonts were designed for sentence case.
-   Think in HSB to modify colors.
-   You can find more interesting variations on a “basic” color (like a CSS default shade of blue or red) by tweaking the hue in either direction.
-   Saturation and brightness are levers that you can move in opposite directions to control luminosity.
-   Find colors that match the same descriptors that you would give your typeface and your overall brand.
-   Use darker shadows or black borders on darker backgrounds—and vice versa.
-   For inline icons, choose or design them to appear as though they were drawn with the same pen as the font you’re using.

You can thank Josh Waitzkin for making me a pedant. I know, you just read an entire essay on _buttons_. But next time you’re struggling with a redesign or even something you’re designing from scratch, try stripping out all the elements that you _think_ you should be including already, and just mess around with the simplest players on the board. Get a feel for the fundamentals, and go from there.

Weird? Sure. But if it’s good enough for a grandmaster, I’ll take it. 
 [https://alistapart.com/article/the-king-vs-pawn-game-of-ui-design/](https://alistapart.com/article/the-king-vs-pawn-game-of-ui-design/) 
 [https://alistapart.com/article/the-king-vs-pawn-game-of-ui-design/](https://alistapart.com/article/the-king-vs-pawn-game-of-ui-design/)
