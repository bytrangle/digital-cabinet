# What are Pure Functions and Side Effects in JavaScript?
## Introduction to JavaScript Functions

A function allows us to place code logically to execute a task. `Functions` are first-class citizens in the JavaScript programming language. You can create, modify a function, use it as an argument to another function, or return from a function. You can also assign a function as a value to a variable. In a nutshell, you will hardly use or write any useful JavaScript code without using functions.

In this article, we will learn about `Pure Function`, its advantages. We will also take a look into `Side Effects` and their impacts.

If you like to learn from video content as well, this article is also available as a video tutorial here: 🙂

</div>

_Please feel free to [subscribe](https://www.youtube.com/tapasadhikary?sub_confirmation=1) for the future content_

A function may take zero or more inputs and produce an output. You may explicitly return output from a function, or it just returns an `undefined`.

A function returning a value explicitly,

```

function testMe(input) {
    
    return `testing ${input}`;
}


testMe(123); 

```

A function not returning a value explicitly,

```

function testMe() {
   
}


testMe(); 

```

So, as we understand the basic usages, let us look into today's `Pure Function` topic. We will also understand the concept, `Side Effects` and its impact on the pure functions.

## Pure Functions and Side Effects with Examples

As a software programmer/developer, you write source code to produce an output based on the inputs. Usually, you write `functions` to perform the tasks based on inputs and produce an output. We need to make sure these functions are,

-   **Predictable**: It produces a predictable output for the same inputs.
-   **Readable**: Anyone reading the function as a standalone unit can understand its purpose completely.
-   **Reusable**: Can reuse the function at multiple places of the source code without altering its and the caller's behavior.
-   **Testable**: We can test it as an independent unit.

A `Pure Function` has all the above characteristics. It is a function that produces the same output for the same input. It means it returns the same result when you pass the same arguments. A pure function shouldn't have any `side effects` to change the expected output.

The function `sayGreeting()` below is a pure function. Can you please guess why?

```
function sayGreeting(name) {
  return `Hello ${name}`;
}

```

It is a pure function because you always get a `Hello <name>` as output for the `<name>` pass as an input. Now, let us see the same function with a bit of change.

```
let greeting = "Hello";

function sayGreeting(name) {
  return `${greeting} ${name}`;
}

```

Is it a pure function? Well, No. The function's output now depends on an outer state called `greeting`. What if someone changes the value of the `greeting` variable to `Hola`? It will change the output of the `sayGreeting()` function even when you pass the same input.

```

sayGreeting('Alex'); 


sayGreeting('Alex'); 

```

So, here we have seen the side-effect of depending on an outer state value that may change without the function being aware of it.

A few more classic cases of the side effects are,

-   Mutating(changing) the input itself.
-   Querying/Updating DOM
-   Logging(even in the console)
-   Making an XHR/fetch call.

Any operation that is not directly related to the final output of the function is called a `Side Effect`. Now let us see an `impure` function where we mutate the input and do something that we are not supposed to in a pure function.

```
function findReverse(users, item) {
    const reversedUsers = users.reverse();
    const found = reversedUsers.find((user) => {
        return user === item;
    });

    document.getElementById('user-found').innerText = found;
}

```

The above function takes two arguments, a collection of users(an array) and an item to find in the array. It finds the item from the end of the array by reversing it. Once the item is found in the array, it set that value as a text to an HTML element using DOM methods.

Here we are breaking two essential principles of the `pure function`.

1.  We are mutating the input.
2.  We are querying and manipulating the DOM

So, what kind of problem we can anticipate? Let's see. A caller will invoke the `findReverse()` function in the following way,

```
let users = ['Tapas', 'Alex', 'John', 'Maria'];
findReverse(users, 'Maria');

```

At this stage, the caller may not know that the function is making a DOM operation unless the caller reads the findReverse() function code. So, `readability` is compromised. The function's output is performing an operation that is not related to the final output.

Also, we have mutated the input array. Ideally, we should have cloned the input and then mutated (reverse) the copy for the find operation. Let us now make it a pure function.

```
function findReverse(users, item) {
    
    const reversedUsers = [ ...users].reverse();

    
    const found = reversedUsers.find((user) => {
        return user === item;
    });

    
    return found;
}

```

Then,

```
let users = ['Tapas', 'Alex', 'John', 'Maria'];
let found = findReverse(users, 'Maria');

```

Now the `findReverse()` function is a pure function. We have removed the side effects of mutating the input, and it returns the intended output. Hence the function is readable, testable as a unit, reusable, and predictable.

Pure function and side effects are the concepts of `functional programming`. You may hit a couple of jargon that needs a friendly clarification.

-   **Referential Transparency**: It means we should be able to replace a function call(or invocation) with its output value without changing the program's behavior. As you see, it is possible only if the function is a `pure function`.

    Let's take a simple pure function,

    ```
    function multipication(x, y) {
     return x * y;
    }

    ```

    So, now in this expression, we can replace the function call with its output value with an assurance of no `side effect`,

    ```
    10 + (multiplication(6, 3) ^ 2);

    ```

    to,
-   **Parallel Code**: Pure functions help in parallel code execution. However, in JavaScript, code runs sequentially by default.

## So, Can I make all functions `Pure Functions`?

Yes, technically, you can. But the application with only pure functions may not do much.

Your application program will have side effects like HTTP calls, logging to console, IO operations, and many more. Please use pure functions in as many places as you find possible. Isolate impure functions(side effects) as much as possible. It will improve your program's readability, debuggability, and testability a lot.

## Conclusion

Embracing functional programming concepts like a pure function, reducing side effects will make your code better to manage and maintain. It means lesser bugs, quick identification of issues, isolating problems, increased reusability and testability.

If you want to explore this topic further and get deeper into functional programming, please pick up this book [Functional-Light JavaScript](https://github.com/getify/Functional-Light-JS) by Kyle Simpson. It's worth reading.

Let's connect. I share my learnings on JavaScript, Web Development, and Blogging on these platforms as well,

-   [Follow me on Twitter](https://twitter.com/tapasadhikary)
-   [Subscribe to my YouTube Channel](https://www.youtube.com/tapasadhikary?sub_confirmation=1)
-   [Side projects on GitHub](https://github.com/atapas) 
    [https://blog.greenroots.info/what-are-pure-functions-and-side-effects-in-javascript](https://blog.greenroots.info/what-are-pure-functions-and-side-effects-in-javascript) 
    [https://blog.greenroots.info/what-are-pure-functions-and-side-effects-in-javascript](https://blog.greenroots.info/what-are-pure-functions-and-side-effects-in-javascript)
