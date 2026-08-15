---
tags: [react, javascript, frontend, web-development, computer-science, example]
aliases: [React Todo App]
created: 2026-08-09
---

# Concrete Example: A Todo List

A small worked example combining [[04 - State & useState Hook]], [[03 - Components & Props]], and [[02 - JSX & Building Blocks]] into a single working component.

```jsx
import { useState } from "react";

function TodoApp() {
  const [todos, setTodos] = useState([]);
  const [input, setInput] = useState("");

  function addTodo() {
    if (input.trim() === "") return;
    setTodos([...todos, input]); // spread + append: never mutate state directly
    setInput("");
  }

  return (
    <div>
      <input value={input} onChange={(e) => setInput(e.target.value)} />
      <button onClick={addTodo}>Add</button>
      <ul>
        {todos.map((todo, i) => (
          <li key={i}>{todo}</li>
        ))}
      </ul>
    </div>
  );
}
```

Notes on what's happening:
- Two independent pieces of state: `todos` (the list) and `input` (the text box's current value) — this pattern of one `useState` per independent value is normal.
- `setTodos([...todos, input])` creates a **new array** rather than mutating `todos` — see [[04 - State & useState Hook#Common mistakes]] for why this matters.
- `key={i}` uses the array index as a key, which is acceptable here since items are only ever appended, not reordered — see [[11 - React Common Mistakes & Tricky Interview Questions]] for when this becomes a problem.

## Related concepts

- [[00 - React MOC]]
- [[04 - State & useState Hook]]
- [[03 - Components & Props]]
- [[11 - React Common Mistakes & Tricky Interview Questions]]
