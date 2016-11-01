## Testing Redux Applications

A big side-benefit of using Redux is that it turns our data flow into
testable ("pure") functions. Let's go back to our `NavBar` component from the [Unit Testing doc](./unit-testing.md) and see what testing the actions and the reducer of it would look like.

This is what our `NavBar` actions look like:

```javascript
// NavBar/actions.js

import { TOGGLE_NAV } from './constants'

export function toggleNav() {
  return { type: TOGGLE_NAV }
}
```

with this reducer:

```javascript
// NavBar/reducer.js

import { TOGGLE_NAV } from './constants'

const initialState = {
  open: false
}

function NavBarReducer(state = initialState, action) {
  switch (action.type) {
    case TOGGLE_NAV:
      return Object.assign({}, state, {
        open: !state.open
      })
    default:
      return state
  }
}

export default NavBarReducer
```

Lets test the reducer first!

### Reducers

First, we have to import `expect`, the reducer and the constant.

```javascript
// NavBar/test/reducer.test.js

import expect from 'expect'
import NavBarReducer from '../reducer'
import { TOGGLE_NAV } from '../constants'
```

Then we `describe` the reducer, and add two tests: we check that it returns the
initial state and that it handles the `toggleNav` action.

```javascript
describe('NavBarReducer', () => {
  it('returns the initial state', () => {

  })

  it('handles the toggleNav action', () => {

  })
})
```

Let's write the actual test code. Since the reducer is just a function, we can call it like any other function and `expect` the output to equal something.

To test that it returns the initial state, we call it with a state of `undefined` (the first argument), and an empty action (second argument). The reducer should return the initial state of the `NavBar`, which is

```javascript
{
  open: false
}
```

Let's put that into practice:

```javascript
describe('NavBarReducer', () => {
  it('returns the initial state', () => {
    expect(NavBarReducer(undefined, {})).toEqual({
      open: false
    })
  })

  it('handles the toggleNav action', () => {

  })
})
```

This works, but we have one problem: We also test the initial state itself. When somebody changes the initial state, this test will fail, even though the reducer correctly returns the initial state.

To fix that, we have to `import` the initial state from the reducer file and check that the reducer returns that. This has one problem: Our initial state isn't `export`ed.

Now, you might be thinking "Ha! easy: simply add an `export` before the `const initialState` in the reducer and boom!"... But that'd be a bad practice as it's an internal (or "private") property of that module
alone and shouldn't really be accessible from the outside at all.

This is where `rewire` comes in.

#### rewire

[Rewire ➝](https://github.com/jhnns/rewire) allows us to access properties we normally couldn't via special
`__get__` and `__set__` methods it injects into modules.

Start by importing rewire **at the top** of your test file:

```javascript
// `NavBar/test/reducer.test.js`

import expect from 'expect'
import rewire from 'rewire'
import NavBarReducer from '../reducer'
import { TOGGLE_NAV } from '../constants'

const initialState = NavBarReducer.__get__('initialState')
```

> Note: You might be wondering why we still import the `NavBarReducer` above.
> The `NavBarReducer` imported with `rewire` isn't the _actual_ reducer, it's a `rewire`d version.

Now we can *really* see whether the `NavBarReducer` returns the initial state if no action is passed.

```javascript
it('returns the initial state', () => {
  expect(NavBarReducer(undefined, {})).toEqual(initialState)
})
```

Done. Test fixed.

### Actions

We have one action `toggleNav` that changes the `NavBar` open state.

A Redux action is a pure function, so testing it isn't any more difficult than testing our `add` function from the first part of this guide.

The first step is to import the action to be tested, the constant it should return and `expect`:

```javascript
// NavBar/test/actions.test.js

import { toggleNav } from '../actions'
import { TOGGLE_NAV } from '../constants'
import expect from 'expect'
```

Then we `describe` the actions:

```javascript
describe('NavBar actions', () => {
  describe('toggleNav', () => {
    it('should return the correct constant', () => {

    })
  })
})
```

> Note: `describe`s can be nested, which gives us nice output, as we'll see later.

And the last step is to add the assertion:

```javascript
it('should return the correct constant', () => {
  expect(toggleNav()).toEqual({
    type: TOGGLE_NAV
  })
})
```

If our `toggleNav` action works correctly, this is the output Mocha will show us:

```
NavBar actions
  toggleNav
    ✓ should return the correct constant
```

And that's it, we now know when somebody breaks the `toggleNav` action.
