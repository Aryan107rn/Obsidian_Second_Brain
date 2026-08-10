---
tags: [javascript, oop, web-development, computer-science, placement-prep]
aliases: [Prototypal Inheritance, ES6 Classes, Prototype Chain, JS OOP]
created: 2026-08-09
---

# Prototypes, Inheritance & Classes

## Prototypal Inheritance

Every JS object has a hidden internal link — `[[Prototype]]` (accessible via `__proto__` or `Object.getPrototypeOf`) — to another object it inherits from. When you access a property, JS first checks the object itself, then walks up this **prototype chain** until it finds the property or reaches `null`.

```javascript
const animal = { eats: true };
const rabbit = Object.create(animal); // rabbit's prototype = animal
rabbit.hops = true;

console.log(rabbit.eats); // true — found via prototype chain
console.log(rabbit.hasOwnProperty("eats")); // false — inherited, not own
console.log(rabbit.hasOwnProperty("hops")); // true
```

### How functions create objects with prototypes (pre-ES6 way)

```javascript
function Animal(name) {
  this.name = name;
}
Animal.prototype.speak = function () {
  console.log(`${this.name} makes a sound`);
};

const dog = new Animal("Rex");
dog.speak(); // "Rex makes a sound"
// `speak` isn't copied onto every instance — it lives once on Animal.prototype,
// and instances find it via the prototype chain. This saves memory.
```

`new Animal("Rex")` does 4 things internally:
1. Creates a new empty object `{}`
2. Sets that object's `[[Prototype]]` to `Animal.prototype`
3. Calls `Animal` with `this` bound to the new object
4. Returns the new object (unless the constructor explicitly returns another object)

## ES6 Classes — Syntactic Sugar Over Prototypes

Classes don't introduce a new inheritance model — `class` is a cleaner syntax over the exact same prototype chain underneath.

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }
  speak() {
    console.log(`${this.name} makes a sound`);
  }
  static create(name) {           // static: called on the class, not instances
    return new Animal(name);
  }
}

class Dog extends Animal {
  speak() {
    super.speak();                 // call the parent's method
    console.log(`${this.name} barks`);
  }
}

new Dog("Rex").speak();
// "Rex makes a sound"
// "Rex barks"
```

**Proof they're the same system:** `Dog.prototype.__proto__ === Animal.prototype` → `true`.

### Class Fields, Private Fields, Getters/Setters

```javascript
class BankAccount {
  #balance;                          // private field (ES2022) — truly inaccessible outside
  constructor(balance) { this.#balance = balance; }

  get balance() { return this.#balance; }         // getter — accessed like a property
  set balance(amount) {                             // setter — validated assignment
    if (amount < 0) throw new Error("Cannot be negative");
    this.#balance = amount;
  }
  deposit(amount) { this.#balance += amount; }
}

const acc = new BankAccount(100);
console.log(acc.balance);  // 100 (calls getter, not a method call!)
acc.balance = 200;           // calls setter
// acc.#balance;              // SyntaxError — truly private, unlike closures which only "hide"
```

### Getters/Setters on Plain Objects

```javascript
const user = {
  firstName: "A", lastName: "S",
  get fullName() { return `${this.firstName} ${this.lastName}`; },
  set fullName(value) { [this.firstName, this.lastName] = value.split(" "); }
};
user.fullName;          // "A S" — accessed like a property, not user.fullName()
user.fullName = "New Name"; // invokes the setter
```

## Object Utility Methods

| Method | Purpose |
|---|---|
| `Object.keys(obj)` | array of keys |
| `Object.values(obj)` | array of values |
| `Object.entries(obj)` | array of `[key, value]` pairs |
| `Object.freeze(obj)` | makes object immutable (**shallow** — nested objects still mutable) |
| `Object.assign(target, src)` | shallow-copies props into target |
| `Object.create(proto)` | creates a new object with the given prototype |
| `structuredClone(obj)` | deep clone (modern browsers/Node) |

## Key Takeaways

- Objects inherit from other objects via the prototype chain — there's no true "class" at the engine level.
- `class`/`extends`/`super` are syntactic sugar over prototype-based inheritance.
- `#field` (ES2022) gives real private state, enforced by the engine — unlike closures which only conventionally hide data.
- Getters/setters let you run code on property access/assignment while keeping normal-looking syntax.
- `Object.freeze()` and spread/`Object.assign` copies are shallow — nested structures still share references.

## Related Concepts
- [[Closures]] — the pre-class way to achieve private state
- [[this Keyword]] — `this` inside class methods follows the same "how it's called" rules
- [[Objects Destructuring Spread Rest]]
- [[Functions in JavaScript]] — constructor functions predate class syntax
- [[JS Interview Questions and Tricky Outputs]]
