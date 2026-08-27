---
tags: [javascript, async, web-development, computer-science, placement-prep, interview-favorite]
aliases: [Promises, async await, Callback Hell, Promise.all, setTimeout, setInterval]
created: 2026-08-09
updated: 2026-08-27
---

# Asynchronous JavaScript

JavaScript is single-threaded — it can only execute one task at a time. Asynchronous patterns enable programs to start long-running tasks (network requests, database lookups, timers) without blocking execution while waiting for results. See [[Event Loop]] for runtime mechanics.

---

## 🖼️ Promise Lifecycle & Async / Await Execution

![[js-async-promises.svg|960]]

---

## Evolution of Async JavaScript

```mermaid
flowchart LR
    A["1. Callbacks (ES5)<br/>Deep nesting / Callback Hell"] --> B["2. Promises (ES6)<br/>.then() / .catch() Chaining"]
    B --> C["3. Async / Await (ES2017)<br/>Synchronous-looking syntax"]
```

Each stage below shows the actual syntax, not just the concept — this is the section to copy-paste from when you forget the exact keywords.

---

## 1. Callbacks

A **callback** is just a function passed as an argument to another function, to be called ("called back") once that function finishes its work.

```javascript
function greetUser(name, callback) {
  console.log("Preparing greeting...");
  callback(name); // the "callback" is invoked here
}

greetUser("Aryan", function (name) {
  console.log(`Hello, ${name}!`);
});
```

Callbacks become **asynchronous** when the outer function delays calling them (e.g. via `setTimeout`, a file read, or a network request) instead of calling them immediately:

```javascript
function fetchDataCallback(callback) {
  setTimeout(() => {
    const data = { id: 1, name: "Aryan" };
    callback(null, data); // Node-style: (error, result)
  }, 1000);
}

fetchDataCallback((err, data) => {
  if (err) return console.error(err);
  console.log("Got:", data);
});
```

The `(err, data)` pattern above is called **error-first callback** style — it's the Node.js convention: the first argument is always the error (or `null` if none), the second is the result.

**Callback Hell**: nesting async callbacks inside async callbacks to express "do this, then that, then that" becomes an unreadable pyramid:

```javascript
getUser(userId, (err, user) => {
  getPosts(user.id, (err, posts) => {
    getComments(posts[0].id, (err, comments) => {
      console.log(comments); // 3 levels deep, and growing
    });
  });
});
```

This nesting — not asynchrony itself — is the actual problem Promises were designed to solve.

---

## 2. `setTimeout` and `setInterval`

These are **Web API** functions (not part of core JS) used to schedule code to run later. They are the simplest way to *create* asynchronous behavior.

### `setTimeout` — run once, after a delay

```javascript
// syntax: setTimeout(callbackFn, delayInMs, ...argsForCallback)
const timeoutId = setTimeout(() => {
  console.log("Runs once, after 2 seconds");
}, 2000);

// cancel it before it fires:
clearTimeout(timeoutId);
```

### `setInterval` — run repeatedly, every N ms

```javascript
let count = 0;
const intervalId = setInterval(() => {
  count++;
  console.log(`Tick ${count}`);
  if (count === 5) clearInterval(intervalId); // must clear manually, or it runs forever
}, 1000);
```

**Important**: `setTimeout(fn, 0)` does NOT run `fn` immediately. It hands `fn` to the Web API, which queues it in the **callback queue** once the delay (0ms) elapses. `fn` only runs after the current synchronous code finishes AND the microtask queue (Promises) is empty. See [[Event Loop]] for the exact ordering.

```javascript
console.log("1");
setTimeout(() => console.log("2"), 0);
console.log("3");
// Output: 1, 3, 2 — NOT 1, 2, 3
```

---

## 3. Promises: States & Chaining

A **Promise** is an object representing the eventual result of an asynchronous operation. It exists to replace nested callbacks with a flat, chainable syntax.

A Promise has 3 states:
1. **Pending** — initial state, neither fulfilled nor rejected.
2. **Fulfilled** — operation completed successfully (`resolve(value)` was called).
3. **Rejected** — operation failed (`reject(error)` was called).

### Creating a Promise

```javascript
const myPromise = new Promise((resolve, reject) => {
  setTimeout(() => {
    const success = true;
    success ? resolve("Success data") : reject(new Error("Operation failed"));
  }, 1000);
});
```

### Consuming a Promise — `.then()` / `.catch()` / `.finally()`

```javascript
myPromise
  .then(data => console.log("Received:", data))   // runs on resolve
  .catch(err => console.error("Caught:", err.message)) // runs on reject
  .finally(() => console.log("Cleanup complete"));      // always runs
```

### Chaining (each `.then()` returns a new Promise)

```javascript
fetchUser(1)
  .then(user => fetchPosts(user.id))   // return value becomes the next .then()'s input
  .then(posts => console.log(posts))
  .catch(err => console.error("Any error in the chain lands here:", err));
```

