# You Sorta Don't Need Absolute Positioning Anymore
This is something that’s going to take ages for me getting used to: we sorta don’t need absolute positioning anymore. Or, rather, we don’t need to rely on it as much as we did in the past. That’s the gist of [this post by Ahmad Shadeed](https://ishadeed.com/article/less-absolute-positioning-modern-css/):

When we have a card that contains text over an image, we often use `position: absolute` to place the content over the image. This is no longer needed with CSS grid.

Whoa, whoa, whoa. Hold your horses there, pal. As much as this makes me panic a bit—learning and unlearning CSS things makes me feel like that—Ahmad shows a great example:

![](https://i0.wp.com/css-tricks.com/wp-content/uploads/2021/10/CleanShot-2021-10-17-at-19.05.40@2x.png?w=560&ssl=1)

Now if you asked me five minutes ago how to make that text stick to the bottom I would’ve said there’s two main ways to go about it. First, you could set the `.card` to `position: relative` and then absolutely position the text with `bottom: 10px` or something. The second way to do it would be [the nifty CSS trick](https://css-tricks.com/the-peculiar-magic-of-flexbox-and-auto-margins/) where you could set the `.card` to `display: flex` and then make the `.card__text` component have `margin-top: auto` on it

Ahmad argues that a good solution is CSS Grid because you can set up the columns and rows and tell that text block to sit at the bottom on the very last row. I’m sure this is how all the cool kids are already thinking about CSS layout and positioning at this point, but to me this feels like a tiny revolution in my brain.

Chris then made [a great example](https://codepen.io/chriscoyier/pen/WNEvrmL) riffing off this idea that messes with my mental model of CSS, too:

![](https://i0.wp.com/css-tricks.com/wp-content/uploads/2021/10/CleanShot-2021-10-17-at-19.12.02@2x.png?w=560&ssl=1)

Both the `Plant` text and the on sale tag are both using CSS Grid, with no CSS absolute positioning in sight. That’s pretty neat to me but it’s going to take a while to change how I think about positioning in CSS. 
 [https://css-tricks.com/newsletter/273-weird-browsers/](https://css-tricks.com/newsletter/273-weird-browsers/)
