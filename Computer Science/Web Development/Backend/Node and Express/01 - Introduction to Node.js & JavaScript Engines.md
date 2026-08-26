# 01 - Introduction to Node.js & JavaScript Engines

## What is a JavaScript engine?

A **JavaScript engine** is a program that reads JS source code and actually executes it — parsing the text, converting it into a form the computer can run, and running it. JavaScript itself is just a language specification (ECMAScript); it needs *something* to carry out its instructions, the same way a recipe needs a cook. That "something" is the engine.

Every environment that runs JS ships its own engine:

| Engine | Made by | Used in |
|---|---|---|
| **V8** | Google | Chrome, Chromium-based browsers (Edge, Brave, Opera), and **Node.js** |
| **SpiderMonkey** | Mozilla | Firefox — historically important: it's the **very first JS engine ever built** (Brendan Eich, 1995, for Netscape) |
| **JavaScriptCore** (aka "Nitro") | Apple | Safari, WebKit |
| **Chakra** | Microsoft | Old Internet Explorer / legacy Edge — discontinued after Edge switched to Chromium + V8 |

**Key point:** these engines all run the *same language* (JS), but they're independent codebases competing on speed and features. A script that works in Chrome should also work in Firefox precisely because both engines implement the same ECMAScript spec — but subtle differences and bugs between engines do exist.

## How an engine like V8 actually runs code (brief)

1. **Parsing** — source code is read and converted into an AST (Abstract Syntax Tree), a structured representation of the code.
2. **Ignition** — V8's interpreter converts the AST into bytecode and starts executing immediately (fast startup, no need to fully optimize up front).
3. **TurboFan** — V8 watches which functions run repeatedly ("hot" code) and recompiles just those into highly optimized native machine code using a Just-In-Time (JIT) compiler — trading a little upfront analysis for much faster execution of code that runs often.

This is why V8 is fast: cheap bytecode execution for code that only runs once, aggressive optimization for code that runs a lot (like a loop processing thousands of items).

## What is Node.js, then?

JavaScript was originally designed to do one thing: run inside a browser and make web pages interactive. It had no way to read a file, open a network socket, or talk to the operating system — the browser sandboxed it deliberately, for security (a webpage shouldn't be able to read your hard disk).

**Node.js takes the V8 engine and embeds it outside the browser**, then bolts on a set of APIs (written in C++ and JS) that let JS talk to the operating system: read/write files, start servers, spawn processes, and more. This is what turned JavaScript from "a scripting language stuck inside a browser" into a general-purpose, server-side language.

So concretely:

```
Node.js = V8 (executes JS) + libuv (C library: event loop, async I/O, thread pool) + Node's own APIs (fs, http, path, os, etc.)
```

- **V8** only knows how to run JavaScript-the-language (variables, functions, closures, objects — whatever's in the ECMAScript spec). It has no concept of "file" or "network."
- **libuv** is a C library that gives Node its event loop and non-blocking I/O — the mechanism that lets Node handle thousands of concurrent operations (file reads, network requests) on a single thread without stalling. (Deep dive: [[Event Loop]].)
- **Node's built-in modules** (`fs`, `http`, `path`, `os`, ...) are the actual glue — C++ bindings exposed as JS functions — that let your JS code do real I/O.

## Why did Node choose V8 specifically?

When Ryan Dahl created Node.js in 2009, he picked V8 because it was:
- **Open-source** and free to embed in another project.
- **Extremely fast**, thanks to its JIT-compilation pipeline (above).
- **Actively developed** by Google with continuous performance investment (since it also had to power Chrome).

## Common mistakes / misconceptions

- **"Node.js is a programming language."** It's not — it's a **runtime environment**. The language is still JavaScript; Node just provides a different environment (and different global APIs) to run that language in, compared to a browser.
- **"V8 and Node.js are the same thing."** V8 is only the engine that executes the JS *language features*. Everything server-specific (reading files, starting an HTTP server) comes from Node's own additions on top of V8, not from V8 itself.
- **Assuming code that runs in the browser will run unchanged in Node.** Since Node has no DOM/BOM (see next note), browser-only code (`document.querySelector`, `window.alert`) will throw errors in Node — the language is shared, but the surrounding APIs are not.

## Related concepts
[[02 - Node.js Runtime vs Browser Environment]]
[[Event Loop]]
[[Asynchronous JavaScript]]
