---
tags: [react, javascript, frontend, web-development, computer-science]
aliases: [React Pitfalls, React Gotchas]
created: 2026-08-09
---

# React Common Mistakes and Edge Cases

A consolidated reference of mistakes and subtleties that span multiple React concepts. See the individual topic notes ([[State (useState)]], [[Hooks and useEffect]], [[Components and Props]]) for mistakes specific to each.

## Missing or wrong `key` prop in lists

When rendering a list with `.map()`, each item needs a stable, unique `key` prop so React can track which item is which across re-renders:

```jsx
{todos.map((todo, i) => (
  <li key={i}>{todo}</li>
))}
```

Using the array **index** as a key works for static lists but causes bugs (wrong item updates, lost input focus) if the list can be reordered or filtered. Prefer a stable unique ID from the data itself when available (e.g. `todo.id`).

## Mutating state directly

Covered in depth in [[State (useState)]]: always create a new array/object rather than mutating the existing one, since React detects changes by reference comparison.

## Calling hooks conditionally or in loops

Covered in depth in [[Hooks and useEffect]]: hooks must run in the same order on every render.

## Treating props as mutable

Covered in depth in [[Components and Props]]: props flow one-way, parent to child; a component must never write to its own `props`.

## Related concepts

- [[React]]
- [[State (useState)]]
- [[Hooks and useEffect]]
- [[Components and Props]]
