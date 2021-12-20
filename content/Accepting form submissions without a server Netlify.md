# Accepting form submissions without a server | Netlify
> Throughout December we’ll be [highlighting a different Netlify feature each day](https://www.netlify.com/blog/2021/12/01/highlighting-a-different-netlify-feature-each-day-in-december/?utm_campaign=featdaily21&utm_source=netlify&utm_medium=blog&utm_content=netlify-forms). It might just be the thing you need to unlock those creative juices, and [dust off that domain](https://www.netlify.com/blog/2021/12/01/dusty-domains-your-forgotten-domains-raise-money-for-charity/?utm_campaign=featdaily21&utm_source=netlify&utm_medium=blog&utm_content=netlify-forms) you registered but never deployed! Keep an eye [on the blog](https://www.netlify.com/blog/2021/12/01/highlighting-a-different-netlify-feature-each-day-in-december/?utm_campaign=featdaily21&utm_source=netlify&utm_medium=blog&utm_content=netlify-forms) and on [Twitter](https://twitter.com/netlify) for each feature!

The need to accept form submissions is usually the first reason you might need to add a server to your site’s stack. After all, those form posts have to go somewhere, right?

But adding a server to your list of things to configure, scale, and maintain, just so that you can allow your visitors to send you data, is an unwelcome leap in complexity.

That’s why Netlify includes the ability to handle form submissions without introducing any other infrastructure into the equation, with [Netlify Forms](https://www.netlify.com/products/forms/?utm_campaign=featdaily21&utm_source=netlify&utm_medium=blog&utm_content=netlify-forms).

![](https://cdn.netlify.com/ad76bff9547d863cf6a772d26607986ccbe23bea/3415e/img/blog/screenshot-form-submissions.jpg)

## How it works

When Netlify deploys your site, it looks at the HTML that you are shipping. This “post-processing” step allows Netlify to do number of things such as optimisations snippet injection (We’re bound to see more details on that another day this month!). It also means that Netlify can see if there are any HTML form elements which you ’d like us to automatically create a backend service for.

## Telling Netlify you want a form handling API

This sounds like it will be tricky. Nah!

To indicate to Netlify that you want a form handler, all you need to do is add an attribute to the form element in your HTML that you care about, and make sure that your form has a name. Like this:

    <form action="/thanks-page" method="POST" name="my-form-name" netlify>
    	...
    </form>

By adding that `netlify` boolean attribute to the form element (or you can use `data-netlify="true"` if you prefer), Netlify now knows to listen for HTTP POST requests that your visitors submit to that form, and to collect the submissions for you. You can find the submissions (which will have been spam-filtered and safely stored for you) in your [Netlify admin](https://app.netlify.com/?utm_campaign=featdaily21&utm_source=netlify&utm_medium=blog&utm_content=netlify-forms) app.

After submitting their form data, your site visitors will be directed to the confirmation page you specify in your form’s `action` attribute.

Sound too good to be true? Yeah we get that a lot!

## What about client-side rendered forms?

The process above assumes that you are serving your forms as HTML to your visitors. But some popular frameworks generate the HTML using client-side JavaScript. Can we support that too? You bet!

Regardless of which JavaScript frameworks you might use for client-side rendering your form, we can provide the beck-end service to handle the submissions. You can see more details about this [in the docs](https://docs.netlify.com/forms/setup/?utm_campaign=featdaily21&utm_source=netlify&utm_medium=blog&utm_content=netlify-forms#javascript-forms).

Netlify Forms open up a number of interesting possibilities for how submissions are handled and how they can trigger events. And you’re not going to need to add a server in order to support these user interactions.

## More information

-   [About Netlify Forms](https://www.netlify.com/products/forms/?utm_campaign=featdaily21&utm_source=netlify&utm_medium=blog&utm_content=netlify-forms)
-   [Docs: Netlify forms](https://docs.netlify.com/forms/setup/?utm_campaign=featdaily21&utm_source=netlify&utm_medium=blog&utm_content=netlify-forms)
-   [Submitting form data with AJAX](https://docs.netlify.com/forms/setup/?utm_campaign=featdaily21&utm_source=netlify&utm_medium=blog&utm_content=netlify-forms#submit-html-forms-with-ajax)
-   [Working with JavaScript forms](https://docs.netlify.com/forms/setup/?utm_campaign=featdaily21&utm_source=netlify&utm_medium=blog&utm_content=netlify-forms#javascript-forms) 
    [https://www.netlify.com/blog/2021/12/16/accepting-form-submissions-without-a-server/](https://www.netlify.com/blog/2021/12/16/accepting-form-submissions-without-a-server/) 
    [https://www.netlify.com/blog/2021/12/16/accepting-form-submissions-without-a-server/](https://www.netlify.com/blog/2021/12/16/accepting-form-submissions-without-a-server/)
