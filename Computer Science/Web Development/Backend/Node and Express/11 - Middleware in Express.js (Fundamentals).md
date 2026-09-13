# 11 - Middleware in Express.js (Fundamentals)

*(Following Piyush Garg's Node.js playlist.)*

## What is it?

**Middleware** is a function that sits *in the middle* of the request-response cycle — it runs **after** a request arrives but **before** your final route handler produces a response. Its signature is always:

```js
function myMiddleware(req, res, next) {
    // ... do something with req/res ...
    next(); // pass control forward
}
```

This is the third argument you haven't used yet in `app.get('/about', (req, res) => {...})` handlers — `next` is a function that, when called, tells Express "I'm done, move on to whatever comes next in the chain."

## Why does it exist?

Many concerns apply to **most or all** of your routes, not just one: parsing request bodies, logging every request, checking authentication, handling errors consistently. Without middleware, you'd have to copy-paste that logic into every single route handler — extremely repetitive and easy to get inconsistent over time. Middleware lets you write that logic **once** and have Express automatically run it for the relevant requests, before your actual route-specific code executes.

## How it works: the middleware chain

Express processes each request through a **chain** of functions, executed strictly in the order they were registered. Each middleware function can do one of three things:

1. **Inspect or modify** `req`/`res` and call `next()` to hand off to the next function in the chain.
2. **End the request-response cycle itself** — e.g. `res.status(401).send("Unauthorized")` — without calling `next()`, stopping the chain right there (useful for auth checks that reject a request before it ever reaches your route logic).
3. Do nothing and forget `next()` entirely — **this is a bug**: the request just hangs forever, since nothing ever tells Express to continue or respond (same failure mode as forgetting `res.end()` in raw `http`, see [[06 - Building an HTTP Server with the http Module]]).

```mermaid
flowchart LR
    Req["Incoming Request"] --> M1["Middleware 1\n(e.g. logging)"]
    M1 -->|next| M2["Middleware 2\n(e.g. body parsing)"]
    M2 -->|next| M3["Middleware 3\n(e.g. auth check)"]
    M3 -->|next| Handler["Route Handler\napp.get/post/etc."]
    Handler --> Res["Response Sent"]
    M3 -->|"or: rejects here"| Res
```

## Registering middleware

```js
// Runs for EVERY incoming request, regardless of path or method
app.use(myMiddleware);

// Runs only for requests whose path starts with '/api'
app.use('/api', myMiddleware);

// Route-specific middleware — runs only for this exact route, before its handler
app.get('/profile', myMiddleware, (req, res) => {
    res.send('Profile page');
});
```

## Types of middleware

| Type | Examples | Where it comes from |
|---|---|---|
| **Built-in** | `express.json()`, `express.urlencoded()`, `express.static()` | Ships with Express itself |
| **Third-party** | `cors`, `morgan`, `helmet` | Installed via npm (see [[03 - npm, package.json & Node Modules]]) |
| **Custom** | Your own logging/auth/validation functions | Written by you |

## A simple custom example

```js
function requestLogger(req, res, next) {
    console.log(`${new Date().toISOString()} - ${req.method} ${req.url}`);
    next(); // don't forget this!
}

app.use(requestLogger); // logs every request, before any route runs
```

## Why order matters

Middleware registered earlier runs earlier — this has real consequences. A body-parsing middleware (see [[12 - express.urlencoded, express.json & req.body]]) **must** be registered before any route handler that reads `req.body`, or `req.body` will simply be `undefined` when your handler runs, because the parsing step never got a chance to happen first.

```js
app.use(express.urlencoded({ extended: true })); // must come BEFORE routes that need req.body

app.post('/signup', (req, res) => {
    console.log(req.body); // works, because the middleware above already ran
});
```

## Common mistakes

- **Forgetting to call `next()`** — the request hangs indefinitely; the client never gets a response and appears frozen.
- **Registering body-parsing middleware after your routes** — `req.body` will be `undefined` in any route defined before the middleware.
- **Calling `next()` and also sending a response in the same middleware** — causes `Error: Cannot set headers after they are sent to the client`, since Express thinks the cycle both ended (via your response) and continued (via `next()`).
- Assuming middleware only runs for one route — `app.use(fn)` with no path applies **globally**, to every request that reaches the app.

## Related concepts
[[09 - Introduction to Express.js]] — the `app` object and route handlers this builds on
[[12 - express.urlencoded, express.json & req.body]] — the most common built-in middleware you'll use immediately
[[06 - Building an HTTP Server with the http Module]] — the raw request/response cycle middleware sits inside of
