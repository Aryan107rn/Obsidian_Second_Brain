---
tags: [react, hooks, architecture, clean-code, web-development, computer-science, placement-prep]
aliases: [Custom Hooks, useFetch, useLocalStorage]
created: 2026-08-15
---

# 07 - Custom Hooks

Custom hooks are one of React’s most powerful design features. They allow you to extract complex stateful logic, side effects, and lifecycle behaviors into reusable, clean JavaScript functions.

---

## 💡 What is a Custom Hook?

A **custom hook** is a regular JavaScript function whose name starts with the prefix **`use`** (e.g., `useFetch`, `useAuth`) and which can call other React hooks (`useState`, `useEffect`, `useContext`, etc.).

### Why Use Custom Hooks?
1. **Stateful Logic Reuse:** Instead of duplicating data-fetching, form-validation, or local storage syncing across 5 components, write it once in a custom hook.
2. **Separation of Concerns:** Keep your components focused on rendering UI. Complex logic belongs in separate files, making components extremely clean and readable.
3. **Testability:** Custom hooks can be unit tested independently of UI components (e.g., using `@testing-library/react-hooks`).

---

## 🚦 Golden Rules of Custom Hooks

* **Rule 1: Name MUST start with `use`.** This tells React and your linter (ESLint) to apply the Rules of Hooks (such as checking that hooks are only called at the top level and not inside conditionals/loops).
* **Rule 2: State is NOT shared!** This is the most common misconception. Calling a custom hook is exactly like running a helper function. Every component that calls a custom hook gets its own completely isolated state and effect contexts. 

```
Component A ──> useLocalStorage("theme") ──> Local state [theme_A, setTheme_A]
Component B ──> useLocalStorage("theme") ──> Local state [theme_B, setTheme_B]
(Updating A's theme does NOT affect B's theme, though both sync to localStorage)
```

---

## 🛠️ Practical, Production-Ready Examples

Here are three industry-standard custom hooks that you will be asked to write in senior-level React coding interviews.

### 🥇 Example A: `useFetch` (Data Fetching with AbortController)
This custom hook handles loading states, error catching, and handles the critical edge case of **network race conditions/stale component unmounting** by aborting requests.

```jsx
import { useState, useEffect } from "react";

export function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    // 1. Instantiating AbortController for cancellation
    const controller = new AbortController();
    setLoading(true);
    setError(null);

    fetch(url, { signal: controller.signal })
      .then((res) => {
        if (!res.ok) throw new Error("Failed to fetch data");
        return res.json();
      })
      .then((data) => setData(data))
      .catch((err) => {
        if (err.name !== "AbortError") {
          setError(err.message);
        }
      })
      .finally(() => setLoading(false));

    // 2. Cleanup: Abort request if component unmounts or URL changes
    return () => {
      controller.abort();
    };
  }, [url]);

  return { data, loading, error };
}
```

---

### 🥈 Example B: `useLocalStorage` (Syncing State to browser storage)
This hook behaves exactly like a standard `useState`, but it automatically synchronizes the value to browser `localStorage` on update.

```jsx
import { useState, useEffect } from "react";

export function useLocalStorage(key, initialValue) {
  // Read value from localStorage on initialization, fallback to initialValue
  const [state, setState] = useState(() => {
    try {
      const item = localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      console.error("localStorage reading error:", error);
      return initialValue;
    }
  });

  // Sync with localStorage on change
  useEffect(() => {
    try {
      localStorage.setItem(key, JSON.stringify(state));
    } catch (error) {
      console.error("localStorage writing error:", error);
    }
  }, [key, state]);

  return [state, setState];
}
```

---

### 🥉 Example C: `useWindowSize` (DOM Event Listeners)
This hook registers a resize event listener on the window and automatically returns the current viewport dimensions.

```jsx
import { useState, useEffect } from "react";

export function useWindowSize() {
  const [windowSize, setWindowSize] = useState({
    width: window.innerWidth,
    height: window.innerHeight,
  });

  useEffect(() => {
    const handleResize = () => {
      setWindowSize({
        width: window.innerWidth,
        height: window.innerHeight,
      });
    };

    // 1. Add Event Listener
    window.addEventListener("resize", handleResize);

    // 2. Clean up event listener on unmount to prevent memory leaks!
    return () => {
      window.removeEventListener("resize", handleResize);
    };
  }, []); // Run on mount and unmount only

  return windowSize;
}
```

---

## 💼 Placement & Interview Q&A

### Q1: Do custom hooks share state between different components?
**Answer:** **No.** Custom hooks share *stateful logic* (the code structure and state/effect declarations), but not the actual *state values* themselves. Every time a component invokes a custom hook, a fresh, isolated state bucket is allocated internally by React for that specific component instance.

### Q2: What happens if you call a custom hook twice inside the same component?
**Answer:** Two completely independent states are generated. For example, if you call `const [num1, setNum1] = useLocalStorage('key1', 0)` and `const [num2, setNum2] = useLocalStorage('key2', 10)` in the same component, they will maintain separate states and sync to different localStorage keys without interfering with each other.

### Q3: Why is a custom hook better than a regular utility function?
**Answer:** A regular utility function can only perform pure mathematical or processing operations; it cannot interact with the React lifecycle, trigger component re-renders, or call React hooks like `useState`, `useEffect`, or `useContext`. Custom hooks can trigger re-renders, bind browser side-effects to component lifecycle phases, and tap directly into React's reactive system.

### Q4: Why is it important to use `AbortController` or a mounting flag inside custom data-fetching hooks?
**Answer:** Without aborting or checking mounting state, a network request could complete *after* the component has already unmounted or *after* the user has changed parameters (e.g. searching for a different user). When the request resolves, calling the state setter on an unmounted component or updating it with stale parameters causes memory leaks, react console warnings, and visual bugs (race conditions).

---

## 🔗 Related Concepts
- [[04 - State & useState Hook]] — The foundation of custom state hooks
- [[05 - Hooks & useEffect Hook]] — Cleanups and listeners in custom hooks
- [[06 - Advanced Built-in Hooks]] — Combining contexts and refs inside custom hooks
- [[10 - Practical Project Guide - Todo List and Beyond]] — Utilizing custom hooks in real-world features