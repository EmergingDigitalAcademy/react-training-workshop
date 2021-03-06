# React Training Workshop (WIP)

Projects:
  - [React BookShelf](https://github.com/EmergingDigitalAcademy/react-workshop-bookstore) -- includes 4 functioning project checkpoints (see README)
  - [GitHub Manager](https://github.com/EmergingDigitalAcademy/react-training-1) -- demo app with lots of goodies by Jaryd
  - [Famous People](https://github.com/EmergingDigitalAcademy/react-workshop-famous-people) -- assignment to practice hooks (no redux) (`solution` branch exists)
  - [Baseball Pitchers](https://github.com/EmergingDigitalAcademy/react-workshop-redux-pitchers) -- assignment to practice redux (`solution` branch exists)

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
  - 8:30am-12:00am What's new with React? Proptypes, Portals, Error Boundaries, Refs, Fragments, Memo, StrictMode (`Code Along w/ GitHub Manager`) (BLAINE/JARYD)
  - 12:00pm-1:00pm Lunch
  - 1:00pm-1:30pm Review of Functional Components & Intro to Hooks (`useState`) (Code along: Convert click counter from CRA to functional w/ hooks) (BLAINE)
  - --> optional: convert a component to functional in `GitHub Manager` app
  - 2:00pm-3:00pm Managing the React LifeCycle (`useEffect`) (Code along: Building a Bookstore: master to step1) (BLAINE)
  - 3:00pm-4:00pm **Practice Assignment 1 (Famous People w/ LiveSolve)**

### Day 2 (Wednesday)
  - 8:00am-8:30am Ice Breaker & Review
  - 8:30am-9:30am Intro to React Router (declarative routing, HashRouter, NavLink) (Code along: Bookstore Details View: step1 to step2) (JARYD)
  - 9:30am-10:30am React Router (`withRouter`, `useHistory`, `useParams`) (JARYD)
  - 9:30am (BLAINE ONLY) Fortran Class 9:30am-10:30am
  - 11:00am-12:00pm Intro to Redux (reducers, store, actions, constants) (BLAINE) (Code Along: Bookstore: step2 to step3, minus sagas) 
  - 12:00pm-1:00pm Lunch
  - 1:00-2:00pm Intro to Redux (connected components, `useSelector`) (BLAINE/JARYD)
  - 2:00pm-3:00pm Intro to Redux Sagas (Code Along: Bookstore: step3 completion (with sagas)) (BLAINE/JARYD)
  - 3:00pm-4:00pm **Practice Assignment 2 (Baseball Pitchers)**

### Day 3 (Thursday)
  - 8:00am-8:30am Ice Breaker & Review
  - 8:30am-10:00am Intro to Testing (component testing, jest, `act`, etc) (JARYD)
  - 10:00am-11:00am Testing `GitHub Manager` project (JARYD)
  - 11:00am-12:00pm Testing Continued (react router, redux, coverage testing, etc) (JARYD/BLAINE)
  - 12:00pm-1:00pm Lunch
  - 1:00pm-2:00pm Testing `Baseball Pitchers` project (JARYD/BLAINE)
  - 2:00pm-3:00pm Continuous Integration Setup (`Baseball Pitchers`) (JARYD)
  - 2:00pm (BLAINE ONLY) Fortran Class 2:00pm-3:00pm
  - 3:00pm-4:00pm Practice Assignment 4 (Add testing to `Bookstore`)

### Day 4 (Monday)
  - 8:00am-8:30am Ice Breaker & Review
  - 8:30am-12:00pm Deep Dive into Continuous Integration & Deployment Testing (JARYD)
  - 12:00pm-1:00pm Lunch
  - 1:00pm-2:00pm Workshop Time (talk through local project strategy)
  - 2:00pm-4:00pm Workshop Time (talk through local project strategy)

### Day 5 (Tuesday)
  - 8:00am - 4:00pm: Misc workshop or project kickoff (build an app using lessons learned this week: router, redux, tests)
  - 8:00am - 3:00pm: Project Work. Break into teams of 2-3, build project together.
  - 3:00pm - 4:00pm: Present Project Work. Debrief.
