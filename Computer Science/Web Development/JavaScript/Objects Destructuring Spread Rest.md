---
tags: [javascript, fundamentals, web-development, computer-science, placement-prep]
aliases: [Destructuring, Spread Operator, Rest Operator, Object Methods JS]
created: 2026-08-08
---

# Objects, Destructuring, Spread & Rest

## Object Basics

```javascript
const person = { name: "Alice", age: 25 };

person.name;         // dot notation
person["age"];        // bracket notation — needed for dynamic keys
person.city = "NYC";  // add a property
delete person.age;    // remove a property

// Dynamic key
const key = "job";
const obj = { [key]: "Engineer" }; // computed property name -> { job: "Engineer" }
```

## Useful Object Methods

```javascript
Object.keys(person);           // ["name", "city"]
Object.values(person);          // ["Alice", "NYC"]
Object.entries(person);         // [["name","Alice"], ["city","NYC"]]
Object.assign({}, person, { age: 30 }); // shallow merge into a new object
Object.freeze(person);          // makes object immutable (shallow)
Object.isFrozen(person);        // true
```

## Destructuring

### Object Destructuring
```javascript
const user = { name: "Bob", age: 30, address: { city: "LA" } };

const { name, age } = user;
console.log(name, age); // "Bob" 30

const { name: fullName } = user;   // rename while destructuring
const { country = "USA" } = user;   // default value if key missing
const { address: { city } } = user; // nested destructuring
```

### Array Destructuring
```javascript
const [first, second, ...rest] = [1, 2, 3, 4, 5];
console.log(first, second, rest); // 1 2 [3, 4, 5]

let a = 1, b = 2;
[a, b] = [b, a]; // swap without a temp variable!
console.log(a, b); // 2 1
```

### Destructuring in Function Parameters
```javascript
function printUser({ name, age = 18 }) {
  console.log(`${name} is ${age}`);
}
printUser({ name: "Carol" }); // "Carol is 18"
```

## Spread Operator (`...`) — Expands

```javascript
const arr1 = [1, 2, 3];
const arr2 = [...arr1, 4, 5];        // [1,2,3,4,5] — non-mutating copy + append

const obj1 = { a: 1, b: 2 };
const obj2 = { ...obj1, c: 3 };       // shallow copy + merge

function sum(a, b, c) { return a + b + c; }
sum(...[1, 2, 3]); // 6 — spreads array into individual arguments
```

## Rest Operator (`...`) — Collects

```javascript
function sum(...nums) {              // rest: gathers args into an array
  return nums.reduce((a, b) => a + b, 0);
}

const { a, ...others } = { a: 1, b: 2, c: 3 };
console.log(others); // { b: 2, c: 3 }
```

**Rule of thumb:** spread is used where values are *expected* (array/object literal, function call); rest is used in *destructuring patterns* or function parameter lists.

## Shallow Copy Gotcha

```javascript
const original = { a: 1, nested: { b: 2 } };
const copy = { ...original };
copy.nested.b = 99;
console.log(original.nested.b); // 99 — nested objects are still shared references!
```
For a true deep copy: `structuredClone(original)` (modern) or `JSON.parse(JSON.stringify(original))` (loses functions/undefined/dates).

## Key Takeaways

- Destructuring extracts values with renaming and defaults; works on objects, arrays, and function params.
- Spread expands a collection; rest collects into a collection — same syntax, opposite direction, context tells them apart.
- Spread/`Object.assign` only do **shallow** copies — nested objects still share references.
- `structuredClone()` is the modern built-in for deep cloning.

## Related Concepts
- [[Variables and Data Types]] — reference vs value copying
- [[Array Methods]] — spread used heavily with arrays
- [[Functions in JavaScript]] — rest parameters, default parameters
- [[ES6+ Modern Features]]
