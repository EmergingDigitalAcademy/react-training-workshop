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

```
// good
function myComponent(props) {
   return <h1>Hello World</h1>
}

// fun idea, but not may not meet style guidelines
const myComponent = (props) => <h1>Hello World</h1>
```
