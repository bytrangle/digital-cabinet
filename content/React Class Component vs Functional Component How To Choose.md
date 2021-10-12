# React Class Component vs Functional Component: How To Choose
There are two ways to create a component in React: class component or functional component. This article demystifies the difference between the two and how to choose which is right for your project.

Components are the core building blocks of your React application. They enable you to break UI into chunks of reusable pieces that can be reused and worked with independently.

There are two ways to create components in React: the class component or the functional component. This article will explain the differences between class and functional components and how to choose between them. If you’re a React developer or new to React, this article clears up some of the confusion between these two types of components in React.

## Differences Between Class Component and Functional Component

### Class Component

The class component, a stateful/container component, is a regular ES6 class that extends the component class of the React library. It is called a stateful component because it controls how the state changes and the implementation of the component logic. Aside from that, they have access to all the different phases of a React lifecycle method.

Before the advent of React Hooks, the class component was the only option to create a dynamic and reusable component because it gave us access to lifecycle methods and all React functionalities.

To demonstrate using the class component, let’s create a simple Counter component that allows users to increase or decrease a number. We will also exhibit a bit of the lifecycle methods in our example below.

In our component example above, we set the initial state with the constructor and use the lifecycle method componentDidMount() to set the state from 0 to 1 when the component is mounted, since we don’t want the count to start at 0.

