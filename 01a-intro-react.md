# React Kickoff
CRA, JSX, ECAMAScript2021, react best practices, naming conventions

What's new with React? Portals, Fragments, Memo, StrictMode, context API, JSX Transform

## Building React Apps

Create React App (CRA) is a project by facebook that enables developers to spin out a react app with opinionated configuration that works out of the box.
Major features include:
  - Webpack (for pipeline, bundling, eslint, building, etc)
  - Babel (for JSX transformations)
  - React (with working demo app)

For all sandbox projects, CRA is fantastic for experimentation. 

```
npx create-react-app my-app
cd my-app
npm start
```

Building a static react app is as simple as `npm run build`

## React Best Practices

### Proptypes

Proptypes are one of the most powerful QoL features of React. Defining proptypes help alleviate typed runtime bugs without the 
need to fully migrate to typescript. Proptypes are built into react and allow each component to specify that nature of the props
that a component uses. Datatypes and required status are the most important.

Proptypes can then be verified with `eslint` to catch runtime errors earlier. Note that proptypes have moved as of React 15.5


``` javascript
import PropTypes from 'prop-types';

class Greeting extends React.Component {
  render() {
    return (
      <h1>Hello, {this.props.name}</h1>
    );
  }
}

Greeting.propTypes = {
  name: PropTypes.string
};

Greeting.defaultProps = {
  name: 'fellow human'
};

export default Greeting;
```

Proptypes also work with functional components, memoized components, and components made with `forwardRef`.

For use with functional components, the only thing to note is you can't use a `default export` anonymous function
(which is discouraged by eslint anyway). You'll need a named function so you can attach propTypes before exporting it.

``` javascript
import PropTypes from 'prop-types';

function Greeting = (props) => <h1>Hello, {props.name}</h1>

Greeting.propTypes = {
  name: PropTypes.string
};

Greeting.defaultProps = {
  name: 'fellow human'
};

export default Greeting;
```

Examples of proptypes (lifted directly from the docs):

``` javascript
import PropTypes from 'prop-types';

MyComponent.propTypes = {
  // You can declare that a prop is a specific JS type. By default, these
  // are all optional.
  optionalArray: PropTypes.array,
  optionalBool: PropTypes.bool,
  optionalFunc: PropTypes.func,
  optionalNumber: PropTypes.number,
  optionalObject: PropTypes.object,
  optionalString: PropTypes.string,
  optionalSymbol: PropTypes.symbol,

  // Anything that can be rendered: numbers, strings, elements or an array
  // (or fragment) containing these types.
  optionalNode: PropTypes.node,

  // A React element.
  optionalElement: PropTypes.element,

  // A React element type (ie. MyComponent).
  optionalElementType: PropTypes.elementType,

  // You can also declare that a prop is an instance of a class. This uses
  // JS's instanceof operator.
  optionalMessage: PropTypes.instanceOf(Message),

  // You can ensure that your prop is limited to specific values by treating
  // it as an enum.
  optionalEnum: PropTypes.oneOf(['News', 'Photos']),

  // An object that could be one of many types
  optionalUnion: PropTypes.oneOfType([
    PropTypes.string,
    PropTypes.number,
    PropTypes.instanceOf(Message)
  ]),

  // An array of a certain type
  optionalArrayOf: PropTypes.arrayOf(PropTypes.number),

  // An object with property values of a certain type
  optionalObjectOf: PropTypes.objectOf(PropTypes.number),

  // An object taking on a particular shape
  optionalObjectWithShape: PropTypes.shape({
    color: PropTypes.string,
    fontSize: PropTypes.number
  }),

  // An object with warnings on extra properties
  optionalObjectWithStrictShape: PropTypes.exact({
    name: PropTypes.string,
    quantity: PropTypes.number
  }),   

  // You can chain any of the above with `isRequired` to make sure a warning
  // is shown if the prop isn't provided.
  requiredFunc: PropTypes.func.isRequired,

  // A required value of any data type
  requiredAny: PropTypes.any.isRequired,

  // You can also specify a custom validator. It should return an Error
  // object if the validation fails. Don't `console.warn` or throw, as this
  // won't work inside `oneOfType`.
  customProp: function(props, propName, componentName) {
    if (!/matchme/.test(props[propName])) {
      return new Error(
        'Invalid prop `' + propName + '` supplied to' +
        ' `' + componentName + '`. Validation failed.'
      );
    }
  },

  // You can also supply a custom validator to `arrayOf` and `objectOf`.
  // It should return an Error object if the validation fails. The validator
  // will be called for each key in the array or object. The first two
  // arguments of the validator are the array or object itself, and the
  // current item's key.
  customArrayProp: PropTypes.arrayOf(function(propValue, key, componentName, location, propFullName) {
    if (!/matchme/.test(propValue[key])) {
      return new Error(
        'Invalid prop `' + propFullName + '` supplied to' +
        ' `' + componentName + '`. Validation failed.'
      );
    }
  })
};
```

