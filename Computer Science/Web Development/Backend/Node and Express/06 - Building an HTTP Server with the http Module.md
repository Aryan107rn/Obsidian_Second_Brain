# 06 - Building an HTTP Server with the http Module

*(Following Piyush Garg's Node.js playlist.)*

## What is it?

**`http`** is one of Node's **core modules** (see [[03 - npm, package.json & Node Modules]]) — it lets you create a web server directly, with no framework, by handling raw HTTP requests and responses yourself.

```js
const http = require('http');
```

Express (which you'll get to later) is itself built **on top of** this exact module — everything Express does (routing, middleware, `req`/`res` helpers) is ultimately implemented using `http` underneath. Learning this first is what makes Express make sense later, instead of feeling like magic.

## Why does it exist?

A web server's job, at the lowest level, is: accept incoming TCP connections, parse the raw HTTP text (request line, headers, body) arriving over that connection, and let your code decide what to send back. Doing this by hand with raw TCP sockets would mean re-implementing the entire HTTP protocol parser yourself. The `http` module does that parsing for you and hands you clean `req` (request) and `res` (response) objects — your job is just to decide, per request, what data to send back.

## `http.createServer(requestListener)`

```js
const server = http.createServer((req, res) => {
  // this function runs once for EVERY incoming request
});
```

### Why does `createServer` take a callback (the `requestListener`)?

Because a server, once started, **keeps running indefinitely** and will receive an unknown number of requests over its lifetime — you can't just "run some code once" the way you would in a script. You need a function that gets **invoked automatically, once per request, with a fresh `req`/`res` pair each time**. That's exactly what a callback is for.

Under the hood, `http.createServer(requestListener)` is shorthand for:
```js
const server = new http.Server();
server.on('request', requestListener);
```
This is Node's standard **event-driven pattern** (the same `EventEmitter` pattern used throughout Node): the server object emits a `'request'` event every time a new HTTP request arrives, and `requestListener` is just the function registered to run whenever that event fires. `createServer` simply does this event registration for you in one step, since "handle a request" is the one thing every HTTP server needs to do.

## `server.listen(port, callback)`

```js
server.listen(8000, () => console.log('server started'));
```
This binds the server to a specific **port** on your machine and starts accepting incoming connections on it. The callback fires once the server has successfully started listening — useful for confirming startup (logging, readiness checks). **One server, one port** — a single `http.Server` instance listens on exactly one port at a time.

## Key `req` (request) properties — what you actually need

| Property / Method | What it gives you |
|---|---|
| `req.url` | The path (+ query string) requested, **relative to the server root** — e.g. `/about` or `/search?q=node`. Not the full URL (no domain/protocol). |
| `req.method` | The HTTP verb used: `'GET'`, `'POST'`, `'PUT'`, `'DELETE'`, etc. |
| `req.headers` | An object of all request headers, with lowercase keys (e.g. `req.headers['content-type']`). |
| `req.on('data', chunk => ...)` | Fires repeatedly as the request **body** arrives, in `Buffer` chunks (bodies arrive as a stream, not a ready-made property — this is why frameworks like Express need body-parsing middleware to give you a convenient `req.body`). |
| `req.on('end', () => ...)` | Fires once the entire body has been received — where you'd assemble the collected chunks into the final body. |

## Key `res` (response) properties/methods

| Method | What it does |
|---|---|
| `res.writeHead(statusCode, headersObj)` | Sets the status code (e.g. `200`, `404`) and response headers **before** sending the body. Must be called before `res.write`/`res.end` if you need custom headers. |
| `res.setHeader(name, value)` | Sets a single response header. |
| `res.write(chunk)` | Sends part of the response body — can be called multiple times for streaming output. |
| `res.end([data])` | **Finalizes and sends** the response, optionally with one last chunk of data. |

**Common mistake:** forgetting to call `res.end()` — without it, the response is never sent, and the client (browser/Postman) hangs indefinitely waiting, because as far as it knows the server hasn't finished replying.

## Your code, explained

```javascript
const http = require("http");
const fs = require("fs");

const server = http.createServer((req, res) => {
    const log = `${Date.now()}:New Req Recieved\n`;
    fs.appendFile('log.txt', log, (err, data) => {
        res.end("Hello From Server Again");
    });
});

server.listen(8000, () => console.log("server started"));
```

Walkthrough:
- Every incoming request triggers the `requestListener` callback.
- `Date.now()` timestamps the request, building a one-line log entry.
- `fs.appendFile(...)` (see [[04 - File Handling in Node.js (fs module)]]) writes that line to `log.txt` **without erasing previous entries** (unlike `writeFile`, which would overwrite the file each time).
- `res.end(...)` is called **inside** the `fs.appendFile` callback — correctly waiting for the (non-blocking, async) file write to finish before responding. If `res.end` were called outside/before this callback, the response could be sent before the log write even completes — not wrong here since the response doesn't depend on the log content, but it's the right pattern to know for cases where the response *does* depend on an async result.
- **Improvement to note:** the `err` parameter in the `fs.appendFile` callback is never checked. In production code, you'd want:
  ```js
  fs.appendFile('log.txt', log, (err) => {
    if (err) {
      res.writeHead(500);
      return res.end("Something went wrong");
    }
    res.end("Hello From Server Again");
  });
  ```

## Upgraded version: manual routing with `switch`

Right now, every route returns the same response. To respond differently based on which path was requested, check `req.url`:

```javascript
const http = require("http");
const fs = require("fs");

const server = http.createServer((req, res) => {
    const log = `${Date.now()}:New Req Recieved: ${req.url}\n`;

    fs.appendFile('log.txt', log, (err) => {
        if (err) {
            res.writeHead(500, { 'Content-Type': 'text/plain' });
            return res.end("Server error while logging request");
        }

        res.writeHead(200, { 'Content-Type': 'text/plain' });

        switch (req.url) {
            case '/':
                res.end("Welcome to the Home Page");
                break;

            case '/about':
                res.end("This is the About Page");
                break;

            case '/contact':
                res.end("This is the Contact Page");
                break;

            default:
                res.writeHead(404, { 'Content-Type': 'text/plain' });
                res.end("404 - Page Not Found");
        }
    });
});

server.listen(8000, () => console.log("server started"));
```

**What changed and why:**
- `req.url` is compared against known paths to decide the response — this **is routing**, in its most manual form.
- A `default` case handles any unrecognized path with a proper `404`, instead of silently returning the home page response for everything (a common beginner bug — always handle the "not found" case explicitly).
- `res.writeHead(200, {...})` is set once, before the `switch`, since every matched case shares the same success status — only the `default` case overrides it to `404`.

**Why this doesn't scale (and why Express exists):**
- Real apps have dozens/hundreds of routes — an ever-growing `switch` becomes unmanageable.
- `req.url` includes query strings (`/about?ref=email` won't match the `'/about'` case exactly) — production routing needs proper URL parsing (`new URL(req.url, 'http://localhost')`) to separate the path from the query.
- There's no clean way to handle dynamic path segments (`/users/123`) with a plain `switch`.
- This exact pain point — matching methods + paths + dynamic segments cleanly — is precisely what a routing framework like Express solves. Understanding *why* you'd want it is easier having built this by hand first.

## Common mistakes

- Forgetting `res.end()` — client hangs forever.
- Comparing `req.url` directly against a path when a query string might be present (`/about?x=1 !== /about`).
- Doing heavy synchronous work inside the request handler — since Node is single-threaded for JS execution (see [[05 - Node.js Event Loop Phases & libuv Thread Pool]]), this blocks **every other concurrent request** the server is trying to handle.
- Not setting a `Content-Type` header — clients may guess the wrong type for your response body.

## Related concepts
[[04 - File Handling in Node.js (fs module)]]
[[05 - Node.js Event Loop Phases & libuv Thread Pool]] — why blocking work in a request handler is dangerous
[[03 - npm, package.json & Node Modules]] — `http` as a core module
