# 08 - HTTP Methods & Method-Based Routing in Node.js

*(Following Piyush Garg's Node.js playlist.)*

## What is it?

An **HTTP method** (also called an HTTP "verb") tells the server what *kind of action* the client wants to perform on a resource — not just *which* resource (that's the path's job — see [[07 - URL Structure & Parsing in Node.js]]), but *what to do with it*.

> This note focuses on the **practical, raw-Node mechanics** of reading and branching on `req.method`. For the deeper semantics of each verb (idempotency, safety, when to use `PUT` vs `PATCH`, status code conventions), see [[REST APIs]] — that note already covers the full design-level picture; this one covers how to actually use `req.method` before you have a framework doing it for you.

## The five primary methods, quick recap

| Method | Purpose |
|---|---|
| **GET** | Retrieve data — read-only. What your browser sends by default when you visit a URL. |
| **POST** | Send data to create a new resource — e.g. submitting a signup/login form, whose data lands in the request body to be stored. |
| **PUT** | Upload/replace data at a specific location — e.g. uploading a file to a specific path. |
| **PATCH** | Partially update an existing resource — change one field without resending the whole record. |
| **DELETE** | Remove a resource. |

## Reading the method: `req.method`

Every incoming request object carries a `.method` property — a plain string like `'GET'`, `'POST'`, `'PUT'`, `'PATCH'`, `'DELETE'` (always uppercase per the HTTP spec).

```js
const server = http.createServer((req, res) => {
  console.log(req.method);   // "GET", "POST", etc.
});
```

## Why you need to check BOTH path and method

A single path often needs to behave differently depending on the method used against it — this is the real-world reason `req.method` matters at all:

```
GET  /signup   →  show the empty registration form
POST /signup   →  process the submitted form data and create the user
```
Same path, two completely different behaviors — determined entirely by the method.

## Combining path + method for manual routing

Extending the router from [[06 - Building an HTTP Server with the http Module]] with method checks:

```js
const http = require('http');

const server = http.createServer((req, res) => {
  const { pathname } = new URL(req.url, `http://${req.headers.host}`);

  if (pathname === '/signup' && req.method === 'GET') {
    res.writeHead(200, { 'Content-Type': 'text/html' });
    return res.end('<form method="POST" action="/signup">...</form>');
  }

  if (pathname === '/signup' && req.method === 'POST') {
    let body = '';
    req.on('data', chunk => { body += chunk; });   // body arrives as a stream (see 06)
    req.on('end', () => {
      // `body` now holds the raw submitted data — e.g. "name=John&email=john@x.com"
      res.writeHead(201, { 'Content-Type': 'text/plain' });
      res.end('User created');
    });
    return;
  }

  res.writeHead(404);
  res.end('Not Found');
});
```

**What's happening:**
- Both checks share the same `pathname` but branch on `req.method` to decide behavior — this `path AND method` combination is the actual unit of routing in real web apps, not path alone.
- The `POST` branch reads the body via `req.on('data', ...)` / `req.on('end', ...)` — reiterating the point from [[06 - Building an HTTP Server with the http Module]]: the body is **not** a ready-made property like `req.body` (that convenience comes from frameworks/middleware); here you assemble it yourself from streamed chunks.
- `201` (Created) is used instead of `200` for the successful `POST` — a REST convention (see [[REST APIs]] status code table) signaling "a new resource was created," not just "request succeeded."

## Why this doesn't scale (and what Express fixes)

With even a handful of routes, each needing multiple methods, this becomes an unmanageable pile of `if` statements checking `pathname && method` combinations by hand. This exact combinatorial explosion — `(number of paths) × (number of methods)` — is precisely what routing frameworks solve. Express (covered later in this vault) replaces this entire pattern with declarative registration:

```js
// What Express lets you write instead (preview — not yet covered in depth):
app.get('/signup', showForm);
app.post('/signup', handleSignup);
```
Same logic, but the framework handles the path+method matching internally instead of you writing nested `if` chains.

## Common mistakes

- Checking only the `pathname` and forgetting the method — leads to accidentally allowing `DELETE` or `PUT` requests to hit a route meant only to handle `GET`.
- Assuming `req.method` might be lowercase — it won't be (`'get'` vs `'GET'`), but defensively calling `.toUpperCase()` before comparing is a safe habit if the comparison value comes from a variable rather than a literal.
- Trying to read the request body before the `'end'` event has fired — the data arrives in chunks over time; reading before it's fully received gives incomplete data.
- Forgetting that `GET`/`DELETE` requests conventionally have **no body** — trying to parse a body from them is usually pointless (any data for these methods normally travels via the URL/query params or path instead).


![[Pasted image 20260831010148.png|904]]

## Related concepts
[[06 - Building an HTTP Server with the http Module]]
[[07 - URL Structure & Parsing in Node.js]]
[[REST APIs]] — full verb semantics: safety, idempotency, status code conventions, and PUT vs PATCH
