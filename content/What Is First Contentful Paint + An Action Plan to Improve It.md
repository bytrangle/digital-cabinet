# What Is First Contentful Paint? + An Action Plan to Improve It
If you could improve your website's performance by 10%, would you?

Site performance scoring is a complex web of metrics, and First Contentful Paint (FCP) is just one factor Google considers when evaluating page load speed. Responsible for 10% of a website's overall performance score, FCP plays an important role in creating a positive user experience for visitors.

A site's First Contentful Paint (FCP) is the total time it takes a page to load from the moment the request is sent to the point that any content is rendered on the screen.

### [→ Download Now: SEO Starter Pack \[Free Kit\]](https://blog.hubspot.com/cs/c/?cta_guid=882f38ab-3d08-4abe-b1c9-fed6ebc3579f&signature=AAH58kGxy28KgQ3h5ipFBlTyBmJB3qJV0w&pageId=56464874054&placement_guid=1d7211ac-7b1b-4405-b940-54b8acedb26e&click=13905fd4-813f-49a5-99ed-011ae1dda2b4&hsutk=&canon=https%3A%2F%2Fblog.hubspot.com%2Fmarketing%2Ffirst-contentful-paint&portal_id=53&redirect_url=APefjpG3ehKgoSYUTOzGd6LnM2eSMEjhHnnhnkUlP-L9gQTOjrKuAtrhO3uXWmF-GAxnq1AX78SxeUPnYWEZoMIFftIHP16NAeQ3WL3BO2cgfYmiwTzuWQoOxmZneqQkpTyDZtgMHjvjE-eyKQXhME8M9p-KOGRhxvkREwj1D1qN8q5P_YwwV64 "→ Download Now: SEO Starter Pack \[Free Kit]")

