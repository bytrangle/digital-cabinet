# A guide to pagination, load more buttons, and infinite scroll - LogRocket Blog
Almost every app has data, and that data has to be visualized in some way. A common method for displaying a set of data is via a list, which can become quickly become long and overwhelming to read.

There are three major ways to display large data sets: using pagination, **load more** buttons, and infinite scroll. In this tutorial, I’ll cover all three methods and discuss the pros and cons of using each one.

## Common methods used to display app data

These methods display large data sets on dynamic web pages while also providing faster initial page load. For example, Google Web uses pagination for its search answers.

![](https://blog.logrocket.com/wp-content/uploads/2021/09/google-using-pagination.jpeg)

Twitter and Facebook use infinite scroll, where users keep scrolling to keep seeing new posts. Infinite scroll has become especially popular on social media channels.

On mobile, instead of using pagination, Google and Google Images use a **load more** button to display search results and images.

![](https://blog.logrocket.com/wp-content/uploads/2021/09/google-show-more-results.jpeg)

This article will compare each of these methods and guide you through implementing them into your apps.

## Using pagination to display data

![](https://blog.logrocket.com/wp-content/uploads/2021/09/google-pagination.jpeg?is-pending-load=1)

### Pros to using pagination

1.  Pagination allows for easy navigation and search. On an eCommerce site, for example, you could easily find a product you previously viewed on a specific page by clicking back to that page
2.  Pagination is ideal for websites that prioritize utility because every result-set has its own page and URL, which is easy to remember and share with page numbers. Sometimes, some data sets need to respect a particular order in which the items can be arranged, and pagination allows for that
3.  This method is great for websites that are not designed to be addictive. Social media sites are addictive because it’s difficult to stop scrolling on them, but traditional pagination forces users to click buttons and navigate through pages to access data, which makes it less addictive

### Cons to using pagination

1.  It’s old tech. Pagination has been around since the dawn of the internet. All outdated websites have it, and this type of pagination is no longer intuitive
2.  No one really looks beyond the first page. Do you ever check out the second page on Google results? Likely not. Clicking the numbers **1, 2,** and **Next** can be a hassle that deters people from looking at more results
3.  Pagination is not as mobile-friendly. On a mobile device, users would rather prefer a **load more** button or infinite scroll because of mobile’s vertical-oriented screen
4.  Every new result is on a new page, resulting in unnecessary load time. This can be solved using shallow routing and pre-rendering, but can still be annoying for users nonetheless

### When to use pagination in your app

If you are building a catalog or e-commerce website for the web, use pagination. However, if you are building any mobile website or an app, you’ll want to consider implementing a **load more** button instead.

> Opinion: If you are displaying data in lists on mobile, then always avoid using this method. This method works well with grids.

## Using a load more button in your app

![](https://blog.logrocket.com/wp-content/uploads/2021/09/spotify-see-all.jpeg?is-pending-load=1)

### Pros of using a load more button

1.  **Load more** buttons look great on mobile devices, and unlike pagination, the **load more** mechanism adds more results to the existing ones without replacing the entire results
2.  To find a result, the user only has to keep clicking **load more** without having to remember what page the item was on
3.  **Load more** allows you to easily measure user interest and engagement by the number of times a user clicks **load more**
4.  This method can be used to help users reach the footer of the website faster, meaning your website ads can gather more impressions

### Cons of using a load more button

1.  It can be difficult to search in, as users won’t remember the position of a particular result. To find something, they’ll mostly have to keep clicking **load more**
2.  With **load more**, potential impressions could be lost because whatever content you are hiding under the **load more** button is less likely to be viewed compared to an infinite scroll setup

### When to use load more buttons

If you are building any list visuals on mobile that do not have infinite scroll, use the **load more** method. You should also use this method if you’re app isn’t a social media app (where infinite scroll would be better to implement) and if you want the data load to be intuitive than pagination.

Keep in mind that in many apps (including social media), users refresh their feeds by swiping up, and, specifically in Instagram, by clicking **New Posts**. These are forms of **load more** mechanisms you can take inspiration from.

Also, you should consider using **load more** if you have tons of content but want users to reach a footer or hide content behind a paywall, or, in this case, a **load more** button.

## Implementing infinite scroll

![](https://blog.logrocket.com/wp-content/uploads/2021/09/infinite-scroll.png?is-pending-load=1)

### Pros to using infinite scroll

1.  Infinite scroll encourages users to engage with your app for a longer period of time. With infinite scrolling, there’s nothing stopping the user from scrolling continuously, and it takes minimum effort to do so compared to the pagination and **load more** methods
2.  Compared to other methods, infinite scroll feels fast to users. Scrolling is seamless, and you can always load more content in the background when the user reaches near to the end of the viewpoint, so they wouldn’t even realize how and when the new content loaded
3.  It’s also good for chronological and algorithm-based dynamically served content. With pagination, your data has to be in a certain order. Using infinite scroll means you have flexibility in deciding how to serve content and in what order, which is ideal for social media apps

### Cons to using infinite scroll

1.  If you have a footer on your website or app, it’ll never be seen
2.  Employing infinite scroll can be hugely addictive for your users, which is probably not great for their digital well-being. As of the time of writing, [a bill was introduced in the Senate](https://www.congress.gov/bill/116th-congress/senate-bill/2314/text) that will ban infinite scroll if passed  
    ![](https://blog.logrocket.com/wp-content/uploads/2021/09/senate-bill.jpeg?is-pending-load=1)
    Of course, it’s unlikely that it’ll pass, but this sheds some light on how influential infinite scroll is when it comes to keeping users hooked on your platform
3.  Infinite scroll is much harder to navigate and search through. With pagination, you can remember specific page numbers to retrieve a particular result. But with **load more** and infinite scroll, people will have to scroll up for very long to find particular content

You can [read more about why you shouldn’t use infinite scroll here](https://blog.logrocket.com/infinite-scroll/).

### When to use infinite scroll

If you are building a mobile app or social media app, or simply want users to continuously scroll through tons of content (i.e., blogs), you should use infinite scroll.

## How infinite scroll works

When the user reaches the end of the viewport, you load more results. In plain vanilla JavaScript, it works like this:

    window.onscroll =  function(ev)  {  if  ((window.innerHeight + window.scrollY)  >= document.body.offsetHeight)  {    }  };  

I have drawn a diagram to help explain how data is traversed through and how cursors work:

![](https://blog.logrocket.com/wp-content/uploads/2021/09/data-diagram.png?is-pending-load=1)

So, imagine you query the backend API and there are 900,000 total entries in the database. You wouldn’t want to fetch all of the entries because it would slow app performance.

> For simplification, let’s use the number 9 instead of 900k.

To display this data, you’d have to divide it into multiple parts. It doesn’t have to be displayed in equal intervals, but it usually is. Here is a list of all the entries in the database.

    {  "data":  \[  { id:  1  },  { id:  2  },  { id:  3  },  { id:  4  },  { id:  5  },  { id:  6  },  { id:  7  },  { id:  8  },  { id:  9  }  \]  }

Now, because it isn’t feasible to fetch them all at once, you can fetch the data in chunks from the database in the backend.

For the first result, you only want to fetch the first three entries.

To fetch the first page, query the database for “First 3 entries, After 0 entries”. For fetching the second page, query for “Fetch First 3 entries, After 3 entries,” and so on.

You may also use terms `limit/offset` instead of `first/skip`.

For pagination data fetching, another method often used is called cursors. When using cursors, the “after” value is used to fetch nodes after a specific point.

![](https://blog.logrocket.com/wp-content/uploads/2021/09/data-diagram-2.png?is-pending-load=1)

Here’s a comprehensive article explaining [why you may want to use cursors](https://uxdesign.cc/why-facebook-says-cursor-pagination-is-the-greatest-d6b98d86b6c0) instead of the `limit/offset` method.

When the backend fetches the selective data from the database, it sends a response that usually looks like this:

    {  "data":  \[  { id:  4  },  { id:  5  },  { id:  6  }  \],  "totalCount":  9,  "fetchedCount":  3,  "pageInfo":  {  "prevCursor":  "prev_4",    "nextCursor":  "after_6",    "hasNextPage":  true    }  }

The front end then iterates over these cursors to fetch the pages of the paginated data.

This method remains similar for pagination, **load more**, and infinite scroll, so you could implement all of these by fetching data using the method described above.

For normal pagination, you would have to use both `next` and `previous` cursors. For **load more** and infinite scroll, you only need the `next` cursor, as you have to iterate through them to keep fetching more data.

## Conclusion

Displaying large data sets in the most optimal manner is possible by using one — or a mixture of — some of these pagination methods. Happy coding.

## [LogRocket](https://logrocket.com/signup/): Full visibility into your web apps

[![](https://blog.logrocket.com/wp-content/uploads/2017/03/1d0cd-1s_rmyo6nbrasp-xtvbaxfg.png?is-pending-load=1)
](https://logrocket.com/signup/)

[LogRocket](https://logrocket.com/signup/) is a frontend application monitoring solution that lets you replay problems as if they happened in your own browser. Instead of guessing why errors happen, or asking users for screenshots and log dumps, LogRocket lets you replay the session to quickly understand what went wrong. It works perfectly with any app, regardless of framework, and has plugins to log additional context from Redux, Vuex, and @ngrx/store.

In addition to logging Redux actions and state, LogRocket records console logs, JavaScript errors, stacktraces, network requests/responses with headers + bodies, browser metadata, and custom logs. It also instruments the DOM to record the HTML and CSS on the page, recreating pixel-perfect videos of even the most complex single-page apps.

[Try it for free](https://logrocket.com/signup/). 
 [https://blog.logrocket.com/guide-pagination-load-more-buttons-infinite-scroll/](https://blog.logrocket.com/guide-pagination-load-more-buttons-infinite-scroll/) 
 [https://blog.logrocket.com/guide-pagination-load-more-buttons-infinite-scroll/](https://blog.logrocket.com/guide-pagination-load-more-buttons-infinite-scroll/)
