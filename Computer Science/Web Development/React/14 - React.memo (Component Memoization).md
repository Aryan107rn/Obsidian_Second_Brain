---
tags: [react, performance, javascript, frontend, web-development, computer-science, placement-prep]
aliases: [React.memo, memo]
created: 2026-08-15
---

# 14 - React.memo (Component Memoization)

## What problem does this solve?

By default, when a parent component re-renders, **every one of its child components re-renders too** — even if that specific child's props didn't change at all. For a small app this is invisible (React's Virtual DOM diffing makes even "wasted" re-renders cheap), but in a large tree with expensive child components (heavy computations, large lists, complex charts), this default behavior can cause real, measurable slowness.

`React.memo` lets a component **skip re-rendering** if its props are unchanged from the previous render.

## How it works

`React.memo` wraps a component and returns a new, memoized version:

```jsx
const ExpensiveRow = React.memo(function ExpensiveRow({ item }) {
  console.log("Rendering row:", item.id);
  return <li>{item.name}</li>;
});

function List({ items, unrelatedCounter }) {
  return (
    <div>
      <p>Unrelated counter: {unrelatedCounter}</p>
      <ul>
        {items.map((item) => (
          <ExpensiveRow key={item.id} item={item} />
        ))}
      </ul>
    </div>
  );
}
```

When `unrelatedCounter` changes and `List` re-renders, each `ExpensiveRow` normally would too. But because it's wrapped in `React.memo`, React does a **shallow comparison** of its previous vs new props (`item`) — since `item` didn't change, React skips re-rendering that row entirely.

## Shallow comparison — the critical detail

`React.memo`'s default comparison is **shallow**: it checks `===` (reference equality) on each prop, one level deep. This matters enormously for objects, arrays, and functions:

```jsx
// Parent re-renders and creates a NEW object every time, even with the same values:
<ExpensiveRow item={{ id: 1, name: "Apple" }} />
```

Here, `{ id: 1, name: "Apple" }` is a **brand new object reference** on every render of the parent, even though its contents look identical. `React.memo`'s shallow comparison sees a different reference and re-renders anyway — the memoization silently does nothing.

This is exactly why `React.memo` is almost always paired with [[06 - Advanced Built-in Hooks|`useMemo` (for object/array props) and `useCallback` (for function props)]] in the parent — they ensure the same reference is passed down when the underlying values haven't actually changed:

```jsx
function List({ items }) {
  const handleClick = useCallback((id) => {
    console.log("clicked", id);
  }, []); // same function reference across renders

  return items.map((item) => (
    <ExpensiveRow key={item.id} item={item} onClick={handleClick} />
  ));
}
```

## Custom comparison function

`React.memo` accepts an optional second argument for custom comparison logic, when shallow comparison isn't precise enough:

```jsx
const Row = React.memo(
  function Row({ item }) { /* ... */ },
  (prevProps, nextProps) => prevProps.item.id === nextProps.item.id // return true to SKIP re-render
);
```

Note the inverted logic versus typical equality checks: this function returns `true` when props are considered equal (skip re-render), unlike most JS comparison conventions.

## When to use

- A component is expensive to render (large lists, heavy computation, complex SVG/canvas work) **and** you've confirmed via profiling (React DevTools Profiler) that it's actually re-rendering unnecessarily and that this is a measured performance problem.
- The component tends to receive the same props repeatedly while its parent re-renders for unrelated reasons.

## When NOT to use

- By default, on every component "just in case" — the shallow comparison itself has a small cost, and for cheap components this cost can exceed the savings. Premature memoization adds code complexity for zero real benefit.
- When props are objects/functions recreated fresh every render without `useMemo`/`useCallback` — the memoization will silently never trigger, giving you the complexity cost with none of the benefit.

## Common mistakes

- Wrapping a component in `React.memo` but passing it new object/array/function literals as props every render — defeats the memoization entirely (see shallow comparison above).
- Assuming `React.memo` prevents re-renders triggered by the component's *own* internal state or context changes — it only blocks re-renders caused by the *parent* re-rendering with unchanged props. If the memoized component uses `useState` or `useContext` internally, it still re-renders normally when those change.
- Reaching for `React.memo` everywhere as a default habit instead of profiling first to find actual bottlenecks.

## Interview Q&A

### Q1: Does `React.memo` prevent a component from ever re-rendering?
**Answer:** No. It only skips re-renders caused by the parent re-rendering with unchanged (shallowly-equal) props. If the memoized component has its own internal state (`useState`) or consumes context (`useContext`) that changes, it re-renders normally regardless of `React.memo`.

### Q2: Why does wrapping a component in `React.memo` sometimes have no effect at all?
**Answer:** Usually because the parent passes a new object, array, or function literal as a prop on every render (e.g. inline `{...}`, `[...]`, or arrow functions). `React.memo`'s default shallow comparison checks reference equality, and a fresh literal is always a new reference — so the comparison always finds "different props" even if the actual values are identical. Fixing this requires wrapping those props with `useMemo`/`useCallback` in the parent so the same reference is reused when values don't change.

## Related concepts

- [[00 - React MOC]]
- [[06 - Advanced Built-in Hooks]] — `useMemo` and `useCallback`, which `React.memo` typically depends on to be effective
- [[01 - Introduction to React & Virtual DOM]] — reconciliation, which `React.memo` short-circuits for a subtree
