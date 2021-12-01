# React Training Workshop (WIP)

Tentative Topics (to be broken down by schedule)

#### React 15 Upgrade Path
  - [React Blog](https://reactjs.org/blog/all.html/) has upgrade docs
  - 15.x -> 15.6
    - Minor upgrade, fix deprecation warnings 
  - 15.6 -> 16.0
    - portals, fragments (initial support), improved server-side rendering support, [DOM attributes](https://reactjs.org/blog/2017/09/08/dom-attributes-in-react-16.html), [error boundaries](https://reactjs.org/blog/2017/07/26/error-handling-in-react-16.html), reduced file size [[16.0 release notes](https://reactjs.org/blog/2017/09/26/react-v16.0.html#upgrading)]
  - 16.0 -> 16.2
    - `<></>` fragments [Notes](https://reactjs.org/blog/2017/11/28/react-v16.2.0-fragment-support.html)
  - 16.2 -> 16.3
    - context API, component lifecycle updates, `createRef`/`forwardRef`, `<StrictMode/>` component. [Notes](https://reactjs.org/blog/2018/03/29/react-v-16-3.html)
  - 16.3 -> 16.4
    - Pointer Events [Notes](https://reactjs.org/blog/2018/05/23/react-v-16-4.html)
  - 16.4 -> 16.5
    - [React Profiler DevTools](https://reactjs.org/blog/2018/09/10/introducing-the-react-profiler.html)
  - 16.5 -> 16.6, 16.7
    - memo, lazy, contextType [Notes](https://reactjs.org/blog/2018/10/23/react-v-16-6.html)
  - 16.7 -> 16.8
    - HOOKS, `act` [Notes](https://reactjs.org/blog/2019/02/06/react-v16.8.0.html)
  - 16.8 -> 16.9 -> 16.13
    - deprecation warnings, prep for major upgrade, async `act`
  - 16.x -> 17.x

#### React Best Practices
  - Proptypes
  - Pure Components
  - Component Documentation
  - `React.memo` for non-dependent child components
  - CSS-in-JS?
  - eslint configuration
  - Import Order (Built-In, External, Internal)
  - Naming conventions: PascalCase for components, camelCase for instances. CamelCase for props.
  - Dont use reserved props (like className)
  - Double Quotes (JSX) vs Single Quotes (regular JS)
  - Multiple return lines wrapped in parenthesis
  - Self-closing tags for components that do not have children
  - Webpack features (like tree shaking)
  - Working with SCSS

#### HTML5 Best Practices
  - alt tags in images (non-react)
  - Using form `onSubmit` with `required` and html5 validators

#### ECMAScript Features
  - Object Destructuring (props, views)
  - Template literals
  - Implicit return with arrow functions

## Tentative Schedule

### Day 1
  - 8:00am-8:30am Ice Breaker
  - 8:30am-10:00am Overview: CRA, JSX, ECAMAScript2021, react best practices, eslint configuration, naming conventions
  - 10:00am-12:00pm What's new with React? Portals, Fragments, Memo, StrictMode, context API, JSX Transform
  - 12:00pm-1:00pm Lunch
  - 1:00pm-2:00pm Upgrade Path to React 17 (Up to 15.6)
  - 2:00pm-3:00pm Upgrade Path to React 17 (Up to 16.13)
  - 3:00pm-4:00pm Practice Assignment 1 (upgrading a react app)

### Day 2
  - 8:00am-8:30am Ice Breaker & Review
  - 8:30am-9:30am Review of Functional Components
  - 9:30am-10:00am Intro to Hooks
  - 10:00am-11:00am Managing local state (useState)
  - 11:00am-12:00pm Working with the react lifecycle (useEffect)  
  - 12:00pm-1:00pm Lunch
  - 1:00pm-2:00pm Intro to Redux (reducers, store)
  - 2:00pm-3:00pm Intro to Redux (actions, constants, useDispatch, useSelector, custom hooks, shoutout to reselector)
  - 3:00pm-4:00pm Practice Assignment 2 (practice with redux)

### Day 3
  - 8:00am-8:30am Ice Breaker & Review
  - 8:30am-10:00am Intro to React Router (declarative routing, HashRouter, NavLink)
  - 10:00am-11:00am Using `useParams` with `useEffect`, `useHistory`
  - 11:00am-12:00pm Practice Assignment 3 (practice with react router)
  - 12:00pm-1:00pm Lunch
  - 1:00pm-2:00pm Intro to Testing (component testing)
  - 2:00pm-3:00pm Testing Continued
  - 3:00pm-4:00pm Practice Assignment 4 (component testing)

### Day 4
  - 8:00am-8:30am Ice Breaker & Review
  - 8:30am-10:00am Intro to Continuous Integration
  - 10:00am-12:00pm Jenkins, Deployment, etc.
  - 12:00pm-1:00pm Lunch
  - 1:00pm-2:00pm Wrapup & Testing Review
  - 2:00pm-4:00pm Workshop Time (talk through local project strategy)

### Day 5
  - 8:00am - 4:00pm: Misc workshop or project kickoff
  - 8:00am - 3:00pm: Project Work. Break into teams of 2-3, build project together.
  - 3:00pm - 4:00pm: Present Project Work. Debrief.
