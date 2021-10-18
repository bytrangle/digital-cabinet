# Inline CSS Custom Properties
[View this newsletter on the web.](https://css-tricks.us2.list-manage.com/track/click?u=155f5a9ccc4e24c318130cace&id=1d26c658ac&e=2df64611c3)

### ![](https://fonts.gstatic.com/s/e/notoemoji/13.1.1/1f632/32.png)

**\[Robin]:** Let’s say you need some margin and padding utility classes. In Tailwind, for example, you’d write some HTML [like this](https://css-tricks.us2.list-manage.com/track/click?u=155f5a9ccc4e24c318130cace&id=a47beddff5&e=2df64611c3):

    <div class="pt-6">...</div>

This sets the `padding-top` CSS property to `6` in the scale. `1` is less padding than `2`, etc. And this can be super handy when you don’t want to litter your CSS with margin and padding values. To be quite frank here, I think this is indeed a very useful thing (even though generally I am not entirely all aboard the utility class hype train).

However! What if we wanted to have these utility classes without using something like Tailwind? Sure, we could use Sass to generate those classes but, as Andy Ford suggests, [we can write these utility classes ourselves with CSS custom properties](https://css-tricks.us2.list-manage.com/track/click?u=155f5a9ccc4e24c318130cace&id=882ed8749c&e=2df64611c3).

First off, Andy gives an example that looks like this in HTML:

    <div style="--p: 4;">...</div>

I know I’m being stereotypically British when I say this but, _ahem_, you wot?

This is extremely weird but also very cool! It’s setting a CSS custom property inline from the HTML which I’ve never done before. In your CSS you’d set up that variable in the `:root`, like this perhaps:

    :root {
      --p: 0;
    }

(If all this is a bit foreign to you then I’d very much recommend our guide on [CSS custom variables](https://css-tricks.us2.list-manage.com/track/click?u=155f5a9ccc4e24c318130cace&id=5b745e2c9a&e=2df64611c3). I return to it an awful lot.)

And then you’d create the class that sets the padding in CSS:

    [style*='--p:'] {
      padding: calc(0.25rem * var(--p)) !important;
    }

So that’s multiplying whatever you set as the `--p` variable by 0.25. And when you put all that together you can use that utility class throughout your HTML, like this:

    <div style="--p: 1;">0.25rem of padding</div>
    <div style="--p: 2;">0.50rem of padding</div>
    <div style="--p: 3;">0.75rem of padding</div>

I think this is an extremely neat—although very peculiar—CSS trick. And, as Andy shows, you need to set variables and write classes for `padding-right`, `padding-left`, etc. You also need to figure out how to deal with breakpoints and media queries.

He writes:

> It’s worth pointing out here that CSS variables assigned in inline styles do not leak out. They’re scoped only to the current element and don’t change the value of the variable globally. Thank goodness! The one oddity I have found so far is that DevTools (at least in Chrome, Firefox, and Safari) do not report the styles using this technique in the “Computed” styles tab.

This is such a neat example of what CSS custom properties can do. We could do the same for `font-size` or changing the `font-family` of an element, too. There are so many different things we could explore with this technique!

* * *

 [https://mail.google.com/mail/u/0/#inbox/FMfcgzGkZsrWNtttnqrhCjHVPBMdSjWP](https://mail.google.com/mail/u/0/#inbox/FMfcgzGkZsrWNtttnqrhCjHVPBMdSjWP)
