---
tags: [javascript, dom, web-development, computer-science, placement-prep]
aliases: [DOM Manipulation, Event Bubbling, Event Delegation, DOM Events]
created: 2026-08-09
---

# DOM Manipulation & Events

The **DOM (Document Object Model)** is the browser's tree representation of an HTML page — JavaScript uses it to read and change what's on screen.

## Selecting & Modifying Elements

```javascript
document.getElementById("id");
document.querySelector(".class");       // first match
document.querySelectorAll("div");        // NodeList of ALL matches (not a live array, but iterable)

el.textContent = "text";                  // sets text safely — no HTML parsing
el.innerHTML = "<b>bold</b>";             // parses as HTML — XSS risk if inserting raw user input!
el.classList.add("active");
el.classList.remove("active");
el.classList.toggle("active");
el.setAttribute("data-id", "5");
el.style.color = "red";
```
**Security note:** never set `innerHTML` with untrusted/user-provided content — it lets attackers inject `<script>` tags (XSS). Use `textContent` for plain text.

## Event Bubbling vs Capturing

An event travels through the DOM in **3 phases**:

```
Capturing (root → target)  →  Target  →  Bubbling (target → root)
```

By default, `addEventListener` listens in the **bubbling** phase.

```javascript
parent.addEventListener("click", handler, true);  // 3rd arg true = capturing phase
child.addEventListener("click", handler);          // default = bubbling phase

e.stopPropagation();   // stops the event from bubbling/capturing further
e.preventDefault();     // stops the default browser action (e.g. link navigation, form submit)
```

## Event Delegation

Attach **one** listener to a common parent instead of many listeners on individual children — leverages bubbling, and automatically works for children added dynamically later.

```javascript
document.querySelector("#list").addEventListener("click", (e) => {
  if (e.target.tagName === "LI") {
    console.log("Clicked:", e.target.textContent);
  }
});
```

**Why this is asked in interviews:** *"Why prefer event delegation over adding a listener to every list item?"* → Better performance (fewer listener objects in memory), automatically covers elements added later without re-binding, and less overall memory usage.

## setTimeout vs setInterval

```javascript
const id = setTimeout(() => console.log("once"), 1000);
clearTimeout(id);       // cancels before it fires

const id2 = setInterval(() => console.log("repeats"), 1000);
clearInterval(id2);     // stops repeated firing — forgetting this is a common memory-leak source
```
See [[Event Loop]] for how these timers actually get scheduled and run relative to Promises.

## Creating / Removing Elements

```javascript
const li = document.createElement("li");
li.textContent = "New item";
list.appendChild(li);
list.removeChild(li);
li.remove();               // modern shorthand
```

## Key Takeaways

- `textContent` is safe for inserting text; `innerHTML` parses HTML and is an XSS risk with untrusted input.
- Events bubble by default; capturing must be explicitly requested via the third argument to `addEventListener`.
- Event delegation is a standard interview + real-world performance pattern — one listener on a parent handles all current and future children.
- Always pair `setInterval` with `clearInterval` to avoid leaking timers that keep running after they're no longer needed.

## Related Concepts
- [[Event Loop]] — how timer/event callbacks get scheduled
- [[Common Coding Patterns]] — debounce/throttle are commonly applied to DOM events (scroll, input)
- [[Error Handling and Memory]] — forgotten listeners as a memory-leak source
