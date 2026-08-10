---
tags: [javascript, dsa, web-development, computer-science, placement-prep, interview-favorite]
aliases: [Debounce, Throttle, Currying, HOF]
created: 2026-08-09
---

# Higher-Order Functions, Currying, Debounce & Throttle

These are among the most-asked **"write this from scratch"** placement coding questions — they all lean on [[Closures]].

## Higher-Order Functions (recap)

A function that takes another function as an argument and/or returns a function. `map`, `filter`, `reduce` (see [[Array Methods]]) are the built-in examples; debounce, throttle, and curry below are custom ones you're expected to implement.

## Currying

Currying transforms a function taking multiple arguments into a chain of functions that each take a single argument.

```javascript
const add = (a) => (b) => (c) => a + b + c;
add(1)(2)(3); // 6
```

**Why it's useful:** enables partial application — pre-filling some arguments to create specialized reusable functions.

```javascript
const discount = (rate) => (price) => price - price * rate;
const tenPercentOff = discount(0.10);
tenPercentOff(200); // 180
```

### Generic Curry Helper (common interview task)

```javascript
function curry(fn) {
  return function curried(...args) {
    if (args.length >= fn.length) {          // fn.length = number of declared params
      return fn(...args);
    }
    return (...next) => curried(...args, ...next);
  };
}

function sum3(a, b, c) { return a + b + c; }
const curriedSum = curry(sum3);
curriedSum(1)(2)(3);   // 6
curriedSum(1, 2)(3);   // 6 — works with partial groups too
curriedSum(1)(2, 3);   // 6
```

## Debounce — run only after activity pauses

Delays execution until a set time has passed **since the last call**. Resets the timer on every new call. Ideal for search-as-you-type, resize handlers.

```javascript
function debounce(fn, delay) {
  let timer;
  return function (...args) {
    clearTimeout(timer);                       // cancel the previous pending call
    timer = setTimeout(() => fn.apply(this, args), delay);
  };
}

// usage:
input.addEventListener("input", debounce(handleSearch, 400));
// handleSearch only fires 400ms after the user STOPS typing
```

## Throttle — run at most once per interval

Guarantees a function runs at a fixed maximum rate, no matter how often it's triggered. Ideal for scroll/resize event handlers where you want regular but limited updates.

```javascript
function throttle(fn, limit) {
  let inThrottle = false;
  return function (...args) {
    if (!inThrottle) {
      fn.apply(this, args);
      inThrottle = true;
      setTimeout(() => (inThrottle = false), limit);
    }
  };
}

// usage:
window.addEventListener("scroll", throttle(handleScroll, 200));
// handleScroll fires at most once every 200ms while scrolling continuously
```

## Debounce vs Throttle — the key interview question

> **Debounce** waits for a pause in activity before running once (good for search-as-you-type, form validation).
> **Throttle** guarantees execution at a steady fixed rate regardless of how often the event fires (good for scroll/resize handlers, rate-limiting API calls).

## Memoization (closures caching results)

```javascript
function memoize(fn) {
  const cache = new Map();
  return function (...args) {
    const key = JSON.stringify(args);
    if (cache.has(key)) return cache.get(key);
    const result = fn(...args);
    cache.set(key, result);
    return result;
  };
}

const slowSquare = (n) => { for (let i = 0; i < 1e8; i++); return n * n; };
const fastSquare = memoize(slowSquare);
fastSquare(5); // slow the first time
fastSquare(5); // instant — cached
```

## Key Takeaways

- Currying breaks a multi-argument function into single-argument steps, enabling partial application.
- Debounce delays until activity stops; throttle enforces a steady maximum rate — know the difference and be ready to implement both from memory.
- Memoization caches results by argument signature, using a closure to keep the cache private.
- All three patterns are direct applications of [[Closures]] — the returned function "remembers" `timer`/`cache`/`inThrottle` across calls.

## Related Concepts
- [[Closures]] — the mechanism that makes all of these work
- [[Functions in JavaScript]] — higher-order functions, `apply`
- [[Array Methods]] — built-in HOFs (`map`/`filter`/`reduce`)
- [[DOM and Events]] — typical use sites for debounce/throttle (input, scroll)
- [[JS Interview Questions and Tricky Outputs]]
