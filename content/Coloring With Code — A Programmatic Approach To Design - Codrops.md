# Coloring With Code — A Programmatic Approach To Design - Codrops
![](https://i7x7p5b7.stackpathcdn.com/codrops/wp-content/uploads/2021/12/ColorPalettes.png)

From our sponsor: [Looking for an intuitive whiteboard style project management tool? Give Shortcut a try for free.](https://srv.buysellads.com/ads/long/x/TH4C2IZ3TTTTTT4FGXHN4TTTTTT6GQNPK6TTTTTTEDZABYVTTTTTTESLPVSNKSJ7537HLRSWP36FP2QYVQCI4WZ727CT)

Color is powerful — it can radically shift our mood, inspire us, and help us express ourselves in a way that few other things can. It is a fundamental building block of design, but it can also be a _little intimidating_.

Often when given the opportunity to play with color, we freeze. Choosing _just_ _one \_can be enough to trigger a kind of iridescent nightmare, not to mention combining lots of them! The options are infinite, and the “rules” somewhat hazy… a potentially overwhelming combination, particularly for those of us used to the _(often)\_ more definite world of code.

In this tutorial, we will be learning how to use familiar tools — a text editor and web browser — to make the process of creating striking color palettes a _lot_ less scary and _(most importantly!)_ fun. 

Let’s do it! 

## Intended audience

This article is perfect for folks who already have a good grasp of HTML, CSS _(knowledge of HSL and RGB colors will be helpful)_, and JavaScript. If you love to make things on the web, but often reach for a pre-curated selection or an automatic “generator” when adding color, you’re in the right spot. 

## Tutorial format 

We won’t be building one single, strictly defined project here. Instead, we will be learning to create three special JavaScript functions, all uniquely suited to generating _beautiful_ color palettes. Once written, these functions will form a solid foundation for our very own suite of programmatic color tools, which can be carried from project to project and iterated on/personalized over time.

## A short introduction to LCH color

Throughout this tutorial, we will be working almost exclusively with LCH colors. LCH stands for lightness _(how dark/light a color is)_, chroma _(how vivid/saturated a color is)_, and hue _(whether a color is red, green, blue…)_.

In short, LCH is a way of representing color just like RGB or HSL, but with a few notable advantages — the most important for this tutorial being its _perceptual uniformity_. I know this sounds a little scary, but I promise it’s not; let me show you what it means!

To start, take a look at these two pairs of **HSL** colors: 

![](https://i7x7p5b7.stackpathcdn.com/codrops/wp-content/uploads/2021/12/hsl-hue-shift-comparison.png)

Notice how, despite both the top and bottom pairs having the same 20-degree hue variance, the difference we _actually_ see is wildly different? This imbalance exists because HSL is _not_ _perceptually uniform_.

Now take a look at the same experiment, this time using **LCH** colors:

![](https://i7x7p5b7.stackpathcdn.com/codrops/wp-content/uploads/2021/12/lch-hue-shift.png)

👋 — _hue values do not align perfectly between HSL and LCH. In LCH, a hue of 0 is more pink, while in HSL, it is a pure red. _

Ah, much better! The change in hue seen here is far more balanced because LCH _is_ _perceptually uniform_. 

Next, let’s take a peek at another two **HSL** colors: 

![](https://i7x7p5b7.stackpathcdn.com/codrops/wp-content/uploads/2021/12/hs-lightness-comparison.png)

These two colors have identical lightness values but appear very different to our human eyes. The yellow on the left is far “brighter” than the blue on the right.

Here’s a similar setup, but with **LCH** colors rather than HSL: 

![](https://i7x7p5b7.stackpathcdn.com/codrops/wp-content/uploads/2021/12/lch-lightness-comparison.png)

That’s more like it! As demonstrated by the image above, lightness values in LCH are far more accurate representations of what we perceive — this, in combination with LCH’s uniform hue distribution, will make our lives a \_lot \_easier when creating harmonious color palettes. 

For now, this is all we need to know, but if you would like to learn more, I highly recommend [this article](https://lea.verou.me/2020/04/lch-colors-in-css-what-why-and-how/) by [Lea Verou](https://twitter.com/LeaVerou). 

_**👋** — We will be using a library in this tutorial, but native LCH support is heading to the browser! In fact, [it is already in Safari, with other browsers currently working on it](https://caniuse.com/css-lch-lab)._

## Following along

Before we write any code, we need a simple development environment. This setup is entirely your choice, but I recommend spinning up a CodePen to follow along with the examples, then moving to a custom setup/repository as and when you need to. Really, all we need here is an HTML/JavaScript file, and we will be using [Skypack](https://www.skypack.dev/) for all library imports, so there’s no need for any fancy build processes, etc.

## Function #1 — “Scientific”

OK! First off, we are generating colors using “traditional” color theory. To get started with this method, let’s take a look at something called a color wheel:

![](https://i7x7p5b7.stackpathcdn.com/codrops/wp-content/uploads/2021/12/color-wheel.png)

Look familiar?

A color wheel is a visual representation of the hues in a color space. The wheel above represents the hues in LCH, incrementing in 30-degree steps, from 0 to 360-degrees — a well-established format. In fact, for hundreds of years, we have used wheels to find colors that work well together!

Here’s how:

We start with a base color. Then, we rotate around the wheel by a certain number of degrees a certain number of times; for a perfect _complementary_ palette, we move 180 degrees once: 

![](https://i7x7p5b7.stackpathcdn.com/codrops/wp-content/uploads/2021/12/complementary-wheel.png)

Lovely! For a _triadic_ palette, we move 120 degrees, twice: 

![](https://i7x7p5b7.stackpathcdn.com/codrops/wp-content/uploads/2021/12/triadic-wheel.png)

See where this is going? By altering the number of steps and rotation amount, we can create several “classic” color palettes:

![](https://i7x7p5b7.stackpathcdn.com/codrops/wp-content/uploads/2021/12/all-palettes.png)

Cool! Let’s take this method and turn it into 1s and 0s.

To keep things moving throughout this tutorial, I’ll show you the code, then break it down step-by-step:

### The code

    function adjustHue(val) {
      if (val < 0) val += Math.ceil(-val / 360) * 360;

      return val % 360;
    }

    function createScientificPalettes(baseColor) {
      const targetHueSteps = {
        analogous: [0, 30, 60],
        triadic: [0, 120, 240],
        tetradic: [0, 90, 180, 270],
        complementary: [0, 180],
        splitComplementary: [0, 150, 210]
      };

      const palettes = {};

      for (const type of Object.keys(targetHueSteps)) {
        palettes[type] = targetHueSteps[type].map((step) => ({
          l: baseColor.l,
          c: baseColor.c,
          h: adjustHue(baseColor.h + step),
          mode: "lch"
        }));
      }

      return palettes;
    }

To break this down:

1.  Define a function `createScientificPalettes` that expects a single `baseColor` argument.
2.  Define the hue steps for several “classic” color palettes.
3.  For each palette type: iterate over each hue step, add the step value to the base hue, and store the resulting color — making sure its `chroma` and `lightness` values match the base. Use a small `adjustHue` function to ensure all hue values are between 0 and 360.
4.  Return the palettes in LCH format.  

### Usage

Awesome! We can call our `createScientificPalettes` function like so:

    const baseColor = {
      l: 50,
      c: 100,
      h: 0,
      mode: "lch"
    };

    const palettes = createScientificPalettes(baseColor); 

In the example above, we pass a `baseColor` object, and the function returns a variety of palettes, all centered around that base. Thanks to LCH, the lightness and intensity of the colors in these palettes will be visually consistent, and the hue modulations highly accurate; this is great for accessibility, as, unlike other color spaces, each color in the palette will have the same _perceived_ contrast. 

Cool! All that’s left to do now is convert the LCH colors to a more usable format. To do so, we can use [Culori](https://culorijs.org/) — an excellent color utility library used throughout this tutorial — to transform the LCH objects to, say, HEX:

    import { formatHex } from "https://cdn.skypack.dev/culori@2.0.0";

    const baseColor = {
      l: 50,
      c: 100,
      h: 0,
      mode: "lch"
    };

    const palettes = createScientificPalettes(baseColor);
    const triadicHex = palettes.triadic.map((colorLCH) => formatHex(colorLCH)); 

_**👋** — Culori requires an explicit `mode` on all color objects. You will notice this included in the code examples throughout this tutorial. _

For our first function, that’s it! Let’s take a look at how we can use it in real life.

### Practical application

One benefit of creating our color palettes with code \_(programmatically) \_is that it makes rapid prototyping/experimentation super easy. Say, for example, we were working on a design and got completely stuck with what color palette to use. Using our `createScientificPalettes` function, alongside some simple CSS custom properties, we can generate near-infinite palettes and test them with our UI in real-time! 

Here’s a CodePen to demonstrate:

### Challenge

Right now, our `createScientificPalettes` function accounts for all palette types, apart from \_monochromatic. C_an you update it to support [monochromatic palettes](https://en.wikipedia.org/wiki/Monochromatic_color)? 

## Function #2 — “Discovery”

So, this function is similar to the previous one but with _quite_ a twist. We are still generating “classic” color combinations, but rather than calculating them scientifically _(adding set “steps” to the hue of a base color), \_we are \_discovering_ them! That’s right; our discovery function will take an array of colors and find the best palette matches within it — analogous, triadic, tetradic, etc. 

Here’s an illustrated example:

![](https://i7x7p5b7.stackpathcdn.com/codrops/wp-content/uploads/2021/12/discovery-example-1-800x463.png)

Using this function, we can discover beautiful palettes within images, color datasets, and more! Let’s see how it works.

### The code

    import {
      nearest,
      differenceEuclidean,
    } from "https://cdn.skypack.dev/culori@2.0.0";

    function isColorEqual(c1, c2) {
      return c1.h === c2.h && c1.l === c2.l && c1.c === c2.c;
    }

    function discoverPalettes(colors) {
      const palettes = {};

      for (const color of colors) {
        const targetPalettes = createScientificPalettes(color);

        for (const paletteType of Object.keys(targetPalettes)) {
          const palette = [];
          let variance = 0;

          for (const targetColor of targetPalettes[paletteType]) {
            
            const availableColors = colors.filter(
              (color1) => !palette.some((color2) => isColorEqual(color1, color2))
            );

            const match = nearest(
              availableColors,
              differenceEuclidean("lch")
            )(targetColor)[0];

            variance += differenceEuclidean("lch")(targetColor, match);

            palette.push(match);
          }

          if (!palettes[paletteType] || variance < palettes[paletteType].variance) {
            palettes[paletteType] = {
              colors: palette,
              variance
            };
          }
        }
      }

      return palettes;
    }

To break this down: 

1.  Pass an array of LCH colors to the `discoverPalettes` function. 
2.  For every color, create the “optimum” target palettes based on it using our `createScientificPalettes` function. 
3.  For every palette, find the closest match for each of its colors. We calculate color matches here using Culori’s [nearest](https://culorijs.org/api/#nearest) and [differenceEuclidian](https://culorijs.org/api/#differenceEuclidean) functions. 
4.  Determine how similar/different the “discovered” palette is to the target. Keep a record of the closest palette matches. 
5.  Return the closest match of each palette type! 

Awesome! This method is super exciting, as it operates much as a human would — looking at a selection of colors and finding the best (but never _perfect_) palettes; this is great, as sometimes, purely mathematic color theory can appear a touch sterile/predictable. 

### Usage 

As a quick reference, here’s how we could use `discoverPalettes` with an array of HEX colors: 

    import {
      converter,
    } from "https://cdn.skypack.dev/culori@2.0.0";

    const toLCH = converter("lch");

    const baseColors = [
      "#FFB97A",
      "#FF957C",
      "#FF727F",
      "#FF5083",
      "#F02F87",
      "#C70084",
      "#9A007F",
      "#6A0076",
      "#33006B"
    ];

    const baseColorsLCH = baseColors.map((color) => toLCH(color));

    const palettes = discoverPalettes(baseColorsLCH); 

**\_\_**👋**\_\_** **—** _ `discoverPalettes` expects a minimum of four colors to function correctly._

### Practical application

One of the most compelling aspects of `discoverPalettes` is its ability to pull coherent color combinations out of just about any source. Here it is, discovering palettes based on images from [Unsplash](https://unsplash.com/):

Cool eh? Extracting palettes from photographs is a fantastic way of working when stuck for ideas, and `discoverPalettes` makes the process _incredibly easy_. This kind of approach, previously available only through “magic” color generators/apps, is now right at our fingers and ready to be tweaked, iterated, and improved to suit our own personal use-cases and preferences!

### Challenge

Right now, our `discoverPalettes` function finds the best matches it can in an array of colors, but it isn’t too easy to control. Can you add a degree of bias/weighting to its selection? How might you modify the function to prioritize brighter colors, for example?

## Function #3 — “Hue Shift”

For our third and final function, we will be taking inspiration from the world of pixel art!

Often when adding shades/highlights to a sprite, pixel artists will not only modulate the lightness/chroma of a color (saturation if working with HSL) but also shift its hue. [Here’s an excellent video on the subject](https://www.youtube.com/watch?v=PNtMAxYaGyg), but in short, this is what it looks like:

![](https://i7x7p5b7.stackpathcdn.com/codrops/wp-content/uploads/2021/12/hue-cycle-800x463.png)

So pretty! As a color becomes lighter, its hue shifts up; as it becomes darker, it shifts down. When applied subtly, this technique helps ensure shades/tints of a color are vivid and impactful. When “dialed up” a little, it is a fantastic way of generating stunning standalone color palettes.

### The code

    function adjustHue(val) {
      if (val < 0) val += Math.ceil(-val / 360) * 360;

      return val % 360;
    }

    function map(n, start1, end1, start2, end2) {
      return ((n - start1) / (end1 - start1)) * (end2 - start2) + start2;
    }

    function createHueShiftPalette(opts) {
      const { base, minLightness, maxLightness, hueStep } = opts;

      const palette = [base];

      for (let i = 1; i < 5; i++) {
        const hueDark = adjustHue(base.h - hueStep * i);
        const hueLight = adjustHue(base.h + hueStep * i);
        const lightnessDark = map(i, 0, 4, base.l, minLightness);
        const lightnessLight = map(i, 0, 4, base.l, maxLightness);
        const chroma = base.c;

        palette.push({
          l: lightnessDark,
          c: chroma,
          h: hueDark,
          mode: "lch"
        });

        palette.unshift({
          l: lightnessLight,
          c: chroma,
          h: hueLight,
          mode: "lch"
        });
      }

      return palette;
    }

To break this down into steps: 

1.  Pass a base color, min/max lightness, and hue step parameters to a `createHueShiftPalette` function. The min/max lightness values determine how dark/light our palette will be at either extreme. The step controls how much the hue will shift at each color.
2.  Store the base color in an array. In the illustration above, this is the middle color. 
3.  Create a loop that iterates four times. Each iteration, add a darker shade to the start of the array and a lighter tint to the end. Here, we use `map` to calculate our lightness values — a function that takes a number that usually exists in one range and converts it to another — and increase or decrease the hue using our `hueStep` variable. Again, `adjustHue` is used here to ensure all hue values are between 0 and 360.
4.  Return the palette! 

### Usage 

Once our `createHueShiftPalette` function is defined, we can use it like so:

    import { formatHex } from "https://cdn.skypack.dev/culori@2.0.0";

    const hueShiftPalette = createHueShiftPalette({
      base: {
        l: 55,
        c: 75,
        h: 0,
        mode: "lch"
      },
      minLightness: 10,
      maxLightness: 90,
      hueStep: 12
    });

    const hueShiftPaletteHex = hueShiftPalette.map((color) => formatHex(color));

    // ["#ffb97a", "#ff957c", "#ff727f", "#ff5083", "#f02f87", "#c70084", "#9a007f", "#6a0076", "#33006b"]

### Practical application

The palettes generated by `createHueShiftPalette` work \_fantastically \_for patterns/graphics; here’s an example using it to create random/generative patterns that differ ever-so-slightly each time they render: 

Cool, right? As just one example using this approach, we can create UI elements that are always fresh and unique to the current user — a lovely way to bring a little joy to the folks who use our websites/applications! 

### Challenge

Right now, the lightness/hue values scale linearly in our `createHueShiftPalette` function. Could you apply some easing to them? Perhaps, starting with a larger/smaller hue shift and reducing/increasing it with each step?

## Wrapping up

Well, folks, that’s all for now! We have learned how to create three beautiful color generation functions, seen how they can be applied and considered how they could be improved/changed. From here, I hope you take these functions and change them to suit you, and hopefully, even write your own!

As developers, we have a unique skill set that is \_perfect \_for creating truly innovative, stunning design. Whether that means creating a color generation tool for designers you work with or adding mind-blowing generative palettes to your website — we should all feel confident in our ability to work with color.

Until next time! 
 [https://tympanus.net/codrops/2021/12/07/coloring-with-code-a-programmatic-approach-to-design/](https://tympanus.net/codrops/2021/12/07/coloring-with-code-a-programmatic-approach-to-design/) 
 [https://tympanus.net/codrops/2021/12/07/coloring-with-code-a-programmatic-approach-to-design/](https://tympanus.net/codrops/2021/12/07/coloring-with-code-a-programmatic-approach-to-design/)
