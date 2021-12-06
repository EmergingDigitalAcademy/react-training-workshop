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

const firstReducer = (state=0, action) => {
    if (action.type === 'BUTTON_ONE') {
        console.log('Button 1 was clicked!');
        return state+1;
    }
    return state;
};

const secondReducer = (state=0, action) => {
    if (action.type === 'BUTTON_TWO') {
        console.log('Button 2 was clicked!');
        return state+1;
    }
    return state;
};

const elementListReducer = (state=[], action) => {
    if (action.type === 'ADD_ELEMENT') {
        console.log(`The element was ${action.payload}`);
        return [...state, action.payload]
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
        <pre>{JSON.stringify(this.props.reduxState)}</pre>
      </div>
    );
  }
}
const mapStateToProps = (reduxState) => ({
  reduxState
});

// `connect` creates this.props.dispatch() and uses the callback to inject redux state into this.props
export default connect(mapStateToProps)(App); 
```

## Access Redux State from Components
In order to put the redux state on the DOM, we want to put in onto `this.props` in our `App.js`. This can be done quite nicely by putting the redux state ono props with the connect function.

```JSX
const mapStateToProps = (reduxState) => ({
  reduxState
});

export default connect(mapStateToProps)(App);
```

and then we can include it in our `render` method by adding this

```JSX
<pre>{JSON.stringify(this.props.reduxState)}</pre>
```

So what is showing up on the DOM? `reduxState` is an object that has a property for each reducer.

```JSON
{"firstReducer":0,"secondReducer":1,"elementListReducer":['blaine']}
```
