---
layout: default
title: js
---
# Functions
Essay Topic: "Thinking in patterns, forests not trees" --- compare and contrast objects produced with __classes__ and objects produced by __factory functions__.

Using a high-level programming language, we can logically move from completing operations with primitive operands towards communication between different objects.  This essay attempts to clarify and bridge this gap between these two seemingly unrelated patterns.

Completing simple operations is usually straight-forward, returning a result with low or no side effects: a __simple construct__.  Consider the statement `a + b = c` which we would write manually from left to right in sequence. The operation `a + b`, (with operands _a_ and _b_, and the _+_ operator) is executed `=`, returns a result `c`, with no side effects.  In a program, the execution is done at runtime, so the script would be simply written as `a + b`, the program is run which is equivalent to `=`, and the program returns the result `c`.  As implicitly instructed, the program immediately discards the result.

Variables solve this futile expenditure by allowing the program to store the result in memory for the duration that the program is running.  Variable declarations are interesting because they improve upon the simple construct mentioned prior to provide new capacities.  Variable declarations themselves, like `d = c`, are operations with operands, operators and a side effect.  The statement `d = c` _is_ the operation (and this expression cannot be reduced any further without affecting what the statement states, for `d` alone is not an operation, neither is `=` alone, etc.)  The assignment `=` operator works on the operands `d` and `c`, where `d` is a reference and `c` is a value.  (The word 'reference' is used here in place of 'symbol' since the entire expression is already using three symbols `d`, `=`, and `c`; using 'symbol' would confuse rather than clarify.)  Once the assignment is made, calling `d` reproduces the value of `c`, with what we know of `c` already being discarded.

There are now two patterns we know of: the simple operation construct, and variable declarations, which is an extension of the simple operation construct.  Instead of moving towards variables, we can extend the simple operation construct towards a different direction, a __function__.

A function `f(x, y) {...}` extends a simple operation by creating a reference to it.  The reference is an enabler of different degrees.  The first advantage conferred, is that the reference defines the operation once, allowing the reference to be called or invoked `n` times to run the operation `n` times.  The second advantage conferred, is that the reference only knows to operate on the operands `x` and `y`, more specifically, a reference to `x` and `y` parameters, not the values of `x` and `y`; the function is agnostic to its input arguments.  The third advantage conferred is that the reference can be stated without being invoked, of which its use case will contribute later to more complex constructs in the form of a callback.

```js
// function declaration
// f is a reference to return x + y
function f(x, y) {
    return x + y
}

// function stated
f;

// function invoked
f();
```

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