---
tags: [typescript, fundamentals, web-development, computer-science, placement-prep]
aliases: [oop, classes, access modifiers, abstract classes, constructor shorthand]
created: 2026-08-23
updated: 2026-08-23
---

# Classes and OOP in TypeScript

TypeScript provides robust object-oriented programming (OOP) capabilities. By adding strict compile-time access controls, constructor shorthand properties, abstract definitions, and interface adherence, it bridges standard JavaScript classes with professional system architectures.

---

## 1. Class Properties & Access Modifiers

In JavaScript, all properties on a class are public by default. TypeScript introduces three access modifiers to restrict access scope strictly at compile-time:

1. **`public`** (Default) — Property can be accessed from anywhere (inside class, subclass, and instances).
2. **`private`** — Property can only be accessed **inside** the class declaring it.
3. **`protected`** — Property can only be accessed **inside** the declaring class and any of its **subclasses**.

```typescript
class Employee {
  public name: string;             // Accessible everywhere
  private salary: number;          // Accessible only in Employee
  protected role: string;          // Accessible in Employee and its subclasses

  constructor(name: string, salary: number, role: string) {
    this.name = name;
    this.salary = salary;
    this.role = role;
  }

  public getSalary() {
    return this.salary; // Allowed: Internal access
  }
}

class Manager extends Employee {
  public increaseSalary() {
    // console.log(this.salary); // ❌ COMPILE ERROR: Property 'salary' is private and only accessible within class 'Employee'.
    console.log(this.role);      // ✅ Valid: role is protected and accessible in subclass.
  }
}
```

---

## 2. Parameter Properties (Constructor Shorthand)

Declaring fields, taking constructor arguments, and assigning fields dynamically is highly repetitive:

```typescript
// The verbose way
class VerboseClass {
  private id: number;
  public name: string;
  constructor(id: number, name: string) {
    this.id = id;
    this.name = name;
  }
}
```

### ✅ The Elegant Way (Constructor Parameter Properties)
You can declare class properties directly inside the constructor parameters by prepending them with an access modifier (`public`, `private`, `protected`) or `readonly`. TypeScript will automatically declare the field and initialize it with the parameter value!

```typescript
class CleanClass {
  constructor(private id: number, public name: string, readonly created: Date) {
    // No assignment body needed! Done automatically under the hood
  }
}

const instance = new CleanClass(101, "Aryan", new Date());
console.log(instance.name); // Prints: Aryan
// console.log(instance.id); // ❌ COMPILE ERROR: Property 'id' is private.
```

---

## 3. Abstract Classes vs. Interfaces

TypeScript supports **Abstract Classes**, which serve as base classes that **cannot be instantiated** directly. They exist solely for inheritance.

### Key Differences Comparison

| Feature | Abstract Class | Interface |
| :--- | :--- | :--- |
| **Instantiation**| Cannot be instantiated directly | Cannot be instantiated |
| **Logic** | Can contain concrete, implemented methods | Contains only declarations (no implementation) |
| **State** | Can hold and manage state variables | Cannot hold/store state fields (only structures) |
| **Erase-ability**| Compiled to a JS class representation | Erased completely at compile-time |

```typescript
abstract class PaymentProcessor {
  abstract process(amount: number): void; // Abstract method (subclasses MUST implement)

  protected printReceipt(amount: number) {
    console.log(`Receipt printed for: $${amount}`); // Concrete helper method (inheritable)
  }
}

class StripeProcessor extends PaymentProcessor {
  process(amount: number): void {
    console.log(`Charging $${amount} via Stripe...`);
    this.printReceipt(amount); // Call concrete method from base
  }
}

// const gateway = new PaymentProcessor(); // ❌ COMPILE ERROR: Cannot create an instance of an abstract class.
const stripe = new StripeProcessor();
stripe.process(100); // Works perfectly!
```

---

## 4. Implementing Interfaces

Interfaces can enforce that a class meets a strict structural contract. A single class can implement multiple interfaces.

```typescript
interface Loggable {
  log(): void;
}

interface Serializable {
  toJSON(): string;
}

class UserSession implements Loggable, Serializable {
  constructor(private userId: string) {}

  log(): void {
    console.log(`User session logged: ${this.userId}`);
  }

  toJSON(): string {
    return JSON.stringify({ userId: this.userId });
  }
}
```

---

## 🔗 Related Concepts

- [[Type Aliases and Interfaces]] — Interfaces and their definitions
- [[Functions in TypeScript]] — Typing `this` bindings inside methods
- [[Generics and Utility Types]] — Creating generic classes and polymorphic factories
