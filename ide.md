---
layout: default
title: ide
---

# npm, Webpack

The `npm init` command initialises a new project in the current folder, in other words, this folder is now the root directory of the project.  A __package.json__ file is created in this space which functions as a config file for the project during development builds and in production builds.

Within the `scripts` node, you can specify special keywords to use in the command `npm run keyword`.  T: Is it possible to run `webpack --watch` outside of `npm run watch`?

There are also specifications for dependencies in the developer environment `devDevependencies`, and in the production environment `Dependencies` nodes.  When running `npm install <package> --save-dev`, this adds an entry to the `devDependencies`; while `npm install <package> --save` adds an entry into `Dependencies`.  Packages are locally installed, and can be inspected, in the __node_modules__ folder.

```json
{
  "name": "restaurant",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "watch": "webpack --watch",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^5.88.2",
    "webpack-cli": "^5.1.4"
  }
}
```

Webpack, _the bundler of all web assets_.  Webpack will take multiple files, process them and output them into files, of fewer numer and smaller size, for application consumption.  Webpack must interact with multiple file types but natively only reads javascript and json, so separate __loaders__ are specified to process the different file types --- this is represented under the `module` node.

Initial configuration of webpack.config.js handles the `entry` and `output`.  The `entry` node specifies the source files that are fed into webpack for processing.  While `output` specifies the destination file that webpack produces for distribution.  This is implicitly stated via the `src` and `dist` folders respectively.

The `devtool` node specifies the type of source mapping available.  Post-processing, the initial source code is transcompiled by webpack into the a different kind of source, one that begets performance at the cost of readability.  Source mapping relates the two source codes, with increased readability in the source map increasing build times.  Hence, the `devtool` node wil take many different values each with their different tradeoffs on build performance and the corresponding source map readability.

The `watch` node of webpack.config.js in conjunction the `"scripts": "watch":` node in package.json allows you to run `npm run watch`.

The `clean` node can help remove un-needed files in a project following a build, but this must be configured further to avoid stale, but absolutely necessary` files from being cleaned up.  Oopsies!

Loaders are specified in the `module` -> `rules` node.  Each rule has a `test` node and a `use` node.  The `test` looks for files described via regular expression, and processes them in the loaders specified under `use`.  Multiple loaders can be used to process the files in a sequence.

By using the `plugins` node, webpack's capabilities can be extended further.

```js
// webpack.config.js
const path = require('path');

module.exports = {
    entry: './src/index.js',
    output: {
        filename: 'main.js',
        path: path.resolve(__dirname, 'dist'),
        // clean: true,
    },
    devtool: 'eval',
    watch: true,
};
```

Readings:
- [npm init](https://nodesource.com/blog/an-absolute-beginners-guide-to-using-npm/)
- [webpack concepts](https://webpack.js.org/concepts/)
- [webpack devtool](https://webpack.js.org/configuration/devtool/)
- [webpack guide: development, webpack watch](https://webpack.js.org/guides/development/)

# Vim
- `cs`, change surrounding quotes or brackets
  - `cs([`
- `ci`, change inner contents of quotes or brackets
  - `ci"`
  - `ci(`

# Pycharm
- `Ctrl + Shift + n`, navigate files in your directory tree with keypresses.