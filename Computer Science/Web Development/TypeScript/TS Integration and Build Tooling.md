---
tags: [typescript, tooling, ecosystem, web-development, computer-science, placement-prep]
aliases: [module resolution, import type, namespace vs module, Vite TypeScript, typescript-eslint]
created: 2026-08-23
updated: 2026-08-23
---

# TS Integration and Build Tooling

TypeScript itself only checks types and (optionally) compiles to JavaScript — it doesn't bundle, minify, or serve your app. This note covers how TypeScript fits into a real project's build pipeline, module system, and tooling.

---

## 1. Module Resolution

When you write `import { thing } from "./utils"`, TypeScript needs to figure out **which file** that refers to and **what types** it exports. The `moduleResolution` setting in `tsconfig.json` (see [[Introduction and Compiler Config]]) controls the algorithm used.

| Setting | Behavior |
|---|---|
| `node` / `node10` | Mimics Node.js's CommonJS `require()` resolution — walks up `node_modules`, checks `package.json` `main` field. Legacy default. |
| `node16` / `nodenext` | Modern Node.js resolution, respects `package.json` `"type": "module"` and `exports` field, distinguishes `.mts`/`.cts` files. |
| `bundler` | For use with modern bundlers (Vite, esbuild, webpack) that have their own resolution logic — most permissive, recommended for frontend projects in 2026. |

**Practical takeaway:** if you're on a modern frontend stack (Vite, Next.js), `"moduleResolution": "bundler"` is almost always correct — it matches what your actual bundler does, avoiding "works in the bundler but TS complains" mismatches.

## 2. `import type` — Type-Only Imports

Explicitly marks an import as **type information only**, guaranteeing it's fully erased at compile time with zero runtime footprint.

```typescript
import type { User } from "./types";     // guaranteed erased — no runtime import statement emitted
import { fetchUser } from "./api";        // a real runtime import

// Mixed imports: separate type and value imports from the same module
import { type Config, loadConfig } from "./config";
```

**Why this matters:** without `import type`, TypeScript has to guess whether an import is type-only (and safe to erase) or has side effects (and must be kept). This ambiguity matters for bundlers doing tree-shaking, and is *required* under `isolatedModules` (relevant when using Babel or esbuild to transpile TS, which process files one at a time without full type information — see [[Introduction and Compiler Config]]).

## 3. Namespace vs. Module

**Modules** (ES modules — `import`/`export`) are the standard, modern way to organize TypeScript code — each file is its own scope.

**Namespaces** (`namespace Foo { ... }`) are a **legacy** TypeScript-only feature predating ES modules, used to group related code under a single global-ish name.

```typescript
// Legacy namespace pattern — avoid in new code
namespace Utils {
  export function double(x: number) { return x * 2; }
}
Utils.double(5);

// Modern module pattern — preferred
export function double(x: number) { return x * 2; }
```

**Rule of thumb:** always prefer ES modules in new code. Namespaces still appear in older codebases and in `.d.ts` files for libraries that predate ES modules (e.g. some jQuery plugin type definitions) — recognize them, but don't write new code this way.

## 4. Project References

For large codebases split into multiple sub-projects (e.g. a monorepo with `packages/core` and `packages/ui`), **project references** let TypeScript build each sub-project independently and incrementally, rather than re-checking the entire codebase on every change.

```json
// tsconfig.json in the root
{
  "references": [
    { "path": "./packages/core" },
    { "path": "./packages/ui" }
  ]
}
```
Each referenced project needs `"composite": true` in its own `tsconfig.json`. Build with `tsc --build` (or `tsc -b`), which understands the dependency graph and only rebuilds what changed.

**When this matters:** monorepos, or any project where full-codebase type-checking has become slow enough to hurt iteration speed.

## 5. Bundling with Vite / Next.js / esbuild

**Key mental model:** modern bundlers **do not use `tsc` to compile your code for the build** — they use a much faster transpiler (esbuild, SWC) that strips types without full type-checking, and `tsc` is run **separately** (usually in CI, or via `tsc --noEmit`) purely as a type-checker.

| Tool | Role |
|---|---|
| **Vite** | Uses esbuild under the hood for near-instant dev server startup and fast transpilation; doesn't type-check during dev by default. |
| **Next.js** | Uses SWC (Rust-based compiler) for transpilation; runs `tsc` separately during `next build` for type errors. |
| **esbuild** | Extremely fast Go-based bundler/transpiler; explicitly does **not** type-check — it just strips TypeScript syntax. |

**Why this split matters:** full type-checking requires understanding the *entire* program (cross-file inference), which is inherently slower than transpiling one file at a time. Splitting "fast transpile for dev speed" from "slow but thorough type-check for correctness" is what makes modern TS tooling feel fast while still catching type errors (usually via a separate `tsc --noEmit` script, or your editor's live type-checking).

## 6. Linting with `typescript-eslint`

ESLint (JavaScript's standard linter) needs a TypeScript-aware parser and rule set to understand TS-specific syntax and catch TS-specific issues (`@typescript-eslint/parser` + `@typescript-eslint/eslint-plugin`).

```json
// .eslintrc example (conceptual)
{
  "parser": "@typescript-eslint/parser",
  "plugins": ["@typescript-eslint"],
  "extends": ["plugin:@typescript-eslint/recommended"]
}
```
**What it catches that `tsc` alone doesn't:** `tsc` checks *types*; ESLint checks *style and common bug patterns* — e.g. `no-explicit-any` (flags lazy `any` usage), `no-unused-vars`, `no-floating-promises` (catches un-awaited async calls, a common source of silent bugs). The two tools are complementary, not redundant.

---

## Common Mistakes
- Assuming `tsc` is what actually builds/bundles a production app — in most modern frontend setups, it's only the type-checker; a separate tool (esbuild/SWC/webpack) does the actual transpilation and bundling.
- Using default `node` module resolution with a modern bundler-based project — causes resolution mismatches (works in the bundler, TS complains, or vice versa). Match `moduleResolution` to your actual tooling.
- Writing new code with `namespace` instead of ES modules — legacy pattern, avoid outside of maintaining old code or certain `.d.ts` declaration scenarios.
- Forgetting `import type` under `isolatedModules` — causes build errors with Babel/esbuild-based toolchains that transpile files independently without cross-file type information.

## Related Concepts
- [[Introduction and Compiler Config]] — `tsconfig.json` options that control module resolution and emit behavior.
- [[Type Assertions and Ambient Declarations]] — how `.d.ts` files get discovered via module resolution.
