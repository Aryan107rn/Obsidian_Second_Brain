---
tags: [typescript, fundamentals, web-development, computer-science, placement-prep, interview-favorite]
aliases: [type guards, narrowing, discriminated unions, union, intersection]
created: 2026-08-23
updated: 2026-08-23
---

# Union Intersection and Type Narrowing

Static typing requires structures to handle dynamic states. TypeScript achieves this by composing types via unions and intersections, and then using compile-time flow analysis (**Type Narrowing**) to resolve specific types in execution blocks.

---

## 🖼️ Narrowing Flow (Type Guards)

When TypeScript sees a union type (e.g. `string | number`), it knows the value is one of those types. It will block actions that aren't valid for *both* types until you "narrow" the type using a control flow statement.

```
       [ Input: string | number ]
                    |
           < typeof x === "string" >
             /                 \
          (Yes)                (No)
           /                     \
[ Target: string ]        [ Target: number ]
(e.g., call .toUpperCase())   (e.g., call .toFixed())
```

---

## 1. Union and Intersection Types

### Union Types (`|`)
Allows a value to be one of several types. A value must conform to at least one of the union constituents.

```typescript
type Id = string | number;

function printId(id: Id) {
  // console.log(id.toUpperCase()); // ❌ COMPILE ERROR: .toUpperCase() doesn't exist on 'number'
  
  if (typeof id === "string") {
    console.log(id.toUpperCase()); // ✅ Safe (Narrowed to string)
  }
}
```

### Intersection Types (`&`)
Combines multiple types into one. The resulting type possesses all features of all intersected types.

```typescript
type Hostable = { host: string; port: number };
type Secure = { ssl: boolean };

type ServerConfig = Hostable & Secure;

const prodServer: ServerConfig = {
  host: "prod.api.com",
  port: 443,
  ssl: true // All properties are required!
};
```

---

## 2. Literal Types

Types can represent **exact specific values** (strings, numbers, or booleans) rather than general classes of values.

```typescript
type Direction = "NORTH" | "SOUTH" | "EAST" | "WEST";
type HTTPStatus = 200 | 404 | 500;

let dir: Direction = "NORTH";
// dir = "UP"; // ❌ COMPILE ERROR: Type '"UP"' is not assignable to type 'Direction'.
```

---

## 3. The Art of Type Narrowing (Type Guards)

TypeScript uses JavaScript execution operators to narrow union types down to narrowest possible segments.

### A. The `typeof` Type Guard (For Primitives)
Checks basic JS primitives (`"string"`, `"number"`, `"boolean"`, `"symbol"`, `"undefined"`, `"object"`, `"function"`).

```typescript
function double(input: string | number) {
  if (typeof input === "number") {
    return input * 2;
  }
  return input + input;
}
```

### B. The `instanceof` Type Guard (For Classes)
Checks if an object is an instance of a specific class constructor.

```typescript
class FileLogger { log(msg: string) { console.log("File:", msg); } }
class ConsoleLogger { log(msg: string) { console.log("Console:", msg); } }

function runLogger(logger: FileLogger | ConsoleLogger) {
  if (logger instanceof FileLogger) {
    logger.log("Active"); // Narrowed to FileLogger
  }
}
```

### C. The `in` Type Guard (For Property Existence)
Checks if a property exists on an object. Excellent for narrowing object types.

```typescript
type Admin = { privileges: string[] };
type Guest = { email: string };

function login(user: Admin | Guest) {
  if ("privileges" in user) {
    console.log("Admin Access granted:", user.privileges);
  } else {
    console.log("Welcome Guest:", user.email);
  }
}
```

### D. User-Defined Type Guards (Type Predicates)
For complex custom objects where `typeof` or `in` are insufficient, you can write a helper function that returns a **Type Predicate** (`parameterName is Type`).

```typescript
interface Fish { swim: () => void; }
interface Bird { fly: () => void; }

// User-Defined Type Guard
function isFish(animal: Fish | Bird): animal is Fish {
  return (animal as Fish).swim !== undefined;
}

function move(animal: Fish | Bird) {
  if (isFish(animal)) {
    animal.swim(); // Safely narrowed to Fish
  } else {
    animal.fly();  // Safely narrowed to Bird (the remaining type)
  }
}
```

---

## 4. Discriminated Unions (Tagged Unions)

The **industry-standard pattern** for complex architectures (e.g. Redux reducers, compiler stages, API responses). A Discriminated Union requires:
1. Object types that share a **common literal property** (the "discriminant" or "tag").
2. A type guard (like a `switch` or `if` statement) checking that literal property.

```typescript
interface NetworkLoadingState {
  state: "loading"; // Discriminant
}

interface NetworkSuccessState {
  state: "success"; // Discriminant
  data: string;
}

interface NetworkFailedState {
  state: "failed";  // Discriminant
  error: Error;
}

type NetworkState = NetworkLoadingState | NetworkSuccessState | NetworkFailedState;

function renderState(network: NetworkState) {
  switch (network.state) {
    case "loading":
      return "Loading spinner...";
    case "success":
      return `Loaded data: ${network.data}`; // Narrowed! Knows data exists
    case "failed":
      return `Failed due to: ${network.error.message}`; // Narrowed! Knows error exists
  }
}
```

---

## 🔗 Related Concepts

- [[Primitive and Special Types]] — Using `never` for exhaustive matching in discriminated unions
- [[Type Aliases and Interfaces]] — Composing types into models
- [[Advanced Type Manipulation]] — Leveraging conditionals and maps over unions
