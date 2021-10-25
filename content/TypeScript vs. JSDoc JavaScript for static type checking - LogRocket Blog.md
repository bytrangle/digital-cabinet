# TypeScript vs. JSDoc JavaScript for static type checking - LogRocket Blog
There’s a debate to be had about whether using JavaScript or TypeScript leads to better outcomes when building a project. The advent of using JSDoc annotations to type a JavaScript codebase introduces a new dynamic to this discussion.

In this guide, we’ll investigate what that looks like and come to an (opinionated) conclusion.

If you’d talked to me in 2018, I would have solidly recommended using TypeScript and steering away from JavaScript.

The rationale is simple: I’m exceedingly convinced of the value that static typing provides in terms of productivity and avoiding bugs in production. I appreciate this can be a contentious issue, but that is my settled opinion on the subject. Other opinions are available.

TypeScript has long had a good static typing story. Because JavaScript is dynamically typed, historically, it has not. Thanks to TypeScript’s support for JSDoc, JavaScript can now be statically type checked.

## What is JSDoc JavaScript?

JSDoc itself actually dates way back to 1999. According to [Wikipedia](https://en.wikipedia.org/wiki/JSDoc):

> JSDoc is a markup language used to annotate JavaScript source code files. Using comments containing JSDoc, programmers can add documentation describing the application programming interface of the code they’re creating.
>
> The TypeScript team have taken JSDoc support and run with it. You can now use a [variant of JSDoc annotations](https://www.typescriptlang.org/docs/handbook/jsdoc-supported-types.html) to provide type information in JavaScript files.

What does this look like? Take the `simpleTypeScript` statement below, for example:

    let myString: string;  

This TypeScript statement could become the equivalent JavaScript statement with a JSDoc annotation:

    /\*\* @type {string} */  let myString;  

This is type-enhanced JavaScript, which the TypeScript compiler can understand and type check.

## Why use JSDoc JavaScript?

Why would you use JSDoc JavaScript instead of TypeScript? Well, there’s a range of possible use cases.

Perhaps you’re writing simple node scripts and you’d like a little type safety to avoid mistakes. Or maybe you want to dip your project’s toe in the waters of static type checking but without fully committing. JSDoc allows for that. Or, your team may simply prefer not to have a compile step.

That, in fact, was the rationale of the webpack team. A little bit of history: webpack has always been a JavaScript codebase. As the codebase grew and grew, there was often discussion about using static typing. However, having a compilation step wasn’t desired.

TypeScript had been quietly adding support for type checking JavaScript with the assistance of JSDoc for some time. Initial support arrived with the `--checkJs` compiler option in [TypeScript 2.3](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-3.html#errors-in-js-files-with---checkjs).

A community member by the name of [Mohsen Azimi](https://twitter.com/mohsen____) started experimentally using this approach to type check the webpack codebase. His [PR](https://github.com/webpack/webpack/pull/6862) ended up being a test case that helped improve the type checking of JavaScript by TypeScript.

TypeScript v2.9 shipped with a whole host of JSDoc improvements as a consequence of the webpack work. Because it’s such a widely used project, this also helped popularize the approach of using JSDoc to type check JavaScript codebases. It demonstrated that the approach could work on a significantly large codebase.

These days, JSDoc type checking with TypeScript is extremely powerful. While not quite on par with TypeScript (not all TypeScript syntax is supported in JSDoc), the gap in functionality is pretty small.

Today, it’s a completely legitimate choice to build a JavaScript codebase with all the benefits of static typing.

## Why use TypeScript?

If you’re starting a project and want to make use of static typing, how do you choose between TypeScript or JavaScript with JSDoc?

Well, unless you have a compelling need to avoid a compilation step, I personally believe TypeScript is the better choice for a number of reasons.

First, the tooling support for using TypeScript directly is better than that for JSDoc JavaScript. At the time of writing, things such as refactoring tools, etc. in your editor work more effectively with TypeScript than with JSDoc JavaScript. That said, these are improving gradually.

Secondly, working with JSDoc is distinctly “noisier” — it requires far more keystrokes to achieve the same level of type safety.

Consider the following TypeScript:

    function stringsStringStrings(p1: string, p2?: string, p3?: string, p4 =  "test"): string {  // ...  }

Compared to the equivalent JSDoc JavaScript:

    /\*\*
     \* @param {string}  p1
     \* @param {string=} p2
     \* @param {string} \[p3\]
     \* @param {string} \[p4="test"\]
     \* @return {string}
     */  function stringsStringStrings(p1, p2, p3, p4)  {  // ...  }

I may be biased by my familiarity with TypeScript, but I find that TypeScript is easier to read and comprehend compared to the JSDoc JavaScript alternative. The fact that all JSDoc annotations live in comments, rather than directly in syntax, makes it harder to follow. (It certainly doesn’t help that many VS Code themes present comments in a very faint color.)

My final reason for favoring TypeScript comes down to falling into the “[pit of success](https://blog.codinghorror.com/falling-into-the-pit-of-success/).” You’re cutting against the grain when it comes to static typing and JavaScript. You can have it, but you have to work that bit harder to ensure that you have statically typed code.

On the other hand, you’re cutting _with_ the grain when it comes to static typing and TypeScript. You have to work hard to opt out of static typing. The TypeScript defaults tend toward static typing, while the JavaScript defaults tend away.

As someone who very much favors static typing, you can imagine how this is compelling to me!

## Which is better: TypeScript or JSDoc JavaScript?

To summarize, in a way, I don’t feel super strongly whether people use JavaScript or TypeScript. That said, having static typing will likely be a benefit to new projects.

Here’s the bottom line: I’m keen that people fall into the pit of success, so my recommendation for a new project would be TypeScript.

I really like JSDoc myself, and will often use it on small projects. It’s a fantastic addition to TypeScript’s capabilities. For bigger projects, I’m more likely to go with TypeScript from the get-go.

But, really, either is a solid choice.

## Writing a lot of TypeScript? [Watch the recording](https://blog.logrocket.com/typescript-meetup-recap/?utm_source=social&utm_medium=organic&utm_campaign=21Q3_WB_TS-Tech-Meetup0930#utm_source=cro-blog&utm_medium=logrocket&utm_campaign=21Q3_WB_TS-Tech-Meetup0930) of our recent TypeScript meetup to learn about writing more readable code.

[![](https://blog.logrocket.com/wp-content/uploads/2021/07/typescript-4-4-more-readable-code.png?is-pending-load=1)
](https://blog.logrocket.com/typescript-meetup-recap/?utm_source=social&utm_medium=organic&utm_campaign=21Q3_WB_TS-Tech-Meetup0930#utm_source=cro-blog&utm_medium=logrocket&utm_campaign=21Q3_WB_TS-Tech-Meetup0930)

TypeScript brings type safety to JavaScript. There can be a tension between type safety and readable code. Watch the recording for a deep dive on some new features of TypeScript 4.4. 
 [https://blog.logrocket.com/typescript-vs-jsdoc-javascript/](https://blog.logrocket.com/typescript-vs-jsdoc-javascript/) 
 [https://blog.logrocket.com/typescript-vs-jsdoc-javascript/](https://blog.logrocket.com/typescript-vs-jsdoc-javascript/)
