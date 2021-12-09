# A deeper dive into testing and continuous integration

We'll be diving a bit deeper into testing react components!

## Topics

- Using `@testing-library/react` and `@testing-library/jest-dom` to test both DOM interactions and the rendering of React components
- Setting up continuous integration (we'll be using Github actions, but talk about CircleCI/TravisCI/Jenkins)

## Tools

- [`@testing-library/react`](https://testing-library.com/docs/react-testing-library/intro)
- [`@testing-library/jest-dom`](https://testing-library.com/docs/dom-testing-library/intro)
- [Github Actions](https://github.com/features/actions)

## Getting some React Component tests!

We'll need to install the required deps into our project:
```bash
npm install --save-dev @testing-library/react @testing-library/jest-dom @testing-library/user-event
```

Once we have our testing libraries available in our `devDependencies`, the real fun begins!

The next step is vital if we want to have any passing tests. Let's create a new file in our `src/` directory, called `setupTests.js`:
```js
// src/setupTests.js

// react-testing-library renders your components to document.body,
// this adds jest-dom's custom assertions
import '@testing-library/jest-dom';
```

This will allow us to test DOM elements simply. DO NOT FORGET THAT FILE.

Now that we've gotten that out of the way... let's actually add a test somewhere.

How about we test a simple component? Something like this:
```jsx
// src/components/Footer/Footer.jsx

import React from 'react';
import './Footer.css';

// This is one of our simplest components
// It doesn't have local state, so it can be a function component.
// It doesn't dispatch any redux actions or display any part of redux state
// or even care what the redux state is, so it doesn't need 'connect()'

function Footer() {
  return <footer>&copy; Emerging Digital Academy</footer>;
}

export default Footer;
```

Let's create a new test file nearby, in the same directory, and call it `Footer.test.jsx`, and put the following in there:
```jsx
// src/components/Footer/Footer.test.jsx

import React from 'react';
import { render, screen } from '@testing-library/react';
import Footer from './Footer';

describe('the Footer component', () => {
  it('should render the "Emerging" text', () => {
    render(
      <Footer />
    );
    expect(screen.getByText(/Emerging/)).toBeInTheDocument();
  });
});
```

Now let's run `npm run test`, and see what happens! Bonus points if you can explain what `/Prime/` does in our `expect()` call.

That was a little on the easy side. What about testing components that interact with redux?

How would we go about testing something like that? Let's say we have an BookList component that looks like this one: https://github.com/EmergingDigitalAcademy/react-workshop-bookstore/blob/step3-redux-router-solution/src/components/BookList/BookList.jsx

Try creating a file in the same directory, and call it `BookList.test.jsx`:
```jsx
// src/components/BookList/BookList.test.jsx

import React from 'react';
import { Provider } from 'react-redux';
import {HashRouter as Router} from 'react-router-dom';
import { render, screen } from '@testing-library/react';
import App from './App';

import store from '../../redux/store';

describe('the BookList component', () => {
  it('should render some text', () => {
    store.dispatch(
      { type: 'SET_BOOKS', payload: [{ author: "Bob", title: "Awesome book" }] },
    );
    render(
      <Provider store={store}>
        <App />
      </Provider>
    );
    expect(screen.getAllByRole('button').length).toBeGreaterThan(0);
  });
});
```

Now we run `npm run test` again (or let it run on it's own if you forgot to exit).

And... bam! Another test, down! We just tested Redux interacting with React by wrapping our tested component in a `<Provider />`, and passing it a store.

What about testing `react-router`, I hear? Well... let's try that out, too!

Let's dive into a really complicated component. Our [`src/components/App/Header.jsx`](https://github.com/EmergingDigitalAcademy/react-workshop-bookstore/blob/step3-redux-router-solution/src/components/App/Header.jsx) component will do.

Once again, create a test file in the same directory, and give it a name like `Header.test.jsx`:
```jsx
// src/components/App/Header.test.jsx

import React from 'react';
import { Provider } from 'react-redux';
import {HashRouter as Router} from 'react-router-dom';
import { render, screen } from '@testing-library/react';
import Nav from './Nav';

import store from '../../redux/store';

describe('the Nav component', () => {
  it('should render a specific string that includes "Books"', () => {
    render(
      <Provider store={store}>
        <Router>
          <Nav />
        </Router>
      </Provider>
    );
    expect(screen.getByText('Books')).toBeInTheDocument();
  });
});
```

And now, run `npm run test` - and would you look at that! We not only are testing interactions with React Router, but we're actually testing interactions with React and Redux, too!

## Automating all the testing things

What good are tests that we don't run? It's fine to run `npm run test` every now and again... but we really need to start running tests on every push to Github. That's what the cool kids are doing. We should probably do it, too.

Github actions is a great tool for running automated tests, as it's free! And it's on Github!

It's... painfully simple to get started.

We need to create a file called a "workflow" to let Github know that we want to use Github actions.

Let's create a new file at `.github/workflows/node.js.yml` with the following content:

```yml
# This workflow will do a clean install of node dependencies, cache/restore them, build the source code and run tests across different versions of node
# For more information see: https://help.github.com/actions/language-and-framework-guides/using-nodejs-with-github-actions

name: Node.js CI

on:
  push:
    branches: [ master ]
  pull_request:
    branches: [ master ]

jobs:
  build:

    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2
    - name: Use Node.js 16.x
      uses: actions/setup-node@v2
      with:
        node-version: 16.x
    - run: npm install
    - run: npm run build --if-present
    - run: npm run test
```

And... let's push it to Github and see what happens! If it doesn't work, let's go over the error messages together and debug it.

## Resources

The Create React App Testing guide: https://create-react-app.dev/docs/running-tests
Writing tests with the testing library: https://testing-library.com/docs/react-testing-library/example-intro
Jest mocks: https://jestjs.io/docs/es6-class-mocks
Github actions node setup: https://github.com/actions/setup-node

