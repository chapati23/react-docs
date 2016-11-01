# npm Commands

## Server

### Development

```Shell
npm start
```

Starts the development server at `localhost:3000`. Changes in the application code will be hot-reloaded.

```Shell
npm run start:tunnel
```
Starts the development server and tunnels it with `ngrok`, making the website
available to the public internet. Useful for testing on different devices in different locations.

### Production

```Shell
npm run start:prod
```

Starts the production server, configured for optimal performance: assets are
minified and served gzipped.

### Change Port

```Shell
npm start -- --port 5000
```


## Build

```Shell
npm run build
```

Prepares your app for deployment. Optimizes and minifies all files, treeshakes the JavaScript piping them to a folder called `build`. Upload the contents of `build` to your web server to see your work live!

## Clean

```Shell
npm run clean:all
```

Removes `./build`, `./stats.json` and `./coverage`


## Generators

```Shell
npm run generate
```

Allows you to auto-generate boilerplate code for common parts of the
application, specifically `component`s, `container`s, and `route`s. You can
also run `npm run generate <part>` to skip the first selection. (e.g. `npm run
generate container`)


## Testing

```Shell
npm test
```

Tests your application with the unit tests specified in the `*.test.js` files
throughout the application.   All the `test` commands allow an optional `-- --grep string` argument to filter
the tests ran by Karma. Useful if you need to run a specific test only.

```Shell
# Run only the Button component tests
npm run test:watch -- --grep Button
```

### Browsers

To choose the browser to run your unit tests in (Chrome by default), run one of
the following commands:

#### Firefox

```Shell
npm run test:firefox
```

#### Safari

```Shell
npm run test:safari
```

#### Internet Explorer

*Windows only!*

```Shell
npm run test:ie
```

### Watching

```Shell
npm run test:watch
```

Watches changes to your application and reruns tests whenever a file changes.

### Performance testing

```Shell
npm run pagespeed
```

With the remote server running (i.e. while `npm run start:prod` is running in
another terminal session), enter this command to run Google PageSpeed Insights
and get a performance check right in your terminal!

### Dependency size test

```Shell
npm run analyze
```

This command will generate a `stats.json` file from your production build, which
you can upload to the [webpack analyzer](https://webpack.github.io/analyse/). This
analyzer will visualize your dependencies and chunks with detailed statistics
about the bundle size.

## Linting

```Shell
npm run lint
```

Lints your JavaScript and CSS.

##### Why not let webpack auto-lint on filesave, you ask?
We've found that this can degrade the build performance quite a bit. "Live linting" should be a concern of the IDE so setting this up is the individual developers responsibility. Lint does run on `precommit` so there is a safety net.

### JavaScript

```Shell
npm run lint:js
```

Only lints your JavaScript.

### CSS

```Shell
npm run lint:css
```

Only lints your CSS.
