# 18. Browser JavaScript & Web APIs

This chapter is especially important for **SDE-2 full-stack interviews**, because it connects JavaScript language behavior with the **browser runtime**.

The most important distinction is:

> **JavaScript is the language. The browser provides the environment and Web APIs that JavaScript can interact with.**

For example:

```js
fetch("/api/users");
```

`fetch()` is **not a core JavaScript language feature**. It is a **Web API provided by the browser**.

Similarly:

```js
document.querySelector("#app");
```

`document` comes from the browser's DOM environment.

So keep this mental model:

```text
                 BROWSER
                    │
        ┌───────────┴────────────┐
        │                        │
   JavaScript Engine          Web APIs
       (V8 etc.)              │
        │                     ├── DOM
        │                     ├── Fetch
        │                     ├── Timers
        │                     ├── WebSocket
        │                     ├── Storage
        │                     └── Workers
        │
        └──────────────┐
                       │
                  Event Loop
```

This is also directly connected to **Point 16 — Event Loop & Concurrency**.

---

# 18.1 Browser Environment

When JavaScript runs in a browser, it gets access to a large collection of browser-provided APIs.

For example:

```js
console.log(window);
console.log(document);
console.log(location);
console.log(navigator);
```

These aren't part of the ECMAScript language itself.

They are provided by the browser environment.

Conceptually:

```text
ECMAScript
   │
   ├── Variables
   ├── Functions
   ├── Objects
   ├── Promises
   ├── Map
   ├── Set
   └── ...
   
Browser APIs
   │
   ├── DOM
   ├── Fetch
   ├── Storage
   ├── WebSocket
   ├── Timers
   ├── Web Workers
   └── ...
```

This distinction is a very common interview question.

---

# 18.2 DOM

DOM = **Document Object Model**.

The browser parses HTML:

```html
<body>
    <div id="app">
        <h1>Hello</h1>
    </div>
</body>
```

and creates an object representation that JavaScript can interact with.

Conceptually:

```text
Document
   │
   └── body
        │
        └── div#app
             │
             └── h1
                  │
                  └── "Hello"
```

JavaScript can then access it:

```js
const app = document.querySelector("#app");

console.log(app);
```

The DOM is essentially the browser's **object representation of the document**.

---

# 18.3 DOM Manipulation

You can create:

```js
const div = document.createElement("div");
```

Modify:

```js
div.textContent = "Hello";
```

Add:

```js
document.body.appendChild(div);
```

Remove:

```js
div.remove();
```

Modify attributes:

```js
div.setAttribute("class", "container");
```

Modify classes:

```js
div.classList.add("active");
```

---

## `textContent` vs `innerHTML`

Important security/performance distinction.

```js
element.textContent = userInput;
```

treats the input as text.

Whereas:

```js
element.innerHTML = userInput;
```

parses it as HTML.

If `userInput` is untrusted, blindly assigning it to `innerHTML` can create an **XSS vulnerability**.

This connects directly to Point 27 — Security.

---

# 18.4 DOM vs Virtual DOM

This is worth revising because you previously asked about it.

### DOM

The browser's actual document object structure.

### Virtual DOM

A programming abstraction used by libraries/frameworks such as React.

Conceptually:

```text
Application State
       ↓
Virtual DOM
       ↓
Diff / Reconciliation
       ↓
Real DOM updates
       ↓
Browser rendering
```

The Virtual DOM is **not a browser feature**.

It is a framework/library concept.

---

# 18.5 DOM Events

The browser is event-driven.

Examples:

```text
click
input
change
submit
keydown
keyup
mouseover
load
DOMContentLoaded
scroll
resize
```

You can listen to events:

```js
button.addEventListener("click", () => {
    console.log("Clicked");
});
```

The browser detects the event and eventually invokes the registered callback.

This connects directly to the event loop:

```text
User clicks
    ↓
Browser detects event
    ↓
Event handling
    ↓
Callback becomes executable
    ↓
Call stack
```

---

# 18.6 Event Listeners

Basic pattern:

```js
element.addEventListener("click", handler);
```

where:

```js
function handler(event) {
    console.log(event);
}
```

The callback receives an `Event` object.

You can inspect:

```js
event.target
event.currentTarget
event.type
```

One important distinction:

### `target`

The actual element where the event originated.

### `currentTarget`

The element whose listener is currently executing.

This becomes important during event propagation.

---

# 18.7 Event Propagation

Suppose:

```html
<div id="parent">
    <button id="child">Click</button>
</div>
```

You click the button.

The event doesn't simply execute on the button.

It propagates through the DOM.

Conceptually:

```text
        document
            │
            ▼
          parent
            │
            ▼
          button
            ▲
            │
            │
```

There are three conceptual stages:

```text
1. Capturing
2. Target
3. Bubbling
```

