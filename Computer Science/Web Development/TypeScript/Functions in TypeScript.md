---
tags: [typescript, fundamentals, web-development, computer-science, placement-prep]
aliases: [overloads, function signatures, construct signatures, parameter typing]
created: 2026-08-23
updated: 2026-08-23
---

# Functions in TypeScript

Functions are the core building blocks of JavaScript applications. TypeScript allows us to type function parameters, return values, optional properties, dynamic binding scopes (`this`), and write sophisticated multi-signature overloads.

---

## 1. Parameters & Return Type Annotations

TypeScript types both the **input parameters** and the **output return values** of functions. If a return type is omitted, TypeScript will attempt to infer it dynamically.

```typescript
function add(x: number, y: number): number {
  return x + y;
}

// Arrow function syntax
const multiply = (x: number, y: number): number => x * y;
```

### Optional & Default Parameters
- **Optional Parameters (`?`):** Must always be declared **after** mandatory parameters. They receive a type of `T | undefined`.
- **Default Parameters:** Automatically infer their type and make the argument optional when called.

```typescript
function greet(name: string, title?: string): string {
  if (title) return `Hello, ${title} ${name}`;
  return `Hello, ${name}`;
}

function calculateTax(amount: number, rate = 0.15): number {
  return amount * rate;
}
```

### Rest Parameters
Rest parameters are used to capture multiple arguments. They must be typed as an array or a tuple:

```typescript
function sumAll(label: string, ...numbers: number[]): string {
  const sum = numbers.reduce((total, n) => total + n, 0);
  return `${label}: ${sum}`;
}
```

---

## 2. Function & Call Signatures

Function types can be declared using **arrow syntax** or **object-style call signatures** (when a function needs to double as an object containing properties).

### Arrow Type Signatures
```typescript
type StringManipulator = (str: string, uppercase: boolean) => string;

const transform: StringManipulator = (str, upper) => {
  return upper ? str.toUpperCase() : str.toLowerCase();
};
```

### Object Call Signatures
In JavaScript, functions are first-class objects and can have properties. We write an object call signature without an arrow:

```typescript
interface SmartLogger {
  defaultLevel: string;
  (message: string, level?: string): void; // Note: No arrow syntax
}

const logger: SmartLogger = Object.assign(
  (msg: string, lvl?: string) => console.log(`[${lvl || logger.defaultLevel}] ${msg}`),
  { defaultLevel: "INFO" }
);
```

### Construct Signatures
Used to type functions that can be invoked with the `new` operator (constructors).

```typescript
interface UserConstructor {
  new (username: string): { username: string; active: boolean };
}
```

---

## 3. Function Overloading

JavaScript functions often accept different types and quantities of arguments. TypeScript supports **Function Overloading** by defining multiple **overload signatures** followed by a single **implementation signature**.

### Enforcing Strict Input-to-Output Mappings
Suppose we want a function that returns a `string` if passed a `string`, or a `Date` if passed a `number`.

```typescript
// 1. Overload Signatures (Public API)
function makeDate(timestamp: number): Date;
function makeDate(m: number, d: number, y: number): Date;

// 2. Implementation Signature (Internal - Must support all overloads)
function makeDate(mOrTimestamp: number, d?: number, y?: number): Date {
  if (d !== undefined && y !== undefined) {
    return new Date(y, mOrTimestamp, d);
  }
  return new Date(mOrTimestamp);
}

const date1 = makeDate(1700000000000); // Resolves to Overload 1
const date2 = makeDate(8, 23, 2026);    // Resolves to Overload 2
// const date3 = makeDate(8, 23);       // ❌ COMPILE ERROR: No overload matches 2 arguments.
```

> ⚠️ **Overload Rule:** The implementation signature is **not** visible to users. Only the overload signatures are publicly callable. The implementation signature must be fully compatible with every overload signature preceding it.

---

## 4. Typing `this` Dynamically

In JavaScript, `this` is bound dynamically based on how a function is called. TypeScript lets you declare the expected type of `this` by adding a parameter named `this` as the **very first parameter** in the function definition.

```typescript
interface Button {
  label: string;
}

function clickHandler(this: Button, event: Event) {
  // TypeScript knows 'this' is a Button and has property 'label'
  console.log(`${this.label} clicked!`); 
}

const btn: Button = { label: "Submit" };

// Safe calling using call/apply/bind
clickHandler.call(btn, new Event("click")); // Valid!

// clickHandler(new Event('click')); // ❌ COMPILE ERROR: The 'this' context of type 'void' is not assignable to method's 'this' of type 'Button'.
```

---

## 🔗 Related Concepts

- [[Primitive and Special Types]] — Understanding `void` and basic parameter types
- [[Type Aliases and Interfaces]] — Defining reusable function schemas
- [[Generics and Utility Types]] — Parametric function signatures
