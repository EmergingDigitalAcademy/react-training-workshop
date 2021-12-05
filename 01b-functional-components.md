# Functional Components Review

Functional components have been around for a long time. They're just components that are represented as functions that return JSX. 
Until the introduction of `hooks`, it was mostly correct to assert that simple components can be functional, and components 
requiring state can be class based. With the advent of hooks, functional components can now have state and access react lifecycle 
methods so this is no longer true.

Functional components are nice because they're simple. They take props as an argument, and return JSX. There's no implicit state, 
no special method names, no hidden constructors, etc. Without hooks, a functional component is essentially equivalent to the 
`render()` method in a class component.

The following components are equivalent:

``` javascript
import { Component } from 'react';

class Header extends Component {
   render() {
      return <h1>Hello, {this.props.name}</h1>;
   }
}

export default Header;
```

``` javascript
function Header(props) {
   return <h1>Hello, {props.name}</h1>;
}

export default Header;
```

Object destructuring is generally recommended with incoming props for readability::

``` javascript
function Header({ name, ...props }) {
   // `props` will have all other props except `name`
   return <h1>Hello, {name}</h1>;
}

export default Header;
```

Functional components can take advantage of clojures and scoped helper functions as well:
``` javascript
function CatsList({ catsArray }) {
   function catAlert(c) {
      alert(c.age);
   }
   return (
     <ul>
        {catsArray.map(c => <li>{c.name} <button onClick={() => catAlert(c)}>Show Age</button></li>)}
     </ul>
   );
}
```

In general it's nice to keep functional components as function declarations rather than named anonymous arrow functions. This
depends on your team's style guide of course:

``` javascript
// good
function myComponent(props) {
   return <h1>Hello World</h1>
}

// fun idea, but not may not meet style guidelines
const myComponent = (props) => <h1>Hello World</h1>
```

## Converting class components to functional components

Converting from a class-based component to a functional component is a fun exercise. There are several layers of difficulty:
  - If a class component only has a `render` method (no state, no lifecycle components), the conversion is as simple because the render function IS the functional component. Simply drop the class component definition and rename `render()` to `function myComponent(props) { ...`, then replace `this.props` with `props` (or use object destructuring as mentioned above
  - To replace local state, use `useState`: `const [state, setState] = useState(DEFAULT_STATE);`. In this simple example, `this.state` is now just `state` and `this.setState` has been replaced with `setState`. Be sure to pass in your default state as the first argument to `useState`.
  - To replace `componentDidMount`, use `useEffect` with an empty dependency argument `useEffect(() => {}, []);`. The body of `useEffect` will be run only after the first render.
  - To replace `componentWillUnmount`, use `useEffect`'s return value. The function returned from `useEffect` will be run when the effect is cleaned up (so in the case of an empty dependency list, this will automatically happen on component unmount.
  - Similarly, `componentDidUpdate` can be replaced with `useEffect` as well. The effect will run if anything in the dependency list is updated. `useEffect(() => {}, [someValue]);` will run whenever `someValue` changes. If `someValue1` is local state that the render uses, this will automatically work as expected.
  - To replace `shouldComponentUpdate`, `React.memo` handles most cases where new props are simply shallow compared to existing props. For more control, the second argument to `React.memo` is a function that has the same behavior as `shouldComponentUpdate`: it takes the new props and old props as arguments, and returns true if the memoization cache should be busted.

See the [Hooks FAQ](https://reactjs.org/docs/hooks-faq.html#how-do-lifecycle-methods-correspond-to-hooks) for more info, expecially the section on ["How do lifecycle methods correspond to hooks?" question](https://reactjs.org/docs/hooks-faq.html);
