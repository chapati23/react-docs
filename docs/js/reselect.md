# Reselect

`Reselect` memoizes ("caches") previous state trees and calculations. This means repeated changes and calculations are fast and efficient, providing us with a performance boost over standard `mapStateToProps` implementations.

The [official documentation ➝](https://github.com/reactjs/reselect)
offers a good starting point. &nbsp;&nbsp;&nbsp; <small style="color: grey; font-weight: normal">(➝ marks external links)</small>


## Usage

There are two different kinds of selectors, simple and complex ones.

### Simple selectors

Simple selectors are just that: They take the application state and select a
part of it.

```javascript
const mySelector = (state) => state.get('someState')

export {
  mySelector
}
```

### Complex selectors

We can also combine simple selectors to build more complex ones which
get nested parts of state via reselect's `createSelector` function. We import other
selectors and pass them `createSelector` call:

```javascript
import { createSelector } from 'reselect'
import mySelector from 'mySelector'

const myComplexSelector = createSelector(
  mySelector,
  (myState) => myState.get('someNestedState')
)

export {
  myComplexSelector
}
```

These selectors can then be used either directly in our containers as
`mapStateToProps` functions or nested with `createSelector` once again:

```javascript
export default connect(createSelector(
  myComplexSelector,
  (myNestedState) => ({ data: myNestedState })
))(SomeComponent)
```

### Adding a new selector

Next to the reducer whose state you want to write a selector for, add a file `selectors.js` and use this boilerplate code:

```JS
import { createSelector } from 'reselect'

const selectMyState = () => createSelector(
    […]
)

export {
  selectMyState
}
```