# ImmutableJS

Immutable data structures offer very performant deep equality checks. This allows us to efficiently determine if our components need to rerender since we know whether the
`props` changed or not.

Check out the [official documentation ➝](https://facebook.github.io/immutable-js/)
for a good explanation of the more intricate benefits it has. &nbsp;&nbsp;&nbsp; <small style="color: grey; font-weight: normal">(➝ marks external links)</small>


## Usage

In our reducers, we make the initial state an immutable data structure with the
`fromJS` function. We pass it an object or array and `fromJS` takes care of
converting it to a compatible immutable data structure. The conversion is performed deeply so that even arbitrarily nested arrays/objects are immutable stuctures.

```JS
import { fromJS } from 'immutable'

const initialState = fromJS({
  myData: 'Hello World!'
})
```

To react to incoming actions our reducers can use the `.set` and the `.setIn`
functions.

```JS
import { SOME_ACTION } from './actions'

// […]

function myReducer(state = initialState, action) {
  switch (action.type) {
    case SOME_ACTION:
      return state.set('myData', action.payload)
    default:
      return state
  }
}
```

We use [reselect ➝](https://github.com/reactjs/reselect) to efficiently cache our computed application
state. Since that state is now immutable, we need to use the `.get` and `.getIn`
functions to select the part we want.

```JS
const myDataSelector = (state) => state.get('myData')

export default myDataSelector
```

To learn more, check out the [reselect doc](./reselect.md).

## Advanced Usage

ImmutableJS provides many immutable data structures like `Map`, `Set` and `List`. The downside to using immutable data structures is that they aren't normal JavaScript data structures. 

That means you must use getters to access properties. For instance you'll do `map.get('property')` instead of `map.property`, and `array.get(0)` instead of `array[0]`. It's not natural and the resulting code can feel a bit cluttered. Sometimes you forget if you're working with a JavaScript or an ImmutableJS object. While it's possible to be clear where you are using immutable objects, you still pass them through the system into places where it's not clear. This makes reasoning about functions harder.

The `Record` data structure tries to get rid of this drawback. `Record` is like a `Map` whose shape is fixed: you can't add new properties after the record is created. The benefit of `Record` is that you can use the standard dot notation to access properties while still leveraging the power of ImmutableJS's `.get`, `.set` and `.merge` methods.

To be able to use `Record` you first have to create its shape, similar to a model definition. With the example above, to create your initial state, you'd write:

```JS
// the shape
const MyStateRecord = Record({
  myData: 'Hello World!'
})

// initialized with myData set to 'Hello World!' by default
const initialState = new MyStateRecord({})
```

Now, if you want to access `myData`, you can just write `state.myData` in your reducer code.
