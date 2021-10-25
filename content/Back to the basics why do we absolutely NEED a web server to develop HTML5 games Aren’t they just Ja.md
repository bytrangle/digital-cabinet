# Back to the basics: why do we absolutely NEED a web server to develop HTML5 games? Aren’t they just JavaScript? | Emanuele Feronato
Usually books, manuals, courses and tutorials – like the ones I am publishing on this blog for 15 years – provide answers, but this time I’m starting with a question that you probably asked yourself many times: when did JavaScript programming become that difficult?

We all remember our first script, when we just needed to open a text editor, create a basic HTML page, insert some code and run it in the browser. And it just worked. Look:

![](https://www.emanueleferonato.com/wp-content/uploads/2021/10/01-1024x330.png)

Just a plain text to start the magic.

Now everything seems to be more complicated, and require web servers, bundlers, configuration files and a lot of other stuff that might scare developers and keep them away from programming HTML5 games.

Why do ne need a web server?

Well, try let’s write another little, simple script:

```

```

`helloworld.txt` contains our “Hello, world!” string, so we should expect to see the same result in the browser in the previous example.

Unfortunately, this is what we get:

![](https://www.emanueleferonato.com/wp-content/uploads/2021/10/02.png)

Something called “CORS policy” blocked our script.

What is a “CORS policy”?

**Cross-Origin Resource Sharing (CORS)** is an HTTP-header based process that defines whether it is safe to allow a cross-origin request.

Normally, a web page may freely embed cross-origin content like images or stylesheets, but some special content like Ajax requests, like the one in our script, are forbidden,  to prevent a malicious script on one page from obtaining access to sensitive data on another web page.

Now the point is: which cross-origin are we talking about? Everything is on our hard disk, isn’t it the same origin?

Yes, but some operations are only supported in HTTP requests, and loading something from an hard disk is not a HTTP request.

This is important for your security, to keep all your hard disk content safe, not exposing every and each file in it.

This is why we need to create a mini HTTP environment on our computer: to grant CORS a safe zone where to work and satisfy security measures.

Are you looking for an easy and free web server to install on your computer?

[XAMPP](https://www.apachefriends.org/) is the most popular cross platform web server solution, anyway if you plan to [build HTML5 games using webpack](https://www.emanueleferonato.com/2021/08/03/working-with-phaser-typescript-and-webpack-step-1/), you won’t need any third-party web server. 
 [https://www.emanueleferonato.com/2021/10/06/back-to-the-basics-why-do-we-absolutely-need-a-web-server-to-develop-html5-games-arent-they-just-javascript/](https://www.emanueleferonato.com/2021/10/06/back-to-the-basics-why-do-we-absolutely-need-a-web-server-to-develop-html5-games-arent-they-just-javascript/) 
 [https://www.emanueleferonato.com/2021/10/06/back-to-the-basics-why-do-we-absolutely-need-a-web-server-to-develop-html5-games-arent-they-just-javascript/](https://www.emanueleferonato.com/2021/10/06/back-to-the-basics-why-do-we-absolutely-need-a-web-server-to-develop-html5-games-arent-they-just-javascript/)
