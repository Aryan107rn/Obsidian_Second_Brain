---
tags: [nodejs, expressjs, backend, web-development, computer-science, moc]
aliases: [Node.js, NodeJS, Backend Development]
created: 2026-08-27
updated: 2026-08-28
---

# Node.js & Express — Backend Development (MOC)

Backend curriculum covering the Node.js runtime, its module system, the `http` module, and eventually the Express framework built on top of it. Notes are numbered sequentially — follow in order if learning from scratch.

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

### Chapter 3: Interacting with the System
* **[[04 - File Handling in Node.js (fs module)]]**
  * *Focus:* The `fs` core module, sync vs callback-based vs promise-based APIs, why sync blocks the event loop, core file operations, encoding gotchas.

### Chapter 4: Node's Concurrency Model
* **[[05 - Node.js Event Loop Phases & libuv Thread Pool]]**
  * *Focus:* Why Node is single-threaded but non-blocking, the 6 libuv event loop phases (timers, pending callbacks, poll, check, close), `process.nextTick` priority, `setImmediate` vs `setTimeout(fn, 0)`, and the libuv thread pool (what uses it, default size, `UV_THREADPOOL_SIZE`).

### Chapter 5: Building a Server from Scratch
* **[[06 - Building an HTTP Server with the http Module]]**
  * *Focus:* `http.createServer(requestListener)` and why it takes a callback, `server.listen`, key `req`/`res` properties (`req.url`, `req.method`, `req.headers`, `res.end`, `res.writeHead`), manual routing via `switch` on `req.url`, and why this doesn't scale (motivating Express).

### Chapter 6: Express.js (upcoming)
*Not yet covered — will be added as topics are taught.*

## 🔗 Connected Concepts
* [[JavaScript MOC]] — the language Node executes
* [[Event Loop]] — general call stack / microtask / macrotask model (prerequisite for Chapter 4)
* [[Package Managers and Build Tools]] — npm/pnpm/yarn comparison from the frontend tooling side
* [[Asynchronous JavaScript]] — promises/callbacks used throughout Node's async APIs