This is the direct fix for callback hell: instead of nesting, each step chains flatly and one `.catch()` handles errors from every step above it.

---

## Promise Combinators Comparison

| Method | Resolution Trigger | Rejection Trigger | Best Used For |
| :--- | :--- | :--- | :--- |
| `Promise.all([...])` | Resolves when **ALL** promises fulfill | Rejects immediately if **ANY** promise fails (Fail-Fast) | Dependent parallel API calls |
| `Promise.allSettled([...])` | Resolves when **ALL** promises finish (Fulfilled or Rejected) | **Never rejects**; returns array of `{status, value/reason}` | Batch processing where partial failure is fine |
| `Promise.race([...])` | Settles as soon as the **FIRST** promise settles | Rejects if the first promise rejects | Timeout wrappers for slow API requests |
| `Promise.any([...])` | Resolves as soon as the **FIRST** promise fulfills | Rejects only if **ALL** promises reject (`AggregateError`) | Redundant CDN or mirror fetching |

```javascript
const results = await Promise.allSettled([fetchUser(1), fetchUser(2)]);
// [{status:"fulfilled", value:{...}}, {status:"rejected", reason: Error}]
```

---

## 4. `async` / `await`

`async`/`await` is **syntactic sugar over Promises** — it lets asynchronous code read like synchronous code, without changing what's happening underneath (a Promise is still created and awaited).

### Declaring an async function

```javascript
// function declaration
async function getUser() {
  return { id: 1, name: "Aryan" };
}

// arrow function
const getUser = async () => {
  return { id: 1, name: "Aryan" };
};

// object method
const api = {
  async getUser() {
    return { id: 1, name: "Aryan" };
  }
};
```

Any function marked `async` **automatically returns a Promise**, even if you `return` a plain value — `return x` inside an async function behaves like `return Promise.resolve(x)`.

### `await` — pausing until a Promise settles

`await` can only be used **inside an `async` function** (or, in modern JS/modules, at the top level of a module). It pauses execution of that function — not the whole program — until the Promise resolves, then unwraps the resolved value.

```javascript
async function loadUserData() {
  const user = await fetchUser();       // pauses here until fetchUser()'s promise resolves
  const posts = await fetchPosts(user.id);
  console.log(posts);
}
```

### Error handling — `try...catch`

Since `await` throws when the Promise rejects, wrap it in `try...catch` (this replaces `.catch()`):

```javascript
async function loadUserData() {
  try {
    const user = await fetchUser();
    const posts = await fetchPosts(user.id);
    return posts;
  } catch (err) {
    console.error("Failed to load:", err.message);
  } finally {
    console.log("Done attempting load");
  }
}
```

### Converting a `.then()` chain to `async/await`

```javascript
// Promise chain
function loadUser() {
  return fetchUser()
    .then(user => fetchPosts(user.id))
    .then(posts => posts)
    .catch(err => console.error(err));
}

// Equivalent async/await
async function loadUser() {
  try {
    const user = await fetchUser();
    const posts = await fetchPosts(user.id);
    return posts;
  } catch (err) {
    console.error(err);
  }
}
```

---

## Sequential vs. Parallel Async Calls

A common mistake: awaiting independent calls one after another wastes time, because each `await` blocks the *next line* until it settles.

```javascript
// ❌ SLOW: Sequential execution (~2000ms total)
const user = await fetchUser();         // waits 1000ms
const settings = await fetchSettings(); // waits another 1000ms after that

// ✅ FAST: Parallel execution (~1000ms total)
const [user, settings] = await Promise.all([
  fetchUser(),
  fetchSettings()
]);
```

Use sequential `await` only when a later call genuinely **depends on** the result of an earlier one (e.g. `fetchPosts(user.id)` needs `user` first). Otherwise, kick both off together with `Promise.all`.

---

## Common Mistakes

- Forgetting `await` on an async call — you get a pending `Promise` object instead of the resolved value.
- Using `await` outside an `async` function (syntax error, except at module top level).
- Forgetting `clearInterval` — the interval keeps firing forever, a common memory/performance leak.
- Assuming `setTimeout(fn, 0)` runs immediately — it always runs after the current call stack and all microtasks clear.
- Awaiting independent Promises sequentially instead of using `Promise.all` (see above).
- Not handling rejections — an unhandled rejected Promise (missing `.catch()` or `try/catch`) can crash Node processes or fail silently in the browser.

---

## 🔗 Related Concepts

- [[Event Loop]] — Microtask queues, callback queue, and execution priority (explains *why* `setTimeout(fn, 0)` behaves the way it does)
- [[Functions in JavaScript]] — Higher-order callback functions
- [[Error Handling and Memory]] — Error catching with `try...catch` and async functions
- [[JS Interview Questions and Tricky Outputs]] — Async output prediction questions
