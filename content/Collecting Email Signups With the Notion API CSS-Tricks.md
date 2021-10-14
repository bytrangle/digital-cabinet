# Collecting Email Signups With the Notion API | CSS-Tricks
A lot of people these days are setting up their own newsletters. You’ve got the current big names like [Substack](https://substack.com/) and [MailChimp](https://mailchimp.com/), companies like Twitter are getting into it with [Revue](https://www.getrevue.co/), and even [Facebook is getting into the newsletter business](https://www.theverge.com/2021/6/29/22555957/facebook-bulletin-newsletter-subscriptions-substack-competitor). Some folks are trying to bring it closer to home with [a self-managed WordPress solution](https://css-tricks.com/video-screencasts/211-building-a-paid-subscription-newsletter-with-mailpoet-woocommerce-wordpress/) via [MailPoet](https://www.mailpoet.com/).

Let’s talk about another possibility: collecting your own email subscribers the good ol’ fashioned way: a `<form>` that submits to an API and puts them into data storage.

The first hurdle in setting up a newsletter is a mechanism to collect emails. Most apps that help you with building and sending newsletters will offer some kind of copy-and-paste signup form example, but most of these options are paid and we are looking for something that is free and completely hosted by us.

Nothing is handier than setting up our own system which we can control. In this tutorial, we will set up our own system to collect emails for our newsletters based on the Jamstack architecture. We will implement an HTML form to collect emails for our newsletter, process those emails with [Netlify Functions](https://www.netlify.com/products/functions/), and save them to a [Notion](https://www.notion.so/) database with the [Notion API.](https://developers.notion.com/)

### But before we begin…

We need the following things:

-   [A Netlify account](https://app.netlify.com/signup)
-   [A Notion account](https://www.notion.so/signup)
-   [A Mailgun account](https://signup.mailgun.com/). If you don’t have one, [click here to create a free account](https://signup.mailgun.com/).

All of these services are free (with limits). And as far as Mailgun goes, we could really use any email service provider that offers an API.

Get the entire code used in this tutorial [over at GitHub](https://github.com/ravgeetdhillon/newsletter-subscription-notion-netlify-function).

### First off, Notion

What is Notion, you ask? Let’s let them [describe it](https://www.notion.so/guides/what-is-notion):

> Notion is a single space where you can think, write, and plan. Capture thoughts, manage projects, or even run an entire company — and do it exactly the way you want.

![](https://i0.wp.com/css-tricks.com/wp-content/uploads/2021/09/s_C6DD37D8ED67AB804309E3524813327121D8EA01ECCF02AF2243019807FF6CCA_1631271964338_notion-front-page-hero.png?resize=2280%2C1632&is-pending-load=1#038;ssl=1)

In other words, Notion is sort of like a database but with a nice visual interface. And each row in the database gets its own “page” where writing content is not unlike writing a blog post in the WordPress block editor.

And those databases can be visualized in a number of ways, from a simple spreadsheet to a Trello-like board, or a gallery, or a calendar, or a timeline, or a… you get the picture.

We are using Notion because we can set up **tables** that store our form responses, making it basically a data store. Notion also just so happened to recently release a [full](https://css-tricks.com/notion-api/) [API for public use](https://css-tricks.com/notion-api/). We can take advantage of that API and interact with it using…

### Netlify Functions

Netlify Functions are serverless API endpoints provided by, of course, Netlify’s hosting platform. We can write them in JavaScript or Go.

![](https://i0.wp.com/css-tricks.com/wp-content/uploads/2021/09/s_C6DD37D8ED67AB804309E3524813327121D8EA01ECCF02AF2243019807FF6CCA_1631272038478_www.netlify.com_products_functions_1.png?resize=2136%2C1550&is-pending-load=1#038;ssl=1)

We could use [Netlify forms](https://www.netlify.com/products/forms/) here to collect form submissions. In fact, [Matthew Ström already shared how that works](https://css-tricks.com/using-netlify-forms-and-netlify-functions-to-build-an-email-sign-up-widget/). But on a free plan, we can only receive 100 submissions per month. So, that’s why we’re using Netlify Functions — they can be invoked 125,000 times a month, meaning we can collect a quarter-million emails every month without paying a single penny.

### Let’s set up the Notion database

The first thing we need to do is create a database in Notion. All that takes is creating a new page, then inserting a full-page table block by typing `/table`.

![](https://i2.wp.com/css-tricks.com/wp-content/uploads/2021/09/s_C6DD37D8ED67AB804309E3524813327121D8EA01ECCF02AF2243019807FF6CCA_1631103149208_image8.png?resize=1999%2C1448&is-pending-load=1#038;ssl=1)

Typing a command in Notion, like `/table`, opens up a contextual menu to insert a block on a page.

Let’s give our database a name: Newsletter Emails.

We want to keep things pretty simple, so all we’re collecting is an email address when someone submits the form. So, really, all we need is a column in the table called Email.

![](https://i0.wp.com/css-tricks.com/wp-content/uploads/2021/09/s_C6DD37D8ED67AB804309E3524813327121D8EA01ECCF02AF2243019807FF6CCA_1631103321428_www.notion.so_ravsamhq_a35339595b6749cf988cabcfb19c2a68_v2a1f3d1ec43f4823b859bb8228800d954.png?resize=2007%2C890&is-pending-load=1#038;ssl=1)

But Notion is a little smarter than that and includes advanced properties that provide additional information. One of those is a “Created time” property which, true to its name, prints a timestamp for when a new submission is created.

![](https://i0.wp.com/css-tricks.com/wp-content/uploads/2021/09/s_C6DD37D8ED67AB804309E3524813327121D8EA01ECCF02AF2243019807FF6CCA_1631103323266_www.notion.so_ravsamhq_a35339595b6749cf988cabcfb19c2a68_v2a1f3d1ec43f4823b859bb8228800d9521.png?resize=2000%2C1335&is-pending-load=1#038;ssl=1)

Now, when a new “Email” entry is added to the table, the date it was added is displayed as well. That can be nice for other things, like calculating how long someone has been a subscriber.

### We also need a Notion API Token

In order to interact with our Notion database, we need to create a Notion integration and get an API token. New integrations are made, not in your notion account, [but on the Notion site when you’re logged into your account](https://www.notion.so/my-integrations). We’ll give this integration a name that’s equally creative as the Notion table: Newsletter Signups.

![](https://i2.wp.com/css-tricks.com/wp-content/uploads/2021/09/s_C6DD37D8ED67AB804309E3524813327121D8EA01ECCF02AF2243019807FF6CCA_1631103483966_www.notion.so_my-integrations2.png?resize=2516%2C1893&is-pending-load=1#038;ssl=1)

Create a new Notion integration and associate it with the workspace where the Newsletter Emails table lives.

Once the integration has been created, we can grab the **Internal Integration Token** (API Token). Hold onto it because we’ll use it in just a bit.

![](https://i2.wp.com/css-tricks.com/wp-content/uploads/2021/09/s_C6DD37D8ED67AB804309E3524813327121D8EA01ECCF02AF2243019807FF6CCA_1631103938446_www.notion.so_my-integrations31.png?resize=2516%2C867&is-pending-load=1#038;ssl=1)

Once the integration has been created, Notion provides a secret API token that’s used to authenticate connections.

By default, Notion Integrations are actually unable to access Notion pages or databases. We need to explicitly share the specific page or database with our integration in order for the integration to properly read and use information in the table. Click on the **Share** link at the top-right corner of the Notion database, use the selector to find the integration by its name, then click **Invite**.

![](https://i0.wp.com/css-tricks.com/wp-content/uploads/2021/09/s_C6DD37D8ED67AB804309E3524813327121D8EA01ECCF02AF2243019807FF6CCA_1631103326172_www.notion.so_ravsamhq_a35339595b6749cf988cabcfb19c2a68_v2a1f3d1ec43f4823b859bb8228800d9511.png?resize=2000%2C1508&is-pending-load=1#038;ssl=1)

### Next is Netlify

Our database is all set to start receiving form submissions from Notion’s API. Now is the time to create a form and a serverless function for handling the form submissions.

Since we are setting up our website on Netlify, let’s install the [Netlify CLI](https://docs.netlify.com/cli/get-started) on our local machine. Open up Terminal and use this command:

    npm install netlify-cli -g

Based on your taste, we can authenticate to Netlify in different ways. You can follow [Netlify’s amazing guide](https://docs.netlify.com/cli/get-started/#obtain-a-token-via-the-command-line) as a starting point. Once we have authenticated with Netlify, the next thing we have to do is to create a Netlify project. Here are the commands to do that:

    $ mkdir newsletter-subscription-notion-netlify-function
    $ cd newsletter-subscription-notion-netlify-function
    $ npm init

Follow along with [this commit in the GitHub project](https://github.com/ravgeetdhillon/newsletter-subscription-notion-netlify-function/tree/edf382f).

### Let’s write a Netlify Function

Before we get into writing our Netlify function, we need to do two things.

First, we need to install the `[@notionhq/client](https://github.com/makenotion/notion-sdk-js)` npm package, which is an official Notion JavaScript SDK for using the Notion API.

    $ npm install @notionhq/client --save

Second, we need to create a `.env` file at the root of the project and add the following two environment variables:

    NOTION_API_TOKEN=<your Notion API token>
    NOTION_DATABASE_ID=<your Notion database>

The database ID can be found in its URL. A typical Notion page has a URL like `https://www.notion.so/{workspace_name}/{database_id}?v={view_id}` where `{database_id}` corresponds to the ID.

We are writing our Netlify functions in the `functions` directory. So, we need to create a `netlify.toml` file at the root of the project to tell Netlify that our functions need to be built from this directory.

    [build]
    functions = "functions"

Now, let’s create a `functions` directory at the root of the project. In the `functions` directory, we need to create an `index.js` file and add the following code. This function is available as an API endpoint at `/.netlify/functions/index`.

    const { Client, LogLevel } = require('@notionhq/client');

    const { NOTION_API_TOKEN, NOTION_DATABASE_ID } = process.env;

    async function addEmail(email) {
      
      const notion = new Client({
        auth: NOTION_API_TOKEN,
        logLevel: LogLevel.DEBUG,
      });

      await notion.pages.create({
        parent: {
          database_id: NOTION_DATABASE_ID,
        },
        properties: {
          Email: {
            title: [
              {
                text: {
                  content: email,
                },
              },
            ],
          },
        },
      });
    }

    function validateEmail(email) {
      const re =
        /^(([^&lt;&gt;()[\\]\\\\.,;:\\s@&quot;]+(\\.[^&lt;&gt;()[\\]\\\\.,;:\\s@&quot;]+)*)|(&quot;.+&quot;))@((\\[[0-9]{1,3}\\.[0-9]{1,3}\\.[0-9]{1,3}\\.[0-9]{1,3}\\])|(([a-zA-Z\\-0-9]+\\.)+[a-zA-Z]{2,}))$/;
      return re.test(String(email).toLowerCase());
    }

    module.exports.handler = async function (event, context) {
      
      if (event.httpMethod != 'POST') {
        return { statusCode: 405, body: 'Method not allowed' };
      }

      
      try {
        var body = JSON.parse(event.body);
      } catch (err) {
        return {
          statusCode: 400,
          body: err.toString(),
        };
        return;
      }

      
      const { email } = body;
      if (!validateEmail(email)) {
        return { statusCode: 400, body: 'Email is not valid' };
      }

      
      await addEmail(email);
      return { statusCode: 200, body: 'ok' };
    }; 

Let’s break this down, block by block.

The `addEmail` function is used to add the email to our Notion database. It takes an `email` parameter which is a string.

We create a new client for authenticating with the Notion API by providing our `NOTION_API_TOKEN`. We have also set the log level parameter to get information related to the execution of commands while we are developing our project, which we can remove once we are ready to deploy the project to production.

In a Notion database, each entry is known as a **page**. So, we use the `notion.pages.create` method to [create a new entry in our Notion database](https://developers.notion.com/reference/post-page). The `module.exports.handler` function is used by Netlify functions that handle the requests made to it.

The first thing we check is the request method. If the request method is something other than `POST`, we send back a `405 - Method not` `allowed` response.

Then we parse the payload (`event.body`) object sent by the front end. If we are unable to parse the payload object, we send back a `400 - Bad request` response.

Once we have parsed the request payload, we check for the presence of an email address. We use the `validateEmail` function to check the validity of the email. If the email is invalid, we send back a `400 - Bad` `request` response with a custom message saying the `Email is not valid`.

After we have completed all the checks, we call the `addEmail` function to store the email in our Notion database and send back a `200 - Success` response that everything went successfully.

Follow along with the code in the GitHub project with [commit ed607db](https://github.com/ravgeetdhillon/newsletter-subscription-notion-netlify-function/tree/ed607db).

### Now for a basic HTML form

Our serverless back-end is ready and the last step is to create a form to collect email submissions for our newsletter.

Let’s create an `index.html` file at the root of the project and add the following HTML code to it. Note that the markup and classes are pulled from Bootstrap 5.

    <!DOCTYPE html>
    <html lang="en">
    <head>
      <meta charset="UTF-8">
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>Notion + Netlify Functions Newsletter</title>
      <link href="<https://cdn.jsdelivr.net/npm/bootstrap@5.1.0/dist/css/bootstrap.min.css>" rel="stylesheet" integrity="sha384-KyZXEAg3QhqLMpG8r+8fhAXLRk2vvoC2f3B09zVXn8CA5QIVfZOJ3BCsw2P0p/We" crossorigin="anonymous">
    </head>

    <body>
      <div class="container py-5">
        <p>
          Get daily tips related to Jamstack in your inbox.
        </p>

        <div id="form" class="row">
          <div class="col-sm-8">
            <input name="email" type="email" class="form-control" placeholder="Your email" autocomplete="off">
            <div id="feedback-box" class="invalid-feedback">
                Please enter a valid email
            </div>
          </div>
          <div class="col-sm-4">
            <button class="btn btn-primary w-100" type="button" onclick="registerUser()">Submit form</button>
          </div>
        </div>

        <div id="success-box" class="alert alert-success d-none">
          Thanks for subscribing to our newsletter.
        </div>
        <div id="error-box" class="alert alert-danger d-none">
          There was some problem adding your email.
        </div>
      </div>
    </body>
    <script>  </script>
    </html>

We have an input field for collecting user emails. We have a button that, when clicked, calls the `registerUser` function. We have two alerts that are shown, one for a successful submission and one for an unsuccessful submission.

Since we have installed Netlify CLI, we can use the `netlify dev` command to run our website on the localhost server:

    $ netlify dev

![](https://i1.wp.com/css-tricks.com/wp-content/uploads/2021/09/s_C6DD37D8ED67AB804309E3524813327121D8EA01ECCF02AF2243019807FF6CCA_1631103411447_localhost_8888_4.png?resize=1374%2C412&is-pending-load=1#038;ssl=1)

Once the server is up and running, we can visit `localhost:8888` and see our webpage with the form.

Next, we need to write some JavaScript that validates the email and sends an HTTP request to the Netlify function. Let’s add the following code in the `<script>` tag in the `index.html` file:

    function validateEmail(email) {
      const re =
        /^(([^<>()[\\]\\\\.,;:\\s@"]+(\\.[^<>()[\\]\\\\.,;:\\s@"]+)*)|(".+"))@((\\[[0-9]{1,3}\\.[0-9]{1,3}\\.[0-9]{1,3}\\.[0-9]{1,3}\\])|(([a-zA-Z\\-0-9]+\\.)+[a-zA-Z]{2,}))$/;
      return re.test(String(email).toLowerCase());
    }

    async function registerUser() {
      const form = document.getElementById('form');
      const successBox = document.getElementById('success-box');
      const errorBox = document.getElementById('error-box');
      const feedbackBox = document.getElementById('feedback-box');
      const email = document.getElementsByName('email')[0].value;

      if (!validateEmail(email)) {
        feedbackBox.classList.add('d-block');
        return;
      }

      const headers = new Headers();
    headers.append('Content-Type', 'application/json');
      
      const options = {
        method: 'POST',
        headers,
        body: JSON.stringify({email}),
      };

      const response = await fetch('/.netlify/functions/index', options);

      if (response.ok) {
        form.classList.add('d-none');
        successBox.classList.remove('d-none');
      }
      else {
        form.classList.add('d-none');
        errorBox.classList.remove('d-none');
      }
    }

In the `registerUser` function, we check the validity of the email input using a RegEx function (`validateEmail`). If the email is invalid, we prompt the user to try again. Once we have validated the email, we call the `fetch` method to send a `POST` request to our Netlify function which is available at `/.netlify/functions/index` with a payload object containing the user’s email.

Based on the response, we show a success alert to the user otherwise we show an error alert.

Follow along with the code in the GitHub project with [commit cd9791b](https://github.com/ravgeetdhillon/newsletter-subscription-notion-netlify-function/tree/cd9791b).

### Getting the Mailgun credentials

Getting emails in our database is good, but not enough, because we don’t know how we are going to use them. So, the final step is to send a confirmation email to a subscriber when they sign up.

We are not going to send a message immediately after a user signs up. Instead, we will set up a method to fetch the email signups in the last 30 minutes, then send the email. Adding a delay to the auto-generated email makes the welcome message feel a bit more personal, I think.

We’re using [Mailgun](https://www.mailgun.com/) to send, but again, we could really use any email service provider with an API. Let’s visit the Mailgun dashboard and, from the sidebar, go to the Settings → API Keys screen.

![](https://i1.wp.com/css-tricks.com/wp-content/uploads/2021/09/s_C6DD37D8ED67AB804309E3524813327121D8EA01ECCF02AF2243019807FF6CCA_1631783001225_app.mailgun.com_app_account_security_api_keys1.png?resize=2078%2C1342&is-pending-load=1#038;ssl=1)

We want the “Private API key.”

Let’s copy the Private API key and add it to the `.env` file as:

    MAILGUN_API_KEY=<your Mailgun api key>

Every email needs to come from a sending address, so one more thing we need is the Mailgun domain from which the emails are sent. To get our Mailgun domain, we need to go to the Sending → Domains screen.

![](https://i1.wp.com/css-tricks.com/wp-content/uploads/2021/09/s_C6DD37D8ED67AB804309E3524813327121D8EA01ECCF02AF2243019807FF6CCA_1631783016258_Group1761.png?resize=1993%2C1341&is-pending-load=1#038;ssl=1)

Mailgun offers a sandbox domain for testing.

Let’s copy our Mailgun sandbox domain and add it to the `.env` file as:

    MAILGUN_DOMAIN=<your Mailgun domain>

### Sending a confrimation email

Before we get into writing our Netlify function for sending an email confirmation, we need to install the `[mailgun-js](https://github.com/mailgun/mailgun-js)` npm package, which is an official Mailgun JavaScript SDK for using the Mailgun API:

    $ npm install mailgun-js --save

We are using the `/.netlify/functions/index` endpoint for sending welcome emails. So, in the `functions` directory, let’s create a new a `welcome.js` file and add the following code:

    const { Client, LogLevel } = require('@notionhq/client');
    const mailgun = require('mailgun-js');

    const {
      NOTION_API_TOKEN,
      NOTION_DATABASE_ID,
      MAILGUN_API_KEY,
      MAILGUN_DOMAIN,
    } = process.env;

    async function fetchNewSignups() {
      
      const notion = new Client({
        auth: NOTION_API_TOKEN,
        logLevel: LogLevel.DEBUG,
      });

      
      let fetchAfterDate = new Date();
      fetchAfterDate.setMinutes(fetchAfterDate.getMinutes() - 30);

      
      
      const response = await notion.databases.query({
        database_id: NOTION_DATABASE_ID,
        filter: {
          or: [
            {
              property: 'Added On',
              date: {
                after: fetchAfterDate,
              },
            },
          ],
        },
      });

      const emails = response.results.map((entry) => entry.properties.Email.title[0].plain_text);

      return emails;
    }

    async function sendWelcomeEmail(to) {
      const mg = mailgun({ apiKey: MAILGUN_API_KEY, domain: MAILGUN_DOMAIN });

      const data = {
        from: `Ravgeet Dhillon <postmaster@${MAILGUN_DOMAIN}>`,
        to: to,
        subject: 'Thank you for subscribing',
        text: "Thank you for subscribing to my newsletter. I'll be sending daily tips related to JavScript.",
      };

      await mg.messages().send(data);
    }

    module.exports.handler = async function (event, context) {
      
      if (event.httpMethod != 'POST') {
        return { statusCode: 405, body: 'Method not allowed' };
      }

      const emails = await fetchNewSignups();

      emails.forEach(async (email) => {
        await sendWelcomeEmail(email);
      });

      return { statusCode: 200, body: 'ok' };
    };

The above code has two main functions which we need to be understood. First, we have a `fetchNewSignups` function. We use the Notion SDK client to fetch the entries in our Notion database. We have used a filter to fetch only those emails which were added in the last 30 minutes. Once we get a response from the Notion API, we loop over the response and map the email into an `emails` array.

Second, we have a `sendWelcomeEmail` function. This function is used to send an email to the new subscriber via Mailgun API.

To bring everything together, we loop over the new emails in the `module.exports.handler` function and send a welcome email to each new email.

This function is available at `localhost:8888/.netlify/functions/welcome`.

Follow along with the code in the GitHub project with [commit b802f93](https://github.com/ravgeetdhillon/newsletter-subscription-notion-netlify-function/tree/b802f93).

### Let’s test this out!

Now that our project is ready, the first thing we need to do is to test the form and see whether the email is stored in our Notion database. Let’s go to `localhost:8888` and enter a valid email address.

![](https://i0.wp.com/css-tricks.com/wp-content/uploads/2021/09/s_C6DD37D8ED67AB804309E3524813327121D8EA01ECCF02AF2243019807FF6CCA_1631272154539_ezgif-3-4aa67ed7fb59.gif?resize=600%2C151&ssl=1)

Perfect! We got the success message. Now let’s head over to our Notion database to see if the email address was captured.

![](https://i2.wp.com/css-tricks.com/wp-content/uploads/2021/09/s_C6DD37D8ED67AB804309E3524813327121D8EA01ECCF02AF2243019807FF6CCA_1631103543169_www.notion.so_ravsamhq_a35339595b6749cf988cabcfb19c2a68_v2a1f3d1ec43f4823b859bb8228800d9531.png?resize=1694%2C899&is-pending-load=1#038;ssl=1)

Awesome, it’s there! We can also see that the **Added On** field has also been auto-filled by Notion, which is exactly what we want.

Next, let’s send a `POST` request to `localhost:8888/.netlify/functions/welcome` from [Postman](https://www.postman.com/) and check our inbox for the subscription confirmation message.

![](https://i1.wp.com/css-tricks.com/wp-content/uploads/2021/09/s_C6DD37D8ED67AB804309E3524813327121D8EA01ECCF02AF2243019807FF6CCA_1631782178605_mail.google.com_mail_u_0__qmailgun11.png?resize=1514%2C629&is-pending-load=1#038;ssl=1)

Woah! We have received the welcome email for subscribing to the newsletter.

Not seeing the email in your inbox? Make sure to check your spam folder.

### What’s next?

The next step from here would be to set up a script that runs every 30 minutes and calls the welcome endpoint. As of now, Netlify doesn’t provide any way to run CRON jobs. So, we can set up GitHub Actions to handle it.

Also, we can secure the welcome endpoint using security mechanisms like a simple password or a JWT token. This will prevent anyone on the Internet from calling the `welcome` endpoint and sending tons of spam to our subscribers.

So, there we have it! We successfully implemented a Jamstack way to collect emails for a newsletter. The biggest advantage of doing something like this is that we can learn a lot about different technologies at once. We wired together services and APIs that collect email addresses, validate submissions, post to databases, and even send emails. It’s pretty cool that we can do so much with relatively so little.

I hope you enjoyed reading and learning something from this article as much as I enjoyed writing it for you. Let me know in the comments how you used this method for your own benefit. Happy coding! 
 [https://css-tricks.com/collecting-email-signups-with-the-notion-api/](https://css-tricks.com/collecting-email-signups-with-the-notion-api/) 
 [https://css-tricks.com/collecting-email-signups-with-the-notion-api/](https://css-tricks.com/collecting-email-signups-with-the-notion-api/)