---

# 18.8 Capturing

The event travels **from the outside toward the target**.

```text
document
   ↓
html
   ↓
body
   ↓
parent
   ↓
button
```

You can listen during capture:

```js
parent.addEventListener(
    "click",
    handler,
    true
);
```

or:

```js
parent.addEventListener(
    "click",
    handler,
    { capture: true }
);
```

---

# 18.9 Bubbling

After reaching the target, the event can travel back upward.

```text
button
   ↑
parent
   ↑
body
   ↑
html
   ↑
document
```

This is the default behavior for many events.

Example:

```js
parent.addEventListener("click", () => {
    console.log("parent");
});

button.addEventListener("click", () => {
    console.log("button");
});
```

Clicking the button can produce:

```text
button
parent
```

because the event bubbles from the button to its parent.

---

# 18.10 `stopPropagation()`

You can stop further propagation:

```js
button.addEventListener("click", (event) => {
    event.stopPropagation();
});
```

This prevents the event from continuing through the propagation path.

Don't confuse this with:

```js
event.preventDefault();
```

They do different things.

### `stopPropagation()`

Controls **event propagation**.

### `preventDefault()`

Prevents the browser's **default action**.

For example:

```js
event.preventDefault();
```

on a link can prevent navigation.

---

# 18.11 Event Delegation

This is a very important SDE-2 browser concept.

Suppose you have:

```html
<ul id="users">
    <li>User 1</li>
    <li>User 2</li>
    <li>User 3</li>
    ...
</ul>
```

Instead of attaching listeners to every `<li>`:

```js
li.addEventListener(...)
```

you can attach one listener to the parent:

```js
users.addEventListener("click", (event) => {
    if (event.target.matches("li")) {
        console.log(event.target.textContent);
    }
});
```

This works because of **event bubbling**.

Conceptually:

```text
li click
   ↓
bubbles
   ↓
ul listener
```

### Why use delegation?

Instead of:

```text
1000 elements
   ↓
1000 event listeners
```

you can have:

```text
1000 elements
   ↓
1 parent listener
```

It can reduce listener management overhead and is particularly useful for **dynamically created elements**.

---

# 18.12 Browser Storage

The browser provides several mechanisms for client-side persistence.

The important ones:

```text
Cookies
localStorage
sessionStorage
```

They have different purposes.

---

# 18.13 Cookies

Cookies are small pieces of data associated with a domain.

They can be sent automatically with HTTP requests depending on their attributes.

Example:

```http
Set-Cookie: sessionId=abc123
```

Important cookie attributes:

```text
HttpOnly
Secure
SameSite
Domain
Path
Expires / Max-Age
```

For authentication, an important pattern is:

```text
HttpOnly
Secure
SameSite
```

An `HttpOnly` cookie cannot be read through:

```js
document.cookie
```

This helps reduce the impact of certain XSS attacks involving token theft.

But remember:

> **HttpOnly does not prevent XSS itself.**

It primarily prevents JavaScript from directly reading that cookie.

---

# 18.14 `localStorage`

Example:

```js
localStorage.setItem("theme", "dark");
```

Read:

```js
const theme = localStorage.getItem("theme");
```

Remove:

```js
localStorage.removeItem("theme");
```

Clear:

```js
localStorage.clear();
```

Its important property:

> Data persists across browser sessions until it is explicitly removed or otherwise cleared by the browser/user.

---

# 18.15 `sessionStorage`

Similar API:

```js
sessionStorage.setItem("token", "abc");
```

But it is associated with the page session / browsing context.

Conceptually:

```text
localStorage
    ↓
persistent

sessionStorage
    ↓
session-oriented
```

Both are accessible to JavaScript running on the relevant origin, so **don't treat them as secure secret storage**.

---

# 18.16 Cookies vs localStorage vs sessionStorage

A useful interview table:

| Feature                          | Cookies                   | localStorage        | sessionStorage                |
| -------------------------------- | ------------------------- | ------------------- | ----------------------------- |
| JS accessible                    | Yes, unless `HttpOnly`    | Yes                 | Yes                           |
| Automatically sent with requests | Yes, when applicable      | No                  | No                            |
| Persistence                      | Configurable              | Persistent          | Session/page-context oriented |
| Typical use                      | Sessions/auth/preferences | Client persistence  | Temporary page-session data   |
| XSS exposure                     | Reduced with `HttpOnly`   | Directly accessible | Directly accessible           |

For authentication, don't blindly say:

> "JWT should always go into localStorage."

The correct answer is:

> Storage strategy depends on the threat model. HttpOnly secure cookies are commonly preferred for session/authentication tokens when appropriate because JavaScript cannot directly read them.

---

# 18.17 Fetch API

`fetch()` is the modern browser API for HTTP requests.

Example:

