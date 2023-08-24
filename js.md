---
layout: default
title: js
---


# Variables
- `var`
- `let`
- `const`
- `"use strict"`

The use case for a variable can be understood when an expression is evaluated.  Expressions return a result when they are evaluated.  If the result is not stored, it is discarded.  Therefore, variables are used to store the result for later use.

Variables are declared and initialised.

```js
let a;
let b = 10;
```

- The variable __a__ is declared but not initialised.  Calling __a__ returns `undefined`.
- The variable __b__ is declared and initialised with a value of 10.  Calling __b__ returns `10`.

`var` should be deprecated as `let` and `const` supersede its use case.  `var` is not block scoped, meaning that it can be accessed outside the block it is declared in.

```js
if (true) {
  var x = 5;
}
console.log(x); // x is 5

if (true) {
  let y = 5;
}
console.log(y); //  ReferenceError: y is not defined
```

`let` is block scoped.  The variable can be declared without being initialised.  The variable is 'dynamically-typed' meaning that the data type doesn't need to be specified upon declaration. `let` allows for reassignment.

```js
let a;
let b = 10;
c = 20; // reassignment
```

`const` is block scoped.  The variable must be declared and initialised as `const` does not allow for reassignment.

```js
const a; // SyntaxError: Missing initialiser in const declaration
const b = 10;
b = 20; // TypeError: Assignment to constant variable
```

`const` cannot be redeclared but can be mutated.
```js
const arr = [];
arr.push(1);
arr.push(2);
console.log(arr); // [1, 2]
```

`'use strict'` prevents variables from being declared in your IDE without the keywords `let` and `const`.  This means that variables can't accidentally be declared in the global scope
```js
'use strict';
words = "the quick brown fox"; // IDE complains about the implicit declaration of words
let sentence = "over the lazy dog";
```

Readings
- [variable declarations](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types#declarations)
- [strict mode](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode)

# Functions
To define function scope... let __aFunction__ exist:

```js
// global scope "space"

function aFunction() {
    // function body "space" of aFunction
}
```

- `aFunction`'s function scope is equivalent to the function body "space".  We may define this scope as `s`.
- The scope which `aFunction` exists is the global scope "space".  We may define this scope as `s-1`, where `-1` means outer or "surfacing" by 1 unit.
- `aFunction` can access variables in the `s` space.
- `aFunction` can access variables in the `s-1` space.

---

To define function scope... let __aFunction__ and __bFunction__ exist:

```js
// global scope "space"

function aFunction() {
    // function body "space" of aFunction
    function bFunction() {
        // function body "space" of bFunction
    }
}
```


Proposition:
- `aFunction`'s function scope is equivalent to function body "space" of `aFunction`.  We define this scope as `s`
- In the context of `aFunction`, the global scope "space" is `s-1`.
- In the context of `aFunction`, the function body "space" of `bFunction` is `s+1`, where `+1` means inner or "deeper" by 1 unit.
- `aFunction` can access variables in the`s` and `s-1` space.
- `aFunction` cannot access variables in the `s+1` space.

Contrast:
- `bFunction`'s function scope is equivalent to function body "space" of `bFunction`.  We may define this scope as `s`.
- In the context of `bFunction`, `aFunction`'s scope is `s-1`.
- In the context of `bFunction`, the global scope is `s-1 -1`, or `s-2`.
- `bFunction` can access variables in the `s-1` and `s-2` space.

Terms:
- In the context of a function's scope being `s`, the function may access variables in the `s` or `s-1` or `s-n` scope, where `n` is an integer.
- However, the function cannot access variables in any `s+1` or `s+n` scopes.
- `aFunction`'s inability to access variables in the `bFunction` scope, while `bFunction` is able to access variables in the `aFunction` scope is the basis of a closure.
- Note: if variables are named the same in multiple scopes, then the function will first look for the variable at the scope closest to `s` before moving to `s-n`

Readings:
- [function scope](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions#function_scope)
- [nested functions and closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions#nested_functions_and_closures)

# Functions
- define functions either as a function declaration or function expression
- passing different data types as arguments into functions: strings, numbers, arrays, objects, functions
- function expression use case: named function expression ("I'll keep this one for later")
- function expression use case: anonymous function ("function on the fly, forever unknown")
- function expression use case: immediate invocation ("ka-me-ha-meee-HA!", immediately invoked function expression)
- shorthand function expressions: arrow syntax ("gotta go fast, haha chilli dogs")
- shorthand arrow function expressions behave differently to long-form function expressions
- callback functions
- conditional function declaration based on if statement and function expression
- methods are functions that belong to an object
- program execution sequence: function definition -> function call -> function execution
- a call must be in the same scope as the function to succeed
- function hoisting, thesis: the call to a function may be evaluated before its function declaration
- function hoisting, contrast: function expressions must always be evaluated first before a call is made
- when a function is called, arguments are passed
- when a function is declared or expressed, parameters are defined

Readings:
- [functions (guide)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions)
- [functions (reference)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions)
- [function declaration](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function)
- [function expression](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/function)
- [shorthand function expressions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
- [callback functions](https://developer.mozilla.org/en-US/docs/Glossary/Callback_function)

# Understanding .bind
- The below code block demonstrates how `this` changes inside a constructor function of a __class__ and a __callback function__.
- The statement on line 15 will affect how line 20 is parsed.
- Without `.bind(this)`, the `this` in `#removeBook` refers to the event that was triggered when clicking `#button`.
- With `.bind(this)`, the `this` in `#removeBook` refers to the `LibraryController` instance.
```js
class LibraryController {
    #library = []
    #books = [
        new Book(),
        new Book(),
        new Book(),
    ]
    #button = document.querySelector('button')
    
    constructor() {
        for (const book of #books) {
            this.#library.push(book)
        }
        
        this.#button.addEventListener('click', this.#removeBook.bind(this))
    }
    
    #removeBook() {
        // logic here to get the index of the book to remove from the #this.library model
        this.#library.splice(index, 1)
        // logic here to remove the node object from the GUI display
    }
}
```