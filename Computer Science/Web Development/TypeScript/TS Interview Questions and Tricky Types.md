---
tags: [typescript, interview-prep, tricky, web-development, computer-science, placement-prep, interview-favorite]
aliases: [structural typing, nominal typing, variance, covariance, contravariance, TypeScript interview questions]
created: 2026-08-23
updated: 2026-08-23
---

# TS Interview Questions and Tricky Types

Deep-mechanics questions that separate "has used TypeScript" from "understands TypeScript's type system" — the kind of questions that show up in placement interviews for roles emphasizing type safety.

---

## 1. Structural Typing vs. Nominal Typing

**This is the single most important conceptual question about TypeScript's type system.**

TypeScript uses **structural typing** ("duck typing"): two types are compatible if they have the same *shape*, regardless of their declared name or where they came from. Languages like Java/C# use **nominal typing**: two types are only compatible if one explicitly declares itself as the other (via `implements`/inheritance), even if the shapes match exactly.

```typescript
interface Point { x: number; y: number; }

function printPoint(p: Point) { console.log(p.x, p.y); }

class Vector { x = 0; y = 0; z = 0; }  // NOT declared as implementing Point!

printPoint(new Vector()); // ✅ Compiles! Vector has x and y — shape matches, name doesn't matter.
```
In Java, this would **not** compile — `Vector` would need `implements Point` explicitly. TypeScript only cares that the shape is compatible ("if it has `x` and `y`, it's a `Point`-shaped enough thing").

**Follow-up trap question — "is this excess-property or structural?":**
```typescript
printPoint({ x: 1, y: 2, z: 3 }); // ❌ COMPILE ERROR here, but...
const obj = { x: 1, y: 2, z: 3 };
printPoint(obj); // ✅ compiles fine!
```
This is **excess property checking** — a special, *stricter* check that only applies to **object literals passed directly**. Once the object is assigned to a variable first, structural typing's normal (looser) rules apply — TypeScript only checks that the required properties exist and match, extra properties on a named variable are fine. This inconsistency is a very common interview "gotcha."

## 2. Covariance and Contravariance

**Variance** describes how subtyping relationships behave when types are used inside generic containers or function signatures.

- **Covariant**: if `Dog` is a subtype of `Animal`, then `Dog[]` is treated as a subtype of `Animal[]` (arrays/most generic positions are covariant in TS — an array of the more specific type is assignable where the more general type is expected).
- **Contravariant**: **function parameter types** work the *opposite* direction — a function that accepts a *more general* type is assignable where a function accepting a *more specific* type is expected.

```typescript
class Animal {}
class Dog extends Animal {}

let animals: Animal[] = [];
let dogs: Dog[] = [];
animals = dogs;   // ✅ covariant — Dog[] assignable to Animal[]

type AnimalHandler = (a: Animal) => void;
type DogHandler = (d: Dog) => void;

let handleAnimal: AnimalHandler = (a) => {};
let handleDog: DogHandler = handleAnimal;   // ✅ contravariant — a function that handles ANY Animal can safely handle Dogs too
// let handleAnimal2: AnimalHandler = (d: Dog) => {};  // ❌ would be unsound — can't guarantee every Animal is a Dog
```
**Intuition:** a function that can handle *any* `Animal` can obviously also handle a `Dog` (since a `Dog` *is* an `Animal`) — so it's safe to use a "more general parameter" function wherever a "more specific parameter" function is expected. This is why parameters flip direction relative to return types/arrays.

**Note:** TypeScript's method parameters are checked *bivariantly* (looser, for historical/practical reasons) while standalone function type parameters are checked *contravariantly* under `strictFunctionTypes` — a subtle inconsistency that occasionally comes up in advanced discussions, but the core contravariance intuition above is what interviews actually test.

## 3. `unknown` vs `any` — the interview classic

Both accept any value, but behave completely differently afterward:

```typescript
let a: any = 10;
a.foo.bar.baz;   // ✅ compiles (and crashes at runtime) — 'any' disables ALL type checking

let u: unknown = 10;
u.foo;            // ❌ COMPILE ERROR — must narrow 'unknown' before using it

if (typeof u === "number") {
  u + 1;          // ✅ now safe — narrowed to number
}
```
**The answer interviewers want:** `unknown` is the type-safe counterpart to `any` — it accepts anything, but *forces* you to narrow the type (via `typeof`, `instanceof`, or a type guard) before doing anything with it. `any` opts a value **entirely out of type checking**, silently propagating to everything it touches. Prefer `unknown` for values of genuinely uncertain type (e.g. JSON.parse results, catch-block errors); reserve `any` for rare, deliberate escape hatches.

