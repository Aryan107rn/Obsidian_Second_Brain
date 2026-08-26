# 03 - npm, package.json & Node Modules

## What is `npm`?

**npm** (Node Package Manager) is the default tool that ships with Node.js for installing, sharing, and managing reusable pieces of code called **packages** (or "libraries"). Instead of writing everything from scratch, you can pull in code someone else already wrote and tested — e.g. `express` for building servers, `axios` for HTTP requests. *(For a full comparison of npm vs pnpm vs yarn, and bundlers like Vite, see [[Package Managers and Build Tools]] — this note focuses on the Node-specific pieces: `npm init`, `package.json`, and the module system itself.)*

## `npm init` and `package.json`

Every Node project is described by a file called **`package.json`** — a JSON file holding the project's metadata: its name, version, entry point, dependencies, and custom scripts.

```bash
npm init        # interactive — asks you name, version, entry point, etc.
npm init -y     # skips all prompts, fills in sensible defaults immediately
```

Running this creates `package.json`, roughly:

```json
{
  "name": "my-backend-app",
  "version": "1.0.0",
  "main": "index.js",
  "scripts": {
    "start": "node index.js"
  },
  "dependencies": {}
}
```

Key fields:
- **`main`** — the entry file loaded when something does `require('my-backend-app')`.
- **`scripts`** — named shortcuts you run via `npm run <name>` (e.g. `npm run start`, `npm run test`) — lets a team standardize commands instead of everyone remembering different flags.
- **`dependencies`** — packages your app needs *to run*; populated automatically whenever you do `npm install <package>`.
- **`devDependencies`** — packages only needed *while developing* (test runners, linters), not in production; installed via `npm install <package> --save-dev`.

**Why `package.json` matters:** it's what makes a project portable. Instead of shipping the entire `node_modules` folder (which can be huge), you ship just `package.json` — anyone can clone your project and run `npm install` to redownload the exact dependencies listed, recreating `node_modules` locally.

## What is a "module"?

A **module** is a self-contained, reusable piece of code — typically one file — that explicitly **exports** the functionality it wants to make available, so other files can **import** and use it. Modularity solves a real problem: without it, every variable/function you write in one file would leak into a shared global scope, and two files defining a function with the same name would silently overwrite each other. Modules give each file its **own private scope** by default; nothing is accessible outside a module unless the module explicitly exports it.

### Three kinds of modules in Node

| Type | What it is | How you use it |
|---|---|---|
| **Core / built-in modules** | Ship with Node itself, written in C++/JS, no installation needed | `require('fs')`, `require('path')`, `require('http')` |
| **Local / user-defined modules** | Files you write yourself in your own project | `require('./utils')` — note the `./`, which tells Node this is a relative file path, not a package name |
| **Third-party modules** | Code published by others, installed via npm into `node_modules` | `require('express')` — no `./` prefix; Node looks inside `node_modules` |

## Module systems: CommonJS vs ES Modules

Node actually supports **two different syntaxes** for defining and using modules — this is a common point of confusion.

### CommonJS (`require` / `module.exports`) — Node's original system
```js
// math.js
function add(a, b) { return a + b; }
module.exports = { add };          // explicitly export what should be public

// app.js
const { add } = require('./math'); // import via require()
console.log(add(2, 3));            // 5
```
- **Synchronous** — `require()` loads and executes the target file immediately, blocking until it's done. Fine for local disk-based module loading (fast), but this is one reason CommonJS doesn't naturally fit browsers (where modules might load over a slow network).
- This has been Node's default and most widely used system historically; the vast majority of existing Node tutorials and packages use it.

### ES Modules (`import` / `export`) — the JS-standard system
```js
// math.mjs (or math.js with "type": "module" in package.json)
export function add(a, b) { return a + b; }

// app.mjs
import { add } from './math.mjs';
console.log(add(2, 3));
```
- This is the **official ECMAScript-standard** module syntax (same `import`/`export` you'd use in frontend React/Vite code — see [[ES6+ Modern Features]]) — Node added support for it later to align with the rest of the JS ecosystem.
- To use it in Node, either name files with a **`.mjs`** extension, or add `"type": "module"` to `package.json` (which then requires plain `.js` files in that project to use `import`/`export`, not `require`).

**Why two systems exist:** CommonJS existed first because ES Modules weren't standardized yet when Node was created (2009); ES Modules were added to the JS language spec in 2015 (ES6) and Node later added support to stay compatible with the language standard and the frontend ecosystem. Both still coexist because too much existing code depends on CommonJS to remove it.

## Common mistakes

- **Overwriting `exports` directly** instead of `module.exports`:
```js
// Broken — reassigns the local variable, doesn't affect what module.exports actually points to
exports = { add };

// Correct
module.exports = { add };
// or, if adding incrementally: exports.add = add;  (this works because it mutates the existing object)
```
- **Mixing `require` and `import` in the same file** — pick one system per project (or per file if using `.mjs`/`.cjs` extensions deliberately); mixing causes `SyntaxError` because Node parses each file assuming one system based on its extension/`package.json` config.
- **Forgetting `"type": "module"`** and then getting `SyntaxError: Cannot use import statement outside a module` when trying to use `import`.
- **Circular dependencies** — module A requires module B, which requires module A again — can result in one side receiving a partially-empty object, since Node returns whatever has been exported *so far* rather than waiting. Best avoided by restructuring shared logic into a third module both depend on.

## Related concepts
[[02 - Node.js Runtime vs Browser Environment]] — `require`/`module` are Node-only globals, unavailable in browsers
[[Package Managers and Build Tools]] — npm vs pnpm/yarn, and bundlers
[[ES6+ Modern Features]] — `import`/`export` syntax as used on the frontend
[[04 - File Handling in Node.js (fs module)]]
