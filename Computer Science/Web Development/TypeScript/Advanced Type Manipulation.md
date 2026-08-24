---
tags: [typescript, advanced, web-development, computer-science, placement-prep, interview-favorite]
aliases: [keyof, typeof query, conditional types, mapped types, template literal types, infer]
created: 2026-08-23
updated: 2026-08-23
---

# Advanced Type Manipulation

TypeScript's type system is itself a small functional programming language that runs at compile time — it can inspect, transform, and generate new types from existing ones. This is how utility types like `Partial<T>` and `Pick<T, K>` (see [[Generics and Utility Types]]) are actually implemented under the hood.

---

## 1. `keyof` — Extracting Property Names as a Union

`keyof` takes an object type and produces a **union of its property names as string literal types**.

```typescript
interface User {
  id: number;
  name: string;
  email: string;
}

type UserKeys = keyof User; // "id" | "name" | "email"

function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}

const user: User = { id: 1, name: "Aryan", email: "a@x.com" };
getProperty(user, "name");  // ✅ valid, returns string
getProperty(user, "age");   // ❌ COMPILE ERROR: "age" is not in keyof User
```
**Why this matters:** `getProperty` is fully type-safe — you cannot pass a key that doesn't exist on the object, and the return type is automatically inferred as the correct property type (`T[K]`), not `any`.

## 2. `typeof` (Type Query) — Deriving a Type from a Value

Not to be confused with JavaScript's runtime `typeof` operator — in a **type position**, `typeof` extracts the *type* of a variable, letting you avoid manually re-declaring a shape that already exists as a value.

```typescript
const config = {
  apiUrl: "https://api.example.com",
  timeout: 5000,
  retries: 3,
};

type Config = typeof config;
// { apiUrl: string; timeout: number; retries: number }

function fetchWithConfig(cfg: Config) { /* ... */ }
```
**When to use it:** whenever a type should exactly mirror an existing constant/config object — keeps a single source of truth instead of maintaining the shape twice (once as a value, once as a duplicated interface).

## 3. Indexed Access Types (`T[K]`)

Lets you look up the type of a specific property, the same way you'd access a value at runtime — but at the type level.

```typescript
interface User {
  id: number;
  address: { city: string; zip: string };
}

type UserId = User["id"];              // number
type Address = User["address"];        // { city: string; zip: string }
type AddressCity = User["address"]["city"]; // string — chains just like value access

type AnyUserValue = User[keyof User];   // number | { city: string; zip: string } — union of ALL property types
```

## 4. Conditional Types (`T extends U ? X : Y`)

A type-level ternary — the type resolves to `X` if `T` is assignable to `U`, otherwise `Y`. This is the foundation of most advanced utility types.

```typescript
type IsString<T> = T extends string ? "yes" : "no";

type A = IsString<string>;  // "yes"
type B = IsString<number>;  // "no"
```

### Distributive Conditional Types
When `T` is a **union**, a naive conditional type distributes over each member automatically:
```typescript
type ToArray<T> = T extends any ? T[] : never;

type Result = ToArray<string | number>;
// Distributes as: ToArray<string> | ToArray<number>
// = string[] | number[]   (NOT (string | number)[] — a common surprise!)
```
This distributive behavior is exactly how `Exclude<T, U>` and `Extract<T, U>` work — they rely on the union being processed member-by-member.

### `infer` — Extracting a Type Mid-Condition
`infer` lets you **capture** a type variable from within a conditional type's structure, instead of just testing it.
```typescript
type ReturnTypeOf<T> = T extends (...args: any[]) => infer R ? R : never;

type Fn = () => string;
type FnReturn = ReturnTypeOf<Fn>; // string
```
`infer R` says: "if `T` matches the shape of a function, capture whatever its return type is and call it `R`." This is precisely how the built-in `ReturnType<T>` and `Parameters<T>` utility types are implemented (see [[Generics and Utility Types]]).

```typescript
// Extracting the element type out of an array
type ElementType<T> = T extends (infer E)[] ? E : never;
type Item = ElementType<string[]>;  // string
```

## 5. Mapped Types

Build a new object type by **iterating over the keys** of an existing type — the type-level equivalent of a `for...in` loop.

```typescript
type MyPartial<T> = { [P in keyof T]?: T[P] };
type MyReadonly<T> = { readonly [P in keyof T]: T[P] };
```

### Modifiers: adding and removing `readonly` / `?`
```typescript
type MyRequired<T> = { [P in keyof T]-?: T[P] };   // '-?' strips optionality
type Mutable<T> = { -readonly [P in keyof T]: T[P] };  // '-readonly' strips readonly
```

### Key Remapping with `as` (renaming keys during mapping)
```typescript
type Getters<T> = {
  [P in keyof T as `get${Capitalize<string & P>}`]: () => T[P]
};

interface Person { name: string; age: number; }
type PersonGetters = Getters<Person>;
// { getName: () => string; getAge: () => number }
```

## 6. Template Literal Types

Build new string literal types by combining/interpolating existing literal types — string manipulation at the type level.

```typescript
type Direction = "left" | "right";
type Padding = `padding-${Direction}`;
// "padding-left" | "padding-right"

type EventName<T extends string> = `on${Capitalize<T>}`;
type ClickEvent = EventName<"click">;  // "onClick"
```

**Built-in string manipulation types:** `Uppercase<T>`, `Lowercase<T>`, `Capitalize<T>`, `Uncapitalize<T>` — all operate on string literal types at compile time.

### Real-world use: typing a strict event emitter
```typescript
type EventMap = { click: MouseEvent; keydown: KeyboardEvent };

type OnHandlers = {
  [K in keyof EventMap as `on${Capitalize<string & K>}`]: (e: EventMap[K]) => void;
};
// { onClick: (e: MouseEvent) => void; onKeydown: (e: KeyboardEvent) => void }
```
Combines mapped types + key remapping + template literals — this is the kind of composition that shows up in well-typed UI libraries.

---

## Common Mistakes
- Confusing runtime `typeof` (JavaScript operator, returns a string like `"object"`) with type-position `typeof` (TypeScript-only, extracts a static type) — they look identical but operate in completely different contexts.
- Forgetting that conditional types **distribute over unions** by default — leads to surprising results like `string[] | number[]` instead of the expected `(string | number)[]`. To opt out of distribution, wrap both sides in a tuple: `[T] extends [U] ? X : Y`.
- Using `infer` without understanding it only captures within a **matching structural position** — `infer R` inside `T extends (infer R)[]` only works if `T` actually has array shape; otherwise it falls through to the `false` branch.

## Related Concepts
- [[Generics and Utility Types]] — the built-in utility types (`Partial`, `Pick`, `ReturnType`, etc.) are all built from the primitives on this page.
- [[Union Intersection and Type Narrowing]] — conditional types and union distribution build directly on union type fundamentals.
- [[TS Interview Questions and Tricky Types]] — custom utility type challenges (`DeepReadonly`, `DeepPartial`) combine everything on this page.
