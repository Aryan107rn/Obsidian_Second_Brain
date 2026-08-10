---
tags: [javascript, fundamentals, web-development, computer-science, placement-prep, interview-favorite]
aliases: [this binding, call apply bind]
created: 2026-08-08
---

# The `this` Keyword

`this` refers to the object that is currently executing the function — but **its value depends entirely on how the function is called**, not where it's defined (except for arrow functions).

## The 4 Binding Rules (in order of precedence)

### 1. Default Binding (plain function call)
```javascript
function show() { console.log(this); }
show(); // `window`/`global` in non-strict mode, `undefined` in strict mode
```

### 2. Implicit Binding (called as a method)
```javascript
const obj = {
  name: "Alice",
  greet() { console.log(this.name); }
};
obj.greet(); // "Alice" — `this` = the object before the dot
```

### 3. Explicit Binding (`call`, `apply`, `bind`)
```javascript
function greet(greeting) { console.log(`${greeting}, ${this.name}`); }
const person = { name: "Bob" };

greet.call(person, "Hi");           // "Hi, Bob" — args passed individually
greet.apply(person, ["Hello"]);      // "Hello, Bob" — args passed as array
const bound = greet.bind(person);
bound("Hey");                          // "Hey, Bob" — bind returns a new function, doesn't call it immediately
```

### 4. `new` Binding (constructor call)
```javascript
function Person(name) { this.name = name; }
const p = new Person("Carol");
console.log(p.name); // "Carol" — `this` = the newly created object
```

## Arrow Functions Don't Have Their Own `this`

Arrow functions inherit `this` **lexically** from their enclosing scope — they ignore all 4 rules above.

```javascript
const obj = {
  name: "Dave",
  regular: function () {
    setTimeout(function () {
      console.log(this.name); // undefined — `this` = window/undefined (default binding)
    }, 100);
  },
  arrow: function () {
    setTimeout(() => {
      console.log(this.name); // "Dave" — arrow inherits `this` from `arrow()`
    }, 100);
  }
};
obj.regular();
obj.arrow();
```

**This is why arrow functions are preferred for callbacks inside methods** — they "just work" without needing `.bind(this)`.

## Losing `this` (a very common bug)

```javascript
const obj = {
  name: "Eve",
  greet() { console.log(this.name); }
};
const fn = obj.greet;
fn(); // undefined — `this` lost because it's now a plain function call
```
**Fix:** `const fn = obj.greet.bind(obj);`

## Precedence Summary Table

| Call type | `this` value |
|---|---|
| `new Fn()` | the new object |
| `fn.call(obj)` / `fn.apply(obj)` / `fn.bind(obj)()` | `obj` |
| `obj.method()` | `obj` |
| `fn()` (plain call) | `undefined` (strict) / global object (non-strict) |
| Arrow function | lexical — inherited from surrounding scope |

## Key Takeaways

- `this` is determined at **call time**, not definition time — except for arrow functions, which use lexical `this`.
- Precedence: `new` > explicit (`call`/`apply`/`bind`) > implicit (method call) > default.
- Detaching a method from its object (`const fn = obj.method`) loses the binding — a classic bug.
- `bind()` returns a new function; `call()`/`apply()` invoke immediately.

## Related Concepts
- [[Closures]] — arrow functions rely on lexical scoping like closures do
- [[Functions in JavaScript]] — regular vs arrow function differences
- [[ES6 Classes and OOP]] — `this` inside class methods
- [[JS Interview Questions and Tricky Outputs]]
