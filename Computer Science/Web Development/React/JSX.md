---
tags: [react, javascript, frontend, web-development, computer-science]
aliases: [JavaScript XML]
created: 2026-08-09
---

# JSX

## What is it?

**JSX** ("JavaScript XML") is a syntax extension that lets you write HTML-like markup directly inside JavaScript. It's what makes React components look like this:

```jsx
const element = <h1>Hi there</h1>;
```

## Why does it exist?

Describing UI is easiest to read and write when it looks like the markup it produces. JSX lets you keep a component's structure (what it looks like) and logic (how it behaves) together in one file, instead of splitting HTML and JS into separate template files.

## How does it work?

JSX is **not valid JavaScript on its own** — browsers cannot run it directly. A build tool (Babel, or esbuild/SWC under Vite) compiles JSX into plain `React.createElement(...)` function calls before the browser ever sees the code:

```jsx
const element = <h1>Hi there</h1>;
// compiles roughly to:
const element = React.createElement("h1", null, "Hi there");
```

Because JSX compiles down to regular JS, you can embed any JavaScript expression inside curly braces `{ }`:

```jsx
const name = "Asha";
const element = <h1>Hello, {name}!</h1>;
```

This is why JSX needs a [[Package Managers and Build Tools|build tool]] (Vite, Webpack, etc.) — the conversion step must happen before the code reaches the browser.

## Example

```jsx
function Greeting() {
  const isLoggedIn = true;
  return (
    <div>
      <h1>Welcome</h1>
      {isLoggedIn && <p>You are logged in.</p>}
    </div>
  );
}
```

`{isLoggedIn && <p>...</p>}` is plain JS embedded in JSX — a common pattern for conditional rendering.

## Common mistakes

- Trying to run JSX files directly without a build step — it will fail; JSX always needs compiling first.
- Using `class` instead of `className` for CSS classes — `class` is a reserved word in JavaScript, so JSX uses `className`.
- Returning multiple sibling elements without wrapping them (JSX requires a single root element per return, or a `<>...</>` Fragment).

## Related concepts

- [[React]] — JSX is how React components describe their UI
- [[Components and Props]] — components are functions that return JSX
- [[Package Managers and Build Tools]] — the tooling that compiles JSX into plain JS
