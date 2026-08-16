# 17 - Tailwind CSS with React

## What is it?

Tailwind is a **utility-first CSS framework**. Instead of writing custom CSS classes with your own names (`.card`, `.btn-primary`) and defining their styles in a separate `.css` file, you style elements by applying many small, single-purpose classes directly in your JSX — each utility class maps to one CSS property (or a tight group of related ones).

```jsx
// Traditional CSS
<button className="btn-primary">Click me</button>
// .btn-primary { background: blue; color: white; padding: 8px 16px; border-radius: 4px; }

// Tailwind
<button className="bg-blue-500 text-white px-4 py-2 rounded">Click me</button>
```

## Why it exists — the problem it solves

Traditional CSS has two recurring pain points as a project grows:

1. **Naming fatigue** — every new element needs a class name (`.card-header-title-wrapper`), and naming things well is genuinely hard. Bad names make CSS files unmaintainable.
2. **Unbounded CSS growth** — CSS files only ever grow. Deleting a component doesn't guarantee its CSS gets deleted too, because you can't easily tell if some rule is still used elsewhere. Specificity conflicts pile up over time (two rules fighting over the same property).

Tailwind's answer: don't write custom CSS at all for most things. Use a fixed, predefined set of utility classes. Since classes are applied inline in JSX, when you delete a component, its styling is deleted with it automatically — there's no orphaned CSS. And because everyone draws from the same finite utility vocabulary, there's no naming decision to make and no specificity war (utilities are single-property, so conflicts are rare).

**Why this pairs especially well with React specifically:** React already co-locates a component's logic and markup in one file. Tailwind extends that same co-location to styling — the component's JSX, behavior, AND appearance all live in one place instead of being split across `.jsx` and `.css` files that you have to keep mentally in sync.

## How it works

Tailwind scans your source files for class names you've actually used and generates a CSS file containing only those utilities (via a build step, using PostCSS). This means:
- The framework ships thousands of possible utility classes, but your final CSS bundle only contains the ones you referenced — unused utilities are never generated.
- Classes are just strings in your JSX; there's no CSS file to open and edit for basic styling.

## Setup (Vite + React, Tailwind v4)

```bash
npm install tailwindcss @tailwindcss/vite
```

```js
// vite.config.js
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import tailwindcss from '@tailwindcss/vite'

export default defineConfig({
  plugins: [react(), tailwindcss()],
})
```

```css
/* src/index.css */
@import "tailwindcss";
```

Import `index.css` once in your app's entry point (`main.jsx`), and every component can now use Tailwind classes directly — no further per-file imports needed.

> Tailwind v4 dropped the old `tailwind.config.js` + `content: [...]` file-scanning setup used in v3. If you're following older tutorials that mention `npx tailwindcss init` or a `content` array, that's the v3 workflow — check which version a guide targets before following it.

## Key concepts

