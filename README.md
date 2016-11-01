<div align="center">
  <!-- Build Status TODO: update to real jenkins URL -->
  <a href="https://travis-ci.org/mxstbr/react-boilerplate">
    <img src="https://travis-ci.org/mxstbr/react-boilerplate.svg" alt="Build Status" />
  </a>
  <!-- Test Coverage TODO: update to real coveralls URL -->
  <a href="https://coveralls.io/r/mxstbr/react-boilerplate">
    <img src="https://coveralls.io/repos/github/mxstbr/react-boilerplate/badge.svg" alt="Test Coverage" />
  </a>
</div>

# Frontend Technology Stack

### Navigation <small style="color: grey; font-weight: normal">(↓ marks in-document links)</small>
| **General** | **JavaScript** | **CSS** | **Testing** |
|----------|----------|----------|----------|
| ↓ [Tooling](#tooling) | ↓ [Javascript Overview](#javascript-overview) | ↓ [CSS & Styling Overview](#css-overview) |  ↓ [Testing Overview](#testing-overview) | [Reselect](./docs/js/reselect.md) |
| ↓ [Deployment](#deployment) | [ImmutableJS](./docs/js/immutablejs.md) | ↓ [Fonts](#fonts) | [Unit Testing](./docs/testing/unit-testing.md) | |
| ↓ [File Glossary](#file-glossary) |[Redux Saga](./docs/js/redux-saga.md) | [CSS Modules](./docs/css/css-modules.md) | [Component Testing](./docs/testing/component-testing.md) |
|| [Routing](./docs/js/routing.md) | [PostCSS](./docs/css/postcss.md) | [Redux Testing](./docs/testing/redux-testing.md) |
|| [i18n](./docs/js/i18n.md)

## JavaScript Overview

### View
Powered by [React ➝](https://facebook.github.io/react/). &nbsp;&nbsp;&nbsp; <small style="color: grey; font-weight: normal">(➝ marks external links)</small>

### Code Organization
We differentiate between **dumb**, reusable and ideally functional **stateless** `components` and **smart** **stateful** `containers`.
Read Dan Abramov's [explanation to this approach ➝](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0).

We organize code primarily **by feature**, not by functionality. That means we co-locate all component-specific code in the same folder. An example for a container component:

```
├ CoolContainer
├── index.js                    // the actual react component
├── actions.js                  // container-specific actions
├── constants.js                // container-specific constants
├── reducer.js                  // container-specific reducer
├── sagas.js                    // container-specific sagas
├── selectors.js                // container-specific selectors
├── styles.css                  // container-specific styles
└── tests/                      // container-specific tests
    ├—— index.test.js
    ├—— actions.test.js
    ├—— reducer.test.js
    ├—— sagas.test.js
    └── selectors.test.js
```

We do keep tests in a subfolder to make it easier to differentiate between `actions.js` and `actions.test.js` etc.

### State management / Reudx
We *manage* application state with [Redux ➝](http://redux.js.org), make it
*immutable* with [ImmutableJS ➝](https://facebook.github.io/immutable-js/), improve *performance*
with [reselect ➝](https://github.com/reactjs/reselect) and handle *side effects* like data fetching or login with [redux-saga ➝](http://yelouafi.github.io/redux-saga/).

If you haven't worked with Redux, it's highly recommended to read through the excellent [official documentation ➝](http://redux.js.org)
and/or watch these free [beginner ➝](https://egghead.io/series/getting-started-with-redux) and [advanced ➝](https://egghead.io/courses/building-react-applications-with-idiomatic-redux) video series by creator [Dan Abramov ➝](https://twitter.com/dan_abramov).

We have more detailed docs on [ImmutableJS](docs/js/immutablejs.md), [reselect](docs/js/reselect.md) and [redux-saga](docs/js/redux-saga.md).


### Routing
We use [react-router ➝](https://github.com/ReactTraining/react-router) for routing and keep the URL state synced to redux with [react-router-redux ➝](https://github.com/reactjs/react-router-redux).
See a short explanation and some code examples in the [routing doc](docs/js/routing.md).

### i18n / Internationalization
We use [react-intl ➝](https://github.com/yahoo/react-intl) for internationalization and pluralization. Read the [i18n doc](docs/js/i18n.md) for implementation details.

## CSS Overview

### Toolchain
| **Tool** | **Description** |
|----------|-----------------|
| [PostCSS ➝](http://postcss.org) | We use PostCSS for all our CSS transformation needs |
| [cssnext ➝](http://cssnext.io) | The latest CSS features without cross-browser pain |
| [CSSModules ➝](https://github.com/css-modules/css-modules) | Avoid global scope littering and keep styles local |
| [Autoprefixer ➝](https://github.com/postcss/autoprefixer) | Automagically™️ adds vendor prefixes |
| [sanitize.css ➝](https://jonathantneal.github.io/sanitize.css/) | A modern take on `normalize.css` |
| [cssnano ➝](http://cssnano.co) | CSS compressor/minifier/optimizer |
| [stylelint ➝](http://stylelint.io) | Catches bugs and keeps our styles consistent |


### Code Organization
* We write our styles as modular and composable as possible.
* No special `/css` directory.
* We co-locate component-specific styles with the actual component code.

Read further docs on [CSS Modules](./docs/css/css-modules.md) and [PostCSS](./docs/css/postcss.md).


## Fonts Overview
### Performant Web Font Loading

If you import web fonts naively, you'll either have blank page until
the fonts are downloaded or face an ugly FOUC (Flash of unstyled content). Both scenarios aren't ideal.

[FontFaceObserver ➝](https://github.com/bramstein/fontfaceobserver) adds a class
to the `body` when the fonts have loaded. (see [`app.js`](../../app/app.js#L26-L36)
and [`App/styles.css`](../../app/containers/App/styles.css))

### Adding a new font

1. Either add the `@font-face` declaration to `App/styles.css` or add a `<link>`
tag to the [`index.html`](../../app/index.html).

2. In `App/styles.css`, specify your initial `font-family` in the `body` tag
with only web-save fonts. In the `body.jsFontLoaded` tag, specify your
`font-family` stack with your web font.

3. In `app.js` add a `<fontName>Observer` for your font.


## Testing Overview

### Toolchain
| **Tool** | **Description** |
|----------|-----------------|
| [Karma ➝](https://karma-runner.github.io) | Test Runner |
| [Mocha ➝](https://mochajs.org) | Test Framework |
| [Expect ➝](https://github.com/mjackson/expect) | Assertion Library |
| [Enzyme ➝](http://airbnb.io/projects/enzyme/) | Shallow Component Rendering |
| [Sinon.JS ➝](http://sinonjs.org) | Spies / Stubs / Mocks |
| [Cheerio ➝](https://www.npmjs.com/package/cheerio) | Browserless DOM testing using Node |
| [Coveralls ➝](https://coveralls.io) | Test Coverage |
| [Snyk ➝](https://snyk.io) | Dependency Vulnerability Checker |

### Test Types
| **Type** | **Description** |
|----------|-----------------|
| [Unit Testing](docs/testing/unit-testing.md) | We use standard unit tests for all non-component code like actions, reducers, sagas, helpers, business logic etc. |
| [Component Testing](docs/testing/component-testing.md) | We use enzyme to test the correct rendering and behavior of our React components |
| Integration/E2E Testing | We consciously decide to start without e2e tests to keep overhead low. It will make sense to add later on when functionality is more well-defined and less likely to change often. |
| Remote Testing | We use [ngrok ➝](https://ngrok.com) to enable remote testers to access our local machine on demand.  |

## Deployment
TODO: Add details on deployment once we have a deployment target…


## Tooling

### npm commands
We have a [detailed doc](./docs/general/npm-commands.md) of available npm scripts with explanations.

### Babel
* ES6/7 support thanks to `babel-react`, `babel-preset-latest` and `babel-preset-stage-0`

### Webpack 2
* Bundling
* Native ES6 Modules
* Hot Module Reloading incl. state preservation
* Treeshaking
* [Webpack's image-loader ➝](https://github.com/tcoopman/image-webpack-loader) optimizes every PNG, JPEG, GIF and SVG image.
* Special images in HTML files

    If you specify your images in the `.html` files using the `<img>` tag, everything
    will work fine. The problem comes up if you try to include images using anything
    except that tag, like meta tags:

    ```HTML
    <meta property="og:image" content="img/yourimg.png" />
    ```

    The webpack `html-loader` does not recognise this as an image file and will not
    transfer the image to the build folder. To get webpack to transfer them, you
    have to import them with the file loader in your JavaScript somewhere, e.g.:

    ```JavaScript
    import 'file?name=[name].[ext]!../img/yourimg.png';
    ```

    Then webpack will correctly transfer the image to the build folder.

### Component Generators
We include a generator for components, containers, sagas, routes and selectors.
Run `npm run generate` to choose from the available generators, and automatically
add new parts of your application!

> Note: If you want to skip the generator selection process,
  `npm run generate <generator>` also works. (e.g. `npm run generate route`)

We use [Plop ➝](https://github.com/amwmedia/plop) to generate new components from predefined templates. You can find all the logic and templates for the generation in [internals/generators]()

### Devtools
[Redux devtools ➝](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd) should work out of the box.

### Server Configurations
For routing to work correctly, we'll need to set up some server configs.

##### Apache
This app includes a `.htaccess` file that does two things:
1. Redirect all traffic to HTTPS because ServiceWorker only works for encrypted
   traffic.
1. Rewrite all pages (e.g. `yourdomain.com/subpage`) to `yourdomain.com/index.html`
   to let `react-router` take care of presenting the correct page.

> Note: For performance reasons you should probably adapt it to run as a static
  `.conf` file (typically under `/etc/apache2/sites-enabled` or similar) so that
  your server doesn't have to apply its rules dynamically per request)

##### Nginx

Also it includes a `.nginx.conf` file that does the same on Nginx server.

### Offline-first
Availability without network connection is powered by a ServiceWorker with a fallback to AppCache for older browsers. All files are included automatically. No manual intervention needed thanks to [Webpack's Offline Plugin ➝](https://github.com/NekR/offline-plugin)

### Add To Homescreen
After repeat visits to the website, users will get a prompt to add the application to their homescreen. Combined with offline caching, this means our web app can be used exactly like a native application (without the limitations of an app store).

The name and icon to be displayed are set in the `app/manifest.json`.


## File Glossary
```
.
├── app                       # The application source code
│   ├── assets                # Fonts & images
│   ├── components            # „Dumb“ Components
│   ├── containers            # „Smart“ Containers
│   ├── tests                 # Global tests e.g. for store, rootReducer etc.
│   ├── translations          # [Generated] i18n locale files (`npm run extract-intl`)
│   └── utils                 # Global helpers and utility-functions like API-helpers etc.
├── dist                      # [Generated] Build output
├── docs                      # Documentation on specific technologies/frameworks etc.
├── internals                 # Tooling & Configuration
│   ├── generators            # Generator templates for new components, containers, routes etc.
│   ├── scripts               # Utility and tooling scripts like extract-i18n, pagespeed analysis etc.
│   ├── testing               # Test config for Karma etc.
│   └── webpack               # Webpack config
├── node_modules              # [Generated] NPM packages
├── .editorconfig             # Editor styleguide for all team members
├── .gitattributes            # Normalizes how git handles certain files
├── .gitignore                # Tells git which files to ignore
├── .snyk                     # snyk.io policy file
├── CHANGELOG                 # [Generated] Auto-generated CHANGELOG on version bump summarizing changes since the last version
├── package.json              # Lists project's dependencies. Configures babel/eslint/stylelint. Specifies npm tasks
└── README.md                 # This file
```
