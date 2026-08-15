---
tags: [javascript, fundamentals, web-development, computer-science, placement-prep, interview-favorite]
aliases: [Hoisting, Temporal Dead Zone, Lexical Scope, TDZ]
created: 2026-08-08
updated: 2026-08-14
---

# Scope and Hoisting

## Types of Scope

1. **Global scope** — Accessible everywhere in the application.
2. **Function scope** — Variables declared with `var` are visible throughout the entire function body.
3. **Block scope** — Variables declared with `let`/`const` are visible only within the nearest `{ }`.
4. **Lexical scope** — A function's access to variables is determined by *where it is written* in source code, not where it is invoked.

---

## 🖼️ JavaScript Scope Hierarchy & Hoisting (TDZ)

![[js-scope-hoisting.svg|960]]

---

## Lexical Scope Example

```javascript
function outer() {
  let x = 10;
  function inner() {
    console.log(x); // 10 — inner "sees" outer's variables lexically
  }
  inner();
}
```

---

## Hoisting & The Temporal Dead Zone (TDZ)

JavaScript moves **declarations** (not initializations) to the top of their scope during the compilation phase:

```javascript
console.log(fn());  // "Hi!" — function declarations are fully hoisted (name + body)
function fn() { return "Hi!"; }

console.log(varHoist); // undefined — declaration hoisted, initialized to undefined
var varHoist = 5;

console.log(letHoist); // ReferenceError — Temporal Dead Zone (TDZ)
let letHoist = 5;

console.log(fnExpr()); // TypeError: fnExpr is not a function (fnExpr is undefined)
var fnExpr = function() { return "Hi"; };
```

---

## Declaration vs Expression Hoisting Comparison

| Syntax Type | Hoisted? | Callable Before Definition? | Error If Called Early |
| :--- | :--- | :---: | :--- |
| `function foo() {}` | Fully hoisted (name + body) | ✅ Yes | None (Executes normally) |
| `var foo = function() {}` | Declaration hoisted as `undefined` | ❌ No | `TypeError: foo is not a function` |
| `let foo = function() {}` | Hoisted but uninitialized (in TDZ) | ❌ No | `ReferenceError: Cannot access 'foo'` |
| `const foo = () => {}` | Hoisted but uninitialized (in TDZ) | ❌ No | `ReferenceError: Cannot access 'foo'` |

---

## Classic `var` vs `let` Loop Trap

```javascript
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 0);
}
// Output: 3, 3, 3 (Single shared function-scoped variable)

for (let j = 0; j < 3; j++) {
  setTimeout(() => console.log(j), 0);
}
// Output: 0, 1, 2 (Fresh block-scoped binding per iteration)
```

---

## 🔗 Related Concepts
- [[Closures]] — Preserving variables across lexical scopes
- [[Variables and Data Types]] — In-depth `var`, `let`, `const` comparison
- [[Functions in JavaScript]] — Function declarations vs. arrow functions
- [[JS Interview Questions and Tricky Outputs]] — Tricky output prediction