### Utility classes map directly to CSS properties
```jsx
<div className="flex items-center justify-between p-4 bg-gray-100 rounded-lg shadow-md">
```
- `flex` → `display: flex`
- `items-center` → `align-items: center`
- `justify-between` → `justify-content: space-between`
- `p-4` → `padding: 1rem` (Tailwind's spacing scale: `4` = `1rem` = `16px`, following a consistent numeric scale rather than arbitrary pixel values)
- `bg-gray-100` → a light gray background from Tailwind's predefined color palette
- `rounded-lg` → `border-radius` at the "large" preset size
- `shadow-md` → a medium box-shadow preset

### Responsive design via breakpoint prefixes
Tailwind is **mobile-first**: an unprefixed utility applies at all sizes, and a prefixed one overrides it starting at that breakpoint and up.
```jsx
<div className="w-full md:w-1/2 lg:w-1/3">
  {/* full width on mobile, half width from tablet (md: ≥768px), a third from desktop (lg: ≥1024px) */}
</div>
```
Common breakpoints: `sm` (640px), `md` (768px), `lg` (1024px), `xl` (1280px), `2xl` (1536px).

### State variants — hover, focus, and beyond
```jsx
<button className="bg-blue-500 hover:bg-blue-600 focus:ring-2 focus:ring-blue-300 disabled:opacity-50">
```
`hover:`, `focus:`, `active:`, `disabled:` prefixes apply a utility only in that state — no separate `:hover` CSS block needed.

### Dark mode
```jsx
<div className="bg-white dark:bg-gray-900 text-black dark:text-white">
```
`dark:` applies when dark mode is active (by default, based on the user's OS preference; can be switched to a manually-toggled class strategy).

## Conditional classes in React (the part unique to using Tailwind *with* React)

Since JSX `className` is just a string, conditional styling means conditionally building that string. Doing this by hand gets messy fast:

```jsx
// Fragile — easy to introduce a double space or miss a condition
<div className={`px-4 py-2 rounded ${isActive ? 'bg-blue-500 text-white' : 'bg-gray-200 text-black'}`}>
```

The standard fix is the `clsx` (or `classnames`) library, which conditionally joins class names cleanly:

```bash
npm install clsx
```
```jsx
import clsx from 'clsx';

<div className={clsx(
  'px-4 py-2 rounded',
  isActive ? 'bg-blue-500 text-white' : 'bg-gray-200 text-black',
  isDisabled && 'opacity-50 cursor-not-allowed'
)}>
```

**Why this matters more in Tailwind than in traditional CSS:** with traditional CSS you'd just conditionally toggle one class name (`.active`) and let the CSS file define what it means. With Tailwind, every visual variation is a *list* of utility classes, so merging/toggling several of them at once is common enough to need a dedicated helper.

### Extracting repeated utility strings into a component
When the same long utility string is reused across a codebase, don't copy-paste it — extract it into a React component so the styling lives in exactly one place, same as you would with any other repeated JSX:
```jsx
function Card({ children }) {
  return <div className="p-6 bg-white rounded-lg shadow-md border border-gray-200">{children}</div>;
}
```
This is Tailwind's answer to reusability — reuse the *component*, not a hand-rolled CSS class.

## When to use

- Rapid UI development where switching between JSX and a CSS file constantly slows you down.
- Projects where consistent design-system constraints (fixed spacing/color scale) are wanted by default rather than enforced by convention.
- Component-based frameworks like React, where styling naturally belongs next to markup.

## When NOT to use / limitations

- Highly custom, illustration-heavy, or animation-heavy designs may fight the utility model — sometimes plain CSS (or CSS-in-JS) is more direct for complex, one-off visual effects.
- Team members unfamiliar with Tailwind's utility names face a real learning curve initially — className strings are dense and can look unreadable to newcomers before the vocabulary clicks.
- Long class lists reduce JSX readability if not managed via component extraction — a `<div className="...">` with 15+ utilities is a sign to extract a component.

## Common mistakes

- **Constructing class names dynamically with string interpolation**, e.g. `` `text-${color}-500` ``. Tailwind's build step works by *scanning source files for literal class name strings* — if the class name is assembled at runtime, the scanner never sees the full string `text-red-500` in your source, so it doesn't generate that CSS, and the style silently doesn't apply. Fix: use a lookup object mapping known values to full literal class strings.
```jsx
// Broken - Tailwind can't see this string at build time
<div className={`text-${color}-500`}>

// Correct - all possible full class names appear literally in source
const colorMap = { red: 'text-red-500', blue: 'text-blue-500' };
<div className={colorMap[color]}>
```
- Forgetting `clsx`/similar and hand-concatenating strings with template literals — easy to end up with stray spaces or missed conditions.
- Fighting specificity by adding custom CSS on top of utilities instead of composing more utilities or extracting a component — defeats the purpose of the utility-first approach.
- Not extracting repeated utility strings into components, leading to the same design tweak (e.g. changing a shadow) needing to be hunted down and changed in dozens of files.

## Related concepts
- [[03 - Components & Props]] — component extraction is how Tailwind achieves style reuse
- [[JavaScript MOC]] — template literals and conditional expressions used to build class strings
- [[02 - JSX & Building Blocks]] — `className` mechanics in JSX
