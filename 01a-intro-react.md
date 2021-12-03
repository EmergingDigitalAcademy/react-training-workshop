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
import { Component } from React;

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

In modern react, functional components with hooks fix most of the easy 'gotchas' that can lead to the kinds of react lifecycle performance issues that `PureComponent` was designed to address. `React.memo` can be can be used to wrap functional components to prevent re-renders when props dont change between parent updates, as the component is only re-rendered if props are found to have changed after running a shallow comparison.

Typically `React.memo` makes sense when:
  - The component is pure (given the same props, the component *always* renders the same output)
  - The component renders often 
  - The component is often given the same props when its parent is re-rendering (child not dependent on parent state)
  - The component is large enough to warrant optimization, where a a shallow props comparison with `memo` may be faster than rendering the entire component.

Here's an example:
```

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
import { memo } from 'React';
const Movie = ({ title }) => (
  <h1>{title}</h1>
);

const MemoizedMovie = memo(Movie);
// or: export default memo(Movie);
```

### React Fragments and JSX Transform (new)
### Portals (new)
### Refs (new)
### DOMAttributes (new)
### Error Boundaries (new)
### StrictMode (new)
### React Profiler DevTools
### Component Documentation
### ECMAScript Tips & Tricks
#### Arrow Functions & Implicit Returns
#### Object Destructuring & Props
#### Array Destructuring
#### Spread Operator
#### Nullisch Coelescing Operator
#### Optional Chaining Operator
### Misc. Tips
#### Multiple return lines wrapped in parenthesis
#### Naming conventions: PascalCase for components, camelCase for instances. CamelCase for props.
#### Import Order (Built-In, External, Internal)
#### Dont use reserved props (like className)
#### Double Quotes (JSX) vs Single Quotes (regular JS)
#### Self-closing tags for components that do not have children
#### Webpack Tree Shaking
