---
tags: [javascript, fundamentals, web-development, computer-science, placement-prep]
aliases: [Equality in JS, "== vs ===", Truthy Falsy]
created: 2026-08-08
---

# Operators and Type Coercion

## == vs ===

- `===` (strict equality): compares value **and** type, no conversion.
- `==` (loose equality): converts types first, then compares — source of many interview "guess the output" questions.

```javascript
5 === "5"     // false (different types)
5 == "5"      // true  (string coerced to number)

null == undefined   // true  (special case)
null === undefined  // false

0 == false     // true
0 === false    // false

NaN == NaN     // false — NaN is never equal to anything, even itself
Number.isNaN(NaN) // true — correct way to check
```

**Placement tip:** Always use `===` unless you have a specific reason. It's a near-universal linting rule and a common interview question ("why is `===` preferred?").

## Truthy and Falsy Values

Only **8 falsy values** exist in JS — everything else is truthy.

```javascript
false, 0, -0, 0n, "", null, undefined, NaN
```

```javascript
if ("0") console.log("truthy!");     // runs — non-empty string
if ([]) console.log("truthy!");       // runs — empty array is truthy
if ({}) console.log("truthy!");       // runs — empty object is truthy
```

## Logical Operators (short-circuiting)

```javascript
let a = null;
console.log(a || "default");       // "default" (|| returns first truthy)
console.log(a ?? "default");       // "default" (?? checks only null/undefined)

let b = 0;
console.log(b || "default");       // "default" — 0 is falsy, may be unwanted!
console.log(b ?? "default");       // 0 — nullish coalescing preserves valid falsy values
```

- `&&` returns the first falsy value or the last value if all are truthy (used for conditional rendering in React: `isLoggedIn && <Welcome/>`).
- `??` (nullish coalescing, ES2020) only falls back on `null`/`undefined`, not on `0`, `""`, or `false`.

## Arithmetic Coercion Gotchas

```javascript
"5" + 3        // "53" (string concatenation wins)
"5" - 3        // 2   (- forces numeric conversion)
"5" * "2"      // 10
true + true    // 2   (booleans coerce to 1/0)
[] + []         // ""  (arrays coerce to strings)
[] + {}         // "[object Object]"
1 + null        // 1   (null -> 0)
1 + undefined   // NaN (undefined -> NaN)
```

## Optional Chaining (`?.`)

```javascript
const user = { profile: { name: "Alice" } };
console.log(user.address?.city); // undefined, no error
console.log(user.profile?.name); // "Alice"
user.sayHi?.();                    // calls only if sayHi exists
```

## Key Takeaways

- Always default to `===`; only use `==` deliberately (e.g. `== null` to catch both `null` and `undefined`).
- Memorize the 8 falsy values — everything else, including `[]` and `{}`, is truthy.
- `??` is safer than `||` when `0`, `""`, or `false` are valid values.
- Optional chaining (`?.`) avoids manual `&&` chains for nested property access.

## Related Concepts
- [[Variables and Data Types]] — the types being compared/coerced
- [[JS Interview Questions and Tricky Outputs]] — coercion is a favorite topic
- [[ES6+ Modern Features]] — `??` and `?.` are ES2020 additions

## Open Questions / To Explore Later
- The full [Abstract Equality Comparison Algorithm](https://tc39.es) behind `==`
