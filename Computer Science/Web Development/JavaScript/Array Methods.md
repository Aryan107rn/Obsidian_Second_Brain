---
tags: [javascript, fundamentals, web-development, computer-science, placement-prep, dsa]
aliases: [map filter reduce, Array Methods JS]
created: 2026-08-08
---

# Array Methods

Placement interviews test both **knowing the right method for a task** and **implementing them from scratch** (polyfills). Arrays are objects under the hood (`typeof [] === "object"`).

## Mutating vs Non-Mutating (very commonly asked)

| Mutates original array | Does NOT mutate (returns new) |
|---|---|
| `push`, `pop`, `shift`, `unshift` | `map`, `filter`, `slice`, `concat` |
| `splice`, `sort`, `reverse`, `fill` | `reduce`, `find`, `some`, `every` |

## Transformation

```javascript
const nums = [1, 2, 3, 4, 5];

nums.map(n => n * 2);            // [2, 4, 6, 8, 10] — new array, same length
nums.filter(n => n % 2 === 0);    // [2, 4] — new array, keeps matching items
nums.reduce((acc, n) => acc + n, 0); // 15 — folds array into single value
```

### `reduce` — the swiss-army knife (build map/filter from it, group data, flatten, etc.)
```javascript
// Sum
nums.reduce((sum, n) => sum + n, 0); // 15

// Count occurrences
["a","b","a","c","b","a"].reduce((acc, val) => {
  acc[val] = (acc[val] || 0) + 1;
  return acc;
}, {}); // { a: 3, b: 2, c: 1 }

// Flatten one level
[[1,2],[3,4]].reduce((flat, arr) => flat.concat(arr), []); // [1,2,3,4]
```

## Searching

```javascript
nums.find(n => n > 3);        // 4 — first matching element
nums.findIndex(n => n > 3);   // 3 — index of first match
nums.includes(3);               // true
nums.indexOf(3);                // 2
nums.some(n => n > 4);         // true — at least one matches
nums.every(n => n > 0);        // true — all match
```

## Slicing / Splicing / Combining

```javascript
nums.slice(1, 3);        // [2, 3] — non-mutating, extracts a portion
nums.splice(1, 2, "x");   // mutates! removes 2 items from index 1, inserts "x"
nums.concat([6, 7]);       // [1,2,3,4,5,6,7] — non-mutating merge
[...nums, 6, 7];             // same result using spread (preferred modern style)
```

## Sorting (mutates + gotcha)

```javascript
[10, 1, 21, 2].sort();
// [1, 10, 2, 21] — WRONG! default sort converts to strings
[10, 1, 21, 2].sort((a, b) => a - b);
// [1, 2, 10, 21] — correct numeric sort, ascending
[10, 1, 21, 2].sort((a, b) => b - a);
// [21, 10, 2, 1] — descending
```
**Interview gotcha:** `Array.prototype.sort()` defaults to lexicographic (string) sorting — always pass a comparator for numbers.

## Iteration

```javascript
nums.forEach(n => console.log(n)); // no return value, just side effects
for (const n of nums) console.log(n); // for...of works on any iterable
```

## Array.from / Array.of / flat

```javascript
Array.from({ length: 5 }, (_, i) => i * 2); // [0,2,4,6,8]
Array.from("hello");                          // ["h","e","l","l","o"]
Array.of(7);                                   // [7]  (vs `Array(7)` -> empty array length 7!)
[1, [2, [3, [4]]]].flat(2);                   // [1, 2, 3, [4]] — flattens 2 levels deep
[1, 2, 3].flatMap(n => [n, n * 2]);           // [1,2,2,4,3,6] — map then flat(1)
```

## Common Polyfill Interview Question: Implement `map`

```javascript
Array.prototype.myMap = function (callback) {
  const result = [];
  for (let i = 0; i < this.length; i++) {
    result.push(callback(this[i], i, this));
  }
  return result;
};
[1, 2, 3].myMap(x => x * 2); // [2, 4, 6]
```

## Key Takeaways

- Know which methods mutate vs return a new array — a top interview trap.
- `reduce` can implement almost every other array method — practice writing `map`/`filter` using `reduce`.
- Always pass a comparator to `sort()` for numbers.
- `Array.from`/spread are the modern way to convert iterables/array-likes to real arrays.

## Related Concepts
- [[Functions in JavaScript]] — array methods are higher-order functions
- [[Objects Destructuring Spread Rest]] — spread syntax used heavily with arrays
- [[Common Coding Patterns]] — array manipulation shows up in most coding-round questions
- [[JS Interview Questions and Tricky Outputs]]
