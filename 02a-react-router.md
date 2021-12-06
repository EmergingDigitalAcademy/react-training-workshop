Show:
  - App Routing Setup
  - `history.push` with class components
  - `useHistory` hook
  - `params` with class components
  - `useParams` hook

# React Router
## SPAs!
In single page applications (SPAs), we often want to show new content without reloading the whole page. For Data, we use AJAX requests, and for view-related things we use whats called client-side routing. This is different than requesting a new HTML page every time we need to see different things on the DOM.

React-router utilizes "hash" urls (e.g. http://localhost:3000/#/home) to allow us to handle our routes on the client. This works because servers ignore everything after the #. With React-router, we can use what the browser ignores to show our users the content they requested.

Luckily, since everything is a component, basic routing is simply selecting which component to show for a given url.

We're building off of examples from here: https://reacttraining.com/react-router/web/example/basic

## Basic Routing

Let's add react router to our project:

```
npm install react-router-dom
```

In `App.js`, let's add a few of our page components:

```JSX
<Home />
<Plants />
<Animals />
```

Great! Now all three show up, but we only want to show one at a time depending on the URL. This is when we'll bring `react-router-dom` into our project:

```JSX
import { HashRouter as Router, Route } from "react-router-dom";
```

Now wrap everything in your render `return` inside of a `Router` tag:

```JSX
<Router>
    <div>
        <Home />
        <Plants />
        <Animals />
    </div>
</Router>
```

Now we're going to determine which view is shown based on the URL path:

```JSX
<Route path="/">
    <Home />
</Route>
<Route path="plants/">
    <Plants />
</Route>
<Route path="/animals">
    <Animals />
</Route>
```

You can think of these as `if` blocks. If our `path` matches, show that `component`. `path` is our URL matcher and `component` is the component we want to show when we match that route. One thing that's weird is that the `Home` Component is showing up everywhere. That's because `/` matches all of these (`/plants` and `/animals` both include `/`). If we only want it to show the home component when it matches `/` exactly, we have to include the word `exact`.

```JSX
<Route path="/" exact>
    <Home />
</Route>
```

## Linking

These routes should now be working when we go to `localhost:3000`, `localhost:3000/#/plants`, and `localhost:3000/#/animals`, but no user will type those in. We need links. Let's add `Link` to our `import`.

```JSX
import { HashRouter as Router, Route, Link } from "react-router-dom";
```

Now add links to the render and it should look like this:

```JSX
import React, { Component } from 'react';
import { HashRouter as Router, Route, Link } from "react-router-dom";

// Page components
import Home from './pages/Home/Home';
import Plants from './pages/Plants/Plants';
import Animals from './pages/Animals/Animals';

function App () {

  return (
    <div >
      <Router>
        <div>
          <ul>
            <li>
              <Link to="/">Home</Link>
            </li>
            <li>
              <Link to="/plants">Plants</Link>
            </li>
            <li>
              <Link to="/animals">Animals</Link>
            </li>
          </ul>

          <hr />
        <Route path="/">
            <Home />
        </Route>
        <Route path="plants/">
            <Plants />
        </Route>
        <Route path="/animals">
            <Animals />
        </Route>
        </div>
      </Router>
    </div>
  );
}

export default App;
```

## Change View Programmatically with Hooks (`useHistory`)

`Link` components are great if all we want to do is change where we are. But what if we want to do other things, too? We'd need to change view after we do other things. Which a `Link` can't do...

Let's add a button to our Home Component.  `onClick` let's fire a function that makes an alert, and then changes where we are.

```jsx
import React from 'react';

function Home () {
    
    const handleClick = () => {
      alert("You are headed to animals");
      ///CHANGE LOCATION???
    };
    
    return (
      <div>
        <h2>Home</h2>
        <button onClick={handleClick} />
      </div>
    );
}
export default Home;
```


But how??

Thankfully, we have the ability to do so! We just need to hook into the proper place.

`import {useHistory} from 'react-router-dom'`

and

`const history = useHistory()`

`history` is the key here! It's a record of everywhere we've been, and the last entry is where we are!

To change where we are, we will just `push` a new location to the array!

```jsx
const handleClick = () => {
  alert("You are headed to animals");
  ///CHANGE LOCATION???
  history.push('/animals');
}; 
```



## Detail View & `useParams` Hook

Often, we want just one thing to be displayed. We can do this with our client side routing!

First, we need a route, and we need it to work with any id. 
            
```JSX
<Route path="/animals/:id" exact>
  <AnimalDetail />
</Route>
```
Component looks like this:

```JSX
import { useParams } from 'react-router-dom';
const AnimalDetail = () => {
   // the useParams hook will give us all of the url params
   // from the path (like /animals/:id) as an object. Use
   // object destructing to save the :id portion to a variable
   // with the name 'id'
   // if it were `:taco` in the route, then it would be `const {taco}`
   const { id } = useParams(); 
   return (
   <>
      <h1>Animal Details</h1>
      <p>Details for animal with id of {id}</p>
   </>);
}

export default AnimalDetail;
```

Note: In many cases, the thing you want to look up by `id` may not be available yet (async fetching, for example). You can use `useEffect` to re-run
the code to look up an item by id, and tie it to the state that will hold the data as a dependency:
``` jsx
  const animals = useSelector(s => s.animals);
  let [thisAnimal, setThisAnimal] = useState({});
  const { id } = useParams();
  useEffect(() => {
    setThisAnimal(animals.find(animal => animal.id === id));
  }, [animals]);
  // etc
```

## Change View Programmatically with Class Based Components (`withRouter`)

`Link` components are great if all we want to do is change where we are. But what if we want to do other things, too? We'd need to change view after we do other things. Which a `Link` can't do...

Let's add a button to our Home Component.  `onClick` let's fire a function that makes an alert, and then changes where we are.

Thankfully, we have the ability to do so!
When you make a `Route`, say 
```JSX
<Route exact path="/" component={Home} />
```

The component rendered gets some props passed to it for free!
Try `console.log(this.props)` in the render of your Home Component.
You get an array called `history` and an object called `match`. 

`history` is the key here! It's an array of everywhere we've been, and the last entry is where we are!

To change where we are, we will just `push` a new location to the array!

```jsx
handleClick = () => {
      alert("You are headed to animals");
      // Change Location
      this.props.history.push('/animals')
    }  
```

`this.props.history` is *only* available to direct children of a `Route` -- we don't get history for free here. Whomp whomp. 

We could pass it down to our button, but that seems...annoying. What if this was a child of a child of a child...ugh.

React-Router totally anticipates our need. There is a Higher-Order Component called `withRouter` that they give us to handle this exact need. HOCs will always return your wrapped component with additional functionality, in this case, the router stuff we want!

Import, and then call the function in your export, passing it your component.

```jsx
import React, {Component} from 'react';
import {withRouter} from 'react-router-dom';

class AnimalButton extends Component {
  handleClick = () => {
      alert("You are headed to animals");
      ///CHANGE LOCATION???
      this.props.history.push('/animals')
    } 

    render() {
      return (
        <button onClick={this.handleClick} />
      )
    }
}

export default withRouter(AnimalButton);
```

Voila! `history` is a thing again!


### Nesting Component in Routes

We can also nest a Component right inside the Route. This allows us to pass any props we want.

```JSX
<Route path="/plants">
  <Plants someProp="hello plants">
</Route>
```

However, this means we lose the automatic usages of `history`! `withRouter` is required when using child components.

### Tradeoffs

```JSX
<Route path="/plants/:id" component={PlantsDetail} />
```

Using `component` prop the component receives `history` and `match` without needing `withRouter`
            
```JSX
<Route path="/plants/:id">
  <PlantsDetail />
</Route>
```

Nesting the component as a child allows other props but then requires `withRouter` in the child component to get `history` and `match`.

## Accessing URL params with Class Based Components (`props.match.params`)

You can access params using `withRouter` and the `props.match.params` prop in the same way as we did with hooks.

First, we need a route, and we need it to work with any id. 
            
```JSX
<Route path="/animals/:id" exact>
  <AnimalDetail />
</Route>
```
Component looks like this:

```JSX
import { Component } from 'react';
import { withRouter } from 'react-router-dom';

class AnimalDetail extends Component
  render() => {
   // the match.params prop will give us all of the url params
   // from the path (like /animals/:id) as an object. Use
   // object destructing to save the :id portion to a variable
   // with the name 'id'
   // if it were `:taco` in the route, then it would be `const {taco}`
   const { id } = this.props.match.params;
   return (
   <>
      <h1>Animal Details</h1>
      <p>Details for animal with id of {id}</p>
   </>);
   }
}

export default withRouter(AnimalDetail);
```
