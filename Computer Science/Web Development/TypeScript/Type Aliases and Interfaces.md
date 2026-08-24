---
tags: [typescript, fundamentals, web-development, computer-science, placement-prep, interview-favorite]
aliases: [type vs interface, interfaces, type aliases, declaration merging, index signatures]
created: 2026-08-23
updated: 2026-08-23
---

# Type Aliases and Interfaces

TypeScript offers two primary syntaxes to describe the shape of an object or type structure: **Type Aliases** (`type`) and **Interfaces** (`interface`). While they are highly interchangeable in day-to-day work, they have distinct differences under the hood in extension, merging, and versatility.

---

## 1. Syntax Overview

### Type Aliases
Provides a name to *any* type signature, including objects, primitives, unions, intersections, and tuples.

```typescript
type Point = {
  x: number;
  y: number;
};

type Name = string; // Primitive alias
type ResponseCode = 200 | 404; // Union alias
```

### Interfaces
Directly models an **object interface structure** (the public contract of an object or class).

```typescript
interface Point {
  x: number;
  y: number;
}
```

---

## 2. Key Differences Comparison

| Feature | Type Alias (`type`) | Interface (`interface`) |
| :--- | :---: | :---: |
| **Declaration Merging** | ❌ No (Throws Duplicate Identifier error) | ✅ Yes (Repeated interfaces merge fields) |
| **Primitives & Unions** | ✅ Yes (e.g. `type Code = string \| number`) | ❌ No (Can only model object structure) |
| **Extension Syntax** | Intersections (`&`) | Subclassing (`extends`) |
| **Tuples & Arrays** | ✅ Direct syntax | ❌ Clunky representation |
| **Performance (large scales)** | Slightly slower (checks intersections recursively) | Faster (built-in hashing for flat merges) |

---

## 3. Deep Dive into Differences

### Difference A: Declaration Merging (Interface Re-opening)
If you declare two interfaces with the exact same name in the same scope, they automatically merge their fields. Type aliases cannot be declared twice.

```typescript
// Part 1: Third-party library
interface Window {
  themeMode: "light" | "dark";
}

// Part 2: Custom application code
interface Window {
  userId: string; // Merges into the existing Window interface!
}

const sysWindow: Window = {
  themeMode: "dark",
  userId: "user_901"
};
```
> 💡 **Why this matters:** Interface merging is highly useful for **extending Global namespaces** (like adding custom properties to Express `Request` objects, or adding variables onto the global Node/DOM window scope).

### Difference B: Extension Syntaxes

#### Interface Extension (`extends`)
Interfaces can inherit properties from other interfaces or even type aliases.

```typescript
interface Animal { name: string; }
interface Dog extends Animal { breed: string; }
```

#### Type Extension (Intersections - `&`)
Types extend via intersection.

```typescript
type Animal = { name: string };
type Dog = Animal & { breed: string };
```

---

## 4. Index Signatures

Index signatures are used when you do not know the exact property keys of an object in advance, but you know the expected value types (e.g. key-value dictionary payloads).

```typescript
interface Dictionary {
  [key: string]: number; // Index signature: any string key maps to a number value
}

const scores: Dictionary = {
  alice: 95,
  bob: 89
};
```

### ⚠️ The Safety Gotcha of Index Signatures
TypeScript will compile accessing *any* random string key without throwing an error, even if that key does not exist. It assumes the key is present.

```typescript
const charlieScore = scores.charlie; // Type inferred as 'number'!
console.log(charlieScore); // Prints 'undefined' at runtime. Potential crash!
```

#### ✅ The Safe Solution: Including `undefined`
To force safe handling, add `undefined` as a potential return value:

```typescript
interface SafeDictionary {
  [key: string]: number | undefined;
}

const safeScores: SafeDictionary = { alice: 95 };
const safeCharlie = safeScores.charlie; // Type is 'number | undefined'
// safeCharlie + 5; // ❌ COMPILE ERROR: Object is possibly 'undefined'. Forces you to check first!
```

---

## 5. Architectural Guidelines: When to Use Which?

1. **Use `interface` when:**
   - You are authoring a **public library** or API where consumers might want to extend properties or use declaration merging.
   - You are modeling **OOP hierarchies** (classes that implement shapes, inheritance hierarchies).
   
2. **Use `type` when:**
   - You need **unions**, **intersections**, or primitives (e.g., `type Id = string | number`).
   - You are doing **type-level programming** (conditional types, mapped types, template literal types).
   - You are writing tuples or function-only signatures.

---

## 🔗 Related Concepts

- [[Classes and OOP in TypeScript]] — Classes implementing interfaces
- [[Union Intersection and Type Narrowing]] — Combining type models with logical logic
- [[Advanced Type Manipulation]] — Manipulating type models programmatically
