---
tags: [react, error-handling, javascript, frontend, web-development, computer-science, placement-prep]
aliases: [componentDidCatch, Error Boundary]
created: 2026-08-15
---

# 13 - Error Boundaries

## What problem does this solve?

By default, if any component throws a JavaScript error during rendering, React doesn't just fail that one component — it **unmounts the entire component tree** below the nearest root, leaving the user with a blank white screen and no way to recover. A single broken widget (say, a third-party chart that throws on bad data) can take down an entire page.

**Error boundaries** are components that catch JavaScript errors anywhere in their child component tree, log them, and display a fallback UI instead of crashing the whole app.

## How it works

An error boundary is defined using two specific **class component** lifecycle methods — this is one of the few places in modern React where you still need a class, because there is no Hook equivalent for this (as of React 18/19, no `useErrorBoundary` Hook exists).

```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  // Called during rendering, after a descendant throws — updates state to trigger fallback UI
  static getDerivedStateFromError(error) {
    return { hasError: true };
  }

  // Called after a descendant throws — used for logging/side effects, not rendering
  componentDidCatch(error, errorInfo) {
    console.error("Caught by ErrorBoundary:", error, errorInfo);
    // e.g. send to an error-tracking service like Sentry
  }

  render() {
    if (this.state.hasError) {
      return <h2>Something went wrong. Please refresh.</h2>;
    }
    return this.props.children;
  }
}
```

Usage — wrap it around any part of the tree you want isolated:

```jsx
<ErrorBoundary>
  <UnreliableWidget />
</ErrorBoundary>
```

If `UnreliableWidget` throws during render, only the boundary's fallback UI shows — the rest of the app (header, sidebar, other widgets) keeps working normally.

## What error boundaries do NOT catch

This is the most commonly tested/misunderstood part:

- **Event handler errors** (e.g. a `try/catch`-less bug inside `onClick`) — these don't crash rendering, so use a regular `try/catch` inside the handler instead.
- **Asynchronous code** — errors inside `setTimeout`, promises, or `async` functions (including inside `useEffect`) are not caught, since the error happens outside React's render call stack.
- **Server-side rendering errors.**
- **Errors thrown in the error boundary's own code** (e.g. inside `componentDidCatch` itself).

## Why no Hook version exists

Error boundaries rely on `getDerivedStateFromError` and `componentDidCatch`, which are tied to the class component lifecycle's error-catching mechanism at the React reconciler level — there's currently no Hook-based equivalent, so this remains one of the rare cases where a class component is still necessary in modern React. In practice, most teams write **one reusable `ErrorBoundary` class** (or use a well-maintained library like `react-error-boundary`) once, then use it everywhere as a plain wrapper component — you rarely write a new class for each use case.

## When to use

- Around independent, isolated sections of a page (a widget, a route, a third-party embed) so one failure doesn't take down the whole app.
- At the top level of a route, so a bug on one page doesn't blank the entire application.
- Around components rendering data you don't fully trust (e.g. from a third-party API).

## Common mistakes

- Expecting an error boundary to catch errors thrown inside `useEffect` or event handlers — it only catches errors thrown during rendering.
- Wrapping the *entire* app in a single error boundary with no finer-grained boundaries — one crash anywhere still blanks everything; place boundaries around independent sections for better fault isolation.
- Trying to write an error boundary as a function component with hooks — not currently possible; it must be a class (or use a library that wraps this class for you).

## Interview Q&A

### Q1: What errors does an error boundary NOT catch?
**Answer:** Errors in event handlers, asynchronous code (`setTimeout`, promises, `async/await`, including inside `useEffect`), server-side rendering, and errors thrown within the error boundary itself.

### Q2: Why can't you write an error boundary using Hooks?
**Answer:** Error boundaries rely on the class lifecycle methods `getDerivedStateFromError` and `componentDidCatch`, which hook into React's reconciler-level error handling. As of the current React version, there is no Hook equivalent for this specific capability, making this one of the few remaining legitimate uses of class components in modern React.

## Related concepts

- [[00 - React MOC]]
- [[01 - Introduction to React & Virtual DOM]] — rendering and the component tree these boundaries interrupt
- [[05 - Hooks & useEffect Hook]] — contrast: async/effect errors are NOT caught by error boundaries
- [[11 - React Common Mistakes & Tricky Interview Questions]]
