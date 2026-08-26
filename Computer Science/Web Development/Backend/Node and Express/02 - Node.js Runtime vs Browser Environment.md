# 02 - Node.js Runtime vs Browser Environment

## What is it?

Both a browser's dev console and a Node.js terminal let you type and run JavaScript. But typing `window` in each gives a completely different result:

```js
// In a browser console:
window
// → the Window object: a huge object with document, alert, localStorage,
//   navigator, location, innerWidth, setTimeout, fetch, etc.

// In a Node.js REPL/terminal:
window
// → ReferenceError: window is not defined
```

Instead, Node has its own top-level object called **`global`**:

```js
// In Node:
global
// → an object containing process, Buffer, require, module,
//   __dirname, __filename, setTimeout, console, etc. — no document, no window.
```

## Why are they different?

This comes down to *what each environment actually is*.

- **`window` represents a browser tab/window.** It exists because a browser needs to give JS a way to control the page and the browser chrome around it — hence `document` (the page's content, i.e. the **DOM** — Document Object Model, a tree representation of the HTML), and things like `navigator`, `location`, `history` (the **BOM** — Browser Object Model, APIs about the browser itself, not the page content).
- **Node.js is not a browser.** There is no tab, no window, no HTML page being rendered, no browser chrome. Node is just a program that runs JS on your machine/server to do backend work (serve requests, read files, etc.). So the concept of "window" simply doesn't apply — there's nothing for it to represent.

Node still needs *some* top-level object to hang global utilities off of (things available everywhere without importing) — that's what `global` is, but it holds runtime-appropriate things instead of page/browser things: `process` (info & control over the running Node process), `Buffer` (handling raw binary data), `__dirname`/`__filename` (current file's location on disk), and the module system pieces (`require`, `module`, `exports`).

### `globalThis` — the environment-agnostic escape hatch

Because different JS environments name their global object differently (`window` in browsers, `global` in Node, `self` in Web Workers), ES2020 introduced **`globalThis`** — a standardized reference to "the global object" that works identically everywhere:

```js
globalThis === window   // true, in a browser
globalThis === global   // true, in Node
```
Use `globalThis` when writing code intended to run in more than one environment (e.g. a shared library).

## Why is UI-related work removed from Node's runtime?

This is really the same root cause as above, from a different angle: **the DOM, BOM, and other "Web APIs" (`document`, `alert`, `localStorage`, pre-Node-18 `fetch`, `window.innerWidth`, etc.) are not part of the JavaScript language at all.** They're not implemented by the JS engine (V8/SpiderMonkey) — they're implemented separately by the **browser vendor**, in C++, as part of the browser's own rendering engine. The browser then exposes those C++ objects into JS's global scope so your scripts can call them.

Node.js embeds *only* the JS engine (V8) — it doesn't embed a rendering engine, because it never needs to draw a webpage. So none of the browser-supplied objects exist in Node; they were never there to begin with, not "removed" as a deliberate restriction on an existing feature.

In their place, Node's creators wrote a **different set of APIs suited to a server environment** — things a browser deliberately blocks for security (a webpage reading your filesystem would be a massive security hole) but that a backend program legitimately needs:

| Need | Browser API | Node API |
|---|---|---|
| Read/write files | *(none — sandboxed)* | `fs` |
| Make outbound network requests | `fetch`, `XMLHttpRequest` | `http`, `https`, `net` |
| Know about the OS | *(none)* | `os` |
| Run/manage the current process | *(none)* | `process` |
| Manipulate a webpage | `document`, DOM APIs | *(none — no page to manipulate)* |
| Browser chrome info | `navigator`, `location` | *(none — no browser)* |

## Common mistakes

- Writing `document.querySelector(...)` or `window.location` in a Node script → `ReferenceError: document is not defined`. These are browser-only globals.
- Writing `require(...)` or using `module.exports` in browser-side JS (e.g. inside a `<script>` tag in an HTML file) → `ReferenceError: require is not defined`. These are Node-only, part of its module system (see [[03 - npm, package.json & Node Modules]]).
- Assuming "JavaScript" as a language includes `fetch` or `document` — it doesn't. Those are APIs *provided by the host environment*, not the ECMAScript language spec itself. This is the single most important mental model shift when moving from frontend to backend JS.

## Edge case worth knowing

Since **Node 18**, `fetch` was added as a global in Node too (borrowed conceptually from browsers, implemented via `undici` under the hood). This can look like Node "gained a browser API," but it doesn't mean Node has a DOM — `fetch` is just a networking convenience function; `document`/`window`/DOM manipulation still don't exist in Node. It shows that runtime boundaries are a design choice by whoever maintains the runtime, not a fixed law — Node's maintainers chose to add `fetch` because it's genuinely useful for making HTTP requests, independent of any browser/page concept.

## Related concepts
[[01 - Introduction to Node.js & JavaScript Engines]]
[[03 - npm, package.json & Node Modules]]
[[DOM and Events]]
[[Event Loop]]
