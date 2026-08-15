---
tags: [react, javascript, frontend, web-development, computer-science, moc]
aliases: [React.js, ReactJS]
created: 2026-08-09
updated: 2026-08-14
---

# React — Declarative UI & Component Architecture

**React** is an open-source JavaScript library for building interactive user interfaces, created by Meta (Facebook). Instead of manually and imperatively editing the browser DOM (e.g. `document.getElementById("count").innerText = 5`), you describe **what the UI should look like for any given state**, and React efficiently calculates and applies the minimal DOM modifications required.

---

## 🖼️ React Architecture: Virtual DOM & Reconciliation

![[react-architecture-diagram.svg|930]]

---

## 💡 Declarative vs. Imperative Mental Model

- **Imperative (Vanilla JS):** "Find element `#btn`. Change background to blue. Append child `<span>`. Update text to 'Saving...'." (High risk of UI bugs and forgotten state synchronization).
- **Declarative (React):** "Render `<Button isLoading={true} />`." React computes the UI automatically from `UI = f(state)`.

---

## 🏗️ Core React Building Blocks

- [[Components and Props]] — Reusable UI components and unidirectional top-down data flow
- [[JSX]] — JavaScript XML syntax extension
- [[State (useState)]] — Local component state that triggers reactive re-renders
- [[Hooks and useEffect]] — Handling side effects, lifecycle events, and data fetching
- [[Todo List Example]] — End-to-end practical walkthrough of state & props in action
- [[React Common Mistakes and Edge Cases]] — Real-world bugs, stale closures, and infinite re-render loops

---

## ⚖️ When to Use vs. When Not to Use

| Use React For | Avoid React For |
| :--- | :--- |
| Dynamic, data-heavy dashboards and SaaS apps | Simple static landing pages / personal blogs (use Astro/HTML) |
| Complex SPAs with extensive user interaction | Simple isolated widgets on existing legacy websites |
| Large team codebases requiring reusable design systems | Projects where build tools/bundler overhead is undesirable |

---

## 🔗 Related Vault Concepts
- [[JavaScript MOC]] — The underlying language powering React
- [[REST APIs]] & [[GraphQL]] — Connecting React frontends to backend services
- [[Package Managers and Build Tools]] — Vite, npm, pnpm, and Next.js tooling
