---
tags: [react, javascript, frontend, web-development, computer-science, hooks]
aliases: [useEffect, React Hooks, Rules of Hooks]
created: 2026-08-09
updated: 2026-08-14
---

# Hooks and useEffect

## What is it?

**Hooks** are functions starting with `use` that let function components tap into React features like state and lifecycle events, without writing a class. They were introduced in React 16.8 (2019) and are now the standard way to write React.

`useEffect` is the Hook for running **side effects** — code that reaches outside of rendering, like fetching data, subscribing to an event, or setting a timer.

---

## 🖼️ useEffect Lifecycle & Cleanup Phases

![[react-useeffect-lifecycle.svg|960]]

---

## Why does it exist?

Before Hooks, only class components could hold state or run code at specific lifecycle points (mount, update, unmount), which meant plain function components were limited to pure display logic. Hooks let any function component do everything a class component could, with less boilerplate.

## How does it work?

The two most common hooks:

| Hook | Purpose |
|---|---|
| [[04 - State & useState Hook\|useState]] | Add state to a component |
| `useEffect` | Run side effects — code outside the normal render flow |

```jsx
import { useState, useEffect } from "react";

function UserProfile({ userId }) {
  const [user, setUser] = useState(null);

  useEffect(() => {
    let isMounted = true;
    fetch(`/api/users/${userId}`)
      .then(res => res.json())
      .then(data => { if (isMounted) setUser(data); });

    // Cleanup function runs on unmount or before re-running
    return () => { isMounted = false; };
  }, [userId]); // dependency array: re-run this effect only when userId changes

  if (!user) return <p>Loading...</p>;
  return <p>{user.name}</p>;
}
```

### The dependency array

The **dependency array** (`[userId]`, the second argument to `useEffect`) tells React when to re-run the effect:

- `[]` (empty) — run once, after the first render only (Mount).
- `[userId]` — re-run whenever `userId` changes.
- omitted entirely — run after *every* render (rarely what you want).

### Rules of Hooks

1. **Only call hooks at the top level**: Never inside loops, conditions, or nested functions.
2. **Only call hooks from React functions**: Either custom hooks or React functional components.

---

## Other common hooks (overview)

| Hook | Used for |
|---|---|
| `useContext` | Reading shared data from a parent without manually passing props down through every level of the tree |
| `useRef` | Holding a mutable value that persists across renders **without** causing a re-render when it changes |
| `useMemo` / `useCallback` | Avoiding expensive recalculations or function recreation on every render (performance optimization) |

---

## Custom hooks

Extract reusable stateful logic into a function starting with `use`:

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
```

---

## 🔗 Related concepts
- [[00 - React MOC]] — Hooks are how modern function-component React accesses state and lifecycle behavior
- [[04 - State & useState Hook]] — `useState` is the most basic Hook
- [[Closures]] — Hooks rely on closures to "remember" values between renders
- [[REST APIs]] — `useEffect` + `fetch` is the typical pattern for loading data from a backend
