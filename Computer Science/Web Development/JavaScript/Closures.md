---
tags: [javascript, fundamentals, web-development, computer-science, placement-prep, interview-favorite]
aliases: [Closure, JS Closures]
created: 2026-08-08
---

# Closures

A **closure** is formed when a function "remembers" the variables from its lexical scope, even after the outer function has finished executing.

> Definition interviewers want to hear: *"A closure is the combination of a function and the lexical environment within which that function was declared, allowing the function to continue accessing variables from its outer scope after that outer function has returned."*

## Basic Example

```javascript
function makeCounter() {
  let count = 0;               // "private" variable
  return function () {
    count++;
    return count;
  };
}

const counter = makeCounter();
console.log(counter()); // 1
console.log(counter()); // 2
console.log(counter()); // 3 — count persists between calls!
```

Each call to `makeCounter()` creates a **new, independent** closure:

```javascript
const counterA = makeCounter();
const counterB = makeCounter();
console.log(counterA()); // 1
console.log(counterA()); // 2
console.log(counterB()); // 1 — separate closure, own `count`
```

## Practical Uses

### 1. Data Privacy / Encapsulation (Module Pattern)
```javascript
function createBankAccount(balance) {
  return {
    deposit(amount) { balance += amount; return balance; },
    withdraw(amount) { balance -= amount; return balance; },
    getBalance() { return balance; }
  };
}
const acc = createBankAccount(100);
acc.deposit(50);
console.log(acc.getBalance()); // 150
// `balance` is inaccessible from outside — true private state
```

### 2. Function Factories
```javascript
function multiplyBy(factor) {
  return (num) => num * factor;
}
const double = multiplyBy(2);
const triple = multiplyBy(3);
console.log(double(5)); // 10
console.log(triple(5)); // 15
```

### 3. Memoization (see [[Common Coding Patterns]])
### 4. `setTimeout` / event handler callbacks retaining state

## The Classic `var` in Loop Interview Question

```javascript
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100);
}
// Output: 3, 3, 3
```
**Why:** `var` is function-scoped — there is only ONE `i` shared by every callback. By the time `setTimeout` fires, the loop has finished and `i` is 3.

**Fix 1 — use `let` (block scope creates a new `i` per iteration):**
```javascript
for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100);
}
// Output: 0, 1, 2
```

**Fix 2 — force a new scope with an IIFE (the pre-ES6 solution):**
```javascript
for (var i = 0; i < 3; i++) {
  (function (capturedI) {
    setTimeout(() => console.log(capturedI), 100);
  })(i);
}
// Output: 0, 1, 2
```

## Key Takeaways

- A closure = function + the lexical scope it was created in.
- Closures let inner functions "remember" outer variables even after the outer function returns.
- Used for data privacy, currying, memoization, and event handler state.
- The `var`/`let` loop question is asked in nearly every JS interview — be able to explain AND fix it two ways.

## Related Concepts
- [[Scope and Hoisting]] — lexical scope is the foundation closures rely on
- [[Functions in JavaScript]] — IIFEs, function factories
- [[Common Coding Patterns]] — debounce, throttle, currying, memoization all use closures
- [[this Keyword]]
- [[JS Interview Questions and Tricky Outputs]]
