---
tags: [react, jsx, frontend, compilation, web-development, computer-science, placement-prep]
aliases: [JavaScript XML, Babel Compilation, SWC]
created: 2026-08-09
updated: 2026-08-15
---

# 02 - JSX & Building Blocks

## 💡 What is JSX?

**JSX** stands for **JavaScript XML**. It is a syntax extension for JavaScript that allows you to write HTML-like markup directly inside a JavaScript file. It combines the visual structure of HTML with the logical power of JavaScript in a single, cohesive file.

```jsx
const element = <h1 className="title">Hello World</h1>;
```

---

## ⚙️ How JSX Works: Under the Hood

Browsers **cannot** read JSX directly. It is invalid JavaScript syntax. 

### 1. The Compilation Step
Before reaching the browser, your JSX code must be compiled (transpiled) into standard, browser-compatible JavaScript. This compilation is performed by build tools like **Babel** or modern rust-based transpilers like **SWC** (used by Vite and Next.js).

```jsx
// 1. What you write in JSX:
const element = <h1 className="title">Hello World</h1>;

// 2. What it compiles to (Classic React < 17):
const element = React.createElement("h1", { className: "title" }, "Hello World");
```

`React.createElement(...)` returns a plain JavaScript object representing a Virtual DOM element:
```javascript
// 3. The resulting Virtual DOM Node (React Element):
{
  type: "h1",
  props: {
    className: "title",
    children: "Hello World"
  }
}
```

### 2. The React 17+ JSX Transform (No `React` import required)
* **Pre-React 17:** Since JSX compiled to `React.createElement(...)`, you were **forced** to write `import React from 'react';` at the top of every single file containing JSX, even if you never called `React` directly in your code.
* **React 17 & 18+:** React introduced a new JSX transform in collaboration with Babel. The compiler automatically imports special functions from the React package behind-the-scenes (like `_jsx('h1', { ... })`). You **no longer need** to write `import React from 'react';` just to use JSX.

---

## 🚦 The 3 Golden Rules of JSX

To write bug-free JSX, you must adhere to three strict syntactic rules:

### 1. Return a Single Root Element (Fragments)
A function can only return one value. Since JSX compiles to function calls (`_jsx(...)`), you cannot return multiple independent elements unless they are wrapped inside a single parent element.

#### ❌ Incorrect (Will trigger compile error):
```jsx
function BadGreeting() {
  return (
    <h1>Hello</h1>
    <p>Nice to meet you.</p>
  );
}
```

####  Correct (Using a Wrapper Div or a Fragment):
If you don't want to pollute the real DOM with redundant nesting `<div>` elements, use a **React Fragment** (`<>...</>`):
```jsx
function GoodGreeting() {
  return (
    <>
      <h1>Hello</h1>
      <p>Nice to meet you.</p>
    </>
  );
}
```

### 2. Close All Tags
In HTML, some tags (like `<img>`, `<br>`, `<input>`) do not require a closing tag. In JSX, **every single tag must be closed**, either explicitly or via self-closing syntax.

* **HTML:** `<input type="text">`
* **JSX:** `<input type="text" />` (Self-closing is mandatory)

### 3. Use `camelCase` for Most Attributes
Because JSX compiles down to JavaScript, and JavaScript has reserved keywords, JSX attributes map directly to JavaScript DOM properties rather than HTML attributes.

* Use `className` instead of `class` (`class` is a reserved keyword in JS for class declarations).
* Use `htmlFor` instead of `for` (`for` is reserved for loop declarations).
* Inline styles must be passed as JavaScript objects with `camelCase` keys:
  ```jsx
  // HTML: <div style="background-color: blue; font-size: 14px;">
  // JSX:
  <div style={{ backgroundColor: "blue", fontSize: "14px" }}></div>
  ```
  *(Note the double curly braces: the outer `{}` is the JSX evaluation block, and the inner `{}` is the JS object literal).*

---

## 🧩 Embedding Dynamic JavaScript expressions `{}`

You can embed any valid JavaScript expression (anything that evaluates to a value) inside JSX using curly braces `{}`:

```jsx
function WelcomeCard() {
  const name = "Asha";
  const points = [10, 20, 30];
  
  return (
    <div className="card">
      <h2>Welcome back, {name}!</h2>
      <p>Your high score is {Math.max(...points)}.</p>
    </div>
  );
}
```

### ⚠️ Statements are NOT Expressions!
You **cannot** place JavaScript statements (like `if` statements, `for` loops, or function declarations) inside JSX curly braces. You must use conditional expressions (ternary operators or logical `&&` operators) or maps instead.

#### Conditional Rendering Pattern
```jsx
function Notification({ count }) {
  return (
    <div>
      {/* 1. Ternary Operator (if-else) */}
      {count > 0 ? <p>You have {count} alerts.</p> : <p>All caught up!</p>}

      {/* 2. Short-circuit Evaluation (if only) */}
      {count > 10 && <span className="warning">Urgent!</span>}
    </div>
  );
}
```

---

## 💼 Placement & Interview Q&A

### Q1: What is JSX and why does it need to be compiled?
**Answer:** JSX is a syntax extension for JavaScript that allows developers to write visual UI structures using an HTML-like syntax inside JS files. It needs to be compiled because browsers do not natively recognize JSX syntax. Compilation tools like Babel or SWC transform JSX elements into standard `React.createElement` (or `_jsx`) function calls, which evaluate to plain JavaScript objects representing Virtual DOM nodes.

### Q2: What is the difference between classic JSX compilation and the React 17+ JSX Transform?
**Answer:** In classic JSX compilation, every JSX tag compiled directly to `React.createElement(...)`, meaning the `React` package had to be imported into every file using JSX. In React 17+, the compiler automatically imports optimized JSX runtime functions (`react/jsx-runtime`) under the hood, removing the requirement to write `import React from 'react';` at the top of your files.

### Q3: What is a React Fragment (`<>...</>`) and why is it used?
**Answer:** A React Fragment is a special built-in component that allows you to group multiple sibling JSX elements together without adding a physical node (like a redundant `<div>`) to the actual DOM. It compiles to empty nodes, preventing DOM bloat and protecting CSS layout flows (such as Flexbox or Grid) that rely on strict parent-child hierarchies.

### Q4: Why can't we use an `if` statement or `for` loop directly inside JSX `{}` braces?
**Answer:** Because JSX curly braces `{}` only accept JavaScript **expressions** (operations that return a single value, such as mathematical evaluations, function calls, or ternary operations). Statements (like `if` or `for`) do not return values directly; they control execution flow. To render conditionally, we must use expressions like the ternary operator (`condition ? x : y`) or logical short-circuiting (`condition && x`). To loop, we use array mapping (`array.map()`).

---

## 🔗 Related Concepts
- [[01 - Introduction to React & Virtual DOM]] — The Virtual DOM tree built by JSX evaluations
- [[03 - Components & Props]] — Components are functions returning JSX
- [[04 - State & useState Hook]] — Hooking up state triggers fresh JSX evaluations
- [[01a - React Installation & Project Setup]] — Setting up your React project and build systems
- [[Package Managers and Build Tools]] — The bundling software hosting Babel/SWC transpilation steps