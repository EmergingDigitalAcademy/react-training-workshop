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
### Pure Components
### React.memo for non-dependent child components (new)
### React Fragments and JSX Transform (new)
### Portals (new)
### Context API (new)
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
