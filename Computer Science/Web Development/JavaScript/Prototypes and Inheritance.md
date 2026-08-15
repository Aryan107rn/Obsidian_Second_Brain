---
tags: [javascript, oop, web-development, computer-science, placement-prep, interview-favorite]
aliases: [Prototypal Inheritance, ES6 Classes, Prototype Chain, JS OOP]
created: 2026-08-09
updated: 2026-08-14
---

# Prototypes, Inheritance & Classes

## Prototypal Inheritance

Every JavaScript object has a hidden internal link — `[[Prototype]]` (accessible via `__proto__` or `Object.getPrototypeOf`) — pointing to another object it inherits from. When you access a property, the JavaScript engine first inspects the object itself, then walks up this **prototype chain** until it finds the property or reaches `null`.

---

## 🖼️ Prototypal Inheritance & Prototype Chain

![[js-prototype-chain.svg|960]]

---

## Prototype Chain Example

```javascript
const animal = { eats: true };
const rabbit = Object.create(animal); // rabbit's prototype = animal
rabbit.hops = true;

console.log(rabbit.eats); // true — resolved via prototype chain
console.log(rabbit.hasOwnProperty("eats")); // false — inherited from animal
console.log(rabbit.hasOwnProperty("hops")); // true — own property
```

---

## How Constructor Functions Create Objects (Pre-ES6)

```javascript
function Animal(name) {
  this.name = name;
}
Animal.prototype.speak = function () {
  console.log(`${this.name} makes a sound`);
};

const dog = new Animal("Rex");
dog.speak(); // "Rex makes a sound"
// `speak` is not duplicated across instances — it resides on Animal.prototype, saving memory.
```

The `new` keyword performs 4 steps internally:
1. Creates a brand new empty object `{}`.
2. Sets the new object's `[[Prototype]]` (`__proto__`) to `Animal.prototype`.
3. Invokes `Animal` with `this` bound to the newly created object.
4. Returns the new object.

---

## ES6 Classes — Syntactic Sugar Over Prototypes

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }
  speak() {
    console.log(`${this.name} makes a sound`);
  }
  static create(name) {
    return new Animal(name);
  }
}

class Dog extends Animal {
  speak() {
    super.speak();
    console.log(`${this.name} barks`);
  }
}

const dogInstance = new Dog("Rex");
dogInstance.speak();
// Proof of underlying prototype chain:
// Dog.prototype.__proto__ === Animal.prototype -> true
```

---

## Private Fields & Getters/Setters

```javascript
class BankAccount {
  #balance; // ES2022 Private field — truly inaccessible outside class body

  constructor(balance) {
    this.#balance = balance;
  }

  get balance() {
    return this.#balance;
  }

  set balance(amount) {
    if (amount < 0) throw new Error("Balance cannot be negative");
    this.#balance = amount;
  }

  deposit(amount) {
    this.#balance += amount;
  }
}
```

---

## 🔗 Related Concepts
- [[Closures]] — Alternative pattern for data privacy
- [[this Keyword]] — How execution context works inside methods
- [[Objects Destructuring Spread Rest]] — Shallow copying vs. prototype linking
- [[Functions in JavaScript]] — Constructor functions
- [[JS Interview Questions and Tricky Outputs]] — Prototype gotchas
