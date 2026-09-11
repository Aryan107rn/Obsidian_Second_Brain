# 09 - Introduction to Express.js

*(Following Piyush Garg's Node.js playlist.)*

## What is it?

**Express** is a minimal, unopinionated **web framework** for Node.js — a third-party package (`npm install express`, see [[03 - npm, package.json & Node Modules]]) built directly on top of the core `http` module (see [[06 - Building an HTTP Server with the http Module]]). It doesn't replace `http` — it wraps it, handling the repetitive, error-prone parts (routing, query parsing, response formatting) so you write far less boilerplate per route.

## Why does it exist? (the pain points it removes)

Everything Express does here directly answers a problem you already hit while building servers manually:

| Manual `http` pain point | How Express fixes it |
|---|---|
| Checking `req.url === '/about' && req.method === 'GET'` by hand for every route (see [[08 - HTTP Methods & Method-Based Routing in Node.js]]) | `app.get('/about', handler)` — path and method are bound together in one declarative call |
| Parsing query strings manually via `new URL()`/`url.parse()` (see [[07 - URL Structure & Parsing in Node.js]]) | `req.query` — already a plain object, parsed for you |
| `res.end()` only sends raw strings/buffers — no automatic content-type handling | `res.send()` — smart about what you pass it (string, object, array, buffer) |
| An ever-growing `if`/`switch` chain that becomes unmanageable | Each route is its own small, independent registration — no giant chain to maintain |

## Setting up a basic Express server

```javascript
const http = require("http");
const express = require("express");

const app = express();

app.get('/', (req, res) => {
    return res.send("From Home Page");
});

app.get('/about', (req, res) => {
    return res.send("From about Page");
});

const myServer = http.createServer(app);
myServer.listen(8000, () => console.log("Server Started!"));
```

### `app.get(path, handler)` — routing, declaratively

```js
app.get('/about', (req, res) => { ... });
```
This single line replaces the entire manual check `if (req.method === 'GET' && req.url === '/about')`. Express exposes one method per HTTP verb — `app.get()`, `app.post()`, `app.put()`, `app.patch()`, `app.delete()` — each automatically matching only that method against the given path. You register as many of these as you have routes; Express internally maintains the routing table and matches incoming requests against it.

### `res.send()` vs `res.end()`

`res.end()` (raw Node, from [[06 - Building an HTTP Server with the http Module]]) sends exactly the bytes/string you give it and nothing more — you're responsible for setting headers like `Content-Type` yourself. `res.send()` is Express's smarter equivalent:
- Passing a **string** → sends it as `text/html` by default.
- Passing an **object or array** → automatically `JSON.stringify`s it and sets `Content-Type: application/json` for you — no manual `JSON.stringify` + header-setting needed.
- Passing a **Buffer** → sends it as binary data appropriately.

This is why Express code looks shorter — a lot of manual header/formatting logic from the raw `http` version is handled implicitly.

## `req.query` — query parameters, already parsed

```javascript
app.get('/about', (req, res) => {
    return res.send("From about Page" + ' hey ' + req.query.name + ' you are ' + req.query.age + ' old ');
});
```

Visiting `/about?name=Aryan&age=21` gives you:
```js
req.query
// → { name: 'Aryan', age: '21' }

req.query.name   // "Aryan"
req.query.age    // "21"  (still a string — same caveat as URLSearchParams in note 07: convert with Number() if doing math)
```

**This is the direct upgrade over [[07 - URL Structure & Parsing in Node.js]]:** you previously had to manually do `new URL(req.url, base).searchParams.get('name')`. Express does that entire parsing step for you automatically, on every request, before your handler even runs — `req.query` is just sitting there ready to use.

## `app.listen()` vs `http.createServer(app)` + `.listen()` — literally the same thing

```javascript
// Version A — explicit, using http directly
const myServer = http.createServer(app);
myServer.listen(8000, () => console.log("Server Started!"));

// Version B — Express shorthand
app.listen(8000, () => console.log("Server Started!"));
```

These **are the same code** — Version B is not a different mechanism, it's Express's convenience wrapper. Internally, `app.listen(...)` does exactly this:
```js
// what app.listen() does under the hood, conceptually
app.listen = function (...args) {
  return http.createServer(this).listen(...args);
};
```

**Why this works at all — the key insight:** `app` itself, returned by `express()`, is actually just a **function** with the exact same signature Node expects for a `requestListener` — `(req, res) => { ... }` (recall from [[06 - Building an HTTP Server with the http Module]] that this is exactly what `http.createServer()` wants as its argument). Express adds `.get()`, `.post()`, etc. as extra properties on top of that function, but underneath, `app` can be handed directly to `http.createServer()` because it *is* a valid request-handling callback — Express just adds routing logic inside that callback before your specific route handler runs.

This is also exactly why your third snippet works with `http`, `fs`, and `url` all commented out/unused — once you're using `app.listen()`, you never need to touch the raw `http` module yourself; Express is doing that internally for you.

## Common mistakes

- Forgetting that `req.query` values are always **strings**, even for things that look numeric (`req.query.age` is `"21"`, not `21`) — same gotcha as `URLSearchParams.get()` in raw Node.
- Calling `res.send()` (or `res.end()`) more than once for the same request — throws `Error: Cannot set headers after they are sent to the client`. Using `return res.send(...)` (as in your code) is good practice specifically to prevent accidentally falling through to another `res.send()` later in the same handler.
- Leaving unused imports around (your snippets still `require('fs')` and `require('url')` without using them, and an empty `myHandler` function) — harmless, but worth cleaning up once you know a route doesn't need them; a sign you're mid-refactor from manual `http` code toward pure Express.
- Assuming `app.get()` matches the method only — it matches **both** the given path and the `GET` method together, same as the manual `pathname && method` check from [[08 - HTTP Methods & Method-Based Routing in Node.js]] would.

## Vanilla `http` vs Express — side-by-side

| Task | Vanilla `http` module | Express |
|---|---|---|
| Create the app/server | `http.createServer((req, res) => {...})` | `const app = express();` |
| Route by path + method | Manual `if (req.method === 'GET' && pathname === '/about')` | `app.get('/about', handler)` |
| Read query parameters | `new URL(req.url, base).searchParams.get('key')` | `req.query.key` — already parsed |
| Send a response | `res.end("text")` — manual header setting for anything else | `res.send(data)` — auto-detects string/object/buffer, sets headers |
| Start the server | `server.listen(port, cb)` | `app.listen(port, cb)` — shorthand for the same thing |
| Scaling to many routes | Ever-growing `if`/`switch` chain | Each route is an independent, readable registration |

## Related concepts
[[06 - Building an HTTP Server with the http Module]] — the raw foundation Express is built on
[[07 - URL Structure & Parsing in Node.js]] — what `req.query` replaces
[[08 - HTTP Methods & Method-Based Routing in Node.js]] — what `app.get`/`app.post`/etc. replace
[[REST APIs]] — designing real routes/resources once past this basic setup stage
