# Webpack

## What was the intended use of ESmodules before module bundling came along?

The intended use of ES modules in JavaScript was to enable modular code structure in the language itself, without the need for external module bundlers. With ES modules, developers can import and export code between files, making it easier to manage and reuse code across a project.

Before module bundlers became popular, developers had to use different approaches to manage their code structure, such as using global variables, namespaces, or the CommonJS module system. However, these approaches had their own limitations and issues, such as global namespace pollution, naming conflicts, or poor performance.

ES modules were designed to address these issues and provide a standardized, built-in way to define and use modules in JavaScript. While module bundlers like Webpack or Rollup are still widely used, especially for larger and more complex projects, ES modules offer a simpler and more lightweight option for smaller projects or simple scripts.

## Why do we install 2 packages (**webpack** and **webpack-cli**) to use Webpack?

**webpack** and **webpack-cli** are two different packages that are commonly used together in a Webpack build setup.

**webpack** is the main Webpack library that is used to bundle your project's source files into a single file (or multiple files) for deployment. It provides the core functionality for processing your source files, including processing dependencies, transforming code, and creating the bundled output.

**webpack-cli**, on the other hand, is a command line interface (CLI) tool that provides a convenient way to run Webpack builds from the command line. The **webpack-cli** package provides the `webpack` command, which is used to run builds using Webpack; i.e., bundle the code along with carrying out any loading processes.

In order to run Webpack builds from the command line, you will need to have both **webpack** and **webpack-cli** installed in your project.



# Babel

Babel is a toolchain that is mainly used to convert ECMAScript 2015+ code into a backwards compatible version of JavaScript in current and older browsers or environments. 
It can do things like converting JSX to `react.createElement()` syntax in JS.

