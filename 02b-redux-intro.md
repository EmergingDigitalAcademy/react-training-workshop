
# Redux

## Why Redux?

Earlier, we had App keep all the state and pass it down to all the components through props. 
As applications get bigger: 

- It's a pain to keep passing props down and down and down.
- Our `App.js` file is also growing out of control when two different components need the same data.

Redux allows us to have a shared application state.  We can pass information from this shared redux state directly to the components that need it.

In simpler terms, this means Redux is a way to make sharable, app-wide, global variables that can be accessed from any component!

Drawing of the `store` reaching out directly to where it is needed.


## Install

`npx create-react-app redux-intro --scripts-version=4.0.1`

`npm install redux@4 react-redux@7`

## Setup

To get redux up and running we need:
- A reducer
- A store
- A Provider

Let's add these to our `index.js` file, and the we'll talk through them in more detail.

index.js
```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import App from './components/App/App';
import { createStore, combineReducers } from 'redux';
import { Provider } from 'react-redux';

// reducer!
const count = (state = 0, action) => {
    console.log(`I'm a reducer! I dont do anything yet`);
    return state;
};

// store!
const storeInstance = createStore(
    count
);

// Provider lets redux and react talk to one another
ReactDOM.render(
  <React.StrictMode>
    <Provider store={storeInstance}>
        <App />
    </Provider>
  </React.StrictMode>,
  document.getElementById('root')
);
```


## Reducers
At its core, redux is a way to make sharable, app-wide (globally accessible) variables. A reducer, at a high level, is just a piece of data, akin to a variable. In our case, we're tracking how many times a button has been clicked, as `count`.

But instead of assigning a our state to a variable:

```js
let [count, setCount] = useState(0);
```

...we _return_ the value of the state from a function:

```js
const count = (state = 0, action) => {
    return state;
}
```

Note that reducer functions have a very specific pattern they must follow: 

- The first argument is the previous value of our state
  - We set the _initial value_ using `state = 0` (compare to `useState(0)`)
- The second value is the action -- we'll get to that in a bit.

For practice, let's try adding another reducer. This time let's track elements on the periodic table. 

Try adding a new reducer called `elementList` on your own...

The final code should look like:

```js
const elementList = (state = [], action) => {
    return state;
};
```

## Combine reducers

We also need to add the new reducer to our store. We need a tool to have more than one, though: `combineReducers`:

```JavaScript
const storeInstance = createStore(
    combineReducers(
        {
            count,
            elementList,
        }
    )
);
```

We'll see exactly what this does in a bit.

## Access Redux State from Components (Hooks)

In order to see the redux state data on the DOM, we'll need to hook into it. We have to say which data we care about. The hook is called `useSelector` which selects data from the store.


```JSX
// useSelector is a hook provided by the react-redux library
import { useSelector } from 'react-redux';

function App () {
    // useSelector accepts a function that tells it what part of the store you want.
    // here we return the whole store
    const reduxStore = useSelector(store => store)

    return (
      <div>
        {/* Render the entire reduxStore to our DOM, as a JS object (JSON) */}
        <pre>{JSON.stringify(reduxStore)}</pre>
      </div>
    );

}

export default App;
```

This is the class-component equivalent using `connect`:
```JSX
import { Component } from 'react';
import { connect } from 'react-redux';

class App extends Component {
  render() {
    return (
      <div>
        {/* Render the entire reduxStore to our DOM, as a JS object (JSON) */}
        <pre>{JSON.stringify(this.props.reduxStore)}</pre>
      </div>
    );
  }
}

// this maps whatever parts we want from the redux store into props
// this populates the arbitrary key this.props.reduxStore
function mapStateToProps(reduxStore) {
  return {
    reduxStore: reduxStore
  }
);
export default connect(mapStateToProps)(App);
```

So what is showing up on the DOM? `reduxState` is an object that has a property for each reducer.

```JSON
{"count":0,"elementList":[]}
```

When we use `combineReducers` each reducer gets its own little bit of space in the overall redux store. They don't get to interact with each other, and each reducer is responsible for updates to its own bit of state.

So, we can get just one part. This is beneficial, because any updates to the data we get will cause our component to rerender. If you grab the whole store, you will rerender with any change to any reducer, which probably isn't efficient.

```js
// one thing
// here we return one part, count
const count = useSelector(store => store.count)
```

Showing just the count would be:

```jsx
<p>Count is: {count}</p>
```

Class component equivalent:
```
// here we return just the data from the count reducer
function mapStateToProps(reduxStore) {
  return {
    count: reduxStore.count
  }
);
```

Showing just the count would be:
``` jsx
<p>Count is: {this.props.count}</p>
```
