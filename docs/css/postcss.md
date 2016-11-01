# PostCSS

[PostCSS ➝](https://github.com/postcss/postcss) is a modular CSS preprocessor based on JavaScript.

## Plugins

These are the PostCSS plugins we use:

| **Plugin** | **Description** |
|----------|----------|
| [postcss-focus ➝](https://github.com/postcss/postcss-focus) | Adds a `:focus` selector to every `:hover` selector for keyboard accessibility. |
| [autoprefixer ➝](https://github.com/postcss/autoprefixer) | Adds vendor prefixes for the last two versions of all major browsers and IE10+. |
| [cssnext ➝](https://github.com/moox/postcss-cssnext) | Transpiles CSS4 features down to CSS3. |
| [cssnano ➝](https://github.com/ben-eb/cssnano) | Optimizes & minifies your CSS file. |

Check out the comprehensive, fully-searchable catalog of available plugins at [postcss.parts ➝](http://postcss.parts)

## Adding a new PostCSS plugin

1. Add the plugin to your project (e.g. `yarn add --save-dev postcss-super-plugin`).
2. Modify `internals/webpack/webpack.dev.babel.js`:
   - Add `const postcssSuperPlugin = require('postcss-super-plugin')`
     to the `// PostCSS plugins` section.
   - Find `postcss: () => [/* ... current set of plugins ... */]` and add
     the new plugin to the list: `postcssPlugins: [/* ... */, postcssSuperPlugin()]`.
3. Restart the server (`CTRL+C`, `npm start`) for the new plugin to become available
  (webpack does not pick up config changes while running).

Before installing a new plugin, make sure that you are not trying to add a feature
that is already available. It is likely that what you are looking for
is already supported by [cssnext ➝](http://cssnext.io/features/)