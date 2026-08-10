---
tags: [javascript, fundamentals, web-development, computer-science, placement-prep]
aliases: [Arrow Functions, IIFE, Higher Order Functions, Callbacks]
created: 2026-08-08
---

# Functions in JavaScript

## Declarations vs Expressions vs Arrow Functions

```javascript
// Function Declaration — hoisted fully
function add(a, b) { return a + b; }

// Function Expression — not hoisted (see [[Scope and Hoisting]])
const subtract = function (a, b) { return a - b; };

// Arrow Function (ES6) — concise, lexical `this`, no own `arguments`
const multiply = (a, b) => a * b;
const square = x => x * x;          // single param, no parens needed
const sayHi = () => console.log("Hi"); // no params
const makeObj = (a, b) => ({ a, b }); // must wrap object literal in ( )
```

### Arrow Function vs Regular Function — Key Differences

| Feature | Regular Function | Arrow Function |
|---|---|---|
| `this` | Dynamic (see [[this Keyword]]) | Lexical (inherited) |
| `arguments` object | Yes | No (use rest params instead) |
| Can be used as constructor (`new`) | Yes | No — throws TypeError |
| Implicit return | No | Yes, for single expressions |
| Hoisting | Fully hoisted if declaration | Not hoisted (TDZ if `const`/`let`) |

## Default & Rest Parameters

```javascript
function greet(name = "Guest") { console.log(`Hi ${name}`); }
greet(); // "Hi Guest"

function sum(...nums) {          // rest parameter collects remaining args into an array
  return nums.reduce((a, b) => a + b, 0);
}
sum(1, 2, 3, 4); // 10
```

## IIFE (Immediately Invoked Function Expression)

Runs once, immediately, and creates its own scope — historically used to avoid polluting global scope (before `let`/modules existed).

```javascript
(function () {
  console.log("Runs immediately!");
})();

(() => {
  console.log("Arrow IIFE works too");
})();
```

## Higher-Order Functions

A function that takes another function as an argument, returns a function, or both.

```javascript
function withLogging(fn) {
  return function (...args) {
    console.log("Calling with", args);
    return fn(...args);
  };
}
const loggedAdd = withLogging((a, b) => a + b);
loggedAdd(2, 3); // logs "Calling with [2, 3]", returns 5
```
`map`, `filter`, `reduce` (see [[Array Methods]]) are the most common built-in higher-order functions.

## Callbacks

A function passed into another function to be executed later.

```javascript
function fetchData(callback) {
  setTimeout(() => callback("data loaded"), 1000);
}
fetchData((result) => console.log(result));
```
Overuse of nested callbacks leads to **"callback hell"** — solved by Promises/async-await, see [[Asynchronous JavaScript]].

## Pure Functions

A function is **pure** if: (1) same input always gives the same output, (2) no side effects (doesn't modify external state).

```javascript
// Pure
function add(a, b) { return a + b; }

// Impure — depends on/modifies external state
let total = 0;
function addToTotal(x) { total += x; return total; }
```
Pure functions are easier to test, debug, and reason about — a common "why does it matter" interview question.

## Function.length and arguments

```javascript
function example(a, b, c) {}
example.length; // 3 — number of declared (non-rest, non-default) params

function old() {
  console.log(arguments); // Arguments object — array-like, NOT a real array
  console.log(arguments.length);
}
```

## Key Takeaways

- Function declarations are hoisted; expressions and arrow functions are not.
- Arrow functions have no own `this`/`arguments` and can't be constructors — great for callbacks, bad for object methods needing `this`.
- Higher-order functions (map/filter/reduce/custom wrappers) are central to functional-style JS.
- Prefer pure functions where possible for predictability and testability.

## Related Concepts
- [[this Keyword]] — biggest reason to choose arrow vs regular function
- [[Closures]] — function factories and IIFEs
- [[Array Methods]] — built-in higher-order functions
- [[Asynchronous JavaScript]] — callbacks evolve into Promises/async-await
- [[Common Coding Patterns]] — currying, debounce, throttle built from these concepts