```js
const response = await fetch("/api/users");

const users = await response.json();
```

Important:

> `fetch()` returns a **Promise**.

Therefore:

```text
fetch()
  ↓
Promise
  ↓
HTTP response
  ↓
response.json()
  ↓
Promise
```

This connects directly to Point 15.

---

# 18.18 Fetch Does Not Reject on HTTP 4xx/5xx

This is a classic interview question.

Consider:

```js
const response = await fetch("/api/users");
```

If the server responds:

```text
404
```

the fetch promise normally **does not reject merely because the HTTP status is 404**.

You should check:

```js
if (!response.ok) {
    throw new Error(`HTTP ${response.status}`);
}
```

Then:

```js
const data = await response.json();
```

This distinction is important:

```text
Network-level failure
       ↓
Promise rejection

HTTP 404 / 500
       ↓
Promise usually resolves
       ↓
response.ok === false
```

---

# 18.19 XMLHttpRequest

Before `fetch()`, browsers commonly used:

```js
XMLHttpRequest
```

Example conceptually:

```js
const xhr = new XMLHttpRequest();

xhr.open("GET", "/api/users");

xhr.onload = () => {
    console.log(xhr.responseText);
};

xhr.send();
```

It is still supported, but modern application code generally prefers `fetch()` or higher-level libraries.

Know it primarily for **legacy code and interviews**.

---

# 18.20 Timers

Browser JavaScript provides:

```js
setTimeout()
setInterval()
clearTimeout()
clearInterval()
```

Example:

```js
setTimeout(() => {
    console.log("Hello");
}, 1000);
```

Important:

> `1000ms` does **not** mean the callback executes exactly after 1000ms.

It means approximately:

> Make the callback eligible after the timer delay; actual execution depends on the event loop and whether the main thread is available.

For example:

```js
setTimeout(() => {
    console.log("timer");
}, 0);

while (true) {}
```

The timer callback won't execute because the main JavaScript thread is blocked.

This connects directly to Point 16.

---

# 18.21 Web APIs

This is the broad category.

Examples:

```text
DOM APIs
Fetch API
Storage API
Timer APIs
WebSocket API
Web Workers
Geolocation
Notifications
Clipboard
Canvas
Media APIs
```

The browser exposes these capabilities to JavaScript.

Therefore:

```text
JavaScript language
        +
Browser Web APIs
        +
Event Loop
        =
Browser application runtime
```

This is one of the most important mental models in the entire JavaScript revision.

---

# 18.22 Web Workers

Normally browser JavaScript executes on the main thread.

That thread handles important work such as:

```text
JavaScript
DOM interaction
UI-related work
event processing
```

Heavy synchronous computation can block it.

Example:

```js
for (let i = 0; i < 10_000_000_000; i++) {
    // expensive work
}
```

The UI can become unresponsive.

A **Web Worker** allows JavaScript to execute in another thread.

Conceptually:

```text
Main Thread
     │
     │ postMessage()
     ▼
Worker Thread
     │
     │ postMessage()
     ▼
Main Thread
```

Example:

```js
const worker = new Worker("worker.js");

worker.postMessage({
    task: "calculate"
});

worker.onmessage = (event) => {
    console.log(event.data);
};
```

Workers communicate using messages rather than directly sharing the normal DOM.

---

# 18.23 Web Workers vs Async JavaScript

This is an important distinction.

### Async JavaScript

Doesn't necessarily mean another thread.

For example:

```js
await fetch("/api");
```

The JavaScript thread isn't blocked waiting for the network operation.

### Web Worker

Actually executes JavaScript separately from the main thread.

So:

```text
Async I/O
    ≠
Multithreading
```

This is an excellent SDE-2 distinction.

---

# 18.24 WebSockets

WebSocket provides a persistent, bidirectional communication channel.

Normal HTTP:

```text
Client ───── request ─────► Server
Client ◄──── response ───── Server
```

WebSocket:

```text
Client ◄══════════════════► Server
       persistent connection
```

After the connection is established, either side can send messages.

Example:

```js
const socket = new WebSocket("wss://example.com");

socket.onopen = () => {
    socket.send("Hello");
};

socket.onmessage = (event) => {
    console.log(event.data);
};
```

Common use cases:

* Chat
* Notifications
* Live dashboards
* Multiplayer games
* Real-time collaboration
* Trading/market data

---

# 18.25 Browser Security Model

This is intentionally high-level in this chapter.

The browser is designed around **isolating web applications from one another**.

One of the most important concepts is:

> **Same-Origin Policy**

An origin is conceptually:

```text
scheme + host + port
```

For example:

```text
https://example.com:443
```

is an origin.

Different origins are generally subject to browser security restrictions.

---

# 18.26 Same-Origin Policy

Suppose your frontend is:

```text
https://app.example.com
```

