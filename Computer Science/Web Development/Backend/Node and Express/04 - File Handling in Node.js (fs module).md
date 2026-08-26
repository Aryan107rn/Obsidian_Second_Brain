# 04 - File Handling in Node.js (fs module)

## What is it?

**`fs`** ("file system") is one of Node's **core modules** — built in, no installation required — that lets your code read, write, update, and delete files and directories on disk.

```js
const fs = require('fs');            // CommonJS
// or
import fs from 'fs';                 // ES Modules
```

## Why does this exist?

Browser JavaScript is deliberately **forbidden** from touching your filesystem — a random webpage reading your hard disk would be a massive security hole. But server-side code has no such restriction and legitimately needs filesystem access all the time: reading config files, writing logs, serving uploaded images, saving user data to disk, reading HTML templates, and so on. Since Node runs outside the browser sandbox (see [[02 - Node.js Runtime vs Browser Environment]]), it's free to expose this capability — and does, via `fs`.

## Three ways to call the same operation

Almost every `fs` function comes in **three flavors** — this is the single most important thing to understand about this module:

| Style | Example | Behavior |
|---|---|---|
| **Synchronous** | `fs.readFileSync(path)` | **Blocks** — the entire Node process stops and waits until the operation finishes before running anything else. |
| **Callback-based (async)** | `fs.readFile(path, callback)` | **Non-blocking** — Node continues running other code immediately; your callback function runs later once the file is read, via the event loop. |
| **Promise-based (async)** | `fs.promises.readFile(path)` or `require('fs/promises')` | Same non-blocking behavior as callbacks, but returns a Promise — lets you use clean `async/await` syntax instead of nested callbacks. |

### Why synchronous methods are dangerous in a server

Node runs your JS on a **single thread**. If you call `fs.readFileSync()` inside a request handler on a running web server, that entire thread freezes until the disk read finishes — **every other incoming request has to wait**, even ones that have nothing to do with that file. This defeats the entire purpose of Node's non-blocking, event-driven design (see [[Event Loop]]).

**Rule of thumb:** use `Sync` methods only for simple one-off scripts or at server *startup* (e.g. reading a config file once before the server starts accepting requests). Inside any request-handling code, always use the callback or promise-based versions.

```js
// ❌ Bad inside a live server — blocks all other requests while reading
app.get('/data', (req, res) => {
  const data = fs.readFileSync('data.json', 'utf-8');
  res.send(data);
});

// ✅ Good — non-blocking, other requests keep being served meanwhile
app.get('/data', async (req, res) => {
  const data = await fs.promises.readFile('data.json', 'utf-8');
  res.send(data);
});
```

## Core operations

```js
const fs = require('fs');
const fsPromises = require('fs/promises');

// Reading a file
fs.readFile('notes.txt', 'utf-8', (err, data) => {
  if (err) throw err;
  console.log(data);
});

// Writing a file — creates it if missing, OVERWRITES if it exists
fs.writeFile('notes.txt', 'Hello world', (err) => { if (err) throw err; });

// Appending — adds to the end instead of overwriting
fs.appendFile('notes.txt', '\nMore text', (err) => { if (err) throw err; });

// Deleting a file
fs.unlink('notes.txt', (err) => { if (err) throw err; });

// Working with directories
fs.mkdir('uploads', (err) => { if (err) throw err; });
fs.readdir('uploads', (err, files) => { console.log(files); }); // array of filenames

// Checking existence / metadata
fs.existsSync('notes.txt');           // true/false, synchronous check (safe — cheap, not I/O-heavy)
fs.stat('notes.txt', (err, stats) => {
  console.log(stats.size, stats.isFile(), stats.isDirectory());
});

// Promise-based equivalent (cleaner with async/await)
async function readNotes() {
  const data = await fsPromises.readFile('notes.txt', 'utf-8');
  console.log(data);
}
```

### The **encoding** gotcha

By default, `fs.readFile` returns a raw **`Buffer`** (binary data), not a readable string:

```js
fs.readFile('notes.txt', (err, data) => {
  console.log(data);       // <Buffer 48 65 6c 6c 6f> — not what you probably wanted
});

fs.readFile('notes.txt', 'utf-8', (err, data) => {
  console.log(data);       // "Hello" — actual string, because encoding was specified
});
```
**Why this happens:** files on disk are just sequences of bytes — Node doesn't assume they're text (they could be an image, a zip file, anything). You must explicitly tell it "interpret these bytes as UTF-8 text" via the encoding argument if you want a string back.

### Combining `fs` with the `path` module

File paths differ between operating systems (`/` on Linux/Mac vs `\` on Windows). The `path` module (also core, no install) builds paths safely across platforms:

```js
const path = require('path');
const fullPath = path.join(__dirname, 'data', 'notes.txt');
// __dirname = the absolute path of the currently running file's folder — a Node-only global (see 02)
```
Using `path.join(__dirname, ...)` instead of hardcoding a relative string like `'./data/notes.txt'` avoids "file not found" bugs caused by the script being run from a different working directory than expected.

## When to reach for streams instead (brief mention)

For very large files (video, large logs, big datasets), loading the entire file into memory with `readFile` can exhaust RAM. Node offers `fs.createReadStream()` / `fs.createWriteStream()` to process a file in small chunks instead of all at once — worth a dedicated note once you get to that stage; for now, know it exists as the answer to "what if the file is huge?"

## Common mistakes

- Forgetting the **error-first callback convention**: every Node callback receives `(err, result)` — always check `if (err)` first. Silently ignoring `err` means failures (missing file, permission denied) go unnoticed.
- Forgetting `'utf-8'` encoding and getting a `Buffer` instead of a string, then being confused by the raw byte output.
- Using `Sync` methods inside a live server's request handlers, freezing the whole app under load (see above).
- With the promise-based API, forgetting to `try/catch` around `await fsPromises.readFile(...)` — a rejected promise with no catch produces an unhandled promise rejection, which can crash the process.
- Assuming `writeFile` appends — it doesn't; it **overwrites** the entire file. Use `appendFile` if you want to add without erasing existing content.

## Related concepts
[[03 - npm, package.json & Node Modules]]
[[02 - Node.js Runtime vs Browser Environment]] — `fs` is a Node-only capability, unavailable in the browser
[[Asynchronous JavaScript]] — callbacks vs promises vs async/await, the pattern `fs` relies on
[[Event Loop]] — why blocking with `Sync` methods is harmful on a single thread