The higher the FCP score, the slower the content loads. When visitors think a page takes too long to load, it can be a major red flag. In a study by Top Designs Firms, 42% of people said they would leave a [poorly functioning website](https://blog.hubspot.com/marketing/web-design-stats-for-2020?_ga=2.44440416.55969710.1617995081-311531060.1617995081).

But a low FCP score shows that the page is loading quickly, which means content will be delivered sooner. And fast-loading content is one way to keep visitors scrolling your site. In fact, Deloitte found that a [0.1-second improvement in load time](https://blog.hubspot.com/marketing/web-design-stats-for-2020?_ga=2.44440416.55969710.1617995081-311531060.1617995081) increased conversions by 8.4% for retail sites and 10.1% for travel sites.

When a millisecond makes a difference, it's best to do whatever you can to improve your site speed. So let's take a look at how to lower FCP to make your site as fast and user-friendly as possible.

## What is First Contentful Paint?

First Contentful Paint (FCP) is the amount of time it takes for a user to see the first content on a website, whether it's images, text, logos, background graphics, or non-white <canvas> elements. FCP evaluates how users experience a website's page load speed by measuring what people actually perceive, rather than the results of a speed test tool.

In the timeline below, you can see FCP play out in the second frame when the first text and image elements appear on the screen.

![](https://lh5.googleusercontent.com/a39SKS876deEhIsp040ntqgIDtpwmrZZ_0UQHMD785WfxLeKz4lhiGlhVnYMety9FTbU4ThiswjnDw5DyRJM4Yh0xzs8hns-qm8u6HY6UKAfJbBBmEL8O0x041UL3r3XN_uZoPl5=s0)

[Image Source](https://web.dev/fcp/)

First Contentful Paint is one of six metrics tracked in the Google [Lighthouse Performance report](https://web.dev/performance-scoring/), along with Time to Interactive, Speed Index, Total Blocking Time, Largest Contentful Paint, and Cumulative Layout Shift. Each metric measures an aspect of page load speed.

**![](https://lh5.googleusercontent.com/syyN6khfvvKbnizrJ_WCzijFrxo6sa7x6ZavuKyXd4gIasI1dMKgK_bf24L_6v0rm1QGWPVIOjvTjeeZ555DVJAKJ3NVSKCY69gsrE2BsWCb33ym9MYVYQYR5lLR_huzn7KRi4bM=s0)**

[Image Source](https://web.dev/first-contentful-paint/)

First Contentful Paint is an important metric for judging the page load timeline because it marks the point where a user can see that something is happening on the screen. Without this reassurance, a user might leave the page to browse a faster website.

First Contentful Paint differs from the [Core Web Vitals](https://blog.hubspot.com/marketing/core-web-vitals) Largest Contentful Paint (LCP) because LCP measures the time it takes for the largest element on a website to become visible. On the other hand, FCP measures the first element to load, which isn't necessarily the largest element.

A quick LCP helps assure people that the main content is useful to them. But a fast FCP reassures people that _something_ is happening on the page, which can keep them around long enough for the rest of the page to load.

## How to Test First Contentful Paint

FCP can be measured in the lab (pre-release) and in the field (real-world users).

Testing FCP in the lab is a good way to work out issues before your site goes live, but it isn't the most accurate way to evaluate performance. That's where field testing comes in, showing you how people interact with your site when there are differences in devices, network connections, and user interactions.

You can use the following tools to test First Contentful Paint:

**Field Tools**

-   [PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights/)
-   [Chrome User Experience Report](https://developers.google.com/web/tools/chrome-user-experience-report)
-   [Search Console (Speed Report)](https://webmasters.googleblog.com/2019/11/search-console-speed-report.html)
-   [web-vitals JavaScript library](https://github.com/GoogleChrome/web-vitals)

**Lab Tools**

-   [Lighthouse](https://developers.google.com/web/tools/lighthouse/)
-   [Chrome DevTools](https://developers.google.com/web/tools/chrome-devtools/)
-   [PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights/)

For this article, let's walk through what it looks like to run a test with Lighthouse – an open-source, automated tool for improving the quality of web pages. (If you've never run this audit before, follow the link for easy step-by-step instructions).

Once you run the test for a given URL, Lighthouse opens a new tab to share the site performance overview. In the example below, the site is performing well in SEO and Accessibility but needs work on Performance and Best Practices.

![](https://lh4.googleusercontent.com/xMdmoOV0LqKMt-xfJJqFq3qTsJEVTdLgenzrnz7JUmtANDGxpA9haEeVFxxMbuYwbjDjCMEAEb4pEHU-gES8kobaHkaZAq9gOrmvCvmZFkImZvZuZmLjXkgwZVmRS-kvlks1szUf=s0)

[Image Source](https://googlechrome.github.io/lighthouse/viewer/?psiurl=https%3A%2F%2Fwww.hubspot.com%2F&strategy=mobile&category=performance&category=accessibility&category=best-practices&category=seo&category=pwa&utm_source=lh-chrome-ext)

Going deeper, the audit also gives scores for each of the six performance metrics, including First Contentful Paint (FCP). In the test shown below, the FCP score is 2.5 seconds – a time that "needs improvement."

![](https://lh3.googleusercontent.com/PwYBURGVSa9lSmYbuWJtYU3Fhke5-5FgSnEJBUYWr6lOBBoPo38QhsT9eMJ6RdKuY05bgE-Ayx1lVm_nu0RH4Iu9euICzS3VJ-t6OUjTCoQYIYdq98_PnjmY0j2IQF7ZOdTfsy7P=s0)

[Image Source](https://googlechrome.github.io/lighthouse/viewer/?psiurl=https%3A%2F%2Fwww.hubspot.com%2F&strategy=mobile&category=performance&category=accessibility&category=best-practices&category=seo&category=pwa&utm_source=lh-chrome-ext)

But you need to know what makes a "good" score in order to improve FCP.

## The Ideal First Contentful Paint Speed

Google recommends a First Contentful Paint scoring of [1.8 seconds or less](https://web.dev/fcp/#what-is-a-good-fcp-score) in order to provide your site visitors with a good browsing experience.

![](https://lh3.googleusercontent.com/aUqPSdW5d_M5YHdD7SRPdhoEm-uGy20kSF0ANa0FKFRBDZKXCSxOzDebnJIUodqXJrI71osUsMk4TOK9ny0Efm7BWLInR_lqDtjKaP2lChQDxP-tm-ofE372HRFVyd47UUAmpOth=s0)

[Image Source](https://web.dev/fcp/)

But what determines your FCP score?

Like all things Google, there's a method to the metric. Your FCP score is determined by comparing your site's FCP time to FCP times for real sites, using data from the [HTTP Archive](https://httparchive.org/reports/loading-speed#fcp). You can dive deeper to see how [Lighthouse determines thresholds and metric scores](https://web.dev/performance-scoring/#metric-scores).

When evaluating your FCP score, Google says "a good threshold to measure is the 75th percentile of page loads, segmented across mobile and desktop devices." This helps get an accurate representation of the user experience.

If your site has a poor FCP score, there are steps you can take to shave off seconds and create a faster site that visitors want to scroll through. But first, let's explore what leads to a poor score.

## What Causes High First Contentful Paint

Large text files, slow server response time, and multiple page redirects can all contribute to a high First Contentful Paint score. If you have a high First Contentful Paint (FCP), it's likely due to one of these factors:

-   Slow font load time
-   Slow server response times (TTFB)
-   High request counts and large transfer sizes
-   Render-blocking resources
-   Unused or inefficient CSS
-   Script-based elements above the fold
-   Lazy loading above the fold
-   Not inlining images above the fold
-   Excessive DOM size
-   Multiple page redirects

But keep in mind, the Lighthouse Performance score is a weighted average of all the metric scores – and the FCP makes up 10% of that total. As a result, the heavily weighted scores will have a larger impact on your overall Performance scoring. Here's a look at how the other Lighthouse metrics are weighted:

![](https://lh6.googleusercontent.com/eAmAefPiLH0lFlN1pIGqtFVxacInd3WSWb1gDbVOz-o8vR0-5aQ6hx2P5rN3nHJZbEm0LUFpG4awXwCUCIwhs07qSd5LWuWITAc8Ko2nnEtUumo16WfOoP2ecCfcXBz4sqi4pxAD=s0)

[Image Source](https://web.dev/performance-scoring/#metric-scores)

If your overall Performance score needs improvement, it can be best to spend time optimizing for Total Blocking Time or Largest Contentful Paint before tackling First Contentful Paint. As you implement good development practices across the site, it's likely your FCP score will lower.

But if you want to improve FCP, you can take a few targeted steps to move from a red to a green score.

## How to Improve First Contentful Paint

It's not always simple to improve a First Contentful Paint (FCP) score. But with the right action plan in place, it's easier to prioritize the major errors that have the greatest impact. Let's break down how to go about it.

### 1. Create a list of high-priority issues.

The first step to lowering the FCP score for any site is to run the list of lab and field tests shared above to understand _exactly_ what you need to work on.

Let's hop back into the Lighthouse performance report from earlier. If the FCP score"needs improvement,"it's best to reference the [opportunities](https://web.dev/lighthouse-performance/#opportunities) or [diagnostics](https://web.dev/lighthouse-performance/#diagnostics) recommendations in the report. To see all of the recommendations, toggle to the "All" tab. Or for recommendations specific to the First Contentful Paint (FCP) score, toggle to the "FCP" tab.

![](https://lh3.googleusercontent.com/1_Ymi4vjY3JGAta4Foq8rLM5YOvNg213ZU-pFJ5yC6uzoEkGqawjWEDCtYTqgeZztDGx-2OHR7B8j7WhmmMDRz5p8i75dE4WfrMjePWbE9bBn_DcT5n9klbMw6eozdPAsXvlhLgu=s0)

[Image Source](https://googlechrome.github.io/lighthouse/viewer/?psiurl=https%3A%2F%2Fwww.hubspot.com%2F&strategy=mobile&category=performance&category=accessibility&category=best-practices&category=seo&category=pwa&utm_source=lh-chrome-ext)

The above test shares two opportunities to improve FCP: eliminate render-blocking resources and ensure text remains visible during the Webfont load.

By learning the top issues affecting FCP, you'll have a list of where to focus and what to fix.

### 2. Learn what to ignore.

Another helpful feature of the Lighthouse performance report is letting you know what you _don't_ need to focus on. This list is generated under the "Passed audits" section of the performance report.

![](https://lh5.googleusercontent.com/TV2wutnjFymBdpbEcn-eazu9mPcWeg3szwS6bAnj7kNvUD72d19MYb4lrakF9nt2YCuZyRQfoz72Iuy4EhPgv3Wv5dIscjfb3toRe410U5vmcqO7mB8mxRR4P6D-XQiKZIi9Ydgc=s0)

[Image Source](https://googlechrome.github.io/lighthouse/viewer/?psiurl=https%3A%2F%2Fwww.hubspot.com%2F&strategy=mobile&category=performance&category=accessibility&category=best-practices&category=seo&category=pwa&utm_source=lh-chrome-ext)

While it's okay to ignore these non-issues, know that Google constantly updates the metrics used to evaluate page load speed. It's good practice to routinely run tests to ensure site performance is on track – you may need to prioritize a "passed audit" one day.

### 3. Work with your web team to fix issues.

Once you know what issues to pay attention to, it's simply a matter of taking action to improve the ones impacting First Contentful Paint (FCP).

This post won't get into the weeds of web development. But these detailed guides from Google are excellent resources for understanding each factor that affects page speed and performance. If one is impacting your FCP score, you can take a look to learn how to fix the issue.

-   [Eliminate render-blocking resources](https://web.dev/render-blocking-resources/)
-   [Minify CSS](https://web.dev/unminified-css/)
-   [Remove unused CSS](https://web.dev/unused-css-rules/)
-   [Pre-connect to required origins](https://web.dev/uses-rel-preconnect/)
-   [Reduce server response times (TTFB)](https://web.dev/time-to-first-byte/)
-   [Avoid multiple page redirects](https://web.dev/redirects/)
-   [Preload key requests](https://web.dev/uses-rel-preload/)
-   [Avoid enormous network payloads](https://web.dev/total-byte-weight/)
-   [Serve static assets with an efficient cache policy](https://web.dev/uses-long-cache-ttl/)
-   [Avoid an excessive DOM size](https://web.dev/dom-size/)
-   [Minimize critical request depth](https://web.dev/critical-request-chains/)
-   [Ensure text remains visible during Webfont load](https://web.dev/font-display/)
-   [Keep request counts low and transfer sizes small](https://web.dev/resource-summary/)

Whether your First Contentful Paint (FCP) score is showing red, yellow, or green, there are always improvements to be made. It's the fun – and sometimes, frustrating – part of web development.

But remember, small changes can have a big impact. Reducing server response times, compressing images, and being aware of the elements above the fold can lower your FCP score, speed up your site, and ensure site visitors have a faster, longer browsing experience.

Originally published Oct 12, 2021 7:00:00 AM, updated October 12 2021 
 [https://blog.hubspot.com/marketing/first-contentful-paint](https://blog.hubspot.com/marketing/first-contentful-paint) 
 [https://blog.hubspot.com/marketing/first-contentful-paint](https://blog.hubspot.com/marketing/first-contentful-paint)
