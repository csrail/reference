---
layout: default
title: ide
---

# Vim

`:help` opens the vim reference manual
`/` to begin searching bookmarks in the `:pattern`
- `<leader>` key is mapped to backslash by default, and can be configured by changing the `mapleader` variable
- `:help leader` for more info

- `:help plugin` and `:help packages` for documentation on setting up plugins
- `~/.vim/plugin/` root plugin directory, place subdirectories of the plugins needed in here

- `CTRL+W` and `ijkl` direct key to change window focus

- Set up __vim-plug__
- `~/.vim/autload` directory which starts plugins on vim loading.  `plug.vim` file is placed here

- `~/.vim/plugged` directory where plugins specified in `.vimrc` file are installed following `:PlugInstall`
- Configure `.vimrc` file with header `call plug#begin()` and footer `call plug#end()`, placing wanted plugins in this block with the notation `Plug 'author/plugin-name'`

```vim
 call plug#begin()
 
  
 Plug 'tpope/vim-surround'
 Plug 'easymotion/vim-easymotion'
  
 call plug#end()
```

- __vim-easy-motion__
- `:help easymotion.txt` to open help documentation
- Example config which reduces the leader key usage by 50%, from 2 to 1 keypresses:

```vim
" <Leader>f{char} to move to {char}
map  <Leader>f <Plug>(easymotion-bd-f)
nmap <Leader>f <Plug>(easymotion-overwin-f)
```

- In Pycharm you need to install IdeaVim-EasyMotion plugin which also installs AceJump.
- The below configuration cannot work with the above configuration, there's a bug which prevents the interface from parsing correctly; would need to study the source to understand the bug before fixing.
```vim
map <Leader> <Plug>(easymotion-prefix)
```


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
    watch: true- `F4`, jumps to target's source.  For javascript the source is the returned function for public use from a factory function.  Pycharm seems unable to go a step further and look for the named function expression the symbol belongs to.,
};
```

Readings:
- [npm init](https://nodesource.com/blog/an-absolute-beginners-guide-to-using-npm/)
- [webpack concepts](https://webpack.js.org/concepts/)
- [webpack devtool](https://webpack.js.org/configuration/devtool/)
- [webpack guide: development, webpack watch](https://webpack.js.org/guides/development/)

# Vim
- `caw`, change a word
- `cs`, change surrounding quotes or brackets
  - `cs([`
- `ci`, change inner contents of quotes or brackets
  - `ci"`
  - `ci(`
- `<ESC>` move into Normal mode
- `O` open new line above cursor
- `o` open new line below cursor
- `gg`, go to first line
- `GG`, go to last line
- `10gg`, go to 10th line
- `10G`, go to 10th line
- `d`, delete operator
- `dd`, delete entire line
- `w`, motion to start of next word, excludes first character
- `e`, motion to end of current word, includes last character
- `$`, motion to end of line, includes last character
- `u`, undo command
- `CTRL-R`, redo command (conflicts with Pycharm default find and replace)
- `/`, search command, search for a phrase then press `<ENTER>`; remembers phrase even when returning from insert mode to normal mode so you can press `n` and `N`.
- `n`, when phrase is searched, press this to find the next entry
- `N`, when phrase is searched, press this to find previous entry (inverse next)
- `%`, find matching parentheses, place cursor on parentheses to use

# Pycharm
- `Ctrl + Shift + n`, navigate files in your directory tree with keypresses.
- `Ctrl + Alt + Shift + n` navigate to symbols; need to use this to search for factory functions as they don't count as classes.
- `Shift + Alt + 7`, finds the target's usages in all places.  A comprehensive lookup which addresses the shortcomings of `F4`.
- `F4`, jumps to target's source.  For javascript the source is the returned function for public use from a factory function.  Pycharm seems unable to go a step further and look for the named function expression the symbol belongs to.
- `Ctrl+B`, go to declaration or usages.
- 