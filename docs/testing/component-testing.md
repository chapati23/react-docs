# Component Testing

## Shallow rendering

React provides us with a nice add-on called the Shallow Renderer. This renders a React component **one level deep**. Let's take a look at what that means with a simple `<Button>` component.

This component renders a `<button>` containing a checkmark icon and some
text:

```javascript
// Button.react.js

import CheckmarkIcon from './CheckmarkIcon.react'

function Button(props) {
  return (
    <button className="btn" onClick={props.onClick}>
      <CheckmarkIcon />
      { React.Children.only(props.children) }
    </button>
  )
}

export default Button
```

It might be used in another component like this:

```javascript
// HomePage.react.js

import Button from './Button.react'

class HomePage extends React.Component {
  render() {
    return(
      <Button onClick={this.doSomething}>Click me!</Button>
    )
  }
}
```

When rendered normally with the standard `ReactDOM.render` function, this will
be the HTML output:

```html
<button>                           <!-- <Button>             -->
  <i class="fa fa-checkmark"></i>  <!--   <CheckmarkIcon />  -->
  Click Me!                        <!--   { props.children } -->
</button>                          <!-- </Button>            -->
```

Conversely, when rendered with the shallow renderer, we'll get a String
containing this "HTML":

```html
<button>              <!-- <Button>             -->
  <CheckmarkIcon />   <!--   NOT RENDERED!      -->
  Click Me!           <!--   { props.children } -->
</button>             <!-- </Button>            -->
```

If we test our `Button` with the normal renderer and there's a problem
with the `CheckmarkIcon` then the test for the `Button` will fail as well. But finding the culprit will be hard. Using the **shallow** renderer, we isolate
the problem's cause since we don't render any other components other than the
one we're testing.

The problem with the shallow renderer is that all assertions have to be done
manually, and you cannot do anything that needs the DOM.

Thankfully, AirBnB has open sourced their wrapper around the React shallow renderer and jsdom called `enzyme`. [enzyme ➝](http://airbnb.io/projects/enzyme/) is a testing utility that offers a nice assertion/traversal/manipulation API.

## Enzyme

Let's test our `<Button>` component. We're going to assess three things: First,
that it renders a HTML `<button>` tag, second that it renders the children we
pass in and third that it handles clicks.

This is our Mocha setup:

```javascript
describe('<Button />', () => {
  it('renders a <button>', () => {})

  it('renders its children', () => {})

  it('handles clicks', () => {})
});
```

Let's start with testing that it renders a `<button>`. To do that we first
`shallow` render it, and then `expect` that a `<button>` node exists:

```javascript
it('renders a <button>', () => {
  const renderedComponent = shallow(
    <Button></Button>
  )
  expect(
    renderedComponent.find("button").node
  ).toExist()
})
```

Done. If somebody breaks our button component by having it render an `<a>` tag
or something else we'll know immediately. Let's do something a bit more advanced
now, and check that our `<Button>` renders its children.

We render our button component with some text, and then verify that our text
exists:

```javascript
it('renders its children', () => {
  const text = "Click me!"
  const renderedComponent = shallow(
    <Button>{ text }</Button>
  )
  expect(
    renderedComponent.contains(text)
  ).toEqual(true)
})
```

Cool. Now for our last and most advanced test: Checking that our `<Button>` handles clicks correctly. We'll use a `Spy` for that. A Spy is a function that knows if, and how often, it has been called. We create the Spy (provided by `expect`), pass it as the `onClick` handler to our
component, simulate a click on the rendered `<button>` element and, lastly,
check if our Spy was called:

```javascript
it('handles clicks', () => {
  const onClickSpy = expect.createSpy();
  const renderedComponent = shallow(<Button onClick={onClickSpy} />);
  renderedComponent.find('button').simulate('click');
  expect(onClickSpy).toHaveBeenCalled();
});
```

And that's how you unit test your components and make sure they work correctly.