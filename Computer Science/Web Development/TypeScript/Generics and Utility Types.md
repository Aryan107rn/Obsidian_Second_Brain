---
tags: [typescript, fundamentals, web-development, computer-science, placement-prep, interview-favorite]
aliases: [generics, utility types, Partial, Pick, Omit, Record, extends, constraints]
created: 2026-08-23
updated: 2026-08-23
---

# Generics and Utility Types

Generics provide a mechanism to write reusable, flexible components that work over a variety of types rather than a single one. This maintains **strict type safety** without defaulting to `any` or casting types dynamically.

---

## 1. Why Generics?

Consider a function that returns the first element of an array:

```typescript
// ❌ DANGEROUS: Returns 'any' — loses type tracking
function firstBad(arr: any[]): any { return arr[0]; }

// ❌ RIGID: Only works for numbers
function firstRigid(arr: number[]): number { return arr[0]; }

// ✅ SAFE & FLEXIBLE: Parameterized with Generic 'Type'
function firstGood<Type>(arr: Type[]): Type {
  return arr[0];
}

const num = firstGood([1, 2, 3]); // Type inferred automatically as 'number'
const str = firstGood(["a", "b"]); // Type inferred automatically as 'string'
```

---

## 2. Generic Interfaces, Types, & Classes

Generics can parameterize interfaces, custom types, and classes to build adaptable data containers.

```typescript
// Generic Interface
interface APIResponse<Data> {
  status: number;
  data: Data;
  error?: string;
}

// Generic Type Alias
type Link<T> = { value: T; next: Link<T> | null };

// Generic Class
class Queue<T> {
  private data: T[] = [];
  push(item: T) { this.data.push(item); }
  pop(): T | undefined { return this.data.shift(); }
}
```

---

## 3. Generic Constraints (`extends`)

Sometimes a function shouldn't accept *just any* type. You can restrict the types that a generic parameter is allowed to receive using the `extends` keyword.

```typescript
interface Lengthwise {
  length: number;
}

// Generic constraint: T must be an object containing a 'length' number property
function logLength<T extends Lengthwise>(arg: T): T {
  console.log("Length is:", arg.length); // Compiles! Length is guaranteed to exist.
  return arg;
}

logLength("hello"); // Valid! Strings have .length
logLength([1, 2]);  // Valid! Arrays have .length
// logLength(12345);   // ❌ COMPILE ERROR: Argument of type 'number' is not assignable to parameter of type 'Lengthwise'.
```

### Multiple Constraints and Intersecting generic params
```typescript
function mergeObjects<T extends object, U extends object>(objA: T, objB: U): T & U {
  return { ...objA, ...objB };
}
```

---

## 4. Default Type Parameters

You can provide default fallbacks for generic arguments, matching JavaScript's default parameters.

```typescript
interface AjaxConfig<T = any> {
  url: string;
  payload: T;
}

const config1: AjaxConfig = { url: "/url", payload: "unknown" }; // Defaults T to any
const config2: AjaxConfig<number> = { url: "/url", payload: 42 }; // Explicitly overrides
```

---

## 5. Master Guide: Built-in Utility Types

TypeScript provides several globally-available utility types to facilitate common type transformations. These are built using **Mapped and Conditional Types**.

### `Partial<T>`
Makes all properties of type `T` optional (`?`).
- **Implementation:** `type MyPartial<T> = { [P in keyof T]?: T[P] };`
```typescript
type User = { id: number; name: string };
const update: Partial<User> = { name: "New Name" }; // Valid! 'id' is omitted.
```

### `Required<T>`
Makes all properties of type `T` mandatory (removes `?`).
- **Implementation:** `type MyRequired<T> = { [P in keyof T]-?: T[P] };` // Note: '-?' removes optionality modifier

### `Readonly<T>`
Makes all properties of type `T` read-only.
- **Implementation:** `type MyReadonly<T> = { readonly [P in keyof T]: T[P] };`

### `Record<Keys, Type>`
Constructs an object type whose keys are `Keys` and values are `Type`.
- **Implementation:** `type MyRecord<K extends keyof any, T> = { [P in K]: T };`
```typescript
type Page = "home" | "about";
const navigation: Record<Page, string> = {
  home: "/home",
  about: "/about"
};
```

### `Pick<T, Keys>`
Constructs a type by picking a subset of properties `Keys` (string literal / union) from `T`.
- **Implementation:** `type MyPick<T, K extends keyof T> = { [P in K]: T[P] };`
```typescript
type User = { id: number; name: string; email: string };
type UserHeader = Pick<User, "id" | "name">; // { id: number; name: string }
```

### `Omit<T, Keys>`
Constructs a type by removing a subset of properties `Keys` from `T`.
- **Implementation:** `type MyOmit<T, K extends keyof any> = Pick<T, Exclude<keyof T, K>>;`
```typescript
type UserMinusEmail = Omit<User, "email">; // { id: number; name: string }
```

### `Exclude<UnionType, ExcludedMembers>`
Excludes from a union type all members that are assignable to another.
- **Implementation:** `type MyExclude<T, U> = T extends U ? never : T;`
```typescript
type PureColors = Exclude<"red" | "blue" | "green", "green">; // "red" | "blue"
```

### `Extract<Type, Union>`
Extracts from `Type` all union members that are assignable to `Union`.
- **Implementation:** `type MyExtract<T, U> = T extends U ? T : never;`

### `NonNullable<Type>`
Removes `null` and `undefined` from a union type.
- **Implementation:** `type MyNonNullable<T> = T extends null | undefined ? never : T;`

### `ReturnType<Type>`
Extracts the return type of a function type.
- **Implementation:** `type MyReturnType<T extends (...args: any) => any> = T extends (...args: any) => infer R ? R : any;` // Note: 'infer' is explained in Advanced Types
```typescript
const fn = () => "hello";
type FnReturn = ReturnType<typeof fn>; // 'string'
```

### `Parameters<Type>`
Extracts the parameter types of a function type as a tuple.
- **Implementation:** `type MyParameters<T extends (...args: any) => any> = T extends (...args: infer P) => any ? P : never;`

---

## 🔗 Related Concepts

- [[Classes and OOP in TypeScript]] — Implementing generic class behaviors
- [[Advanced Type Manipulation]] — How Utility Types are implemented under the hood
- [[TS Interview Questions and Tricky Types]] — Writing custom complex utilities (e.g., `DeepReadonly`, `OmitByValue`)
