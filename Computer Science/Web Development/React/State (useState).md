---
tags: [react, javascript, frontend, web-development, computer-science, hooks]
aliases: [useState, React State]
created: 2026-08-09
---

# State (useState)

## What is it?

**State** is data a component owns and manages itself, which can change over time — e.g. a counter value, whether a modal is open, or text typed into an input. Unlike [[Components and Props|props]], a component *can* update its own state, and doing so is what triggers React to re-render it.

## Why does it exist?

Components need a way to "remember" and react to changing data that originates inside themselves (not passed down from a parent) — user input, toggles, fetched data, etc. `useState` is React's mechanism for this.

## How does it work?

State is managed with the `useState` **Hook**:

```jsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0); // 0 is the initial value

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

- `count` — the current value.
- `setCount` — the *only* correct way to update it. Calling `setCount` tells React "re-render this component with the new value." Directly mutating `count` (e.g. `count = count + 1`) will **not** trigger a re-render and is a bug.
- `useState(0)` — `0` is used only on the very first render; React remembers the current value across re-renders internally.

## Example: updating array/object state immutably

```jsx
const [todos, setTodos] = useState([]);

function addTodo(text) {
  setTodos([...todos, text]); // spread + append: creates a NEW array
}
```

`setTodos([...todos, text])` is used instead of `todos.push(text)` — see Common Mistakes below for why.

## Common mistakes

- **Mutating state directly**: `todos.push(text)` then calling `setTodos(todos)` often fails to re-render, because React compares old and new state **by reference** — pushing mutates the same array reference, so React thinks nothing changed. Always create a new array/object (`setTodos([...todos, text])`, `setUser({...user, name: "new"})`).
- Reading the state variable immediately after calling its setter and expecting the new value — see the async note below.

## Edge cases / Important details

- State setters (like `setCount`) are **asynchronous** in effect — React may batch multiple `setState` calls together and apply them in one re-render, so reading the state variable immediately after calling its setter still shows the old value within that same function.
- A component re-renders whenever its own state changes, or whenever its parent re-renders (which by default re-renders all of that parent's children too, unless optimized with `React.memo`, `useMemo`, or `useCallback` — advanced topics, not yet covered).
- Function components run their **entire function body** on every render — anything expensive inside the function body (not inside `useEffect` or `useMemo`) re-runs every time.

## Related concepts

- [[React]] — state is one of React's two core data sources (alongside props)
- [[Components and Props]] — state is owned by a component, unlike props which are received
- [[Hooks and useEffect]] — `useState` is one of React's built-in Hooks
- [[Objects Destructuring Spread Rest]] — `{...obj}` / `[...arr]` spreading is central to updating state immutably
- [[Closures]] — `useState` setter callbacks rely on closures to "remember" values across renders
