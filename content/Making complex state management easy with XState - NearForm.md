# Making complex state management easy with XState - NearForm
## Simplify state machines and statecharts with JavaScript

Let’s look at managing application state from a different perspective. If you’re a frontend developer or a backend developer, you probably deal with state management daily. XState is a JavaScript/TypeScript implementation of the finite state machine and statecharts that will make your life easier.  
The companion code to this blog post is available at [https://github.com/nearform/react-xstate-demo](https://github.com/nearform/react-xstate-demo).

### The what, why and how of state machines

Before we get down to the nitty-gritty of using XState, let’s look at the state machine and its building blocks.

![](https://www.nearform.com/wp-content/uploads/2021/10/on-off.png)

A simple on/off switch with two states. Image from [https://statecharts.dev/](https://statecharts.dev/)

#### What are state machines?

A finite state machine is a mathematical model of computation describing the behaviour of a system. The number of states in a state machine is finite (hence finite state machine). Based on a current state and given some input, the state machine performs a state transition. A state machine can only ever be in exactly one of its possible finite states.  
So, the building blocks of a state machine are:

-   A finite number of states
-   An initial state
-   Conditions for each state transition

#### What are statecharts?

The difference between state machines and statecharts is that you can organise states in a hierarchy with statecharts. Simply put, you can create sub-states by nesting state machines.

![](https://www.nearform.com/wp-content/uploads/2021/10/statechart.png)

A simple statechart. Image from [https://statecharts.dev/](https://statecharts.dev/)

A state without a sub-state is called an atomic state. A state with a sub-state is called a compound state.

#### What are the benefits?

State machines are a common way of describing states in a business process. They are an excellent communication tool because they are generally understood (or can be learned) by people with non-developer backgrounds. And diagrams certainly communicate information better than text.

##### No impossible states

One of the things I especially like about the statecharts is that they make impossible states unpresentable.

What is a _state_, anyway? A state is the set of all variables in a computer program and their values at any point in time. If a program has four independent boolean variables, there are theoretically 16 (2 to the power of 4) possible states. With every new variable, the number of states increases exponentially. Five variables correspond to 32 possible states, six variables to 64 states.

What are impossible states? Let’s consider a simple program that controls a fan. The fan can be either powered on or powered off and be either oscillating or not oscillating. Let’s see what a table of all possible variable combinations would look like:

![](https://www.nearform.com/wp-content/uploads/2021/10/Screenshot-2021-10-07-at-11.17.27.png)

Have you noticed the combination of power \[off] and oscillation \[on]? A fan can’t be both powered off and oscillating at the same time. In other words, this state is impossible and should not be presented to the user (or even to a developer).

With state charts, this sort of problem is nothing you have to worry about.

![](data:image/svg+xml,%3Csvg%20xmlns%3D%27http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%27%20width%3D%271132%27%20height%3D%27404%27%20viewBox%3D%270%200%201132%20404%27%3E%3Crect%20width%3D%271132%27%20height%3D%27404%27%20fill-opacity%3D%220%22%2F%3E%3C%2Fsvg%3E)

A statechart of a fan with oscillation

As you can see, there are only three possible states:

1.  powered_off
2.  powered_on + oscillating_off
3.  powered_on + oscillating_on

We can now toggle the oscillation on and off only when we’re in the powered_on state. The impossible state powered_off + oscillating_on is no longer possible.

##### Some other benefits

There are many more benefits when using state machines or statecharts, including:

-   The behaviour (when things happen) is decoupled from the actual component (what happens).
-   They produce lower bug counts than traditional code.
-   As the complexity of your program grows, statecharts scale well.
-   You explore all the possible states of your program.

### Meet the XState

As mentioned earlier in the article, XState is a JavaScript/TypeScript implementation of finite state machines and statecharts.  
With regards to the fan statechart, this is what its definition looks like using XState:

Copy to Clipboard

​x

    import { createMachine } from 'xstate';

    ​

    const machine = createMachine({

     initial: "powered_off",

     states: {

     powered_on: {

     initial: "oscillating_off",

     on: {

     TURN_OFF: 'powered_off' 

     },

     states: {

     oscillating_off: {

     on: {

     OSCILLATE_ON: 'oscillating_on'

     }

     },

     oscillating_on: {

     on: {

     OSCILLATE_OFF: 'oscillating_off'

     }

     }

     }

     },

     powered_off: {

     on: {

     TURN_ON: 'powered_on'

     }

     }

     }

    });

    ​

    ​

#### How to create a state machine with XState

In order to create a state machine with XState, you import the `createMachine` function and pass it a machine configuration object.  
Best practice is to start with just the initial state and a list of all possible states your machine can be in at one given time.

Let’s consider a simple program that controls a light bulb with adjustable brightness (33%, 66%, 100%) and can also be in an off state.

Copy to Clipboard

    import { createMachine } from 'xstate';

    ​

    const machine = createMachine({

     initial: 'off',

     states: {

     off: {},

     '33%': {},

     '66%': {},

     '100%': {}

     }

    });

After we have defined all the possible states, it’s time to think about the transitions between the states — more specifically, what events will trigger the transitions (i.e. what could happen in the application), what the users could do and what external events might change the state.

To keep things simple, the program has just a single button that will cycle through the state in this order: off → 33% → 66% → 100% → off. It seems logical to name the event CYCLE.

Defining transitions in a state is very simple: Create an `on` property inside the step definition. This property holds a map from an event to a new state — i.e. if I’m in the `off` state and a `CYCLE` event occurs, I want the machine to transition to a `33%` state.

Copy to Clipboard

    import { createMachine } from 'xstate';

    ​

    const machine = createMachine({

     initial: 'off',

     states: {

     off: {

     on: {

     CYCLE: '33%',

     }

     },

     '33%': {

     on: {

     CYCLE: '66%',

     }

     },

     '66%': {

     on: {

     CYCLE: '100%',

     }

     },

     '100%': {

     on: {

     CYCLE: 'off',

     }

     }

     }

    });

This is all it takes to create a very simple state machine. To test a transition from `off` state to `33%` state, you use a `transition` method on the machine. The `transition` method takes two arguments — a state and an event — and produces the next state.

Copy to Clipboard

    const nextState = machine.transition('off', 'CYCLE');

    console.log(nextState.value); // 33%

This is what the diagram for this machine looks like:

![](data:image/svg+xml,%3Csvg%20xmlns%3D%27http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%27%20width%3D%271156%27%20height%3D%27350%27%20viewBox%3D%270%200%201156%20350%27%3E%3Crect%20width%3D%271156%27%20height%3D%27350%27%20fill-opacity%3D%220%22%2F%3E%3C%2Fsvg%3E)

However, this method means we have to keep track of the machine’s state ourselves. There’s a better way.

#### Interpreted state machines

We can use an interpreted state machine to keep track of the state for us. To create an interpreted state machine, we pass the machine created with `createMachine` into the `interpret` function. The result of the `interpret` function is called a service.

Copy to Clipboard

    import { interpret } from 'xstate';

    ​

    const service = interpret(machine).start();

When the service is created, we can use the `send` method to send events to the state service. Just remember to always start the service with the `start()` method.

Copy to Clipboard

    const service = interpret(machine).start();

    service.send('CYCLE'); // service.state.value == 33%

    service.send('CYCLE'); // service.state.value == 66%

    service.send('CYCLE'); // service.state.value == 100%

    service.send('CYCLE'); // service.state.value == off

#### Creating side effects

Every application needs side effects, and applications created with XState are no exception. Some of the many ways to produce side effects with state machines made with XState include:

-   Actions on entering a state
-   Actions on exiting a state
-   Activities during the lifetime of a machine state
-   Invoking services (Promises, Callbacks, Observables, other XState machines)

You can find many more in [the official documentation](https://xstate.js.org/docs/guides/start.html).

#### Usage with React

It’s also very straightforward to use the XState together with React (or Vue, RxJS, Ember, Stencil, Svelte). For that purpose, you use the [@xstate/react](https://github.com/statelyai/xstate/tree/main/packages/xstate-react) package. XState itself must also be installed as a project dependency. The [documentation](https://xstate.js.org/docs/recipes/react.html#local-state) offers plenty of tips on how to improve performance and how to avoid unnecessary re-renders.

To make the lightbulb example from the above work in React, we just import the `useMachine` hook from @xstate/react and pass it the state machine we have already created.

Copy to Clipboard

    import { useMachine } from "@xstate/react";

    import { machine } from "./machine";

    ​

    function App() {

     const \[current, send\] = useMachine(machine);

    ​

     return (

     <div className="App">

     <div className="lightbulb" data-brightness={current.value}></div>

     <div className="state">Machine state: <strong>{current.value}</strong></div>

     <button onClick={() => send('CYCLE')}>Cycle</button>

     </div>

     );

    }

    ​

    export default App;

After adding a little bit of styling, this is the end result:

![](data:image/svg+xml,%3Csvg%20xmlns%3D%27http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%27%20width%3D%27932%27%20height%3D%27402%27%20viewBox%3D%270%200%20932%20402%27%3E%3Crect%20width%3D%27932%27%20height%3D%27402%27%20fill-opacity%3D%220%22%2F%3E%3C%2Fsvg%3E)

Lightbulb state machine in React

You can find the complete source code at [https://github.com/nearform/react-xstate-demo](https://github.com/nearform/react-xstate-demo).

#### State machines diagrams

XState has a fantastic tool to paste in your state machine code and get back an interactive diagram. This makes it really easy to explore all the machine’s possible states and what events lead to the respective states. The tool is located at [https://xstate.js.org/viz/](https://xstate.js.org/viz/).

Not only can you paste the state machine code into the tool mentioned above, but you can also use [@xstate/inspect](https://github.com/statelyai/xstate/tree/main/packages/xstate-inspect). This package makes it possible to visualise your application’s XState machine so that you can better understand its behaviour.

### It all depends on how you use it

XState is an excellent tool, but, like every tool, it must be used in the appropriate way. It is not magic, and it is not going to solve all your problems.  
State machines are a great fit if you need to control the application’s flow carefully. This avoids much of the traditional tooling around to keep things in the correct state.

If you start using XState, it does not mean that everything will have to live inside the state machine. Some things need to be controlled on a local level inside a component rather than on a global level inside a state machine. That goes for both the state and the side effects.

Make smaller state machines rather than one enormous state machine. Programming is about composability, making larger things from smaller things — and state machines compose well.

#### Useful resources

[https://www.itemis.com/en/yakindu/state-machine/documentation/user-guide/overview_what_are_state_machines](https://www.itemis.com/en/yakindu/state-machine/documentation/user-guide/overview_what_are_state_machines)  
[https://workflowengine.io/blog/why-developers-never-use-state-machines/](https://workflowengine.io/blog/why-developers-never-use-state-machines/)  
[https://statecharts.dev/](https://statecharts.dev/)  
[https://egghead.io/lessons/xstate-course-intro-and-overview](https://egghead.io/lessons/xstate-course-intro-and-overview) 
 [https://www.nearform.com/blog/making-complex-state-management-easy-with-xstate/](https://www.nearform.com/blog/making-complex-state-management-easy-with-xstate/) 
 [https://www.nearform.com/blog/making-complex-state-management-easy-with-xstate/](https://www.nearform.com/blog/making-complex-state-management-easy-with-xstate/)
