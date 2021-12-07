# Hooking into React Lifecycle methods (useEffect)

## React Lifecycle Methods

When building react applications, often times it may be desired to tap into what are known as the React Lifecycle Methods. The most common cases are:
  - Run some code after a component mounts (`componentDidMount`, replaced by `useEffect`)
  - Run some code before a component unmounts (`componentWillUnmount`, replaced by `useEffect`)
  - Run some code after a component updates (`componentDidUpdate`, replaced by `useEffect`)
  - Determine whether an update should be triggered based on comparing new props to old props (`shouldComponentUpdate`, replaced by `React.memo`)
  - Getting derived state from props (`getDerivedStateFromProps`, you [probably dont need it](https://reactjs.org/docs/hooks-faq.html#how-do-i-implement-getderivedstatefromprops).)
  - [More info availble at the Hooks FAQ](https://reactjs.org/docs/hooks-faq.html#how-do-lifecycle-methods-correspond-to-hooks)

All of these cases can be accomodated with hooks. This adds a bit of depth to the `useEffect`, with the added bonus that this new way of thinking about react components helps alleviate common lifecycle bugs that can tend to pop up with class based components.

## What is useEffect?

Component Renders -> Effects May Run -> Component Renders (if state was updated)

`useEffect`, in essence, is a hook that manages 'side effects'. Side effects, as defined by the React Team, are basically anything that affects something outside of the scope of the current function that's being executed. This could include asyncronous behavior, manipulating unmanaged DOM elements (like `<title>` tags), managing loading state, starting / stopping timers, and all the associated cleanup with previously mentioned functionality.

Think of side effects as: **any code that needs to be run that does not directly affect the returned JSX**. In other words, if the logic does not cause the returned JSX to be different in some way, it is a side effect and should be handled independently with `useEffect`.

### Run some code after every render

This is the simplest use of useEffect. Take care not to set state here, because it can cause an infinite loop.
``` jsx
import { useEffect } from 'react';

// Runs after first mount and after every component update
function myComponent(props) {
  useEffect(() => {
    console.log('The component has updated!');
    // if we updated state here (like setState(Math.random()), infinite loop!
  }); // note: missing second argument
  return <h1>Hello World</h1>;
}
```

### Run some code ONLY after the first render

To run some code ONLY after the first render, we use the second argument to useEffect. This argument is an array of dependencies for the effect, and if
any of the dependencies change, the effect code will be ran again. With an empty dependency list `[]`, the code will only be run after the first render.

``` jsx
import { useEffect } from 'react';

// logs only once, after component has mounted
function myComponent(props) {
  useEffect(() => {
    console.log('The component has been mounted!');
  }, []); // note the empty dependency array
  return <h1>Hello World</h1>;
}
```

### Run some code ONLY when a dependency changes

In some cases, you may want to run some code after some dependency has changed (like an `isLoading` boolean). Simply add the dependency to the dependency array.
``` jsx
import { useEffect } from 'react';

// Logs when a dependent prop changes (and runs some side effect like changing document title)
function myComponent({ isLoading, title }) {
  useEffect(() => {
    if (isLoading) {
       console.log('We are still loading upstream data!');
       document.title = 'Loading...'
    } else {
       console.log('We are done loading upstream data!');
       document.title = title;
    }
  }, [isLoading]);
  return <h1>Hello World</h1>;
}
```

### Cleaning up an effect (and running code on unmount)

All effects can clean up after themselves. If a function is returned from `useEffect`, the function will be ran before the next effect is run, and when the component is unmounted.

``` jsx
import { useEffect } from 'react';

// Console logs only on mount and unmount
function myComponent(props) {
  useEffect(() => {
    console.log('Component has mounted!');
    return () => console.log('component has unmounted!');
  }, []);
  return <h1>Hello World</h1>;
}
```

The cleanup function is great for things like closing sockets, clearing intervals, etc. For example:
``` jsx
import { useEffect } from 'react';

// Logs every 2 seconds, but stops when component is unmounted
function myComponent({name }) {
  useEffect(() => {
    const timerId = setInterval(() => console.log(`Hello, ${name}`, 2000);
    return () => clearInterval(id);
  }, [name]);
  return <h1>Hello, {name};
}
```

