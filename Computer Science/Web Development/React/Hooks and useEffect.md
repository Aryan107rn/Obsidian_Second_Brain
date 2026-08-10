---
tags: [react, javascript, frontend, web-development, computer-science, hooks]
aliases: [useEffect, React Hooks, Rules of Hooks]
created: 2026-08-09
---

# Hooks and useEffect

## What is it?

**Hooks** are functions starting with `use` that let function components tap into React features like state and lifecycle events, without writing a class. They were introduced in React 16.8 (2019) and are now the standard way to write React.

`useEffect` is the Hook for running **side effects** — code that reaches outside of rendering, like fetching data, subscribing to an event, or setting a timer.

## Why does it exist?

Before Hooks, only class components could hold state or run code at specific lifecycle points (mount, update, unmount), which meant plain function components were limited to pure display logic. Hooks let any function component do everything a class component could, with less boilerplate — this is why modern React code rarely uses classes.

## How does it work?

The two most common hooks:

| Hook | Purpose |
|---|---|
| [[State (useState)\|useState]] | Add state to a component |
| `useEffect` | Run side effects — code outside the normal render flow |

```jsx
import { useState, useEffect } from "react";

function UserProfile({ userId }) {
  const [user, setUser] = useState(null);

  useEffect(() => {
    fetch(`/api/users/${userId}`)
      .then(res => res.json())
      .then(data => setUser(data));
  }, [userId]); // dependency array: re-run this effect only when userId changes

  if (!user) return <p>Loading...</p>;
  return <p>{user.name}</p>;
}
```

### The dependency array

The **dependency array** (`[userId]`, the second argument to `useEffect`) tells React when to re-run the effect:

- `[]` (empty) — run once, after the first render only.
- `[userId]` — re-run whenever `userId` changes.
- omitted entirely — run after *every* render (rarely what you want).

### Rules of Hooks

Only call hooks at the top level of a component (never inside loops, conditions, or nested functions), and only from React function components or other hooks. React relies on hooks being called in the **exact same order** on every render to correctly match state to the right `useState`/`useEffect` call.


## Other common hooks (overview)

Beyond `useState` and `useEffect`, a few other hooks come up often:

| Hook | Used for |
|---|---|
| `useContext` | Reading shared data from a parent without manually passing props down through every level of the tree |
| `useRef` | Holding a mutable value that persists across renders **without** causing a re-render when it changes (e.g. referencing a DOM element directly) |
| `useMemo` / `useCallback` | Avoiding expensive recalculations or function recreation on every render (performance optimization) |

These are covered in more depth in their own notes when explored later (see [[React#Open Questions / To Explore Later]]).

## Custom hooks

You can write your **own** hooks — a regular function (name must start with `use`) that internally calls `useState`/`useEffect`/etc. — purely to extract and reuse stateful logic across multiple components.

```jsx
function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetch(url)
      .then(res => res.json())
      .then(data => {
        setData(data);
        setLoading(false);
      });
  }, [url]);

  return { data, loading };
}

// used in any component:
function UserProfile({ userId }) {
  const { data: user, loading } = useFetch(`/api/users/${userId}`);
  if (loading) return <p>Loading...</p>;
  return <p>{user.name}</p>;
}
```

This isn't a separate React feature — it's just the `use` naming convention plus the Rules of Hooks, applied to your own function. It's the standard way to avoid duplicating the same `useState`/`useEffect` logic across multiple components.

## Common mistakes

- **Forgetting the `useEffect` dependency array**: putting `[]` when the effect actually depends on a prop/state value causes the effect to keep using a stale, outdated value forever — known as a **stale closure**.
- **Calling hooks conditionally**: `if (x) { useState(...) }` breaks React's hook-order tracking and causes hard-to-debug errors.
- Omitting the dependency array entirely when you only meant to run something once — causes the effect to re-run after every single render, often creating infinite loops (e.g. an effect that itself sets state with no dependency array).

## Related concepts

- [[React]] — Hooks are how modern function-component React accesses state and lifecycle behavior
- [[State (useState)]] — `useState` is the most basic Hook
- [[Closures]] — Hooks rely on closures to "remember" values between renders
- [[REST APIs]] — `useEffect` + `fetch` is the typical pattern for loading data from a backend on component mount
