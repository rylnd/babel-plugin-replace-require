<h1 align="center">babel-plugin-replace-require</h1>

<p align="center">
  <a href="https://raw.githubusercontent.com/FormidableLabs/babel-plugin-replace-require/master/LICENSE.txt">
    <img src='https://img.shields.io/badge/license-MIT-blue.svg?style=flat-square' />
  </a>
  <a href="https://badge.fury.io/js/babel-plugin-replace-require">
    <img src="https://badge.fury.io/js/babel-plugin-replace-require.svg" alt="npm version" height="18">
  </a>
  <a href='http://travis-ci.org/FormidableLabs/babel-plugin-replace-require'>
    <img src='https://secure.travis-ci.org/FormidableLabs/babel-plugin-replace-require.svg?branch=master' />
  </a>
</p>

<h4 align="center">
  Replace <code>require</code> output generated from <code>import</code> calls.
</h4>

***

There are often situations where you'd like to pass a different `require`
function into a `require("foo")` call like `specialOtherRequire("foo")`. This is
quite easy in CommonJS, yet challenging in ES-next `import`'s because the
outputted `require` is not directly under user control.

This plugin allows `import` statements to conditionally have the `require` call
rewritten in generated output.

## Installation

The plugin is available via [npm](https://www.npmjs.com/package/babel-plugin-replace-require):

```
$ npm install babel-plugin-replace-require
```

## Usage

Provide an object of token, code replacement string pairs. The code replacement
expressions are actually _parsed_ and inserted into the AST.

**.babelrc**: Our configuration

```js
{
  "plugins": [
    ["replace-require", {
      "GLOBAL_REQUIRE": "global.myBetterRequire",
      "REQUIRED_REQUIRE": "require('require-from-somewhere-else')"
    }]
  ]
}
```

**src/index.js**: A source file with es6 / Node.js type imports.

```js
// es6 style
import foo from "GLOBAL_REQUIRE/foo";

// CommonJS style
const foo = require("REQUIRED_REQUIRE/foo");
```

**lib/index.js**: The outputted file, processed by the plugin.

```js
// es6 style
var _foo = global.myBetterRequire("foo");

var _foo2 = _interopRequireDefault(_foo);

function _interopRequireDefault(obj) { return obj && obj.__esModule ? obj : { default: obj }; }

// CommonJS style
var bar = require('require-from-somewhere-else')("bar");
```

## Related Projects

### Builder

This plugin was written to help implement the
[module pattern][] in [`builder`](http://formidable.com/open-source/builder/)
archetypes for enabling dependency encapsulation.

### Webpack

This plugin is useful for code patterns that work in Node.js for alternate
`require`'s. If the code needs to run on the frontend via webpack, the
[`webpack-alternate-require-loader`](https://github.com/FormidableLabs/webpack-alternate-require-loader)
can further process the output of this plugin into fully-resolved modules
analogous to what Node.js would do.

## Contributions

Contributions welcome! See [CONTRIBUTING.md](CONTRIBUTING.md)

[module pattern]: https://github.com/FormidableLabs/builder#node-require-resolution-and-module-pattern
