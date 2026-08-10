---
tags: [javascript, interview-prep, web-development, computer-science, placement-prep, interview-favorite]
aliases: [JS Interview Questions, Placement Questions JS, Predict the Output]
created: 2026-08-09
---

# JS Interview Questions & Tricky Outputs

Highest-frequency JavaScript questions across product-based and service-based company interviews. Each links to the full explanation note — use this page as a **rapid-recall drill list** before an interview.

## ⭐ Most-Asked (High Priority)

**Q: Explain closures with a real example.**
A: A function that retains access to its outer scope's variables even after the outer function has returned. Classic example: a counter factory. → [[Closures]]

**Q: `var` vs `let` vs `const` — explain scope and hoisting differences.**
A: Function vs block scope, Temporal Dead Zone behavior, reassignment rules. → [[Scope and Hoisting]], [[Variables and Data Types]]

**Q: Explain the event loop / predict `console.log` output order with `setTimeout` + `Promise`.**
A: Sync code → microtasks (Promises) → macrotasks (`setTimeout`). → [[Event Loop]]

**Q: What is the difference between `==` and `===`?**
A: `==` coerces types before comparing; `===` checks type and value strictly, no coercion. → [[Operators and Type Coercion]]

**Q: Explain `this` in different contexts (global, method, arrow function, event handler).**
A: Depends entirely on **how** a function is called — except arrow functions, which use lexical `this`. → [[this Keyword]]

**Q: What is currying? Write a curry function.**
A: Transforms `f(a,b,c)` into `f(a)(b)(c)`. → [[Common Coding Patterns]]

**Q: Implement debounce / throttle from scratch.**
A: Extremely common coding-round question. → [[Common Coding Patterns]]

**Q: `Promise.all` vs `Promise.allSettled` vs `Promise.race` vs `Promise.any`?**
A: Fail-fast (`all`/`race`) vs wait-for-all (`allSettled`) vs first-success (`any`). → [[Asynchronous JavaScript]]

**Q: What is prototypal inheritance? How is it different from classical inheritance?**
A: Objects inherit directly from other objects via a prototype chain, rather than from a class blueprint — though ES6 `class` provides class-like syntax over the same system. → [[Prototypes and Inheritance]]

**Q: Difference between `call`, `apply`, and `bind`.**
A: `call`/`apply` invoke immediately (args as list vs array); `bind` returns a new function with `this` permanently bound. → [[this Keyword]]

**Q: Explain the difference between synchronous and asynchronous code, and why JS needs the event loop despite being single-threaded.**
A: JS executes one thing at a time on the call stack, but delegates slow work (timers, network, I/O) to browser/Node APIs, whose callbacks are queued and run later. → [[Event Loop]]

**Q: What is hoisting, and how does it differ for `var`, `let`/`const`, and function declarations?**
A: All are hoisted, but `var` initializes to `undefined`, `let`/`const` stay in the TDZ until their line runs, and function declarations are hoisted with their full body. → [[Scope and Hoisting]]

**Q: How does `async`/`await` relate to Promises under the hood?**
A: `async`/`await` is syntactic sugar over Promises — `await` pauses execution of the `async` function until the Promise settles, without blocking the main thread. → [[Asynchronous JavaScript]]

## Medium Priority

**Q: What is a memory leak in JS and how do you prevent it?**
A: Unreleased references (timers, listeners, closures holding large data) that prevent garbage collection. → [[Error Handling and Memory]]

**Q: Explain event delegation and why it's useful.**
A: Attach a single listener to a parent and use `e.target` to identify the actual clicked child — better performance, works with dynamically added elements. → [[DOM and Events]]

**Q: What are pure functions? Why do they matter?**
A: Same input always produces the same output, no side effects — predictable, testable, safe for optimizations like memoization. → [[Functions in JavaScript]]

**Q: What is the output of `typeof null` and why?**
A: `"object"` — a long-standing JS bug from the original implementation, kept for backward compatibility. → [[Variables and Data Types]]

**Q: Explain shallow copy vs deep copy.**
A: Shallow copy (spread, `Object.assign`) copies only the first level — nested objects are still shared by reference. Deep copy (`structuredClone`, JSON methods, or recursion) fully duplicates nested structures. → [[Objects Destructuring Spread Rest]]

**Q: What's the difference between `null` and `undefined`?**
A: `undefined` = variable declared but not assigned a value (or a missing property). `null` = explicitly assigned "no value" by the developer. → [[Variables and Data Types]]

## Lower Priority

**Q: Difference between synchronous `forEach` and using `map`/`filter`/`reduce` for transforming data?**
A: `forEach` has no return value and is used purely for side effects; `map`/`filter`/`reduce` return new data and support functional-style chaining. → [[Array Methods]]

## Type Coercion Quick-Recall Table

| Expression | Result |
|---|---|
| `1 + '1'` | `'11'` (number → string) |
| `'5' - 1` | `4` (string → number) |
| `true + true` | `2` |
| `[] + []` | `''` |
| `[] + {}` | `'[object Object]'` |
| `[] == false` | `true` |
| `null == undefined` | `true` |
| `null === undefined` | `false` |
| `NaN == NaN` | `false` |

## Predict-the-Output Practice (self-test before reading the answer)

```javascript
// 1.
for (var i = 0; i < 3; i++) { setTimeout(() => console.log(i), 0); }
// Answer: 3 3 3  → see [[Closures]]

// 2.
console.log("1");
setTimeout(() => console.log("2"), 0);
Promise.resolve().then(() => console.log("3"));
console.log("4");
// Answer: 1 4 3 2 → see [[Event Loop]]

// 3.
console.log(typeof null);
console.log([10, 2, 33].sort());
// Answer: "object" , [10, 2, 33] (string sort — WRONG for numbers without a comparator)
// → see [[Variables and Data Types]], [[Array Methods]]

// 4.
const obj = { name: "A", greet() { console.log(this.name); } };
const fn = obj.greet;
fn();
// Answer: undefined — `this` lost on plain call → see [[this Keyword]]
```

## Key Takeaways

- Interviewers reuse the same ~15-20 question patterns constantly — the closure-in-loop bug, the event-loop trace, `this`-losing-context, and `debounce`/`throttle` implementation are near-guaranteed.
- Being able to both **explain** and **live-code** closures, currying, debounce/throttle, and a Promise combinator comparison covers most JS interview rounds.
- Always practice tracing output order for mixed `sync`/`Promise`/`setTimeout` code — it's the single most common "gotcha" format.

## Related Concepts
All topic notes above — this page is the index/drill-sheet for [[JavaScript MOC]].
