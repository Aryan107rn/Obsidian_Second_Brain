---
tags: [javascript, tooling, npm, vite, react, web-development, computer-science]
aliases: [npm vs pnpm vs yarn, Vite, Package Managers, Bundlers, Build Tools]
created: 2026-08-09
---

# Package Managers and Build Tools

## What is it?

Setting up a modern JS/React project involves two **separate categories** of tools that are often confused because they're listed together:

1. **Package managers** (npm, pnpm, yarn) — install and manage third-party code ("dependencies") your project needs.
2. **Build tools / bundlers** (Vite, Webpack, Parcel, Next.js) — convert JSX into plain JS, combine many files into optimized bundles, and run a dev server with hot reload while you develop.

They solve different problems and can be mixed freely (e.g. `pnpm` + `Vite`, or `npm` + `Next.js`).

## Why do we need them?

The browser only understands plain HTML/CSS/JS. It cannot:
- Fetch and install third-party libraries like React itself → **package manager's job**.
- Understand JSX (`<div>` inside JS), or efficiently load dozens/hundreds of separate source files → **build tool's job**.

Without these tools you'd have to manually download every library's code and manually convert JSX to plain JS calls (`React.createElement(...)`) by hand — impractical at any real scale.

## Category 1: Package managers

A package manager reads `package.json` (the list of dependencies your project needs) and downloads the actual code into a `node_modules` folder.

| Tool | What it is | Key trait |
|---|---|---|
| **npm** | Node Package Manager — ships built-in with Node.js. The universal default. | Always available, simple; historically slower/more disk-heavy than alternatives |
| **yarn** | Meta's faster/more reliable alternative to early npm. | Similar usage to npm; introduced lockfiles and better caching before npm caught up |
| **pnpm** | "Performant npm." Keeps one global store of packages and **links** to it instead of copying into every project. | Much faster installs, saves disk space across many projects, stricter dependency resolution (catches bugs npm/yarn silently allow) |

**Analogy:** if `node_modules` is a library of books each project needs, npm/yarn each buy a fresh copy of every book per project (simple but wasteful). pnpm keeps one shared shelf and gives each project a pointer to it — same content, far less storage.

Commands are nearly identical across all three: `npm install react` / `pnpm add react` / `yarn add react`.

## Category 2: Build tools / bundlers

A bundler/build tool:
- Converts JSX → plain JS (via Babel, or esbuild/SWC in Vite's case).
- Bundles many source files into optimized output.
- Runs a **dev server** with **hot reload** — saved changes appear in the browser almost instantly without a full page refresh.

| Tool | Notes |
|---|---|
| **Vite** | Current default/recommended tool for React (and most frontend frameworks) as of 2026. Uses native ES modules + esbuild during dev, so startup and updates are near-instant even in large projects. |
| **Create React App (CRA)** | The old official standard (2016–2022). **Deprecated** — officially removed from React's docs. Avoid for new projects. |
| **Parcel** | "Zero-config" bundler, easy to pick up. Mostly displaced by Vite for React specifically. |
| **Webpack** | Highly configurable bundler that powered CRA and most of the React ecosystem for years. Still common in large/legacy codebases; slower and much more config-heavy than Vite. Rarely chosen to start new projects now. |
| **Next.js** | Not just a bundler — a full **framework** built on React adding server-side rendering, file-based routing, and its own build pipeline (Turbopack internally). Use when you need more than a client-side app (e.g. SEO, server rendering). |

### Babel — the compiler behind the bundler

A bundler needs something to actually convert JSX and modern JS syntax into plain, browser-compatible JS. That translation step is done by a **compiler**, and historically that compiler was **Babel**.

```jsx
// what you write
const element = <h1>Hi</h1>;

// what Babel outputs
const element = React.createElement("h1", null, "Hi");
```

- **Create React App / Webpack** historically used Babel directly for this JSX/syntax transformation.
- **Vite** uses **esbuild** for this same job during development instead — esbuild does the same kind of transformation but is written in Go, making it roughly 10–100x faster than Babel. Vite still uses Babel internally for some production-build cases, but you rarely configure it directly.

You don't usually interact with Babel yourself in a modern Vite project — it's invisible tooling. It's worth knowing the term mainly because ".babelrc" or "Babel plugin" still show up in older projects and docs.

**Common mistake**: confusing Babel/esbuild (the **compilers** that transform JSX/syntax) with Webpack/Vite (the **bundlers** that call those compilers as part of a larger pipeline) — they're different layers, not competing alternatives.

## Example: starting a project

```bash
# Vite + npm (most common recommendation today)
npm create vite@latest my-app -- --template react
cd my-app
npm install
npm run dev
```

```bash
# Vite + pnpm (faster installs)
pnpm create vite my-app --template react
cd my-app
pnpm install
pnpm dev
```

```bash
# Next.js, when you need SSR / routing / framework features
npx create-next-app@latest my-app
```

`npm create vite@latest` **scaffolds** a ready-made project (folders, configs, a sample component) instead of making you assemble everything file by file.

## Which should you use?

- **Bundler: Vite**, by default. It's the React team's current recommendation — fast, minimal config. Avoid CRA (dead) and raw Webpack (too much manual setup) unless you have a specific legacy reason.
- **Framework vs plain React**: need SEO, server rendering, or file-based routing → **Next.js**. Building a client-side-only app (dashboard, internal tool, SPA) → **Vite + React** is enough.
- **Package manager**: any of npm/pnpm/yarn work. **npm** is the safe universal default (ships with Node, zero setup). **pnpm** is worth adopting once you juggle multiple projects, for speed and disk savings. **yarn** is a fine middle ground but has lost mindshare to pnpm recently. No wrong choice for a beginner.

## Common mistakes

- Confusing "which bundler" with "which package manager" — they're independent choices and can be mixed (e.g. `pnpm create vite`).
- Starting a new project with Create React App — it's deprecated and no longer maintained.
- Assuming you always need Next.js — most simple client-side apps don't need SSR/routing baked in; plain Vite + React is lighter and sufficient.

## Related concepts

- [[00 - React MOC]] — the library these tools set up and serve
- [[ES6+ Modern Features]] — the `import`/`export` module syntax bundlers process
- [[JavaScript MOC]]
