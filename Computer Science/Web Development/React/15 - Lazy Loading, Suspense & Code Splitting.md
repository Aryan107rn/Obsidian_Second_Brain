---
tags: [react, performance, code-splitting, javascript, frontend, web-development, computer-science, placement-prep]
aliases: [React.lazy, Suspense, Code Splitting]
created: 2026-08-15
---

# 15 - Lazy Loading, Suspense & Code Splitting

## What problem does this solve?

By default, a bundler ([[Package Managers and Build Tools|Vite/Webpack]]) packages your **entire app** into one JavaScript file (or a few large ones). As an app grows — dozens of routes, heavy libraries (charting, PDF viewers, rich text editors) — that bundle grows too, and the user must download and parse all of it before the app becomes interactive, even for a page or feature they may never visit.

**Code splitting** breaks the bundle into smaller chunks that load **on demand**, only when actually needed — e.g. the code for a `/settings` page only downloads when the user navigates there, not on initial load.

## `React.lazy` — lazily loading a component

```jsx
import { lazy } from "react";

const SettingsPage = lazy(() => import("./SettingsPage"));
```

`import("./SettingsPage")` (dynamic import syntax, not `import ... from`) tells the bundler: don't bundle this file into the main chunk — split it into its own separate file, fetched only when this line actually executes.

## `Suspense` — showing a fallback while it loads

Since the lazy component's code hasn't downloaded yet the first time it's needed, React needs something to display in the meantime. `Suspense` provides that fallback:

```jsx
import { lazy, Suspense } from "react";

const SettingsPage = lazy(() => import("./SettingsPage"));

function App() {
  return (
    <Suspense fallback={<p>Loading settings...</p>}>
      <SettingsPage />
    </Suspense>
  );
}
```

Flow: React renders the `fallback` immediately → fetches the `SettingsPage` chunk over the network → once loaded, swaps the fallback out for the real `SettingsPage` content.

## Combining with routing

The most common real-world use — split each route into its own chunk so users only download the code for pages they actually visit:

```jsx
import { lazy, Suspense } from "react";
import { Routes, Route } from "react-router-dom";

const Home = lazy(() => import("./pages/Home"));
const Settings = lazy(() => import("./pages/Settings"));
const Profile = lazy(() => import("./pages/Profile"));

function App() {
  return (
    <Suspense fallback={<p>Loading...</p>}>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/settings" element={<Settings />} />
        <Route path="/profile" element={<Profile />} />
      </Routes>
    </Suspense>
  );
}
```

See [[08 - React Router & Navigation]] for the routing pieces (`Routes`, `Route`) used here.

## When to use

- Route-based splitting — nearly always worth doing; each page/route becomes its own chunk (the most common and highest-value use case).
- Heavy, rarely-used features — a PDF export tool, a rich chart library, an admin-only panel — that most users never touch on a given visit.
- Anything gated behind a condition the user might never reach (a modal, an admin-only view).

## When NOT to use

- Small components used on every page load anyway — splitting them adds an extra network request for no benefit, since they'd need to load immediately either way.
- Over-splitting into too many tiny chunks — each chunk is a separate network request; too many small requests can hurt performance more than one moderately-sized bundle would, especially on slower connections.

## Common mistakes

- Forgetting to wrap a `lazy` component in `Suspense` — React throws an error if a lazy component suspends without a `Suspense` ancestor to catch it.
- Using `import()` (dynamic import) syntax incorrectly — it must be a function call returning a promise: `lazy(() => import("./X"))`, not `lazy(import("./X"))` (the latter starts the fetch immediately, defeating lazy loading, and doesn't match the retry mechanism `lazy` expects).
- Not providing a meaningful `fallback` — an empty or jarring fallback (e.g. nothing at all) creates a layout shift or blank flash when the real content pops in.

## Interview Q&A

### Q1: What's the difference between `React.lazy` and `Suspense`?
**Answer:** `React.lazy` defines *what* to load lazily — it wraps a dynamic `import()` so the component's code is split into a separate chunk and fetched only when rendered. `Suspense` defines *what to show while waiting* — it's the fallback UI mechanism that displays until the lazy component's code finishes downloading. They're used together: `lazy` creates the lazily-loaded component, `Suspense` catches its loading state.

### Q2: Why is route-based code splitting usually the highest-value place to apply this?
**Answer:** Because a user typically only interacts with one or a few routes per session — code for routes they never visit is pure waste if bundled upfront. Splitting per-route means the initial bundle only contains what's needed to render the first page, dramatically improving initial load time, while other routes load on-demand exactly when the user navigates to them.

## Related concepts

- [[00 - React MOC]]
- [[08 - React Router & Navigation]] — the most common pairing for code splitting
- [[Package Managers and Build Tools]] — the bundler mechanics (Vite/Webpack) that actually perform the chunk splitting
- [[01 - Introduction to React & Virtual DOM]] — SPA architecture this optimizes
