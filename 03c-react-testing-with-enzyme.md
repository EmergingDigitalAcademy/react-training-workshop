# Using Enzyme to test react components!

We'll be diving a bit deeper into testing react components!

## Topics

- Using `enzyme` to test react components

## Tools

- [`enzyme`](https://enzymejs.github.io/enzyme/)
- [`@wojtekmaj/enzyme-adapter-react-17`](https://enzymejs.github.io/enzyme/)

## Getting some React Component tests!

We'll need to install the required deps into our project:
```bash
npm install --save-dev enzyme @wojtekmaj/enzyme-adapter-react-17
```

Once we have our testing libraries available in our `devDependencies`, the real fun begins!

The next step is vital if we want to have any passing tests. Let's create a new file in our `src/` directory, called `setupTests.js`:
```js
// src/setupTests.js

import { configure } from 'enzyme';
import Adapter from '@wojtekmaj/enzyme-adapter-react-17';

// Setup enzyme to use our React 17 adapter. Use a different adapter for different versions!
configure({ adapter: new Adapter() });
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
import { shallow } from 'enzyme';
import Footer from './Footer';

describe('the Footer component', () => {
  it('should render the "Emerging" text', () => {
    const wrapper = shallow(
      <Footer />
    );
    expect(wrapper.find('footer').length).toEqual(1);
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
import { shallow } from 'enzyme';
import BookList from './BookList';

import store from '../../redux/store';

describe('the BookList component', () => {
  it('should render some text', () => {
    store.dispatch(
      { type: 'SET_BOOKS', payload: [{ author: "Bob", title: "Awesome book" }] },
    );
 
    const wrapper = shallow(
      <Provider store={store}>
        <BookList />
      </Provider>
    );
    expect(wrapper.find('footer').length).toBeGreaterThan(0);
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
import { shallow } from 'enzyme';
import Nav from './Nav';

import store from '../../redux/store';

describe('the Nav component', () => {
  it('should render a list of navigation items', () => {
    const wrapper = shallow(
      <Provider store={store}>
        <Router>
          <Nav />
        </Router>
      </Provider>
    );
    expect(wrapper.find('li').length).toBeGreaterThan(0);
  });
});
```

And now, run `npm run test` - and would you look at that! We not only are testing interactions with React Router, but we're actually testing interactions with React and Redux, too!


## Resources

The Create React App Testing guide: https://create-react-app.dev/docs/running-tests
Writing tests with enzyme: https://enzymejs.github.io/enzyme/docs/api/shallow.html
Jest mocks: https://jestjs.io/docs/es6-class-mocks
