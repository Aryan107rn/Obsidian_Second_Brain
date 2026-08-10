---
tags: [javascript, async, web-development, computer-science, placement-prep, interview-favorite]
aliases: [Promises, async await, Callback Hell, Promise.all]
created: 2026-08-09
---

# Asynchronous JavaScript

JavaScript is single-threaded — it can only do one thing at a time. Asynchronous patterns let it start slow operations (network requests, timers, file I/O) without freezing the rest of the program while waiting. See [[Event Loop]] for exactly how this is scheduled under the hood.

## Callbacks & Callback Hell

The original async pattern: pass a function to be called later, when the result is ready.

```javascript
getUser(id, (user) => {
  getPosts(user.id, (posts) => {
    getComments(posts[0].id, (comments) => {
      console.log(comments); // deeply nested — "callback hell" / "pyramid of doom"
    });
  });
});
```
Problems: hard to read, hard to handle errors consistently, hard to run things in parallel.

## Promises

A **Promise** is an object representing a value that will be available now, later, or never. It has three states:

```
pending  →  fulfilled (resolved)
pending  →  rejected
```
Once **settled** (fulfilled or rejected), a Promise's state and value never change again.

```javascript
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    const success = true;
    success ? resolve("data") : reject("error");
  }, 1000);
});

promise
  .then(data => console.log(data))     // runs if resolved
  .catch(err => console.error(err))     // runs if rejected (or any error thrown in the chain)
  .finally(() => console.log("done"));  // always runs
```

**Chaining** — each `.then()` returns a new Promise, allowing sequential async steps without nesting:
```javascript
getUser(id)
  .then(user => getPosts(user.id))
  .then(posts => getComments(posts[0].id))
  .then(comments => console.log(comments))
  .catch(err => console.error("Something failed:", err)); // catches errors from ANY step above
```

## Promise Combinators

| Method | Behavior |
|---|---|
| `Promise.all([...])` | Resolves when **all** succeed (returns array of results); rejects immediately if **any** fails ("fail-fast") |
| `Promise.allSettled([...])` | Waits for **all** to settle regardless of outcome; returns `{status, value/reason}` for each — never short-circuits |
| `Promise.race([...])` | Settles as soon as the **first** promise settles (whether it resolves or rejects) |
| `Promise.any([...])` | Resolves as soon as the **first** one fulfills; rejects only if **all** fail |

```javascript
const results = await Promise.all([fetchA(), fetchB(), fetchC()]);
// if fetchB() rejects, the whole Promise.all rejects immediately

const results2 = await Promise.allSettled([fetchA(), fetchB(), fetchC()]);
// [{status:"fulfilled", value:...}, {status:"rejected", reason:...}, ...]
// — useful when you want results even if some fail
```

## async/await — Syntactic Sugar Over Promises

```javascript
async function fetchUser() {
  try {
    const res = await fetch("/api/user");   // pauses this function (not the whole thread) until settled
    const data = await res.json();
    return data;
  } catch (err) {
    console.error("failed:", err);           // catches rejected promises too
  }
}
```

**Key fact:** an `async` function **always returns a Promise**, even if you `return` a plain value — it gets auto-wrapped.

```javascript
async function greet() { return "hi"; }
greet();              // Promise {<fulfilled>: "hi"} — NOT the string directly
greet().then(console.log); // "hi"
```

## Sequential vs Parallel Async Calls (common performance interview question)

```javascript
// SLOW — sequential: each await blocks the next from starting
const a = await fetchA();  // waits ~1s
const b = await fetchB();  // waits another ~1s -> total ~2s

// FAST — parallel: both requests start immediately
const [a, b] = await Promise.all([fetchA(), fetchB()]); // total ~1s
```
Rule: only await sequentially when the second call actually **depends** on the result of the first; otherwise, fire them together with `Promise.all`.

## Key Takeaways

- Promises replace nested callbacks with chainable `.then()`/`.catch()`, fixing "callback hell."
- `Promise.all` fails fast; `allSettled` waits for everything; `race`/`any` return on the first settle/fulfill respectively.
- `async`/`await` is syntactic sugar over Promises — `await` pauses the async function (not the main thread) until the Promise settles.
- Always `Promise.all` independent async calls instead of awaiting them one by one — huge, commonly-tested performance win.

## Related Concepts
- [[Event Loop]] — how Promise callbacks (microtasks) get scheduled and prioritized
- [[Functions in JavaScript]] — callbacks are just functions passed as arguments
- [[Error Handling and Memory]] — try/catch with async/await
- [[JS Interview Questions and Tricky Outputs]]
