---
tags: [react, javascript, frontend, web-development, computer-science]
aliases: [React Components, Props]
created: 2026-08-09
---

# Components and Props

## What is it?

A **component** is a JavaScript function that returns a description of UI (via [[JSX]]). Components can be nested inside each other, forming a tree, just like HTML elements nest.

**Props** ("properties") are how a parent component passes data down to a child component — similar to function arguments.

## Why does it exist?

Breaking a UI into components lets you build small, independent, reusable pieces instead of one giant page description. Props are the mechanism that lets those independent pieces receive the data they need from whoever is using them, while keeping data flow predictable.

## How does it work?

### Components and nesting

```jsx
function Greeting() {
  return <h1>Hello, world!</h1>;
}

function App() {
  return (
    <div>
      <Greeting />
      <p>Welcome to React.</p>
    </div>
  );
}
```

`App` is the **parent**; `Greeting` is its **child**.

### Props: passing data down

```jsx
function Greeting(props) {
  return <h1>Hello, {props.name}!</h1>;
}

<Greeting name="Asha" />
<Greeting name="Ravi" />
```

Each `<Greeting />` call is independent and renders with its own `name`. Props are **read-only** — a component must never modify the props it receives; data flows **one-way**, from parent to child only.

## Common mistakes

- **Treating props as mutable**: `props.name = "new"` inside a component is a bug. If a child needs to change something, the parent must pass down a function (often paired with [[State (useState)|state]]) that the child calls — the parent still owns the update.
- Forgetting that each rendered instance of a component (e.g. two `<Greeting />` calls) is fully independent — they don't share data unless explicitly passed the same props or state.

## Related concepts

- [[React]] — components are React's core building block
- [[JSX]] — what components return to describe their UI
- [[State (useState)]] — data a component manages itself, as opposed to props received from a parent
- [[Functions in JavaScript]] — components are just JavaScript functions
