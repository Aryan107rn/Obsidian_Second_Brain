---
tags: [javascript, fundamentals, web-development, computer-science, placement-prep]
aliases: [var let const, JS Data Types, Primitives]
created: 2026-08-08
---

# Variables and Data Types

JavaScript has three ways to declare variables and a fixed set of data types split into **primitives** (immutable, stored by value) and **objects** (mutable, stored by reference).

## var vs let vs const

| | `var` | `let` | `const` |
|---|---|---|---|
| Scope | Function-scoped | Block-scoped | Block-scoped |
| [[Scope and Hoisting\|Hoisting]] | Hoisted, initialized as `undefined` | Hoisted, but in "temporal dead zone" | Hoisted, in TDZ |
| Re-declaration | Allowed | Not allowed | Not allowed |
| Re-assignment | Allowed | Allowed | Not allowed (but object/array *contents* can mutate) |
| Attaches to `window`/global object | Yes (in browsers) | No | No |

```javascript
console.log(a); // undefined (hoisted)
var a = 10;

console.log(b); // ReferenceError (temporal dead zone)
let b = 20;

const c = { name: "Alice" };
c.name = "Bob";      // OK — mutating the object, not reassigning
c = {};                // TypeError — cannot reassign a const
```

**Placement tip:** Interviewers love asking "why avoid `var`?" — Answer: function-scoping + hoisting to `undefined` causes bugs (e.g. leaking out of `if`/`for` blocks), and it silently allows re-declaration.

## Primitive Data Types (7 total)

1. `Number` — `42`, `3.14`, `NaN`, `Infinity` (all numbers are 64-bit floats, no separate int type)
2. `String` — `"hello"`, `'hi'`, `` `template` ``
3. `Boolean` — `true` / `false`
4. `undefined` — a declared variable with no assigned value
5. `null` — intentional "no value" (note: `typeof null === "object"` — a famous JS bug kept for backward compatibility)
6. `Symbol` — unique, immutable identifier (ES6+), often used as hidden object keys
7. `BigInt` — for integers beyond `Number.MAX_SAFE_INTEGER` (`123n`)

```javascript
typeof 42          // "number"
typeof "hi"         // "string"
typeof true         // "boolean"
typeof undefined    // "undefined"
typeof null         // "object"  <-- classic gotcha
typeof Symbol()     // "symbol"
typeof 10n          // "bigint"
typeof {}           // "object"
typeof []            // "object"  (arrays are objects!)
typeof function(){} // "function"
```

## Primitives vs Reference Types

Primitives are copied **by value**; objects/arrays/functions are copied **by reference**.

```javascript
let x = 10;
let y = x;
y = 20;
console.log(x); // 10 — unaffected

let obj1 = { val: 10 };
let obj2 = obj1;
obj2.val = 20;
console.log(obj1.val); // 20 — same object in memory!
```

## Type Conversion vs Coercion

- **Conversion**: explicit, done by the developer (`Number("5")`, `String(5)`, `Boolean(0)`).
- **Coercion**: implicit, done by JS engine during operations (see [[Operators and Type Coercion]]).

```javascript
Number("42")     // 42
Number("abc")    // NaN
String(42)       // "42"
Boolean("")      // false
Boolean("0")     // true — non-empty string is always truthy!
```

## Key Takeaways

- Prefer `const` by default, `let` when reassignment is needed, avoid `var`.
- 7 primitive types + `object` (which covers arrays, functions, dates, etc.).
- `typeof null === "object"` is a historical bug — always check with `value === null` instead.
- Primitives copy by value; objects copy by reference — this trips up interview candidates on "output" questions.

## Related Concepts
- [[Scope and Hoisting]] — how `var`/`let`/`const` behave before their declaration line
- [[Operators and Type Coercion]] — `==` vs `===` and implicit conversions
- [[Objects Destructuring Spread Rest]] — working with reference types
- [[JS Interview Questions and Tricky Outputs]]

## Open Questions / To Explore Later
- Deep dive into IEEE 754 floating point precision issues (`0.1 + 0.2 !== 0.3`)
- `Symbol` use cases in library design
