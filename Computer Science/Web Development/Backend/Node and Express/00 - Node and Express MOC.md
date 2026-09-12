---
tags: [nodejs, expressjs, backend, web-development, computer-science, moc]
aliases: [Node.js, NodeJS, Backend Development]
created: 2026-08-27
updated: 2026-09-02
---

# Node.js & Express — Backend Development (MOC)

Backend curriculum covering the Node.js runtime, its module system, the `http` module, and the Express framework built on top of it. Notes are numbered sequentially — follow in order if learning from scratch.

*(Following Piyush Garg's Node.js YouTube playlist.)*

## 🗺️ Roadmap

### Chapter 1: The Node.js Runtime
* **[[01 - Introduction to Node.js & JavaScript Engines]]**
  * *Focus:* What a JS engine is, V8/SpiderMonkey/JavaScriptCore, why Node chose V8, how Node = V8 + libuv + C++ bindings.
* **[[02 - Node.js Runtime vs Browser Environment]]**
  * *Focus:* `window` vs `global` vs `globalThis`, why DOM/BOM APIs don't exist in Node, why Node has entirely different APIs (fs, http, os) instead.

### Chapter 2: Modules & Package Management
* **[[03 - npm, package.json & Node Modules]]**
  * *Focus:* `npm init`, anatomy of `package.json`, what a module is, core vs local vs third-party modules, CommonJS (`require`/`module.exports`) vs ES Modules (`import`/`export`) in Node.
* **[[10 - Semantic Versioning & Dependency Management]]**
  * *Focus:* MAJOR.MINOR.PATCH structure, caret `^` vs tilde `~` update ranges, why to avoid `@latest`, `package-lock.json`, and manual version locking.

### Chapter 3: Interacting with the System
* **[[04 - File Handling in Node.js (fs module)]]**
  * *Focus:* The `fs` core module, sync vs callback-based vs promise-based APIs, why sync blocks the event loop, core file operations, encoding gotchas.

### Chapter 4: Node's Concurrency Model
* **[[05 - Node.js Event Loop Phases & libuv Thread Pool]]**
  * *Focus:* Why Node is single-threaded but non-blocking, the 6 libuv event loop phases (timers, pending callbacks, poll, check, close), `process.nextTick` priority, `setImmediate` vs `setTimeout(fn, 0)`, and the libuv thread pool (what uses it, default size, `UV_THREADPOOL_SIZE`).

### Chapter 5: Building a Server from Scratch
* **[[06 - Building an HTTP Server with the http Module]]**
  * *Focus:* What an HTTP server is conceptually, `localhost` and ports, `http.createServer(requestListener)` and why it takes a callback, `server.listen`, key `req`/`res` properties, manual routing via `if`/`switch` on `req.url`, and why this doesn't scale (motivating Express).

### Chapter 5b: URLs, Query Strings & HTTP Methods
* **[[07 - URL Structure & Parsing in Node.js]]**
  * *Focus:* URL anatomy (protocol, domain, path, query params), why `req.url` is unparsed raw text, parsing with the WHATWG `URL` class vs the legacy `url` module, `.pathname` vs `.searchParams`.
* **[[08 - HTTP Methods & Method-Based Routing in Node.js]]**
  * *Focus:* Reading `req.method`, why routing needs path **and** method together, reading a POST body manually via streamed chunks, and why this manual approach motivates a framework.

### Chapter 6: Express.js
* **[[09 - Introduction to Express.js]]**
  * *Focus:* `app.get(path, handler)` as the declarative replacement for manual method+path checks, `res.send()` vs `res.end()`, `req.query` (automatic query parsing — replaces manual `URL`/`searchParams` work), and why `app.listen()` is literally shorthand for `http.createServer(app).listen()`. Includes a full vanilla-`http`-vs-Express comparison table.

## 🔗 Connected Concepts
* [[JavaScript MOC]] — the language Node executes
* [[Event Loop]] — general call stack / microtask / macrotask model (prerequisite for Chapter 4)
* [[Package Managers and Build Tools]] — npm/pnpm/yarn comparison from the frontend tooling side
* [[Asynchronous JavaScript]] — promises/callbacks used throughout Node's async APIs
* [[REST APIs]] — full HTTP verb semantics, idempotency, and status code conventions (deeper dive beyond Chapter 5b)
* [[HTTP Versions & the QUERY Method]] — HTTP/1.1 vs HTTP/2, and the new `QUERY` method (RFC 10008, 2026)