Refer the [Babel docs](https://babeljs.io/docs/en/) for more info.

`@babel/preset-env` figures out what the latest standards of browser support and the amount of backwards compatibility the code should have, based on usage statistics in the web. So, in short, it gives the babel transpiler some standards to transpile to.

## Demonstration of some babel transpilation taking place

Source code:
```javascript
const obj = {a: "alpha", b: "bravo"};
const newObj = {...obj, c: "charlie"}
```

This code uses the spread operator, which is not supported by most browsers at this time. So, if run 'babel-loader' on this that transpiles this code using the `@babel/core` module according to the guidelines provided by `@babel/preset-env` module.

Transpiled code (*production mode, but with multi-line formatting*):
```javascript
(() => {
    function r(t) {
        return r = "function" == typeof Symbol && "symbol" == typeof Symbol.iterator ? function(r) {
            return typeof r
        } : function(r) {
            return r && "function" == typeof Symbol && r.constructor === Symbol && r !== Symbol.prototype ? "symbol" : typeof r
        }, r(t)
    }

    function t(r, t) {
        var e = Object.keys(r);
        if (Object.getOwnPropertySymbols) {
            var n = Object.getOwnPropertySymbols(r);
            t && (n = n.filter((function(t) {
                return Object.getOwnPropertyDescriptor(r, t).enumerable
            }))), e.push.apply(e, n)
        }
        return e
    }

    function e(r) {
        for (var e = 1; e < arguments.length; e++) {
            var o = null != arguments[e] ? arguments[e] : {};
            e % 2 ? t(Object(o), !0).forEach((function(t) {
                n(r, t, o[t])
            })) : Object.getOwnPropertyDescriptors ? Object.defineProperties(r, Object.getOwnPropertyDescriptors(o)) : t(Object(o)).forEach((function(t) {
                Object.defineProperty(r, t, Object.getOwnPropertyDescriptor(o, t))
            }))
        }
        return r
    }

    function n(t, e, n) {
        return (e = function(t) {
            var e = function(t, e) {
                if ("object" !== r(t) || null === t) return t;
                var n = t[Symbol.toPrimitive];
                if (void 0 !== n) {
                    var o = n.call(t, "string");
                    if ("object" !== r(o)) return o;
                    throw new TypeError("@@toPrimitive must return a primitive value.")
                }
                return String(t)
            }(t);
            return "symbol" === r(e) ? e : String(e)
        }(e)) in t ? Object.defineProperty(t, e, {
            value: n,
            enumerable: !0,
            configurable: !0,
            writable: !0
        }) : t[e] = n, t
    }
    e(e({}, {
        a: "alpha",
        b: "bravo"
    }), {}, {
        c: "charlie"
    })
})();
```

Transpiled code (*development mode*):
```js
/*
 * ATTENTION: The "eval" devtool has been used (maybe by default in mode: "development").
 * This devtool is neither made for production nor for readable output files.
 * It uses "eval()" calls to create a separate source file in the browser devtools.
 * If you are trying to read the output file, select a different devtool (https://webpack.js.org/configuration/devtool/)
 * or disable the default devtool with "devtool: false".
 * If you are looking for production-ready output files, see mode: "production" (https://webpack.js.org/configuration/mode/).
 */
/******/ (() => { // webpackBootstrap
/******/ 	var __webpack_modules__ = ({

/***/ "./src/index.js":
/*!**********************!*\
  !*** ./src/index.js ***!
  \**********************/
/***/ (() => {

eval("function _typeof(obj) { \"@babel/helpers - typeof\"; return _typeof = \"function\" == typeof Symbol && \"symbol\" == typeof Symbol.iterator ? function (obj) { return typeof obj; } : function (obj) { return obj && \"function\" == typeof Symbol && obj.constructor === Symbol && obj !== Symbol.prototype ? \"symbol\" : typeof obj; }, _typeof(obj); }\nfunction ownKeys(object, enumerableOnly) { var keys = Object.keys(object); if (Object.getOwnPropertySymbols) { var symbols = Object.getOwnPropertySymbols(object); enumerableOnly && (symbols = symbols.filter(function (sym) { return Object.getOwnPropertyDescriptor(object, sym).enumerable; })), keys.push.apply(keys, symbols); } return keys; }\nfunction _objectSpread(target) { for (var i = 1; i < arguments.length; i++) { var source = null != arguments[i] ? arguments[i] : {}; i % 2 ? ownKeys(Object(source), !0).forEach(function (key) { _defineProperty(target, key, source[key]); }) : Object.getOwnPropertyDescriptors ? Object.defineProperties(target, Object.getOwnPropertyDescriptors(source)) : ownKeys(Object(source)).forEach(function (key) { Object.defineProperty(target, key, Object.getOwnPropertyDescriptor(source, key)); }); } return target; }\nfunction _defineProperty(obj, key, value) { key = _toPropertyKey(key); if (key in obj) { Object.defineProperty(obj, key, { value: value, enumerable: true, configurable: true, writable: true }); } else { obj[key] = value; } return obj; }\nfunction _toPropertyKey(arg) { var key = _toPrimitive(arg, \"string\"); return _typeof(key) === \"symbol\" ? key : String(key); }\nfunction _toPrimitive(input, hint) { if (_typeof(input) !== \"object\" || input === null) return input; var prim = input[Symbol.toPrimitive]; if (prim !== undefined) { var res = prim.call(input, hint || \"default\"); if (_typeof(res) !== \"object\") return res; throw new TypeError(\"@@toPrimitive must return a primitive value.\"); } return (hint === \"string\" ? String : Number)(input); }\n// import getCars from './getCars'\n\n// console.log(\"Inside index.js LOL\");\n\n// getCars();\n\nvar obj = {\n  a: \"alpha\",\n  b: \"bravo\"\n};\n// Using the spread operator, which is not supported by most browsers at this time\nvar newObj = _objectSpread(_objectSpread({}, obj), {}, {\n  c: \"charlie\"\n});\n\n//# sourceURL=webpack://firebase-testing/./src/index.js?");

/***/ })

/******/ 	});
/************************************************************************/
/******/ 	
/******/ 	// startup
/******/ 	// Load entry module and return exports
/******/ 	// This entry module can't be inlined because the eval devtool is used.
/******/ 	var __webpack_exports__ = {};
/******/ 	__webpack_modules__["./src/index.js"]();
/******/ 	
/******/ })()
;
```

> ***Note***: This is subject to change as the default guidelines for the minimum amount backwards compatibility changes in the `@babel/preset-env` module.


# Building

## Just resolving imports and exports

We can use ***Webpack*** with no separate setup, by just keeping an organized directory structure and file naming convention to prevent errors during bundling.

These include:
- Keeping the name of the js file that is intended to be our main entry point as `index.js` and placing it in the `src` folder in our root directory
- Keeping the import paths and locations of the rest of our files relative to this.

This is enough if we want to just resolve keep multiple js files, resolve imports between them during bundling and resultingly have a single js file in our production code.

## Making our bundled code compatible with older browsers (using babel-loader in webpack)

This is where Babel comes in because the bundled code generated by Webpack is not necessarily compatible with older browsers

## `development` mode for more readable bundled code

## Rebuilding automatically on save

As explained above, the **webpack-cli** package provides us with the `webpack` command used to run builds using Webpack.

It has a `--watch` flag which builds the existing files and then watches for any file changes. If any file changes occur, it rebuilds those specific files in this mode.

## Webpack `devServer` for hot reloading