To learn more, check out the full [list of Proptypes](https://reactjs.org/docs/typechecking-with-proptypes.html)


### Functional Components

Functional components have been around for a long time. They're just components that are represented as functions that return JSX. Until the introduction of `hooks`, it was mostly correct to assert that simple components can be functional, and components requiring state can be class based. With the advent of hooks, functional components can now have state and access react lifecycle methods so this is no longer true.

Functional components are nice because they're simple. They take props as an argument, and return JSX. There's no implicit state, no special method names, no hidden constructors, etc. Without hooks, a functional component is essentially equivalent to the `render()` method in a class component.

The following components are equivalent:

``` javascript
import { Component } from 'react';

class Header extends Component {
   render() {
      return <h1>Hello, {this.props.name}</h1>;
   }
}
```

``` javascript
function Header(props) {
   return <h1>Hello, {props.name}</h1>;
}
```

With object destructuring on the props argument and implicit return with an arrow function, functional components can be quite compact:

``` javascript
const Header = ({ name }) => <h1>Hello, {name}</h1>;
```

Functional components can take advantage of clojures and scoped helper functions as well:
``` javascript
function CatsList({ catsArray }) {
   const catAlert = cat => alert(cat.age);
   return (
     <ul>
        {catsArray.map(c => <li>{c.name} <button onClick={() => catAlert(c)}>Show Age</button></li>)}
     </ul>
   );
}
```

### Pure Components and React.memo

React is smart enough to compare the output of a component to what's already on the DOM and only update when something has changed, but sometimes optimization of the component itself can save time too.

When working with class-based components, one can choose to inherit from `React.PureComponent` instead of `React.Component`. A pure component is one that will not automatically update if props and state have not changed. A shallow comparison is used to determine this, and `shouldComponentUpdate` is not implemented. This leads to dramatic performance enhancements in many cases.

In modern react, functional components with hooks fix most of the easy 'gotchas' that can lead to the kinds of react lifecycle performance issues that `PureComponent` was designed to address. `React.memo` is a Higher Order Component (HOC) that can be can be used to wrap functional components to prevent re-renders when props dont change between parent updates, as the component is only re-rendered if props are found to have changed after running a shallow comparison.

Typically `React.memo` makes sense when:
  - The component is pure (given the same props, the component *always* renders the same output)
  - The component renders often 
  - The component is often given the same props when its parent is re-rendering (child not dependent on parent state)
  - The component is large enough to warrant optimization, where a a shallow props comparison with `memo` may be faster than rendering the entire component.

Here's an example:
``` javascript
const Movie = ({ title }) => (
  <h1>{title}</h1>
);

const MovieDetails = ({title, year, revenue}) => (
  <div>
    <Movie title={title} />
    Annual Revenue for {year}: {revenue}
  </div>
);

const MovieList = () => (
  <main>
    <MovieDetails
      title='Gone With The Wind'
      year=1930
      revenue='$100,000'
    />
    <MovieDetails
      title='Gone With The Wind'
      year=1931
      revenue='$200,000'
    />
    <MovieDetails
      title='Gone With The Wind'
      year=1932
      revenue='$300,000'
    />
  </main>
);
```

Here we can see that the `<Movie />` component is consistently given the same props, which is a great candidate for memoization (especially if we had hundreds or thousands of these components rendered in the app). By exporting the component wrapped in `React.memo`, React would cache the component's output and skip the render if there is already an output generated for the given props.

To set up memoization, just wrap the function with `React.memo`:

```
import { memo } from 'react';
const Movie = ({ title }) => (
  <h1>{title}</h1>
);

const MemoizedMovie = memo(Movie);
// or: export default memo(Movie);
```

There is also the `useMemo` hook, which has the same functionality as `React.memo`, but it can be used for arbitrary functions (including components).

```
const func = useMemo(() => updateValue(), [someVal]);
```

The above memoized function will only call `updateValue` when the value of `someVal` has changed, otherwise it will return the cached output.


### React Fragments and JSX Transform (new)

JSX Fragments are awesome. Traditionally with JSX a component must always return a single element with an unlimited number of children. React has introduced several improvements to help with the case where a component wants to return several JSX elements but it is not desirable to wrap them in a DOM element like a `<div>` or `<span>`. 

This is not legal in JSX:
``` javascript
const Header = () => (
  <h1>Welcome to my site</h1>
  <h2>We're glad you're here!</h2>
);
```

Traditionally you would have to wrap these in some DOM element that you may not actually want to have on the page:
``` javascript
const Header = () => (
  <section>
    <h1>Welcome to my site</h1>
    <h2>We're glad you're here!</h2>
  </section>
);
```

But maybe you don't actually want to have a `<section>` there. JSX Fragments save the day! They provide a JSX element that does not map to a DOM element.
``` javascript
const Header = () => (
  <React.Fragment>
    <h1>Welcome to my site</h1>
    <h2>We're glad you're here!</h2>
  </React.Fragment>
);
```

Or even better:

``` javascript
const Header = () => (
  <>
    <h1>Welcome to my site</h1>
    <h2>We're glad you're here!</h2>
  </>
);
```

If you need to add a `key` prop to a Fragment, you must use the `<React.Fragment>` syntax (short-hand is not compatible with props).

### Portals

Portals are a new feature that, simply put, allow components to render children that are mounted somewhere else on the DOM. They're really useful for generating modals, because a child component may want to pop-up a modal, but that modal's html should be on the top-level DOM so it can cleanly overflow the entire page without having to rely on CSS shenanigans.

The other advantage is that browser events propagate up through the React component tree as expected, even though from the Browser DOM's point of view, the child component is not actually rendered inside the parent element.

Given the following HTML:
``` html
<html>
  <body>
    <div id="app-root"></div>
    <div id="modal-root"></div>
  </body>
</html>
```

``` javascript
const appRoot = document.getElementById('app-root');
const modalRoot = document.getElementById('body'); // 'body' is the default if not otherwise specified when creating a portal

class Modal extends React.Component {
  constructor(props) {
    super(props);
    this.el = document.createElement('div');
  }

  componentDidMount() {
    // The portal element is inserted in the DOM tree after
    // the Modal's children are mounted, meaning that children
    // will be mounted on a detached DOM node.
    modalRoot.appendChild(this.el);
  }

  componentWillUnmount() {
    modalRoot.removeChild(this.el);
  }

  render() {
    return ReactDOM.createPortal(
      this.props.children,
      this.el
    );
  }
}

class Parent extends React.Component {
  state = {clicks: 0};

  handleClick = () => {
    // This will fire when the button in Child is clicked,
    // updating Parent's state, even though button
    // is not direct descendant in the DOM.
    this.setState(state => ({
      clicks: state.clicks + 1
    }));
  }

  render = () => (
      <div onClick={this.handleClick}>
        <p>Number of clicks: {this.state.clicks}</p>
        <Modal>
          <button>Click</button>
        </Modal>
      </div>
    );
  }
}
ReactDOM.render(<Parent />, appRoot);
```

### Refs (`useRef` and `forwardRef`)

Refs are used to track an instance of an actual DOM element across renders (even if the DOM element is destroyed and re-created). This is useful to expose browser API calls (like `autofocus` or `media controls`) to be used directly by a component.

To create and track a ref, simply call `createRef` or the `useRef()` hook with functional components, and pin it to a rendered component with the `ref` prop.

``` javascript
class CustomTextInput extends React.Component {
  constructor(props) {
    super(props);
    // create a ref to store the textInput DOM element
    this.textInput = React.createRef();
  }

  focusTextInput = () => {
    // Explicitly focus the text input using the raw DOM API
    // Note: we're accessing "current" to get the DOM node
    this.textInput.current.focus();
  }

  render() {
    // tell React that we want to associate the <input> ref
    // with the `textInput` that we created in the constructor
    return (
      <div>
        <input
          type="text"
          ref={this.textInput} />
        <input
          type="button"
          value="Focus the text input"
          onClick={this.focusTextInput}
        />
      </div>
    );
  }
}
```

Functional equivalent with the `useRef` hook:
``` javascript
import { useRef } from 'react';

function CustomTextInput(props) {
  // textInput must be declared here so the ref can refer to it
  const textInput = useRef(null);
  
  function handleClick() {
    textInput.current.focus();
  }

  return (
    <div>
      <input
        type="text"
        ref={textInput} />
      <input
        type="button"
        value="Focus the text input"
        onClick={handleClick}
      />
    </div>
  );
}
```

There are some other special cases (like accessing tagging a functional component with a `ref` which isnt possible). Check out the [docs](https://reactjs.org/docs/refs-and-the-dom.html).

You can also forward references between components using the special `forwardRef` API.

### Error Boundaries

Error Boundaries are a feature that allow a component to catch thrown errors of its children through the `componentDidCatch` lifecycle method. Currently there is no hook equivalent, so Error Boundaries can only be used in class components (but all children can of course be functional or class components). The biggest advantage of Error Boundaries is that a crash that occurs during the render lifecycle will no longer cause a blank page to be rendered, but instead the error can be caught and a friendly error message can be shown to the user.

A simple example of Error Boundary Higher Order Component:

``` javascript
class ErrorBoundary extends React.Component {
  state = { hasError: false };

  componentDidCatch(error, errorInfo) {
    // You can also log the error to an error reporting service
    this.setState({ hasError: true });
    logErrorToMyService(error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      // You can render any custom fallback UI
      return <h1>Something went wrong.</h1>;
    }

    return this.props.children; 
  }
}
```

### StrictMode

`<StrictMode>` is a component that you can use for preparing for react upgrades. In short it turns on deprecation warnings and highlights potential problems in an application. There's no visible UI, but instead it activates additional checks and warnings for its descendents.

In a fresh CRA project, `App` is wrapped in `StrictMode` like so:
``` javascript
import { StrictMode } from 'react';

//... component code and imports
ReactDOM.render(<StrictMode><App /></StrictMode>, appRoot);
```

Strict mode is updated with each version of React, and can help prepare for upcoming features, experimental features that are in use, and deprecations.

  - Identifying components with unsafe lifecycles
  - Warning about legacy string ref API usage
  - Warnings about deprecated findDOMNode usage
  - Detecting legacy context API and unexpected side effects

### ECMAScript Tips & Tricks
#### Arrow Functions & Implicit Returns

Arrow functions do not mess with `this`, so they do not need to be bound using `this.bind` with class components. They also can use implicit returns which is nice for compact code.

``` javascript
const cats = animalsArray.filter(function(animal) {
   return animal.type === 'cat'
});
```

can become:

``` javascript
const cats = animalsArray.filter(animal => animal.type === 'cat');
```

This also works great when mapping over arrays to create JSX output:
``` javascript
function CatsList = (catsArray) => (
  <ul>
    {catsArray.map(cat => <li>{cat.name}</li>)}
  </ul>
);
```

#### Object Destructuring & Props

Using object destructuring to pop out props is a handy trick and adds to readability:

``` javascript
const myComponent = (props) => {
   const catsArray = props.catsArray;
   return <ChildComponent arrayToShow={catsArray} {...props} />
}
```

becomes

``` javascript
const myComponent = (props) => {
   const { catsArray } = props;
   return <ChildComponent arrayToShow={catsArray} {...props} />
}
```

with implicit return: 

``` javascript
const myComponent = ({catsArray, ...theRest}) => <ChildComponent array1={catsArray} {...theRest} />
```

#### Array Destructuring

Array destructuring works the same way. Very useful with `useState`:

``` javascript
const [x, y] = [1, 2]; // x is 1, y is 2
```

``` javascript
const [state, setState] = useState(0); // useState returns an array of 2 things: a value and a setter function
```

#### Optional Chaining Operator

Optional chaining operator nice for walking through objects that may not have expected nested structure. It's useful when combined with the nullish coelescing operator too.

``` javascript
let human = { name: 'blaine' };
console.log(human?.favoriteFood ?? 'pizza'); // logs 'pizza'
```
