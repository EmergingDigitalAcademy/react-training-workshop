# Unit Testing

Topics:
- Types of Testing
- Unit Testing 
- Example with Test Cases
- Unit testing w/ Jest
- Other Test Tools


## Types of Testing
Testing applications helps to ensure that they function correctly and meet project requirements. 

There are a lot of different types of testing... 

- Unit Testing
- Integration Testing
- Functional Testing
- Automated UI Testing
- System Testing
- Stress Testing
- Performance Testing
- Usability Testing
- Acceptance Testing
- Regression Testing
- Beta Testing

We're not going to talk about them all, but you can read more about them here: [Types of Testing](https://www.codeproject.com/Tips/351122/What-is-software-testing-What-are-the-different-ty)

Decent pyramid image of where testing fits in together: https://gist.github.com/leggetter/7927622


### Unit Testing

Today we'll be introducing **unit testing**, which is a very common developer task. For unit testing, we write tests to ensure individual functions are behaving as expected. We write tests to call the function with different input values (both good and bad values) and then check the output against our expectations.

As an example, we will create and test a function that takes in two numbers and returns the sum of the two numbers.

What are some things that we might test?  What are our test cases?

> There's generally a pretty obvious main use case, but we need to try to the less expected or unexpected too. 

- Pass in two numbers, 1 & 1, and expect it to return 2.
- Pass in a positive and negative number (1, -2), expect it to still return sum (-1).
- Pass in a decimal numbers (-1.5, 3), expect it to still return sum (1.5).
- Pass in only one value... 
  - What do we expect? May need to ask for clarification or make a best guess.
  - Assume the second number is 0 and return back the original number.
- Pass in non-numeric values... 
  - What do we expect? Again, may need to get clarification.
  - Try to convert to numbers. If convertable, then return sum. If not return `NaN`.


## Test Driven Development (TDD)
Test Driven Development (TDD) is the process of writting your tests *before* you write your code. This requires you to think about your code in advance and leads to improved code.  

Behavior Driven Development (BDD) is often used with TDD. It encourages you to focus on the behavior that you want to test, not how that behavior is implemented (or coded) in the function you are testing. It encourages you to say, in the test, what a function *should* do. 

We will be practicing both TDD & BDD as we learn to test the functions we are writing.

