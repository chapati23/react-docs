# Unit Testing

Unit testing is the practice of testing the smallest possible *units* of our
code, functions. We run our tests and automatically verify that our functions
do the thing we expect them to do. We assert that, given a set of inputs, our
functions return the proper values and handle problems.

We use this glob pattern to find unit tests `app/**/*.test.js` - this tells
Mocha to run all files that end with `.test.js` anywhere within the `app`
folder. Use this to your advantage, and put unit tests in a subdirectory next to the files you
want to test so relevant files stay together.

Imagine a navigation bar, this is what its folder might look like:

```
NavBar                   # Wrapping folder
├── index.js             # Actual component
├── styles.css           # Styles
├── actions.js           # Actions
├── reducer.js           # Reducer
└── test                 # Tests folder
    ├── actions.test.js  # Actions tests
    └── reducer.test.js  # Reducer tests
```

## Basics

Let's pretend we're testing this function. It's situated in the `add.js` file:

```javascript
// add.js

export function add(x, y) {
  return x + y
}
```

### Mocha

[Mocha ➝](http://mochajs.org) is our unit testing framework. Its API, which we write tests with, is
speech-like and easy to use.

We're going to add a second file called `add.test.js` with our unit tests inside. Running said unit tests requires us to enter `mocha add.test.js` into the command line.

First, we `import` the function in our `add.test.js` file:

```javascript
// add.test.js

import { add } from './add.js'
```

Second, we `describe` our function:

```javascript
describe('add()', () => {

})
```

Third, we tell Mocha what `it` (our function) should do:

```javascript
describe('add()', () => {
  it('adds two numbers', () => {

  })

  it('doesnt add the third number', () => {

  })
})
```

That's the entire Mocha part. Onwards to the actual tests.

### expect

Using [expect ➝](https://github.com/mjackson/expect), we `expect` our function to return the same thing every time given the same input.

First, we have to import `expect` at the top of our file, before the tests:

```javascript
import expect from 'expect';

describe('add()', () => {
  // ...
})
```

We're going to test that our function correctly adds two numbers. We're going to take some chosen inputs and `expect` the result `toEqual` the corresponding output:

```javascript
// ...
it('adds two numbers', () => {
  expect(add(2, 3)).toEqual(5)
})
// ...
```

Lets add the second test, which determines that our function doesn't add the
third number if one is present:

```javascript
// ...
it('doesnt add the third number', () => {
 expect(add(2, 3, 5)).toEqual(add(2, 3))
})
// ...
```

> Note: Notice that we call `add` in `toEqual`. I won't tell you why, but just
  think about what would happen if we rewrote the expect as `expect(add(2, 3, 5)).toEqual(5)`
  and somebody broke something in the add function. What would this test
  actually test?

If our function works, Mocha will show this output when running the tests:

```
add()
  ✓ adds two numbers
  ✓ doesnt add the third number
```

Let's say an unnamed colleague of ours breaks our function:

```javascript
// add.js

export function add(x, y) {
  return x * y
}
```

Now our function doesn't add the numbers anymore, it multiplies them. Imagine the consequences to our code that uses the function!

Thankfully, we have unit tests in place. Because we run the unit tests before we deploy our application, we'll see this output:

```
add()
  1) adds two numbers
  ✓ doesnt add the third number

  1) add adds two numbers:
    Error: Expected 6 to equal 5
```

This tells us that something is broken in the add function before any users get
the code. Congratulations, you just saved time and money :)