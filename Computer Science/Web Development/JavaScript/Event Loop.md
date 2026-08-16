---
tags: [javascript, async, web-development, computer-science, placement-prep, interview-favorite]
aliases: [Call Stack, Microtask Queue, Macrotask Queue, Event Loop JS]
created: 2026-08-09
updated: 2026-08-14
---

# Event Loop, Call Stack & Task Queues

JavaScript is a **single-threaded** language with **one Call Stack** — meaning it can execute only one line of code at a time. Yet, it handles long-running timers, network requests, and user clicks without freezing the browser UI. The **Event Loop** is the coordinator that makes this asynchronous concurrency possible.

---

## 🖼️ Visual Architecture: Follow the Numbered Steps (1 → 2 → 3 → 4)

![[js-event-loop-architecture.svg|960]]

---

## 🧩 The 4 Main Components Explained Simply

| Component | Role | How It Works |
| :--- | :--- | :--- |
| **1. Call Stack (LIFO)** | Code Execution | Where your JavaScript code runs line by line (Last-In, First-Out). Synchronous functions are pushed onto the stack and popped off when they return. |
| **2. Web APIs / libuv** | Background Waiting | Where async tasks (*waiting for 3s timer*, *downloading an HTTP response*, *listening for clicks*) wait **outside** the main JavaScript thread so they don't freeze the page. |
| **3A. Microtask Queue (VIP 🥇)** | High Priority Queue | Holds callbacks from `Promise.then()`, `Promise.catch()`, `async/await` continuations, and `queueMicrotask`. |
| **3B. Macrotask Queue** | Standard Priority Queue | Holds callbacks from `setTimeout()`, `setInterval()`, DOM events, and I/O. |
| **4. The Event Loop 🔄** | The Gatekeeper | A continuous loop that watches: *"Is the Call Stack empty? If yes, drain the entire Microtask queue first, then take ONE task from the Macrotask queue."* |

---

## 🚦 The 3 Golden Rules of Event Loop Execution

1. **Synchronous code always runs first**: No callback from any queue can enter the Call Stack until the stack is **100% empty**.
2. **Microtasks have VIP Priority (Full Drain)**: When the stack empties, the Event Loop executes **ALL pending Microtasks** (even if microtasks schedule more microtasks) before touching the Macrotask queue.
3. **One Macrotask per tick**: The Event Loop takes only **ONE** task from the Macrotask queue, pushes it to the Call Stack, and then immediately checks and drains the Microtask queue again.

---

## 🧪 Step-by-Step Traced Example (Classic Placement Interview Question)

```javascript
console.log("1");

setTimeout(() => {
  console.log("2");
}, 0);

Promise.resolve().then(() => {
  console.log("3");
});

console.log("4");

// 🎯 Final Output Order: 1, 4, 3, 2
```

### 🚀 Step-by-Step State Transition Trace

Instead of a complex timeline, let's track the **exact state** of the Call Stack and Queues at each line of code:

| Step | Code Line Executed | Call Stack (LIFO) | Microtask Queue (VIP 🥇) | Macrotask Queue | 🖥️ Output | Visual Trace / Explanation |
| :---: | :--- | :--- | :--- | :--- | :--- | :--- |
| **1** | `console.log("1")` | `["console.log('1')"]` | `[]` | `[]` | `1` | **Synchronous**: Pushed to stack, executed, and popped. Prints `1`. |
| **2** | `setTimeout(cb2, 0)` | `["setTimeout"]` | `[]` | `[cb2]` | `1` | **Asynchronous**: Handed to Web API. Since timer is `0ms`, Web API immediately moves the callback `cb2` to the Macrotask Queue. |
| **3** | `Promise.resolve().then(cb3)`| `["Promise.resolve"]`| `[cb3]` | `[cb2]` | `1` | **Asynchronous**: Promise resolves instantly and registers `cb3` in the Microtask Queue. |
| **4** | `console.log("4")` | `["console.log('4')"]` | `[cb3]` | `[cb2]` | `1, 4` | **Synchronous**: Executed immediately. Prints `4`. Call Stack is now **completely empty**. |
| **5** | *Draining Microtasks (VIP)* | `["cb3"]` | `[]` *(Empty)* | `[cb2]` | `1, 4, 3` | **Event Loop Action**: Since stack is empty, it drains the Microtask Queue first. Moves `cb3` to Call Stack. Prints `3`. |
| **6** | *Executing Macrotask* | `["cb2"]` | `[]` | `[]` *(Empty)* | `1, 4, 3, 2` | **Event Loop Action**: With both the stack and Microtasks empty, the Event Loop takes **ONE** task from the Macrotask queue (`cb2`). Prints `2`. |

### 📊 Visual Execution Flow

```mermaid
flowchart TD
    S1["1. console.log('1')"] --> S2["2. setTimeout callback registered"]
    S2 --> S3["3. Promise.then callback queued"]
    S3 --> S4["4. console.log('4')"]
    S4 --> S5["Call stack empty"]

    S5 --> M1["5. Run microtask callback"]
    M1 --> M2["Microtask queue empty"]

    M2 --> T1["6. Run macrotask callback"]
    T1 --> T2["All queues empty"]

    classDef sync fill:#DBEAFE,stroke:#2563EB,color:#111827,stroke-width:2px
    classDef empty fill:#DCFCE7,stroke:#16A34A,color:#111827,stroke-width:2px
    classDef micro fill:#FEF3C7,stroke:#D97706,color:#111827,stroke-width:2px
    classDef macro fill:#FCE7F3,stroke:#DB2777,color:#111827,stroke-width:2px
    class S1,S2,S3,S4 sync
    class S5,M2,T2 empty
    class M1 micro
    class T1 macro
```

---

## ⚖️ Microtasks vs. Macrotasks Cheat Sheet

| Feature | Microtasks (Promises) | Macrotasks (Timers / I/O) |
| :--- | :--- | :--- |
| **Examples** | `Promise.then()`, `await`, `queueMicrotask` | `setTimeout()`, `setInterval()`, DOM Events |
| **Priority** | 🥇 **Highest** (VIP) | 🥈 Standard |
| **Execution Amount** | **All** pending items in one go | **One** task at a time per tick |
| **Starvation Risk** | ⚠️ Yes: Recursive microtasks will starve rendering and timers! | ❌ No: Yields to microtasks and rendering |

---

## 🔗 Related Concepts
- [[Asynchronous JavaScript]] — Promises, async/await, and microtask scheduling
- [[Functions in JavaScript]] — Callback functions executed by the event loop
- [[JS Interview Questions and Tricky Outputs]] — Output-prediction problems and edge cases