## 4. Custom Utility Type Challenges

These test whether you actually understand mapped/conditional types (see [[Advanced Type Manipulation]]), not just that you can use the built-in ones.

### `DeepReadonly<T>` — recursively readonly, not just one level
```typescript
type DeepReadonly<T> = {
  readonly [P in keyof T]: T[P] extends object ? DeepReadonly<T[P]> : T[P];
};

interface Nested { a: { b: { c: number } } }
type Frozen = DeepReadonly<Nested>;
// { readonly a: { readonly b: { readonly c: number } } }
```
Built-in `Readonly<T>` only freezes the top level — this recursive version demonstrates understanding that mapped types can call themselves.

### `DeepPartial<T>` — recursively optional
```typescript
type DeepPartial<T> = {
  [P in keyof T]?: T[P] extends object ? DeepPartial<T[P]> : T[P];
};
```

### `MyOmit<T, K>` — implementing `Omit` from scratch
```typescript
type MyExclude<T, U> = T extends U ? never : T;
type MyPick<T, K extends keyof T> = { [P in K]: T[P] };
type MyOmit<T, K extends keyof any> = MyPick<T, MyExclude<keyof T, K>>;
```
Demonstrates composing conditional types (`Exclude`) with mapped types (`Pick`) — exactly how the real built-in `Omit` is defined.

### `TupleToUnion<T>` — extracting a union from a tuple's members
```typescript
type TupleToUnion<T extends readonly unknown[]> = T[number];

type Tuple = [1, 2, 3];
type Union = TupleToUnion<Tuple>;  // 1 | 2 | 3
```
Uses indexed access with `number` as the "key" — indexing a tuple/array type with `number` gives the union of all its element types.

## 5. Debugging Compiler Performance

Large codebases can develop **slow type-checking** (`tsc` taking minutes, editor lag). Common causes interviewers ask about:

- **Overly deep conditional/recursive types** — recursive utility types like `DeepReadonly` above can blow up compile time on deeply nested real-world types; TS also has a hard recursion depth limit and will error with "Type instantiation is excessively deep."
- **Large union types** — a union with hundreds of literal members (e.g. auto-generated from a huge enum-like dataset) slows down assignability checks, since TS may need to check against every member.
- **`any` leaking through and disabling narrowing widely** — ironically, *not* a performance fix; excessive `any` doesn't help compile speed and removes safety.
- **Diagnosing it:** `tsc --extendedDiagnostics` or `tsc --generateTrace` produce timing breakdowns showing which files/types are slow to check.

---

## Quick-fire Q&A (common phrasing in interviews)

**Q: What's the difference between `interface` and `type`?**
A: See [[Type Aliases and Interfaces]] in full — short version: interfaces support declaration merging and are generally preferred for object shapes meant to be extended; type aliases can represent unions, tuples, and other non-object shapes that interfaces cannot.

**Q: Why does `unknown` exist if `any` already accepts anything?**
A: `any` disables type checking entirely and propagates silently; `unknown` accepts anything but forces narrowing before use — see section 3 above.

**Q: What does `as const` actually do?**
A: Narrows a value to its most specific literal type and makes it deeply readonly, instead of TypeScript's default type-widening behavior — see [[Type Assertions and Ambient Declarations]].

**Q: Is TypeScript sound?**
A: No — TypeScript intentionally makes some unsound tradeoffs for practicality (e.g. bivariant method parameter checking, array index access not automatically including `undefined` unless `noUncheckedIndexedAccess` is enabled). It catches the overwhelming majority of real bugs but isn't a formal proof system.

## Related Concepts
- [[Advanced Type Manipulation]] — the mapped/conditional type mechanics used throughout the custom utility type challenges here.
- [[Type Aliases and Interfaces]] — structural typing applies to both, but declaration merging is interface-only.
- [[Generics and Utility Types]] — the built-in utilities these custom challenges are re-implementing from scratch.
