
# Redux State

With functional components, nothing changes with the creation of the store, reducers, and configuration boilerplate. The main difference
is how state gets into your components. We use the `useSelector` and `useDispatch` hooks to grab interact with redux directly.

To dispatch actions, we simply use `dispatch({ type: 'ACTION_TYPE', payload: {} });`, and accessing data is as simple as 
`const animals = useSelector(store => store.animals);`

`useSelector` automatically subscribes the component to the reducer, forcing updates (and fresh data) when the redux store changes.

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
import { useSelector, useDispatch } from 'react-redux';

function App(props) {
  // local state for tracking the form
  const [newElement, setNewElement] = useState('');
  
  // pull the entire store out of redux as a simple case
  const reduxState = useSelector(store => store);
  
  // set up dispatch so we can send actions to redux
  const dispatch = useDispatch();
  
  const handleElementChange = (event) => setNewElement(event.target.value);

  return (
    <div>
      {/* Dispatching an action */}
      <button onClick={() => dispatch({ type: 'BUTTON_ONE' })}>Button One</button>
      <button onClick={() => dispatch({ type: 'BUTTON_TWO' })}>Button Two</button>
      <input onChange={handleElementChange} />
      <button onClick={() => dispatch({ type: 'ADD_ELEMENT', payload: newElement })}>Add Element</button>
      <pre>{JSON.stringify(reduxState)}</pre>
    </div>
  );
}

export default App;
```

## Access Redux State from Components
In order to put the redux state on the DOM, we can use the `useSelector` hook to pull whatever we want from redux. The callback function
will return whatever reducer data we'd like to subscribe to. You can use as many `useSelector` calls as you like.

```JSX
const reduxState = useSelector(store => store);
const onlyFirstReducer = useSelector(store => store.firstReducer);
```

So what is showing up on the DOM? `reduxState` is an object that has a property for each reducer.

```JSON
{"firstReducer":0,"secondReducer":1,"elementListReducer":['blaine']}
```
