# CSS Modules

With CSS Modules, all class names are locally scoped by default. This means
no more bugs from classname clashes. Being able to compose primitives to build
up behaviour also lets us bring programming best practice to CSS: Composable & DRY styles.

For a detailed explanation see the [official documentation ➝](https://github.com/css-modules/css-modules)

## Usage

Write your CSS normally in the `styles.css` file in the component folder:

```css
/* styles.css */

.saveBtn {
  composes: btn from '../components/btn'; // similar concept as SASS Mixins

  background-color: green;
  color: white;
}
```

Then you can `import` the CSS file in your component and reference the
class name in the `className` prop:

```javascript
// index.js
import styles from './styles.css'

// ...inside render()...
return (
  <button className={ styles.saveBtn }>Save!</button>
)
```

## Integrating Global CSS

Because class names in CSS Modules are locally scoped by default, there is some
additional setup and consideration that must be taken to work correctly with 
traditional global CSS.

Let's use [Bootstrap](http://getbootstrap.com/) as an example.  First of all,
because we are in the React environment, it is widely recommended to not use
the Javascript code that is packaged with Bootstrap, but rather to re-write that
code in a React-friendly way. As an additional constraint for this example, let's use npm and webpack to manage our dependencies so that there is no need to manually add any script tags to `index.html`.

### Preparation
Edit `package.json` and make the following modifications:

```json
  "dllPlugin": {
    ...
    "exclude": [
      "bootstrap-css-only",
      ...
    ],
    ...
  },
  "dependencies": {
    ...
    "bootstrap-css-only": "3.3.6",
    "react-bootstrap": "0.30.0",
    ...
  }
```
The `exclude` configuration change is necessary to ensure that the dllPlugin build
process doesn't attempt to parse the global CSS. If you don't do this there will be an error during the build process and you won't be able to run the application.

Now edit `internals/config.js` and make the following modifications

```javascript
const ReactBoilerplate = {
  ...
  dllPlugin: {
    defaults: {
      ...
      exclude: [
        'bootstrap-css-only',
        ...
      ],
      ...
}
```

And finally edit `app/app.js`, and add the following after the line `import 'sanitize.css/sanitize.css'`

```javascript
...
import 'sanitize.css/sanitize.css'
import 'bootstrap-css-only/css/bootstrap.min.css'
```

### Usage

There are multiple approaches you can use to apply and override the global CSS.

You can apply the global styles directly:

```javascript
<div className="container-fluid"></div>
```

Or you can override global styles in your CSS module:

```css
:global .container-fluid {
  margin-left: 20px;
}
```