and it requests:

```text
https://api.other.com
```

That's cross-origin.

The browser applies security rules to determine whether JavaScript is allowed to access the response.

This is where **CORS** comes in.

---

# 18.27 CORS

CORS = **Cross-Origin Resource Sharing**.

The server can tell the browser:

```http
Access-Control-Allow-Origin: https://app.example.com
```

which allows the browser to expose the response to that origin's JavaScript under the applicable CORS rules.

Very important:

> **CORS is primarily a browser security mechanism.**

It isn't fundamentally a server-to-server restriction.

For example:

```text
Node.js server
     │
     └── request → another server
```

doesn't automatically face browser CORS enforcement simply because it is HTTP.

---

# 18.28 XSS Connection

Suppose an attacker injects:

```html
<script>
    stealData();
</script>
```

into your page.

That is **Cross-Site Scripting (XSS)**.

The attacker is attempting to execute JavaScript in the context of your application.

Common defenses include:

```text
Input/output handling
Content Security Policy
Safe DOM APIs
Avoiding unsafe HTML injection
HttpOnly cookies for sensitive session cookies
```

This will be covered deeply in Point 27.

---

# 18.29 The Browser Runtime — Putting Everything Together

This is the most important part of Point 18.

Consider:

```js
button.addEventListener("click", async () => {
    const response = await fetch("/api/users");

    const users = await response.json();

    console.log(users);
});
```

What is happening?

```text
User clicks button
        ↓
Browser event system
        ↓
Event callback
        ↓
JavaScript executes
        ↓
fetch()
        ↓
Browser Web API handles network operation
        ↓
JavaScript can continue / callback awaits Promise
        ↓
Network completes
        ↓
Promise becomes settled
        ↓
Continuation becomes a microtask
        ↓
Event loop schedules it
        ↓
JavaScript resumes
        ↓
response.json()
        ↓
DOM/application logic
```

Notice how many chapters are connected:

```text
Functions
   ↓
Closures
   ↓
Promises
   ↓
Async/Await
   ↓
Event Loop
   ↓
Browser Web APIs
   ↓
DOM / Events
```

That's why Point 18 comes naturally after Point 16.

---

# 18.30 What You Should Be Interview-Ready On

For your SDE-2 preparation, I'd prioritize these:

### 🔴 Very High Priority

```text
DOM
Event propagation
Capturing vs bubbling
Event delegation
Fetch
Timers
Browser vs JS runtime
CORS
Same-Origin Policy
localStorage vs cookies
WebSockets
Web Workers
```

### 🟡 High Priority

```text
BOM
sessionStorage
XMLHttpRequest
DOM manipulation
preventDefault vs stopPropagation
target vs currentTarget
```

### 🟢 Know Conceptually

```text
Browser Web APIs
Browser security model
Storage lifecycle
DOM architecture
```

---

# 18.31 SDE-2 Questions You Should Be Able to Answer

After this chapter, you should comfortably answer:

1. **Is `fetch()` part of JavaScript?**
2. **What is the difference between JavaScript and the browser runtime?**
3. **What is the DOM?**
4. **What is event propagation?**
5. **Capturing vs bubbling?**
6. **What is event delegation and why use it?**
7. **`event.target` vs `event.currentTarget`?**
8. **`preventDefault()` vs `stopPropagation()`?**
9. **Cookies vs localStorage vs sessionStorage?**
10. **Why are HttpOnly cookies useful?**
11. **Does `fetch()` reject on HTTP 404?**
12. **What does `setTimeout(fn, 0)` actually mean?**
13. **What happens if the main thread is blocked?**
14. **Async JavaScript vs Web Worker?**
15. **WebSocket vs HTTP?**
16. **What is CORS?**
17. **What is Same-Origin Policy?**
18. **Why is event delegation useful for dynamically created elements?**
19. **How can event listeners cause memory leaks?**
20. **How does a browser event eventually execute a JavaScript callback?**

---

## The core mental model for Point 18

If you remember only one thing:

```text
                 BROWSER
                    │
       ┌────────────┼─────────────┐
       │            │             │
      DOM        Web APIs       Storage
       │            │
       │       ┌────┼─────┬──────┐
       │       │    │     │      │
       │     Fetch Timer  WS   Workers
       │       │    │     │
       └───────┴────┴─────┴────────┐
                                   │
                              Event Loop
                                   │
                                   ▼
                            JavaScript Engine
                                   │
                              Call Stack
```

**Point 16 taught us how asynchronous work gets scheduled.**

**Point 18 teaches us where much of that asynchronous work actually comes from in a browser.**

And that's the key SDE-2 perspective:

> **JavaScript itself is relatively small; the browser runtime gives JavaScript access to the DOM, networking, timers, storage, workers, WebSockets, and many other capabilities.**