If you try out the example [live](https://codesandbox.io/s/class-component-pfjrg) here, you will notice that the counts 0 and 1 are shortly displayed after each other. When the component is first rendered, it will swiftly show the count of 0 from the initial state—whereas after the component did mount actually, the componentDidMount will run to set a new count state of 1.

We also implement two functionalities to the component (the handleIncrement() and handleDecrement() functions) to increase and decrease the counter when the user clicks the increment or decrement button.

You can see for the class-based component we took several steps to create this dynamic component. We created the class with a constructor and a render method. We set the initial state with this.state statement in the constructor. We use this.setState() to update the states and lifecycle method like componentDidMount to instantly update the state when the component mounted.

Now, let’s convert the class component to a functional component to differentiate between them.

### Functional Component

Functional components are simply JavaScript functions. Before the advent of hooks in React 16.8, they were mostly referred to as stateless or presentational components because then they only accepted and returned data to be rendered to the DOM.

Before, the class component was the only option to access more React features such as state and React lifecycle methods. However, with hooks, you can implement state and other React features, and, most importantly, you can write your entire UI with functional components.

With hooks, composing components in React is more straightforward. React has two most commonly used hooks: the state (useState) and the effect (useEffect) hooks. We will demonstrate how to use both in the example below. However, if you are new to React, you can learn more about React Hooks [here](https://reactjs.org/docs/hooks-intro.html).

To demonstrate the difference between functional and class components, let us reimplement the previous class Counter component to a functional component.

Since you already understand what this Counter component does from our previous explanation, let’s look at how functional components implement this compared to the class component.

First of all, you don’t need a constructor or the render methods since it’s just a function and not a class. Hooks enable you to integrate all the necessary features of React Library previously available only to the class component, so with useState, you can add states to functional components. As we did [above](https://codesandbox.io/s/functional-component-n5ede), we imported useState from React to set the initial state of count to 0. The useState hook will return a pair of values: the current state and a function that updates it. Take a look at the code section below as compared to how we implement states in class component with this.state and this.setState.

The state hook will return a pair of values: the current count, and a function setCount that updates the state count. Can you see how simple it is to implement states in a functional component?

If you have worked with the class component before, you should be familiar with the React lifecycle methods such as componentDidMount and componentWillUnmount. Before, we didn’t have this ability in functional components, but now with the useEffect hook, you can implement React lifecycle methods. The effect hook enables you to perform side effects in functional components. You can think of useEffect as componentDidMount, componentDidUpdate and componentWillUnmount combined.

From the above functional component example, to implement the lifecycle method like componentDidMount in the class component, we used the useEffect hook:

With this effect hook, you notify React your component needs to do something after rendering. Then React will remember the function you passed and call it later after performing the DOM updates.

So in the above example, we set the count state variable and then tell React we need to use an effect. A function is passed to the useEffect hook. This function we passed is our effect, and inside our effect, we updated the state count. You will notice the counts 0 and 1 displayed shortly after each other. The first render of the component shows the count of 0 from the initial state. Then after the component is mounted, the useEffect hook will run to update the new count state to 1.

Also, if you look at the useEffect function, you will notice the empty array as a second argument. This is to make sure the effect hook only triggers when the component mounts and unmounts. If you experiment by removing the second argument, you will run into an infinite loop of increasing the count by 1. This is because the effect hook always runs after the state has changed. Since the effect hook triggers another state change, it will run again and again to increase the count.

That’s a lot of explanation—just trying to clarify for those new to React.

### Reiterating the Differences

**Syntax**  
From the demonstration, the apparent difference is the syntax. Personally, I found the functional component easier to understand compared to the class component, although this might be different for a developer from object-oriented programming like Java.

The class component uses ES6 class syntax, and it extends React components with a render method that returns React elements. On the other hand, functional components with hooks are purely JavaScript functions that also return React elements.

**State and Lifecycle Methods**  
Before the introduction of hooks, functional components were stateless. However, with React 16.8, you can implement states with the useState hook to create a stateful component (just like the class component).

Also, with lifecycle methods, you can use the useEffect hook with functional components to accomplish the same effect as you would with lifecycle methods such as componentDidMount, componentDidUpdate and componentWillUnmount combined with the class component.

You can read more about states and lifecycle methods in React component [here](https://reactjs.org/docs/state-and-lifecycle.html).

## How To Choose Between Function or Class Component

After explaining the differences between the two components and how they are used to build components in React, we will look into how to choose between class and functional components in this section. And also learn reasons why to always consider functional components in your new React applications.

Nonetheless, this is not a judgment between the two. From experience, React developers have different opinions and preferences between the two components. So if you have different opinions about this section, kindly share and engage me in the comment section.

Before we proceed, we need to understand why functional components were introduced to replace the class component. According to the React team, these are the motivations for introducing hooks in functional components:

-   It is hard to reuse stateful logic between components in the class component.
-   Complex components are hard to understand in the class component.
-   Class confuses both people and machines.

Read more in full detail about the motivations in the [React Docs](https://reactjs.org/docs/hooks-intro.html).

The React team recommended that new apps should be built with functional components and hooks. So, you should really consider the functional component approach when working with a new React project—unless your team prefers the class-based approach. But, if you’re new to React, the knowledge of the class component also comes in handy. Perhaps you might need to migrate a legacy codebase written with a class component to a functional component.

In my personal opinion, I will share with you my experience working with both class and functional components. Why you should always choose functional components:

-   Functional components with hooks are concise and more straightforward to code with. They perform exactly as the class component; this implies no difference between the two other than syntax.
-   By using just functional components in your project, you drastically eliminate the need to refactor the class component into a functional component when it grows.
-   Since classes confuse both people and machines, most especially the this keyword, you don’t have to worry about this anymore in functional components.
-   No need for unnecessary method binding like we always do in the class component.
-   Sharing stateful logic between components is tedious in a class-based approach.

Besides, the React team recently announced React docs will be focusing on explaining React using functional components and hooks. This update doesn’t mean the class component will be deprecated—it will still be around for years to come. Likewise, the class component docs will still be available for developers that need to use them.

## Conclusion

I hope you enjoyed reading through this article. We explained the differences between the two approaches of composing components in React. The class component is a regular ES6 class that extends the React component library to create a stateful component. In contrast, functional components with hooks can be used to build stateful or presentational components. We also explained how to choose between the two components and why you should always consider functional components in your React projects. 
 [https://www.telerik.com/blogs/react-class-component-vs-functional-component-how-choose-whats-difference](https://www.telerik.com/blogs/react-class-component-vs-functional-component-how-choose-whats-difference) 
 [https://www.telerik.com/blogs/react-class-component-vs-functional-component-how-choose-whats-difference](https://www.telerik.com/blogs/react-class-component-vs-functional-component-how-choose-whats-difference)
