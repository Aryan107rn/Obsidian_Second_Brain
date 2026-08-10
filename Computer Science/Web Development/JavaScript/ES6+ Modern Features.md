---
tags: [javascript, es6, web-development, computer-science, placement-prep]
aliases: [ES6 Features, Modern JavaScript, Map and Set, Optional Chaining, Nullish Coalescing, Generators]
created: 2026-08-09
---

# ES6+ Modern Features

## Template Literals

```javascript
const name = "Aryan";
console.log(`Hello, ${name}! 2+2 = ${2 + 2}`); // interpolation + multi-line strings
const multiLine = `line 1
line 2`; // real newlines, no \n needed
```

## Optional Chaining (`?.`) & Nullish Coalescing (`??`)

```javascript
const city = user?.address?.city;   // undefined instead of throwing, if address is null/undefined
const port = config.port ?? 3000;    // falls back ONLY on null/undefined
const port2 = config.port || 3000;   // falls back on ANY falsy value — different behavior!
```
**Common mistake:** using `||` for defaults when `0` or `""` are valid values — it wrongly overrides them. Use `??` instead. See [[Operators and Type Coercion]] for the full falsy-value list.

## Map & Set

`Map` and `Set` are dedicated collection types (ES6), distinct from plain objects/arrays.

```javascript
const map = new Map();
map.set("a", 1).set("b", 2);           // chainable
map.get("a");        // 1 — keys can be ANY type (object, function, etc.), unlike object keys which coerce to strings
map.has("a");         // true
map.delete("a");
map.size;              // 1
for (const [key, val] of map) console.log(key, val);

const set = new Set([1, 2, 2, 3]);     // Set {1, 2, 3} — auto-dedupes
[...new Set([1, 1, 2, 2, 3])];          // [1, 2, 3] — common array-dedupe trick
```

### Map vs plain Object

| | `Map` | `Object` |
|---|---|---|
| Key types | Any type | String/Symbol only (others coerced to string) |
| Key order | Insertion order guaranteed | Mostly insertion order, but integer-like keys sort first |
| Size | `.size` | `Object.keys(obj).length` |
| Iterable directly | Yes (`for...of`) | No (need `Object.entries` first) |
| Performance | Better for frequent add/remove | Better for static, JSON-shaped data |

### WeakMap & WeakSet

Like `Map`/`Set`, but keys **must be objects** and are held **weakly** — if no other reference to a key exists, it can be garbage-collected. Useful for attaching private/metadata to objects without causing memory leaks.

```javascript
const cache = new WeakMap();
function process(obj) {
  if (cache.has(obj)) return cache.get(obj);
  const result = expensiveComputation(obj);
  cache.set(obj, result);
  return result;
}
// no need to manually clean up — entries vanish when `obj` is no longer referenced elsewhere
```

## Modules — `import`/`export`

```javascript
// math.js
export const add = (a, b) => a + b;
export default function multiply(a, b) { return a * b; }

// app.js
import multiply, { add } from "./math.js";
```
See [[Error Handling and Memory]] for CommonJS vs ES Modules comparison.

## Generators

A function that can **pause** and **resume**, producing a sequence of values lazily instead of computing them all at once.

```javascript
function* idGenerator() {
  let id = 1;
  while (true) yield id++;
}
const gen = idGenerator();
gen.next().value; // 1
gen.next().value; // 2 — resumes exactly where it paused, keeps its own state
```
Useful for lazy sequences, custom iterators, and (historically) as the basis for async flow control before `async/await` existed.

## Symbol & BigInt

- **`Symbol()`** — creates a guaranteed-unique value, often used as non-colliding object keys (e.g. to avoid clashing with future property names).
- **`BigInt`** — for integers beyond `Number.MAX_SAFE_INTEGER` (2^53 - 1), written with an `n` suffix: `123n`.

```javascript
const id = Symbol("id");
const obj = { [id]: 123 }; // won't collide with any string key, even another "id"

const huge = 9007199254740993n; // BigInt — safe beyond Number's precision limit
```

## Key Takeaways

- `??` is safer than `||` for defaults when `0`/`""`/`false` are valid values; `?.` avoids manual null checks on nested access.
- `Map`/`Set` solve real limitations of plain objects/arrays: any-type keys, guaranteed order, direct iteration, better add/remove performance.
- `WeakMap`/`WeakSet` prevent memory leaks when associating data with objects that may be discarded later.
- Generators pause/resume execution with `yield` — the conceptual ancestor of `async/await`.

## Related Concepts
- [[Operators and Type Coercion]] — falsy values relevant to `??` vs `||`
- [[Asynchronous JavaScript]] — generators as a precursor to async/await
- [[Array Methods]] — `Set` for deduping arrays
- [[JS Interview Questions and Tricky Outputs]]
