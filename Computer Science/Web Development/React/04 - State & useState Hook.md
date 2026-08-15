---
tags: [react, javascript, frontend, web-development, computer-science, hooks]
aliases: [useState, React State]
created: 2026-08-09
updated: 2026-08-14
---

# State (useState)

## What is it?

**State** is data a component owns and manages itself, which can change over time — e.g. a counter value, whether a modal is open, or text typed into an input. Unlike [[03 - Components & Props|props]], a component *can* update its own state, and doing so is what triggers React to re-render it.

---

## 🖼️ useState Render & Update Lifecycle

![[react-usestate-lifecycle.svg|960]]

---

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

- State setters (like `setCount`) are **asynchronous** in effect — React batches multiple `setState` calls together and applies them in one re-render, so reading the state variable immediately after calling its setter still shows the old value within that same function.
- **Functional updates** should be used when the next state depends on the previous state:
  ```jsx
  setCount((prev) => prev + 1); // Guarantees correct value even in batched updates
  ```

## Related concepts

- [[00 - React MOC]] — state is one of React's two core data sources (alongside props)
- [[03 - Components & Props]] — state is owned by a component, unlike props which are received
- [[05 - Hooks & useEffect Hook]] — `useState` is one of React's built-in Hooks
- [[Objects Destructuring Spread Rest]] — `{...obj}` / `[...arr]` spreading is central to updating state immutably
- [[Closures]] — `useState` setter callbacks rely on closures to "remember" values across renders
