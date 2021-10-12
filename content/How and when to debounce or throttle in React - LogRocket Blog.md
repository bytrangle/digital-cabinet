# How and when to debounce or throttle in React - LogRocket Blog
Have you ever been in a situation where you’re typing in an input field and your computer or phone stops responding, or you’re scrolling through a webpage and it becomes unresponsive?

This usually happens when there are event listeners attached to the actions being performed —like getting realtime results for a search query as you type, or fetching new posts or tweets on social media platforms while you’re still scrolling.

Here’s an example of when showing realtime search results as we type in an input field can make our computer unresponsive:

![](https://blog.logrocket.com/wp-content/uploads/2021/09/unresponsive-input-field-demo.gif)

In the demo above, I’m trying to search for a particular city in a large list of data. I have my virtual keyboard open, so you can see that I’m typing, but our input field stopped being responsive.

This is because our city list contains over 70,000 cities, so you can imagine how much work that is for our computer processor.

As if filtering through this large list of data is not enough, we’re asking our computer to do this 16 times for the 16 keystrokes in the word “Saint Petersburg”. So when we type “S” in the input field, our computer tries to find all the cities that have the letter “S” in them. And while it’s still doing that, we ask it to look for all the cities with “Sa” in them. Same thing for “Sai”, etc., until we’re finished typing out “Saint Petersburg”.

In our demo, we’re not making a server request to get our filtered cities. If we were, that’d be even more expensive and time-consuming. Fortunately, there’s a way to solve our city filter problem, and in this article, we’re going learn how to do that in React, through debouncing and throttling.

## What’s debouncing and throttling?

As you just saw in our city filter problem — where we’re asking our computer to start another process while it’s still trying to complete the previous one — debouncing and throttling are two different ways that we can prevent a function from running in multiple instances at the same time.

This means that in our city filter, we won’t be able to ask our computer to filter out cities with “Sa” while it’s still trying to do the same thing for cities with “S” in their names. We’ll either have to prevent the second process from happening, or stop the first one and then start the second.

If we decide to delay the first process for a given amount of time to see if our user wants to type something else, so that if they do, we’ll cancel the first one and then work on the second instead, that would be debouncing.

If we decide to prevent the second process from happening by making sure that our function can only run once in a given interval, that would be throttling.

For our city filter app, we’ll be using debouncing to solve our problem. If our user wants to search for “Saint Petersburg”, what we’re going to do is give the user a list of cities containing “Saint Petersburg”, not every city that starts with “S” or “Sa” or “Sai”. So, we wait until we’re sure that the user is done typing. It’s possible that our user wants just “Saint” and not “Saint Petersburg”, but we can’t be sure until we wait.

## Our problem-solving process

Here’s the process we’re going to use for our our app. When a user types “S”, we wait for half a second (or three seconds — it can be any amount of time) before we start filtering through our large list of data. And if the user goes ahead to type “a”, we start waiting again for another 500 milliseconds from the last keystroke. In this way, we can delay our filter function until 500 milliseconds after our user types “g”, the last keystroke for Saint Petersburg.

So instead of asking our computer to filter through a list of over 70,000 data 13 times, it does that just once.

In JavaScript, the `setTimeout()` method is a perfect candidate for implementing this solution. Here’s what our component looks like at the moment:

    import cities from  'cities-list'  import  { useState }  from  'react'  const citiesArray =  Object.keys(cities)  const  App  =  ()  =>  {  const  \[filteredCities, setFilteredCities\]  = useState(\[\])  const doCityFilter = query =>  {  if  (!query)  return setFilteredCities(\[\]) setFilteredCities(citiesArray.filter( city => city.toLowerCase().includes(query.toLowerCase())  ))  }  return  (  <div className="container">  <h1>Find your favourite cities</h1>  <form className="mt-3 mb-5">  <input
              type="text" className="px-2" placeholder="search here..." onChange={event  =>  (doCityFilter(event.target.value))}  />  </form>  <div>  {filteredCities?.map((city, index)  =>  (  <p key={index}>{city}</p>  ))}  </div>  </div>  )  }  export  default  App

To set this up on your computer, create a new React app with [Create React App](https://blog.logrocket.com/tag/create-react-app), and replace the content of your `App.js` file with the code above. You’ll also need to install `cities-list` by running `npm install cities-list` on your terminal.

In our `App.js` file, we’re using the React `useState` Hook to store our filtered cities, which we are returning inside the JSX code. Our `<input  />` tag has an `onChange` event listener that takes the user’s input and runs the `doCityFilter()` function with it. This happens every time the value of the input field changes.

Inside of our `doCityFilter()` function, we’re using the `Array.filter()` method to filter through the `citiesArray` that we’ve gotten from our `cities-list` package, and then the `setFilteredCities` method to update the value of `filteredCities`.

## Using JavaScript `setTimeout` to debounce

Since the filter process is happening inside the `setFilteredCities` function call, let’s use the `setTimeout` method to delay it by 500ms:

    const doCityFilter = query =>  {  if  (!query)  return setFilteredCities(\[\]) setTimeout(()  =>  { console.log('====>', query) setFilteredCities(citiesArray.filter( city => city.toLowerCase().includes(query.toLowerCase())  ))  },  500)  }

![](https://blog.logrocket.com/wp-content/uploads/2021/09/debounce-step-one-demo.gif)

While this delays our function, notice that we have another problem. Our filter method is still being called for each of the keystrokes — again, that’s 16 times in total. We only ended up delaying each of the 16 calls by 500ms. What we want is to cancel the previous `setTimeout` method whenever we hit a new key, so that if we have 16 keystrokes, we’ll end up calling the filter function for just the last key. Let’s use the `cleartimeout` method to implement this:

    let filterTimeout const doCityFilter = query =>  { clearTimeout(filterTimeout)  if  (!query)  return setFilteredCities(\[\]) filterTimeout = setTimeout(()  =>  { console.log('====>', query) setFilteredCities(citiesArray.filter( city => city.toLowerCase().includes(query.toLowerCase())  ))  },  500)  }

For our new `doCityFilter` function, we started by declaring a mutable variable named `filterTimeout` outside of its scope. This is so that we can assign our `setTimeout` method to the variable, and then use that to clear our timeout before we create another one. Here’s what our app looks like now:

![](https://blog.logrocket.com/wp-content/uploads/2021/09/successful-debounce-from-scratch-demo.gif)

We just successfully debounced our function! Our app is responsive throughout and our filter process runs only once.

## Using Lodash to debounce

For our use case, the first solution works. But we might want to cover more conditions for our debouncing, like immediately invoking a function if something happens, or invoking a function on the leading or trailing edge of the wait timeout, and even many other conditions that might be needed for other use cases.

If you have to implement all this yourself for every function, you’ll be writing more code than you initially bargained for, which will in turn increase the complexity of your codebase.

Fortunately, [Lodash](https://lodash.com/) has a `debounce` method with more useful functionalities than we can cover. And even more fortunately, you can install just this `debounce` function so that you don’t end up bringing a whole library into your app because of one method.

Let’s run the following command on our terminal to install `lodash.debounce`:

    npm install lodash.debounce

Next, we’ll import it in our `App.js` file. Let’s do that on line 3.

    ...  import debounce from  'lodash.debounce'

The `lodash.debounce` method expects three arguments:

-   The function we want to debounce
-   The wait time
-   An options object for other configurations

Our first instinct might be to debounce the `setFilteredCities` function call like we did with our `setTimeout` implementation:

    const doCityFilter = query =>  {  if  (!query)  return setFilteredCities(\[\])  const debouncedFilter = debounce(()  =>  { console.log('====>', query) setFilteredCities(citiesArray.filter( city => city.toLowerCase().includes(query.toLowerCase())  ))  },  500) debouncedFilter()  }

But this will still result in 16 function calls for the city, Saint Petersburg, and our city filter still does not work as expected:

![](https://blog.logrocket.com/wp-content/uploads/2021/09/using-lodash-step-one-demo.gif)

So, there is something more we must do.

### Working with `Lodash.debounce` and the React `useCallback` Hook

We can use React’s [`useCallback` Hook](https://blog.logrocket.com/react-hooks-cheat-sheet-unlock-solutions-to-common-problems-af4caf699e70/) to prevent our function from being recreated each time the value of our input field changes.

First, let’s import `useCallback` from ‘react’:

    ...  import  { useCallback, useState }  from  'react'  ...

Next, we’ll move our `debouncedFilter` variable outside the `doCityFilter()` function, and as its value, we’ll invoke the `useCallback` Hook and supply the debounce function call as its argument:

    ...  const debouncedFilter = useCallback(debounce(query => setFilteredCities(citiesArray.filter( city => city.toLowerCase().includes(query?.toLowerCase())  )),  500),  \[\]  )  const doCityFilter = query =>  {  ...  }

We can now call our `debouncedFilter` method inside the `doCityFilter` function:

      const doCityFilter = query =>  {  if  (!query)  return setFilteredCities(\[\]) debouncedFilter(query)  }

If we run our app, you’ll see that it works as expected:

![](https://blog.logrocket.com/wp-content/uploads/2021/09/using-lodash-debounce-successful-demo.gif)

For this use case, we can ignore the dependency warning. But you can see that our code now looks complex.

Let’s avoid all these extra steps by debouncing the whole `doCityFilter` function, instead of the `setFilteredCities` function call inside it.

    const doCityFilter = debounce(query =>  {  if  (!query)  return setFilteredCities(\[\]) console.log('====>', query) setFilteredCities(citiesArray.filter( city => city.toLowerCase().includes(query.toLowerCase())  ))  },  500)

When we debounce the whole `doCityFilter` function, we don’t need the `useCallback` Hook anymore. Our function now looks a lot simpler and our app works as expected:

![](https://blog.logrocket.com/wp-content/uploads/2021/09/lodash-debounce-successful-no-usecallback-hook.gif)

## Throttling with `Lodash.throttle`

We can also use Lodash to throttle functions. When we throttle a function, it will only be invoked once in a given period, no matter how many times its corresponding event listener is triggered. Just like `lodash.debounce`, we can install just `lodash.throttle` by running the following command on our terminal:

    npm install lodash.throttle 

Next, we’ll use the following line of code to import it:

    import throttle from  'lodash.throttle'

Its usage is similar to the `lodash.debounce` method. We call the `throttle` method and supply the function we want to debounce as its first argument, the wait time (in milliseconds) as the second argument, and an optional config object as the third argument.

Throttling won’t be a good solution for our filter problem, so we won’t be using this for our app.

## Easier debouncing with `react-debounce-input`

There’s an even easier way to implement input debouncing in React. Let’s learn how to use the `react-debounce-input` package to solve our city filter problem.

The package comes with a `DebounceInput` component that we can use in place of the `<input  />` tag. It has an inbuilt debounce functionality, so we won’t need any external debounce method to debounce our `onChange` event.

Run this command on your terminal to install the `react-debounce-input` package:

    npm install react-debounce-input

Next, we’ll import `DebounceInput` from `react-debounce-input`:

    ...  import  {  DebounceInput  }  from  'react-debounce-input'  ...  

Instead of our current `<input/>` tag in the JSX code, let’s paste this:

    <DebounceInput  className="px-2"  placeholder="search here..."  minLength={1}  debounceTimeout={500}  onChange={event => (doCityFilter(event.target.value))}
    />

Our `<DebounceInput  />` component comes with a `debounceTimeout` prop for specifying how long we want to wait before the `onChange` function is called. Here, we’ve used 500 milliseconds. We also have a `minLength` for specifying the minimum number of inputs we want before our `onChange` event is triggered.

Now that we are using the `<DebounceInput  />` component, we can go back to the simple version of the `doCityFilter()` function:

    const doCityFilter = query =>  {  if  (!query)  return setFilteredCities(\[\]) setFilteredCities(citiesArray.filter( city => city.toLowerCase().includes(query.toLowerCase())  ))  }

Let’s preview another demo of our app:

![](https://blog.logrocket.com/wp-content/uploads/2021/09/debouncing-with-react-debounce-input.gif)

You can see that our city filter app works as expected. That’s all we need to debounce our function with `react-debounce-input`! I consider this the best solution for our filter problem because it’s a lot less code, which makes it easier to maintain and implement in multiple parts of our app.

## Conclusion

In this article, we’ve learned how to debounce and throttle functions in [React](https://blog.logrocket.com/tag/react), and we were able to fix our city filter problem with different implementations. While they all work, it’s good to understand that one solution is not going to fit every scenario. So, go with the simplest one that works for your use case.

You can find [the repo for this example](https://github.com/ebenezerdon/react-debounce-example) on my GitHub. In the repo, you’ll find different branches for the implementations. If you want to keep in touch, consider subscribing to my [YouTube channel](https://youtube.com/ebenezerdon) and following me on [LinkedIn](https://www.linkedin.com/in/ebenezerdon/). Keep building!

## Full visibility into production React apps

Debugging React applications can be difficult, especially when users experience issues that are hard to reproduce. If you’re interested in monitoring and tracking Redux state, automatically surfacing JavaScript errors, and tracking slow network requests and component load time, [try LogRocket](https://www2.logrocket.com/react-performance-monitoring). [![](https://files.readme.io/27c94e7-Image_2017-06-05_at_9.46.04_PM.png?is-pending-load=1)
](https://www2.logrocket.com/react-performance-monitoring)[![](https://blog.logrocket.com/wp-content/uploads/2017/03/1d0cd-1s_rmyo6nbrasp-xtvbaxfg.png?is-pending-load=1)
](https://www2.logrocket.com/react-performance-monitoring)

[LogRocket](https://www2.logrocket.com/react-performance-monitoring) is like a DVR for web apps, recording literally everything that happens on your React app. Instead of guessing why problems happen, you can aggregate and report on what state your application was in when an issue occurred. LogRocket also monitors your app's performance, reporting with metrics like client CPU load, client memory usage, and more.

The LogRocket Redux middleware package adds an extra layer of visibility into your user sessions. LogRocket logs all actions and state from your Redux stores.

Modernize how you debug your React apps — [start monitoring for free](https://www2.logrocket.com/react-performance-monitoring). 
 [https://blog.logrocket.com/how-and-when-to-debounce-or-throttle-in-react/](https://blog.logrocket.com/how-and-when-to-debounce-or-throttle-in-react/) 
 [https://blog.logrocket.com/how-and-when-to-debounce-or-throttle-in-react/](https://blog.logrocket.com/how-and-when-to-debounce-or-throttle-in-react/)
