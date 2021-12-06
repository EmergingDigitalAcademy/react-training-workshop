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

## Tentative Schedule

### Day 1 (Tuesday)
  - 8:00am-8:30am Ice Breaker
  - 8:30am-10:00am Overview: CRA, JSX, ECAMAScript2021, react best practices, eslint configuration, naming conventions (BLAINE)
  - 10:00am-12:00pm What's new with React? Portals, Fragments, Memo, StrictMode, context API, JSX Transform (`Code Along w/ GitHub Manager`) (JARYD)
  - 12:00pm-1:00pm Lunch
  - 1:00pm-2:00pm Review of Functional Components & Intro to Hooks (`useState`) (Code along: Convert click counter to functional w/ hooks) (BLAINE)
  - 2:00pm-3:00pm Managing the React LifeCycle (`useEffect`) (Code along: Building a Bookstore) (BLAINE)
  - 3:00pm-4:00pm **Practice Assignment 1 (Famous People w/ LiveSolve)**

### Day 2 (Wednesday)
  - 8:00am-8:30am Ice Breaker & Review
  - 8:30am-9:30am Intro to React Router (declarative routing, HashRouter, NavLink) (Code along: Bookstore Details View) (JARYD)
  - 9:30am-10:30am React Router (`withRouter`, `useHistory`, `useParams`) (JARYD)
  - 9:30am (BLAINE ONLY) Fortran Class 9:30am-10:30am
  - 11:00am-12:00pm Intro to Redux (reducers, store, actions, constants) (BLAINE)
  - 12:00pm-1:00pm Lunch
  - 1:00-2:00pm Intro to Redux (connected components, `useSelector`) (Code Along: Bookstore Redux) (BLAINE)
  - 2:00pm-3:00pm Intro to Redux Sagas (Code Along: Bookstore Redux) (BLAINE)
  - 3:00pm-4:00pm **Practice Assignment 2 (Baseball Pitchers?)**

### Day 3 (Thursday)
  - 8:00am-8:30am Ice Breaker & Review
  - 8:30am-10:00am Intro to Testing (component testing, jest, `act`, etc) (JARYD)
  - 8:30am-10:00am Testing Continued (JARYD)
  - 10:00am-11:00am Testing Continued (react router, redux, coverage testing, etc) (JARYD)
  - 11:00am-12:00pm Testing Continued (JARYD)
  - 12:00pm-1:00pm Lunch
  - 1:00pm-2:00pm Testing Continued (JARYD)
  - 2:00pm-3:00pm Testing Continued (JARYD)
  - 2:00pm (BLAINE ONLY) Fortran Class 2:00pm-3:00pm
  - 3:00pm-4:00pm Practice Assignment 4 (component testing)

### Day 4 (Monday)
  - 8:00am-8:30am Ice Breaker & Review
  - 8:30am-10:00am Intro to Continuous Integration (JARYD)
  - 10:00am-12:00pm Jenkins, Deployment, etc. (JARYD)
  - 12:00pm-1:00pm Lunch
  - 1:00pm-2:00pm Workshop Time (talk through local project strategy)
  - 2:00pm-4:00pm Workshop Time (talk through local project strategy)

### Day 5 (Tuesday)
  - 8:00am - 4:00pm: Misc workshop or project kickoff (build an app using lessons learned this week: router, redux, tests)
  - 8:00am - 3:00pm: Project Work. Break into teams of 2-3, build project together.
  - 3:00pm - 4:00pm: Present Project Work. Debrief.
