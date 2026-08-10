---
tags: [javascript, web-development, computer-science, placement-prep]
aliases: [try catch, Custom Errors, Memory Leaks, localStorage, JSON methods, CommonJS vs ESM]
created: 2026-08-09
---

# Error Handling, Modules & Memory

## try/catch/finally & Custom Errors

```javascript
try {
  JSON.parse("invalid json");    // throws a SyntaxError
} catch (err) {
  console.error(err.message);
} finally {
  console.log("always runs");    // runs whether or not an error occurred
}

class ValidationError extends Error {
  constructor(msg) {
    super(msg);
    this.name = "ValidationError";  // custom errors extend the built-in Error class
  }
}
throw new ValidationError("Invalid input");
```
`try/catch` also works around `await` to catch rejected Promises — see [[Asynchronous JavaScript]].

## CommonJS vs ES Modules

| | CommonJS (Node legacy) | ES Modules |
|---|---|---|
| Syntax | `require()` / `module.exports` | `import` / `export` |
| Loading | Synchronous | Asynchronous, statically analyzable |
| Where used | Older Node.js code | Modern Node.js & all browsers/bundlers |

See [[ES6+ Modern Features]] for `import`/`export` syntax examples.

## JSON Methods

```javascript
JSON.stringify({ a: 1, b: [1, 2] }); // '{"a":1,"b":[1,2]}' — object to JSON string
JSON.parse('{"a":1}');                 // { a: 1 } — JSON string to object
```
`JSON.stringify` silently drops `undefined` values, functions, and `Symbol`s — a common gotcha when using it as a quick "deep clone" trick.

## Garbage Collection & Memory Leaks

JS uses automatic garbage collection (primarily **mark-and-sweep**): objects with no remaining reachable references become eligible for collection and are eventually freed.

**Common memory leak sources (frequently asked):**
- Forgotten `setInterval`/event listeners that hold references and are never cleared (see [[DOM and Events]])
- Closures unintentionally keeping large objects alive longer than needed (see [[Closures]])
- Detached DOM nodes still referenced by a JS variable after being removed from the page
- Global variables that silently accumulate data over the app's lifetime

## localStorage vs sessionStorage vs Cookies

| | `localStorage` | `sessionStorage` | Cookies |
|---|---|---|---|
| Persists | Until manually cleared | Until the tab is closed | Configurable expiry |
| Size limit | ~5–10MB | ~5–10MB | ~4KB |
| Sent to server? | No | No | Yes, with every matching HTTP request |

```javascript
localStorage.setItem("theme", "dark");
localStorage.getItem("theme");    // "dark"
localStorage.removeItem("theme");
localStorage.clear();
```
**Practical takeaway:** cookies are the right choice for data the server needs on every request (e.g. session tokens); `localStorage`/`sessionStorage` are better for client-only data (UI preferences) since they aren't automatically transmitted.

## Key Takeaways

- `try/catch/finally` handles both sync errors and (with `await`) rejected Promises; custom error classes should `extend Error`.
- CommonJS is synchronous and Node-only; ES Modules are the modern, statically-analyzable standard everywhere.
- Memory leaks in JS almost always come from forgotten references — timers, listeners, closures, or globals.
- Cookies travel with every HTTP request automatically; Web Storage APIs do not — pick based on whether the server needs the data.

## Related Concepts
- [[Asynchronous JavaScript]] — try/catch with async/await
- [[Closures]] — a common accidental source of memory leaks
- [[DOM and Events]] — event listener cleanup
- [[ES6+ Modern Features]] — import/export module syntax
