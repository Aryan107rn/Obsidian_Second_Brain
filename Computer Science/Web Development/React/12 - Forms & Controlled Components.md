---
tags: [react, forms, javascript, frontend, web-development, computer-science, placement-prep]
aliases: [Controlled Components, Uncontrolled Components, React Forms]
created: 2026-08-15
---

# 12 - Forms & Controlled Components

## What problem does this solve?

Forms are the primary way users send data *into* your app (login, search, checkout, settings). Plain HTML lets form inputs manage their own value internally — you'd have to reach into the DOM to read it. React's declarative model (state → UI) needs a way to keep an input's value in sync with component state instead, so the rest of your app can react to what the user typed as they type it (live validation, character counters, conditionally enabling a submit button, etc.).

## Controlled components — the standard approach

A **controlled component** is an input whose value is driven entirely by React state — the DOM element never holds its own "source of truth" value.

```jsx
function LoginForm() {
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");

  function handleSubmit(e) {
    e.preventDefault(); // stop the browser's default full-page-reload form submission
    console.log({ email, password });
  }

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
      />
      <input
        type="password"
        value={password}
        onChange={(e) => setPassword(e.target.value)}
      />
      <button type="submit">Log in</button>
    </form>
  );
}
```

The flow, every keystroke:
1. User types a character → browser fires the input's `onChange`.
2. `setEmail(e.target.value)` updates state with the new full string.
3. React re-renders, passing the **new** state value back into `value={email}`.
4. The input visually shows the new value — but only because React told it to, not because the DOM updated itself.

This round-trip (type → event → state update → re-render → input reflects state) is why it's called "controlled": React is in full control of what the input displays at all times.

### Why `e.preventDefault()` matters

A `<form>`'s default browser behavior on submit is to navigate to a new URL (a full page reload), which would blow away all your React state and app. `e.preventDefault()` stops that so you can handle the submission with JavaScript (e.g. an API call) while staying a single-page app.

## Uncontrolled components — the alternative

An **uncontrolled component** lets the DOM manage the input's value itself; React only reaches in to *read* the value when needed, via a [[06 - Advanced Built-in Hooks|`useRef`]]:

```jsx
function UncontrolledLogin() {
  const emailRef = useRef(null);

  function handleSubmit(e) {
    e.preventDefault();
    console.log(emailRef.current.value); // read the DOM value directly, only on submit
  }

  return (
    <form onSubmit={handleSubmit}>
      <input type="email" ref={emailRef} defaultValue="" />
      <button type="submit">Log in</button>
    </form>
  );
}
```

Note `defaultValue` instead of `value` — this just sets the *initial* DOM value once; React doesn't touch it afterward.

## Controlled vs uncontrolled — when to use each

| | Controlled | Uncontrolled |
|---|---|---|
| Source of truth | React state | The DOM itself |
| Re-renders on every keystroke | Yes | No |
| Live validation / char counters | Easy (value is always in state) | Harder (must read DOM manually) |
| Best for | Most forms — validation, conditional UI, controlled submit buttons | Very simple forms, file inputs (which *can't* be controlled — browsers don't allow setting a file input's value programmatically for security reasons), performance-critical forms with many fields where per-keystroke re-renders are a measured bottleneck |

**Default recommendation**: use controlled components unless you have a specific reason not to (file inputs, or a measured performance problem with a very large form).

## Common mistakes

- **Forgetting `onChange` on a controlled input**: setting `value={email}` without `onChange` makes the input **read-only** — React always resets it back to the (unchanging) state value, so the user's keystrokes appear to do nothing. React will also warn about this in the console.
- **Switching between controlled and uncontrolled**: starting an input's `value` as `undefined` (e.g. `value={user?.email}` before `user` loads) then later giving it a real string flips it from uncontrolled to controlled mid-lifetime, which React warns about. Fix: initialize state to `""`, never `undefined`.
- **Not calling `e.preventDefault()`** in `onSubmit` — causes an unwanted full page reload.
- Reading `e.target.value` asynchronously after the event handler has already returned — React 17+ no longer pools/reuses the event object in a way that invalidates this by default, but in general, extract the value synchronously within the handler to be safe.

## Interview Q&A

### Q1: What's the fundamental difference between controlled and uncontrolled components?
**Answer:** In a controlled component, the input's displayed value comes from React state — every render, React explicitly sets `value`, so the input can never diverge from state. In an uncontrolled component, the DOM node holds its own value internally, and React only reads it on demand via a ref. Controlled components make React the single source of truth; uncontrolled components let the DOM be the source of truth.

### Q2: Why does React warn "a component is changing an uncontrolled input to be controlled"?
**Answer:** This happens when an input's `value` prop starts as `undefined` or `null` (making it uncontrolled) and later becomes a defined string (making it controlled) across re-renders. React can't switch an input's control mode mid-lifetime cleanly, so it warns. Fix by always initializing state to a defined empty value (`""`) rather than `undefined`.

## Related concepts

- [[00 - React MOC]]
- [[04 - State & useState Hook]] — controlled inputs are driven by `useState`
- [[06 - Advanced Built-in Hooks]] — `useRef` is how uncontrolled inputs are read
- [[11 - React Common Mistakes & Tricky Interview Questions]]