[More information on TDD & BDD](https://codeutopia.net/blog/2015/03/01/unit-testing-tdd-and-bdd/)


## Unit Testing with Jest 
We are going to use a package called Jest to help us test our functions. 

### Getting Started
Let's make a new directory and node project:

```
mkdir unit-test-intro
cd unit-test-intro
npm init -y
```

To get started with testing, we need to add Jest to our project. We'll use npm to get the Jest package and add it to our project as a dependency.

```
npm install --save-dev jest
```

by adding `--save-dev` we told npm to add this as a development dependency to our project.

Look at your `package.json` file now. Notice that a new property `devDependenies` was added:
```JavaScript
{
  "name": "01-02-unit-testing-intro",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "jest": "^23.6.0"
  }
}
```


### Write the Test

So to practice TDD, we're going to write our test first. Then we'll write the code for our actual function.

We'll start by creating a `tests` folder in our project and adding a new JavaScript file called `sum.test.js`.

We'll start by requiring in our code file, then write a test for our first test case:

**sum.test.js**
```JavaScript
const sum = require('../modules/sum');

test('sum of 1 and 2 gives 3', () => {
  expect(sum(1, 2)).toBe(3);
});
```

### Test Structure

Notice how we have structured this test code. `test` is a global function provided by Jest which will encapsulate a "test." We can give it a string of descriptive text. It also requires a function that will define the actual executable test code to run.

### Expect API

We're using a method called `toBe()` to check the equality of values. It works similarly to using `===` but also handles objects pretty well. There are a whole slew of specific methods for testing other things, like Booleans, Arrays, NaN, etc.

[expect API](https://jestjs.io/docs/en/expect)


### Write the Function

Now let's write the code for our function in a way that we expect it to pass the test.

Let's create a `modules` folder in our project and add a `sum.js` file with our function in it.
**sum.js**
```JavaScript
const sum = function (value1, value2) {
  return value1 + value2;
}

module.exports = sum;
```


### Run Tests
Now let's setup our `test` script in our `package.json` to run our test files with Jest. 

Update the existing test script to look like this, or add if not there:
```JavaScript
  "scripts": {
    "test": "jest"
  }
```

This tells Node to run Jest, which will go through your project and look for test files to run. 

> By default it looks for `.js` and `.jsx` files inside of __tests__ folders, as well as any files with a suffix of `.test` or `.spec` (e.g. Component.test.js or Component.spec.js). It will also find files called `test.js` or `spec.js`.


### Add More Tests
We had a lot of test cases identified earlier. 

There are four more tests to write. We should write a test for each one.

- [X] Pass in two numbers, 1 & 1, and expect it to return 2.
- [ ] Pass in a positive and negative number (1, -2), expect it to still return the sum (-1).
- [ ] Pass in a decimal numbers (-1.5, 3), expect it to still return the sum (1.5).
- [ ] Pass in only one value... 
  - What do we expect? May need to ask for clarification or make a best guess.
  - Assume the second number is 0 and return back the original number.
- [ ] Pass in non-numeric values... 
  - What do we expect? Again, may need to get clarification.
  - Try to convert to numbers. If convertable, then return sum. If not return `NaN`.

The next two test cases test our sum with different types of numbers - negative and decimal values. 

What do you think? Will our code still behave properly?

Well, there's only one way to find out. 

Try writing the next two tests and running them. Do the tests pass?

```JavaScript
test('sum (with negative) using 1 and -2 gives -1', () => {
  expect(sum(1, -2)).toBe(-1);
});

test('sum (with decimals) using -1.5 and 3 gives 1.5', () => {
  expect(sum(-1.5, 3)).toBe(1.5);
});
```

> Note that it is helpful to include information on what you are testing the test statement. 

The next two test cases are a little more interesting. They do a bit more to try and break the rules of how we might expect the function to work. 

The first one says only pass in one argument. Let's try this out and see what happens.

```JavaScript
test('sum (with only 1 value) using 3 gives 3', () => {
  expect( sum(3) ).toBe( 3 );
});
```

This test case fails.  Notice that the test log says which test(s) failed as well as what the expected value was expected and what the test actually got back.

```
 FAIL  tests/sum.test.js
  ✓ sum of 1 and 2 gives 3 (5ms)
  ✓ sum (with negative) using 1 and -2 gives -1
  ✓ sum (with decimals) using -1.5 and 3 gives 1.5
  ✕ sum (with only 1 value) using 3 gives 3 (8ms)

  ● sum (with only 1 value) using 3 gives 3

    expect(received).toBe(expected) // Object.is equality

    Expected: 3
    Received: NaN
```

Now we need to go fix our code so that this test will pass.

If the second parameter isn't passed in, then let's set it to 0:
```JavaScript
const sum = function (value1, value2) {
  if ( !value2 ) {
    value2 = 0;
  }
  return value1 + value2;
}
```

Now we can try the test again, and *fingers crossed* we will have fixed the bug!

```
 PASS  tests/sum.test.js
  ✓ sum of 1 and 2 gives 3 (4ms)
  ✓ sum (with negative) using 1 and -2 gives -1 (1ms)
  ✓ sum (with decimals) using -1.5 and 3 gives 1.5
  ✓ sum (with only 1 value) using 3 gives 3 (1ms)

Test Suites: 1 passed, 1 total
Tests:       4 passed, 4 total
```

Awesome!

One more test case to go!  
Try writing the last test using number strings, like `'1'` and `'2'`. 

Does the test pass?

- No, we get '12' because JavaScript will concatenate strings. 

We need to fix our code again.  Let's use the built-in JavaScript function [`parseInt`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseInt) to attempt to convert the strings to numbers.

```JavaScript
const sum = function (value1, value2) {
  if (!value2) {
    value2 = 0;
  }
  return parseInt(value1) + parseInt(value2);
}
```

Running the tests again, we can see that while the tests still fail, we fixed the one that had been broken. BUT we broke a different test case.

This is a great example of why these unit tests are so awesome. If we didn't have all the tests in the test file, we might have only tested the function with Strings to see if we had fixed the issue. Then we might have missed that we broke the function with decimals.

To fix this, change `parseInt` to [`parseFloat`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseFloat).

Now all our tests should pass, so we're done right???

Nope, not quite... 
That last test case had two possible outcomes and we only checked one. We didn't test that we get `NaN` if the values can't convert to a number.

Let's write one more test:
```JavaScript
test(`sum (with strings) using 'foo' and 'bar' gives NaN`, () => {
  expect( sum('foo', 'bar') ).toBe( NaN );
});
```

Woot! All our tests pass!

Now we can pass our awesome `sum` module off to others to use in their code with confidence that it behaves as expected. 

As a side benefit, we can also pass along the tests which help to show how we expect this code to behave. It documents our expected behaviors.

