---
tags: [typescript, fundamentals, web-development, computer-science, placement-prep]
aliases: [any, unknown, never, void, tuple, primitive, types]
created: 2026-08-23
updated: 2026-08-23
---

# Primitive and Special Types

TypeScript matches the basic primitives of JavaScript and adds critical, specialized types to describe unique scenarios (such as dynamic data, unreachable execution branches, and rigid tuple structures).

---

## 🧭 Type Hierarchy in TypeScript

The TypeScript type system is structured hierarchically. At the very top is the Top Type (`any` and `unknown`), and at the absolute bottom is the Bottom Type (`never`).

```
          [ any ]     [ unknown ]       <-- Top Types (Can represent any value)
             \           /
         [ Object / {} / Boxed ]
             /     |     \
    [ string ] [ number ] [ boolean ] ... <-- Concrete Primitives
             \     |     /
          [ null ]  [ undefined ]
             \           /
               [ never ]                <-- Bottom Type (Represents empty set / impossible value)
```

---

## 1. Primitives

TypeScript maps one-to-one with JavaScript primitives. Annotations are lowercase:

```typescript
const username: string = "Aryan";
const count: number = 42; // Handles integers, floats, NaN, and Infinity
const isActive: boolean = true;
const uniqueId: symbol = Symbol("id");
const emptyValue: null = null;
const notDefined: undefined = undefined;
```

---

## 2. Special Types Masterclass

### `any` (The Escape Hatch)
Declaring a variable as `any` disables all type-checking. The compiler assumes you know what you are doing and permits any action, including accessing nonexistent methods.
- **Danger:** Propagates bugs to other parts of the application, rendering TypeScript useless.
- **When to use:** Migrating old JavaScript codebases, or when writing unit tests to quickly stub nested objects.

### `unknown` (The Safe Counterpart to `any`)
Like `any`, any value can be assigned to `unknown`. However, **unlike** `any`, you cannot perform operations on an `unknown` variable or assign it to other types without first validating (narrowing) its type.

```typescript
let value: unknown = "Hello";

// ❌ COMPILE ERROR: Object is of type 'unknown'
// console.log(value.toUpperCase()); 

// ✅ Correct Approach: Narrow the type first
if (typeof value === "string") {
  console.log(value.toUpperCase()); // Safe!
}
```

### `never` (The Bottom Type)
`never` represents the type of values that **never occur**. It is used in two primary scenarios:
1. **Functions that never return:** Functions that throw errors or run in infinite loops.
2. **Exhaustive pattern matching:** Enforcing that every possible branch of a union is covered.

```typescript
// 1. Unreachable end
function throwError(message: string): never {
  throw new Error(message);
}

// 2. Exhaustive check
type Color = "red" | "blue" | "green";

function handleColor(color: Color) {
  switch (color) {
    case "red": return "Stop";
    case "blue": return "Water";
    case "green": return "Go";
    default:
      // If Color adds a new value later, this throws a compile-time error!
      const _exhaustiveCheck: never = color;
      return _exhaustiveCheck;
  }
}
```

### `void`
Used to indicate that a function returns **no meaningful value** (typically returns `undefined` implicitly).
- **Difference from `undefined`:** A function returning `void` can exit without a `return` statement. A function returning `undefined` must explicitly call `return undefined;`.

---

## 3. Structured Data: Arrays and Tuples

### Arrays
Arrays are typed either via brackets (`type[]`) or generics (`Array<type>`):

```typescript
const numbers: number[] = [1, 2, 3];
const names: Array<string> = ["Alice", "Bob"];
```

### Tuples
Tuples are **fixed-length arrays** where each index has a specific, pre-defined type.

```typescript
let coordinate: [number, number] = [40.7128, -74.0060];
let userSession: [string, number, boolean] = ["session_id", 1024, true];

// ❌ COMPILE ERROR: Length must be exactly 3
// userSession = ["id", 5];

// ⚠️ Gotcha: Tuple boundaries are NOT enforced with push/pop at runtime!
userSession.push("added_secret"); // Compiles! (An inherent TS design limitation)

// ✅ Solution: Readonly Tuples
const secureSession: readonly [string, number] = ["id", 42];
// secureSession.push(5); // ❌ COMPILE ERROR: Property 'push' does not exist on 'readonly' tuple.
```

---

## 4. Object Type Annotations

Objects can be typed inline or structured. Properties can be marked as optional (`?`) or read-only (`readonly`).

```typescript
type User = {
  readonly id: number;      // Cannot be re-assigned after initialization
  username: string;
  email?: string;           // Optional property (string | undefined)
};

const player: User = {
  id: 101,
  username: "vortex"
};

// player.id = 102; // ❌ COMPILE ERROR: Cannot assign to 'id' because it is a read-only property.
```

---

## 🔗 Related Concepts

- [[Union Intersection and Type Narrowing]] — Utilizing `unknown` with narrowing and `never` in exhaustiveness checks
- [[Type Aliases and Interfaces]] — Defining complex object shapes
- [[Generics and Utility Types]] — Typing dynamic structures cleanly
