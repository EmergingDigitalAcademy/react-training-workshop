# Redux State

With class-based components, nothing changes with the creation of the store, reducers, and configuration boilerplate. The main difference
is how state gets into your components. We can wrap our component with the `connect` HOC, which provides `props.dispatch` and `props.store`. 

To dispatch actions, we simply use `props.dispatch({ type: 'ACTION_TYPE', payload: {} });`, and with `connect` we pass in a function that
will map the redux store into our own props, called `mapStateToProps` that can be as simple as: `const mapStateToProps = (store) => {animals: store.animals};` 
(this will provide `props.animals` which will have all of our animals from the animals reducer that is mounted to `store.animals`

Let's start from here:

`index.js`
```JavaScript
import React from 'react';
import ReactDOM from 'react-dom';
import App from './components/App/App';
import { createStore, combineReducers } from 'redux';
import { Provider } from 'react-redux';

const firstReducer = (state, action) => {
    if (action.type === 'BUTTON_ONE') {
        console.log('Button 1 was clicked!');
    }
    return {};
};

const secondReducer = (state, action) => {
    if (action.type === 'BUTTON_TWO') {
        console.log('Button 2 was clicked!');
    }
    return {};
};

const elementListReducer = (state, action) => {
    if (action.type === 'ADD_ELEMENT') {
        console.log(`The element was ${action.payload}`);
    }
    return state;
};

// The store only accepts one reducer
const storeInstance = createStore(
    combineReducers(
        {
            firstReducer,
            secondReducer,
            elementListReducer,
        }
    )
);

ReactDOM.render(<Provider store={storeInstance}><App /></Provider>, document.getElementById('root'));
```

`App.js`
```JavaScript
import React, { Component } from 'react';
import { connect } from 'react-redux';

class App extends Component {
  state = {
    newElement: ''
  }

  handleElementChange = (event) => {
    this.setState({
      newElement: event.target.value,
    });
  }

  render() {
    return (
      <div>
        {/* Dispatching an action */}
        <button onClick={() => this.props.dispatch({ type: 'BUTTON_ONE' })}>Button One</button>
        <button onClick={() => this.props.dispatch({ type: 'BUTTON_TWO' })}>Button Two</button>
        <input onChange={this.handleElementChange} />
        <button onClick={() => this.props.dispatch({ type: 'ADD_ELEMENT', payload: this.state.newElement })}>Add Element</button>
      </div>
    );
  }
}

export default connect()(App); 
```

## Reducer State

So at this point, we know that we need to have `state` in the reducer, but what is it, and what does it do?

In order to take a look at the redux state, let's log it out and see what the value is!

```JSX
const firstReducer = (state, action) => {
    if (action.type === 'BUTTON_ONE') {
        console.log('state', state);
        console.log('Button 1 was clicked!');
    }
    return {};
};

const secondReducer = (state, action) => {
    if (action.type === 'BUTTON_TWO') {
        console.log('state', state);
        console.log('Button 2 was clicked!');
    }
    return {};
};

const elementListReducer = (state, action) => {
    if(action.type === 'ADD_ELEMENT') {
        console.log('state', state);
        console.log(`The element was ${action.payload}`);
    }
    return {};
};
```

What if we change what is being returned?

```JSX
const firstReducer = (state, action) => {
    if (action.type === 'BUTTON_ONE') {
        console.log('state', state);
        console.log('Button 1 was clicked!');
    }
    return 1;
};
```

`state` starts out as whatever it ended as last time!

What if we return `state` itself?

```JSX
const firstReducer = (state, action) => {
    if (action.type === 'BUTTON_ONE') {
        console.log('state', state);
        console.log('Button 1 was clicked!');
    }
    return state;
};
```

Woah, big error! State starts as undefined, and we can't return undefined from a reducer, so let's use ES6 syntax to set a default value for our state if it is undefined:

```JSX
const firstReducer = (state = 0, action) => {
    if (action.type === 'BUTTON_ONE') {
        console.log('state', state);
        console.log('Button 1 was clicked!');
    }
    return state;
};
```

Ok... but we're just logging the same thing every time. What if we wanted to grow the number every time the button was clicked?

```JSX
const firstReducer = (state = 0, action) => {
    if (action.type === 'BUTTON_ONE') {
        console.log('state', state);
        console.log('Button 1 was clicked!');
        return state + 1;
    }
    return state;
};
```

Great! We can log our state, and we can change our state. 

## Access Redux State from Components
What if we want to put our state on the DOM?

In order to put the redux state on the DOM, we want to put in onto `this.props` in our `App.js`. This can be done quite nicely by putting the redux state ono props with the connect function.

```JSX
const putReduxStateOnProps = (reduxState) => ({
  reduxState
});

export default connect(putReduxStateOnProps)(App);
```

and then we can include it in our `render` method by adding this

```JSX
<pre>{JSON.stringify(this.props.reduxState)}</pre>
```

So what is showing up on the DOM? `reduxState` is an object that has a property for each reducer.

```JSON
{"firstReducer":0,"secondReducer":{},"elementListReducer":{}}
```

When we use `combineReducers` each reducer gets its own little bit of space in the overall redux store. They don't get to interact with each other, and each reducer is responsible for updates to its own bit of state. 

Let's update the other reducers to have more meaningful state.

Button 1 goes up, so let's make Button 2 go down!
```JavaScript
const secondReducer = (state = 100, action) => {
    if(action.type === 'BUTTON_TWO') {
        console.log(`Button 2 was clicked!`);
        return state - 1;
    }
    return state;
};
```

For the elements, we want to add to an array so we can see all the elements added. 
```JavaScript
const elementListReducer = (state = [], action) => {
    switch (action.type) {
        case 'ADD_ELEMENT':
            return [ ...state, action.payload ];
        default:
            return state;
    }
}
```

> __Important__ Remember not to push into the array. For React/Redux to recognize the changes we need to return a new object.
