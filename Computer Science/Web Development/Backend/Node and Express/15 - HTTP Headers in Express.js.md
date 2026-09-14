# 15 - HTTP Headers in Express.js

*(Following Piyush Garg's Node.js playlist.)*

## What is it?

**Headers** are metadata attached to an HTTP request or response — extra information *about* the message, sent alongside the actual body, but not part of it.

**The mail analogy:** think of an HTTP request/response like a physical package being mailed. The **body** is the actual contents inside the box. The **headers** are everything written on the outside of the package — sender address, destination, "fragile," weight, what's inside (so the courier knows how to handle it) — all metadata the postal service and recipient need *before* opening the box. A server (or any intermediary, like a proxy) can inspect headers without needing to parse the entire body first.

## Why do headers exist?

The body of a request/response can be almost anything — HTML, JSON, an image, plain text — and can arrive in different encodings, compressed or not, at various sizes. Headers exist so the sender can **describe** the body (and other context) in a small, structured, always-in-the-same-place way, so the receiver knows how to handle it *before* processing the whole payload. Examples of what this "context" actually looks like:

| Header | What it tells the receiver |
|---|---|
| `Content-Type` | What format the body is in (`application/json`, `text/html`, `multipart/form-data`, etc.) — this is exactly what lets Express's body-parsing middleware decide how to parse `req.body` (see below). |
| `Content-Length` | How many bytes the body contains. |
| `User-Agent` | What client/browser/tool is making the request (e.g. `Mozilla/5.0...`, or `PostmanRuntime/7.x` when using Postman). |
| `Authorization` | Credentials/tokens for authenticating the request. |
| `Cache-Control` | How (and whether) the response should be cached. |

## Inspecting headers in practice

- **Browser DevTools** — open the Network tab, click any request, and both **Request Headers** and **Response Headers** are shown in full — useful for seeing exactly what a browser sends by default (`User-Agent`, `Accept`, cookies, etc.) and what a server responded with (`Content-Type`, `Set-Cookie`, caching headers).
- **Postman** — the Headers tab shows headers you can set on outgoing requests, and the response panel shows all headers the server sent back — including ones you never explicitly set yourself (Express and Node add several automatically, like `Content-Type`, `Content-Length`, and `Date`).

## Reading headers in Express

```js
app.get('/', (req, res) => {
    console.log(req.headers);              // the full headers object, all lowercase keys
    console.log(req.headers['user-agent']); // one specific header
    console.log(req.get('User-Agent'));     // equivalent — Express's convenience method, case-insensitive
});
```
`req.get(name)` is the Express-provided shortcut for reading a single header — preferred over indexing `req.headers` directly because it's case-insensitive (HTTP header names are case-insensitive by spec, but object keys in JS are not — `req.headers['User-Agent']` would actually miss, since Node lowercases all incoming header names).

## Setting headers in Express

```js
app.get('/download', (req, res) => {
    res.setHeader('Content-Type', 'application/pdf');
    res.setHeader('X-Custom-Info', 'generated-by-my-api');
    res.send(fileBuffer);
});

// Or, set multiple at once:
res.set({
    'Content-Type': 'application/json',
    'X-Custom-Info': 'generated-by-my-api'
});
```
Recall from [[06 - Building an HTTP Server with the http Module]] and [[09 - Introduction to Express.js]]: headers must be set **before** the body is sent (`res.send()`/`res.end()`) — once the response has started streaming, headers can no longer be changed.

## Custom headers and the `X-` prefix convention

A long-standing convention for application-specific, non-standard headers is prefixing them with `X-` — e.g. `X-Custom-Info`, `X-Request-ID`, `X-RateLimit-Remaining` — signaling "this isn't a standard HTTP header, it's something specific to this application."

**Important nuance worth knowing precisely:** this convention was formally **deprecated by the IETF in RFC 6648 (2012)**, which recommends *not* prefixing new custom headers with `X-` at all — the reasoning being that headers occasionally graduate from "custom" to becoming an official standard later, and stripping the `X-` prefix at that point breaks existing integrations depending on the old name. In practice, though, the `X-` convention remains extremely widespread in real-world APIs (many major companies still use it) because it's an easy, unambiguous visual signal for "custom header" even if it's no longer the officially recommended approach. Worth knowing both the convention **and** that it's technically deprecated — you'll see it constantly in real APIs regardless.

## Middleware mechanics: how headers drive body parsing

This connects directly back to [[12 - express.urlencoded, express.json & req.body]]: body-parsing middleware doesn't guess the body's format — it reads the **`Content-Type` header** to decide whether to act at all.

```js
app.use(express.urlencoded({ extended: true })); // only parses bodies where Content-Type is application/x-www-form-urlencoded
app.use(express.json());                          // only parses bodies where Content-Type is application/json
```

If an incoming request's `Content-Type` doesn't match what a given parsing middleware expects, that middleware effectively does nothing for that request (it calls `next()` without touching `req.body`) — which is exactly the mechanism behind the common mistake noted in [[12 - express.urlencoded, express.json & req.body]]: a Content-Type/parser mismatch silently leaves `req.body` empty, because the correct middleware for that content type was never actually triggered.

## Common mistakes

- Reading `req.headers['User-Agent']` (capitalized) directly and getting `undefined` — incoming header keys are always lowercased by Node; use `req.get('User-Agent')` instead, or lowercase the key yourself.
- Trying to set a header **after** calling `res.send()`/`res.end()` — throws `Error: Cannot set headers after they are sent to the client`.
- Assuming `X-` is still the "correct" way to name custom headers because it's common — it's common, but officially deprecated; either is defensible depending on what conventions your team/API already follows.
- Forgetting that a `Content-Type` mismatch between client and server-side middleware is a **silent** failure — no error is thrown; `req.body` just quietly ends up empty, which can be a confusing bug to trace without knowing this mechanism.

## Related concepts
[[12 - express.urlencoded, express.json & req.body]] — the `Content-Type`-driven parsing this note explains the mechanism behind
[[11 - Middleware in Express.js (Fundamentals)]]
[[06 - Building an HTTP Server with the http Module]] — `req.headers` in raw Node, before Express's `req.get()` convenience
[[13 - API Testing with Postman]] — inspecting headers in practice
