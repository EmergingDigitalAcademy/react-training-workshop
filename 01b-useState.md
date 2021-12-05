# React Local State with Hooks

## Getting started

To start, consider the following simple component:

```jsx

function App () {
    return (
        <main>
            <h1>React Local State</h1>
            Hello! My name is Blaine.
            <button>Click me!</button>
            <p>I've clicked the button 0 times.</p>
        </main>;
    )
}
```

Let's use `useState` to create two pieces of local state, one to track the `name` and one to track the click count.
## React State

The pattern for useState:
`const [myStateVariable, setMyStateVariable] = useState('initial value');`

Think of it this way: useState is a function, that you give the initial value to. It can be any datatype.

It returns to us an array, with two things inside, and uses `array destructuring` to set the values of `myStateVariable` and `setMyStateVariable`. 
useState returns an array. It accepts the default value of the state variable.

Written in full it could look like:

```jsx
let whatUseStateReturns = useState(false);
let myThing = whatUseStateReturns[0];
let changeMyThing = whatUseStateReturns[1];
```

You get a state variable at index 0, and a function for how to change that state variable at index 1.

As written, you could read this as 
"Make two variables, grabbing the first thing out of the array and call it name, and grab the second thing out of the array and call it setName"
With useState -- the first thing in the array is ALWAYS the variable and the second thing is ALWAYS a function that sets that variable.
   

```jsx
import { useState } from 'react';

function App () {
    // destructured array
    //const [myStateVariable, setMyStateVariable] = useState('initial value');

    const [name, setName] = useState('Blaine');
    const [count, setCount] = useState(0);

    return (
        <main>
            <h1>React Local State</h1>
            Hello! My name is {name}.
            <button onClick={() => setCount(count + 1)}>Click me!</button>
            <p>I've clicked the button {count} times.</p>
        </main>;
    )
}

```

## Named Function
If you wanted to do more than just setCount, you should use a named function. 

Something like:
```jsx
 const handleClick = () => {
    console.log('clicked');
    setCount(count + 1);
}
//...

<button onClick={handleClick}>Click me!</button>
```

## Default State as a function

`useState` also can accept a function as the first argument. The return value of this function is used as the default state.

```jsx
import { useState } from 'react';

function App () {
    const [name, setName] = useState(() => 'Blaine');
    const [count, setCount] = useState(() => 0);

    return (
        <main>
            <h1>React Local State</h1>
            Hello! My name is {name}.
            <button onClick={() => setCount(count + 1)}>Click me!</button>
            <p>I've clicked the button {count} times.</p>
        </main>;
    )
}
```

If you need to set default state to something that is available after the first render (like data from an upstream async fetch), you can use `useEffect` to update state at a later time (see the notes on `useEffect)`.

If we wanted to track both pieces of state inside one object, care must be taken to ensure state is set to a new object instance so that the react update cycle is triggered. The spread operator can help with this. Example:

```jsx
import { useState } from 'react';

function App () {
    const [state, setState] = useState({ name: 'Blaine', count: 0 });

    return (
        <main>
            <h1>React Local State</h1>
            Hello! My name is {state.name}.
            <button onClick={() => setState({ ...state, count: count+1)}>Click me!</button>
            <p>I've clicked the button {state.count} times.</p>
        </main>;
    )
}
```
