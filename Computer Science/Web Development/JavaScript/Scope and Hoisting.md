---
tags: [javascript, fundamentals, web-development, computer-science, placement-prep]
aliases: [Hoisting, Temporal Dead Zone, Lexical Scope]
created: 2026-08-08
---

# Scope and Hoisting

## Types of Scope

1. **Global scope** — accessible everywhere.
2. **Function scope** — variables declared with `var` are visible throughout the entire function.
3. **Block scope** — variables declared with `let`/`const` are visible only within the nearest `{ }`.
4. **Lexical scope** — a function's access to variables is determined by *where it is written* in the code, not where it's called from.

```javascript
function outer() {
  let x = 10;
  function inner() {
    console.log(x); // 10 — inner "sees" outer's variables lexically
  }
  inner();
}
```

## Hoisting

JavaScript moves **declarations** (not initializations) to the top of their scope during the compile phase.

```javascript
console.log(fn());  // "Hi!" — function declarations are fully hoisted
function fn() { return "Hi!"; }

console.log(varHoist); // undefined — declaration hoisted, value not
var varHoist = 5;

console.log(letHoist); // ReferenceError — Temporal Dead Zone
let letHoist = 5;

console.log(fnExpr()); // TypeError — fnExpr is undefined at this point
var fnExpr = function() { return "Hi"; };
```

### Temporal Dead Zone (TDZ)

`let` and `const` ARE hoisted, but they stay in an uninitialized "dead zone" from the start of the block until their declaration line is executed. Accessing them there throws a `ReferenceError`.

## Function Declaration vs Expression vs Arrow Function Hoisting

| Type | Hoisted? | Callable before definition? |
|---|---|---|
| `function foo(){}` | Fully hoisted (name + body) | Yes |
| `var foo = function(){}` | Only `var foo` hoisted (as `undefined`) | No — TypeError |
| `let foo = function(){}` | In TDZ | No — ReferenceError |
| `const foo = () => {}` | In TDZ | No — ReferenceError |

## Classic Closure + Loop Hoisting Trap

```javascript
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 0);
}
// 3, 3, 3 — var is function-scoped, all callbacks share the same i

for (let j = 0; j < 3; j++) {
  setTimeout(() => console.log(j), 0);
}
// 0, 1, 2 — let creates a new binding per iteration
```

This is one of the **most asked placement interview questions** — see [[Closures]] for the full explanation.

## Key Takeaways

- Hoisting moves declarations, not assignments, to the top of scope.
- `var` → hoisted + initialized to `undefined`. `let`/`const` → hoisted but in TDZ until declared.
- Function declarations are fully hoisted (safe to call before defined); function expressions/arrow functions are not.
- The `var` vs `let` loop question is extremely common — know it cold.

## Related Concepts
- [[Closures]] — why the loop + `var` trap happens
- [[Variables and Data Types]] — `var`/`let`/`const` comparison table
- [[Functions in JavaScript]] — function declarations vs expressions
- [[JS Interview Questions and Tricky Outputs]]
