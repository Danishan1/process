Absolutely. **Point 19 — Node.js Runtime** is one of the highest-value areas for your SDE-II preparation because this is where your JavaScript knowledge turns into **backend/runtime engineering**.

For SDE-II, I don't want to treat this as:

> "What is Node.js?"

We should be able to reason about:

> **What exactly happens inside a Node.js process when a request arrives, when it performs I/O, when it consumes CPU, how memory is managed, how concurrency works, and how we scale or isolate work.**

We'll go deep, but **not into V8 source-code / engine-internals Staff-level territory**.

---

# 19. Node.js JavaScript Runtime — SDE-II

## 19.1 First: What Exactly Is Node.js?

The most important definition:

> **Node.js is a JavaScript runtime that allows JavaScript to execute outside the browser.**

It is built around several major components:

```text
                 NODE.JS
                    │
        ┌───────────┼────────────┐
        │           │            │
       V8       Node APIs     libuv
        │           │            │
   Executes JS   fs/http/etc.   Async I/O
        │           │            │
        └───────────┼────────────┘
                    │
               Event Loop
```

A simplified architecture:

```text
Application JavaScript
          │
          ▼
     Node.js APIs
          │
    ┌─────┴─────┐
    ▼           ▼
   V8          libuv
    │           │
    │           ├── Event Loop
    │           ├── Async I/O
    │           └── Thread Pool
    │
    ▼
Native / OS capabilities
```

There are three terms you should clearly distinguish:

### V8

Executes JavaScript.

### Node.js APIs

Expose backend capabilities such as:

```js
fs
http
crypto
stream
process
```

### libuv

Provides much of Node's cross-platform asynchronous I/O infrastructure and event-loop implementation.

---

# 19.2 Node.js Is Not "Single-Threaded"

This is probably the most common Node.js interview trap.

People often say:

> "Node.js is single-threaded."

That's an oversimplification.

A better SDE-II answer:

> **JavaScript execution in a Node.js process normally occurs on a single main thread, but Node.js itself uses additional native threads and OS facilities for certain operations, and it supports explicit worker threads and child processes.**

Conceptually:

```text
Node.js Process
│
├── Main JavaScript thread
│      │
│      └── V8 + Event Loop
│
├── libuv
│      └── Thread Pool
│
└── OS / kernel
       └── async networking / I/O
```

This distinction becomes extremely important when discussing performance.

---

# 19.3 V8

V8 is Google's JavaScript engine.

Node.js uses V8 to execute JavaScript.

For example:

```js
const x = 10;
const y = 20;

console.log(x + y);
```

V8 is responsible for executing this JavaScript.

Conceptually:

```text
JavaScript
    ↓
Parser
    ↓
Internal representation
    ↓
Execution / JIT optimization
    ↓
Machine code
```

For SDE-II, know:

* V8 executes JavaScript.
* V8 manages JavaScript memory.
* V8 has a garbage collector.
* V8 performs runtime optimizations.
* Node.js embeds V8.

You don't need to memorize V8's internal compiler pipeline yet.

---

# 19.4 Node APIs

Node exposes functionality that browsers don't normally provide.

For example:

```js
const fs = require("fs");
```

or:

```js
import fs from "node:fs";
```

Then:

```js
fs.readFile(...)
```

Node provides APIs for:

```text
File system
Networking
HTTP
Streams
Buffers
Cryptography
Processes
Child processes
DNS
Timers
Events
Workers
```

This is the major difference from browser JavaScript.

Browser:

```text
DOM
Fetch
WebSocket
localStorage
```

Node:

```text
fs
http
net
process
Buffer
stream
child_process
worker_threads
```

---

# 19.5 Event-Driven Architecture

Node applications are heavily based around events.

For example:

```js
server.on("request", handler);
```

or:

```js
socket.on("data", handler);
```

or:

```js
stream.on("end", handler);
```

The basic model:

```text
Event occurs
     ↓
Event emitted
     ↓
Listener/callback invoked
```

Node's `EventEmitter` is central to this model.

Example:

```js
const EventEmitter = require("events");

const emitter = new EventEmitter();

emitter.on("userCreated", (user) => {
    console.log(user);
});

emitter.emit("userCreated", {
    id: 1
});
```

This is synchronous from the perspective of the `emit()` call: listeners are invoked as part of the emission unless the listener itself schedules asynchronous work.

That detail can matter in interviews.

---

# 19.6 EventEmitter vs Event Loop

Don't confuse these.

### EventEmitter

An application-level mechanism for:

```text
publish event → invoke listeners
```

### Event Loop

A runtime mechanism for:

```text
schedule and execute asynchronous callbacks/tasks
```

They can work together, but they are not the same thing.

---

# 19.7 Node.js Event Loop

This connects directly to Point 16.

Suppose:

```js
console.log("A");

setTimeout(() => {
    console.log("B");
}, 0);

console.log("C");
```

You get:

```text
A
C
B
```

because the timer callback doesn't interrupt currently executing synchronous JavaScript.

Conceptually:

```text
Call Stack
    │
    ▼
Execute synchronous JS
    │
    ▼
Event Loop
    │
    ▼
Execute eligible callbacks
```

But Node's event loop has more structure than the simplified browser diagram.

---

# 19.8 Node Event Loop Phases

At SDE-II, know the major phases conceptually:

```text
┌───────────────────────────┐
│ timers                    │
├───────────────────────────┤
│ pending callbacks         │
├───────────────────────────┤
│ idle / prepare            │
├───────────────────────────┤
│ poll                      │
├───────────────────────────┤
│ check                     │
├───────────────────────────┤
│ close callbacks           │
└───────────────────────────┘
```

The important ones for interviews are:

### timers

Callbacks for timers such as:

```js
setTimeout()
setInterval()
```

### poll

Handles many I/O-related callbacks.

### check

Where:

```js
setImmediate()
```

callbacks execute.

### close callbacks

For certain close events:

```js
socket.on("close", ...)
```

You don't need to memorize every internal detail.

You need to understand **why different asynchronous APIs may execute at different points**.

---

# 19.9 `process.nextTick()`

Node has a special mechanism:

```js
process.nextTick(() => {
    console.log("next tick");
});
```

This is particularly important.

`process.nextTick()` callbacks are processed **before the event loop continues to its next phase**.

Therefore abusing it can cause starvation.

Example:

```js
function loop() {
    process.nextTick(loop);
}

loop();
```

You can effectively prevent the event loop from making progress.

That's a production problem.

---

# 19.10 `setImmediate()` vs `setTimeout()`

Another classic Node interview question.

```js
setTimeout(() => {
    console.log("timeout");
}, 0);

setImmediate(() => {
    console.log("immediate");
});
```

From the main module, the ordering can depend on timing/runtime conditions.

But inside an I/O callback, `setImmediate()` is typically executed before a zero-delay timer scheduled there.

Example:

```js
fs.readFile("file.txt", () => {
    setTimeout(() => {
        console.log("timeout");
    }, 0);

    setImmediate(() => {
        console.log("immediate");
    });
});
```

The important SDE-II lesson is:

> **Don't assume `setTimeout(fn, 0)` means "run immediately" or that it universally beats/loses to `setImmediate()`. Their scheduling points differ.**

---

# 19.11 Non-Blocking I/O

This is the heart of Node.

Suppose:

```js
const data = await fs.promises.readFile("large.txt");
```

It would be disastrous if the main JavaScript thread had to sit there doing nothing while the OS reads the file.

Instead, Node can initiate the operation and allow the JavaScript thread to continue handling other work.

Conceptually:

```text
JavaScript
    │
    │ start file read
    ▼
Node / libuv
    │
    ▼
OS / thread pool
    │
    │ file operation
    ▼
completion
    │
    ▼
Event loop
    │
    ▼
JavaScript callback/continuation
```

Therefore:

> **Non-blocking I/O means JavaScript doesn't synchronously wait for the I/O operation to finish before continuing execution.**

---

# 19.12 Non-Blocking Doesn't Mean "No Waiting"

This distinction matters.

Consider:

```js
const data = await fs.promises.readFile("file.txt");
```

The `await` pauses the **async function's continuation**, not the entire Node.js process.

Other requests can continue.

For example:

```text
Request A
   ↓
await DB query
   │
   │
   ├──────────────┐
   │              │
Request B       Request C
   ↓              ↓
execute         execute
```

That's why Node can handle many concurrent I/O-bound operations efficiently.

---

# 19.13 But CPU Work Is Different

Suppose:

```js
app.get("/heavy", (req, res) => {
    for (let i = 0; i < 10_000_000_000; i++) {
        // CPU-heavy work
    }

    res.send("done");
});
```

This runs synchronously on the main JavaScript thread.

While it runs:

```text
Request A
   │
   ▼
CPU-heavy loop
   │
   ├── Request B waits
   ├── Request C waits
   ├── Request D waits
   └── Event loop blocked
```

This is one of the most important Node production concepts:

> **Node is excellent for I/O-bound concurrency but CPU-heavy JavaScript can block the event loop.**

---

# 19.14 File System APIs

Node provides:

```js
import fs from "node:fs";
```

There are synchronous APIs:

```js
fs.readFileSync(...)
```

and asynchronous APIs:

```js
fs.readFile(...)
```

or:

```js
await fs.promises.readFile(...)
```

For server request handling, generally prefer asynchronous APIs.

Bad:

```js
app.get("/file", (req, res) => {
    const data = fs.readFileSync("large.txt");

    res.send(data);
});
```

because the synchronous operation can block the event loop.

Better:

```js
app.get("/file", async (req, res) => {
    const data = await fs.promises.readFile("large.txt");

    res.send(data);
});
```

Though for large files, we'll later see why **streams** are often even better.

---

# 19.15 Networking

Node has low-level networking APIs:

```text
net
dns
tls
http
https
```

For example:

```js
import net from "node:net";
```

You can create TCP servers.

At a higher level:

```js
import http from "node:http";
```

and:

```js
const server = http.createServer((req, res) => {
    res.end("Hello");
});
```

Frameworks such as Express are built on top of Node's HTTP capabilities.

Conceptually:

```text
Express
   ↓
Node HTTP
   ↓
TCP
   ↓
Operating system networking
```

This is an important SDE-II architectural boundary.

---

# 19.16 HTTP

Node can directly create HTTP servers.

```js
import http from "node:http";

const server = http.createServer((req, res) => {
    res.writeHead(200, {
        "Content-Type": "application/json"
    });

    res.end(JSON.stringify({
        message: "Hello"
    }));
});

server.listen(3000);
```

Express essentially provides a higher-level abstraction around this.

Understanding this helps when debugging things like:

* request lifecycle
* headers
* streaming
* connection handling
* keep-alive
* middleware
* HTTP errors

---

# 19.17 Streams

Streams are **very important for backend SDE-II**.

Suppose you need to send a 2 GB file.

Naive approach:

```js
const data = await fs.promises.readFile("2GB-file");
res.end(data);
```

You potentially load a huge amount of data into memory.

A stream processes data incrementally.

```text
Disk
 │
 ▼
Chunk 1 ──►
Chunk 2 ──►
Chunk 3 ──►
Chunk 4 ──►
...
 │
 ▼
HTTP Response
```

This is:

> **Streaming instead of buffering the entire payload.**

---

# 19.18 Types of Streams

Node has four major categories:

```text
Readable
Writable
Duplex
Transform
```

### Readable

You consume data.

```text
File → application
```

### Writable

You write data.

```text
Application → file
```

### Duplex

Both readable and writable.

### Transform

Reads data, transforms it, and produces output.

For example:

```text
compressed input
      ↓
gzip transform
      ↓
compressed output
```

---

# 19.19 Stream Backpressure

This is an **excellent SDE-II topic**.

Suppose:

```text
Producer
   │
   │ produces very fast
   ▼
Consumer
   │
   │ consumes slowly
```

Without flow control:

```text
Producer
   ↓
memory
   ↓
memory
   ↓
memory
   ↓
OOM
```

Backpressure allows the consumer to tell the producer:

> "Slow down; I can't consume data this quickly."

Conceptually:

```text
Fast Producer
      ↓
   Stream
      ↓
Slow Consumer

      ↑
 BACKPRESSURE
```

This is one reason streams are useful for production systems.

---

# 19.20 Buffers

Node's `Buffer` represents raw binary data.

Example:

```js
const buffer = Buffer.from("Hello");

console.log(buffer);
```

You may see bytes like:

```text
<Buffer 48 65 6c 6c 6f>
```

This matters because backend systems constantly deal with binary data:

```text
Files
TCP packets
Images
Videos
Encryption
Compression
Network protocols
```

---

# 19.21 Buffer vs String

A string:

```js
"Hello"
```

represents text.

A Buffer:

```js
Buffer.from("Hello")
```

represents bytes.

You can convert:

```js
const buffer = Buffer.from("Hello");

const text = buffer.toString();
```

Understanding this becomes important when dealing with:

* streams
* file uploads
* cryptography
* network protocols
* multipart data

---

# 19.22 `process`

Node exposes the current process through:

```js
process
```

For example:

```js
console.log(process.pid);
console.log(process.version);
console.log(process.platform);
```

It provides access to:

```text
Environment variables
Arguments
Process ID
Signals
Exit
stdin
stdout
stderr
```

---

# 19.23 Environment Variables

Very common backend pattern:

```js
const port = process.env.PORT;
```

For example:

```text
PORT=3000
DATABASE_URL=...
REDIS_URL=...
```

You generally don't hardcode environment-specific configuration:

```js
const dbPassword = "my-secret";
```

Instead:

```js
const dbPassword = process.env.DB_PASSWORD;
```

Production systems commonly separate:

```text
Code
  +
Configuration
```

This connects to your **Production Engineering** chapter later.

---

# 19.24 Process Signals

Node processes can receive operating-system signals.

For example:

```js
process.on("SIGTERM", () => {
    console.log("Shutdown requested");
});
```

This becomes extremely important in production.

Suppose Kubernetes or another process manager wants to terminate your application.

It may send:

```text
SIGTERM
```

Your application should perform graceful shutdown:

```text
Receive SIGTERM
      ↓
Stop accepting new requests
      ↓
Finish existing requests
      ↓
Close DB connections
      ↓
Close Redis connections
      ↓
Close server
      ↓
Exit
```

This is an SDE-II production concept worth knowing.

---

# 19.25 Worker Threads

Now we solve CPU-bound work.

Node provides:

```js
worker_threads
```

A worker runs JavaScript in a separate thread.

Conceptually:

```text
Main Thread
     │
     │ worker.postMessage()
     ▼
Worker Thread
     │
     │ expensive computation
     ▼
     │
     │ postMessage()
     ▼
Main Thread
```

Example:

```js
const worker = new Worker("./worker.js");

worker.postMessage(data);

worker.on("message", (result) => {
    console.log(result);
});
```

This prevents CPU-heavy JavaScript from blocking the main event loop.

---

# 19.26 Worker Threads vs Event Loop

Suppose you need to calculate:

```text
large mathematical computation
image processing
CPU-heavy parsing
compression
```

Doing it directly:

```text
Main Thread
     ↓
CPU work
     ↓
Event loop blocked
```

Using worker threads:

```text
Main Thread
     │
     ├── handles requests
     │
     └── Worker
           ↓
        CPU work
```

This is the correct mental model.

---

# 19.27 Worker Threads vs Child Processes

These are not the same.

### Worker Thread

```text
Same process
Different thread
```

### Child Process

```text
Separate OS process
```

Conceptually:

```text
Node Process
│
├── Main Thread
│
└── Worker Thread
```

versus:

```text
Process A
   │
   └── Process B
```

---

# 19.28 Child Processes

Node provides:

```js
child_process
```

with APIs such as:

```js
spawn()
exec()
execFile()
fork()
```

A child process is useful when you need:

* process isolation
* execute external programs
* separate failure boundaries
* independent memory space
* system commands

For example:

```js
spawn("python", ["script.py"]);
```

Your Node application can communicate with another process.

---

# 19.29 Worker vs Child Process — Interview Comparison

|                         | Worker Thread     | Child Process         |
| ----------------------- | ----------------- | --------------------- |
| Isolation               | Lower             | Higher                |
| Memory                  | Same process      | Separate process      |
| Communication           | Message passing   | IPC                   |
| Startup                 | Generally lighter | Generally heavier     |
| Shared memory           | Possible          | No normal shared heap |
| CPU workloads           | Excellent         | Excellent             |
| Process crash isolation | No                | Yes                   |

A good rule:

> Use workers when you need parallel JavaScript computation inside the application process; use child processes when you need stronger process isolation or need to execute another program/process.

---

# 19.30 Node Module System

Node historically used:

```js
CommonJS
```

Example:

```js
const express = require("express");
```

and:

```js
module.exports = something;
```

Modern Node also supports:

```js
ES Modules
```

Example:

```js
import express from "express";
```

and:

```js
export default something;
```

---

# 19.31 CommonJS

Example:

```js
const math = require("./math");

console.log(math.add(2, 3));
```

Export:

```js
module.exports = {
    add,
    subtract
};
```

CommonJS uses:

```text
require()
module.exports
exports
```

---

# 19.32 ESM

Example:

```js
import { add } from "./math.js";

console.log(add(2, 3));
```

Export:

```js
export function add(a, b) {
    return a + b;
}
```

Or:

```js
export default function add(a, b) {
    return a + b;
}
```

---

# 19.33 CommonJS vs ESM

At SDE-II, understand:

| CommonJS                                  | ESM                       |
| ----------------------------------------- | ------------------------- |
| `require()`                               | `import`                  |
| `module.exports`                          | `export`                  |
| Historically Node's default               | Modern JS module standard |
| Dynamic loading naturally via `require()` | Static module structure   |
| Common in older Node projects             | Common in modern projects |

One important point:

> **ES Modules are part of the ECMAScript standard; CommonJS is a Node.js module system.**

---

# 19.34 Node.js Architecture — Complete Picture

Now put everything together.

```text
                         NODE.JS PROCESS
                              │
               ┌──────────────┴──────────────┐
               │                             │
        JavaScript Thread              Native/Runtime
               │                             │
               ▼                             ▼
              V8                           libuv
               │                             │
               │                    ┌────────┼─────────┐
               │                    │        │         │
               │                 Event    Thread     OS I/O
               │                  Loop     Pool
               │                    │
               └────────────┬───────┘
                            │
                            ▼
                    Node.js APIs
                            │
       ┌────────────┬───────┼───────────┬───────────┐
       ▼            ▼       ▼           ▼           ▼
      HTTP          FS    Streams     Buffer     Process
       │
       ▼
    Express
       │
       ▼
  Your Application
```

And if CPU work appears:

```text
                         Node Process
                              │
               ┌──────────────┴─────────────┐
               │                            │
          Main Thread                  Worker Thread
               │                            │
          Event Loop                    CPU work
               │
          HTTP requests
```

Or stronger isolation:

```text
             Node Process
                  │
                  │ IPC
                  ▼
            Child Process
                  │
            External program
```

---

# 19.35 The Most Important Node.js Production Question

Imagine your API suddenly becomes slow.

You need to determine:

```text
Is it...
│
├── CPU-bound?
│
├── Database latency?
│
├── Network latency?
│
├── Event-loop blocked?
│
├── Memory pressure?
│
├── GC pressure?
│
├── Too many concurrent requests?
│
├── Slow downstream service?
│
└── Resource exhaustion?
```

That's how an SDE-II should think.

Not:

> "Node is slow."

But:

> **"Which part of the execution model is the bottleneck?"**

---

# 19.36 Scenario: API Becomes Slow

Suppose:

```text
100 concurrent requests
```

and each request does:

```js
await db.query(...);
```

This can be fine because DB I/O is asynchronous.

But if each request does:

```js
for (let i = 0; i < 1e10; i++) {
    // CPU
}
```

then:

```text
Request 1
   ↓
CPU-heavy JS
   ↓
EVENT LOOP BLOCKED
   ↓
Request 2 waits
Request 3 waits
Request 4 waits
...
```

Therefore:

> **Concurrency doesn't automatically mean parallel CPU execution.**

Node gives you excellent I/O concurrency, but CPU parallelism requires mechanisms such as worker threads or multiple processes.

---

# 19.37 What Does "Node Can Handle 10,000 Connections" Actually Mean?

It does **not** mean:

```text
10,000 JavaScript threads
```

Instead, many connections can be maintained while most are waiting on I/O.

Conceptually:

```text
Connection 1 ── waiting DB
Connection 2 ── waiting network
Connection 3 ── waiting file
Connection 4 ── waiting network
...
Connection 10000 ── waiting
```

The event-driven architecture allows the main thread to handle callbacks as operations complete.

That's why Node is particularly strong for:

```text
APIs
Real-time applications
WebSockets
I/O-heavy services
Proxy services
Gateway services
```

---

# 19.38 Node.js Memory

This connects directly to Point 17.

A Node process has:

```text
V8 heap
Native memory
Buffers
libuv resources
OS resources
```

Therefore memory usage isn't simply:

> "JavaScript objects."

For example, a large Buffer may involve memory outside the normal V8 heap.

This matters when diagnosing:

```text
RSS
heap usage
external memory
buffer growth
```

At SDE-II, you should understand that **process memory ≠ just JavaScript heap**.

---

# 19.39 Graceful Shutdown

A production Node service shouldn't simply die.

A better lifecycle:

```text
START
  ↓
Listen for requests
  ↓
SIGTERM
  ↓
Stop accepting new work
  ↓
Allow active requests to finish
  ↓
Close connections
  ↓
Flush important work
  ↓
Exit
```

Example:

```js
process.on("SIGTERM", async () => {
    server.close(async () => {
        await db.close();
        process.exit(0);
    });
});
```

Real production implementations need to account for timeouts and failure cases, but the concept is what matters.

---

# 19.40 What I Want You to Know for SDE-II

We can divide Point 19 into three levels.

## 🔴 Must Know Deeply

```text
Node.js vs JavaScript
V8
Event-driven architecture
Event loop
Non-blocking I/O
Blocking vs non-blocking
CPU-bound vs I/O-bound
HTTP server
Streams
Backpressure
Buffers
process
Environment variables
Worker threads
Child processes
CommonJS vs ESM
Graceful shutdown
```

## 🟡 Should Know

```text
libuv
Thread pool
EventEmitter
setImmediate
process.nextTick
Node event-loop phases
TCP basics
Signals
IPC
```

## 🟢 Conceptual

```text
V8 internals
JIT internals
GC implementation details
libuv source internals
OS kernel implementation
```

Those belong closer to **Staff-level/runtime-specialist knowledge**.

---

# 19.41 SDE-II Interview Questions

This chapter should eventually allow you to answer these without memorized definitions:

### Runtime

1. What exactly is Node.js?
2. Is Node.js single-threaded?
3. What is V8?
4. What is libuv?
5. What happens when Node executes JavaScript?

### Concurrency

6. Why is Node good for I/O-heavy applications?
7. Why does CPU-heavy code hurt Node performance?
8. What happens when the event loop is blocked?
9. How does Node handle thousands of connections?
10. Does `async/await` create a new thread?

### Event loop

11. Explain the Node.js event loop.
12. `setTimeout(0)` vs `setImmediate()`.
13. `process.nextTick()` vs Promise microtasks.
14. What is event-loop starvation?
15. How would you detect event-loop blocking?

### Streams

16. What is a stream?
17. Why use streams instead of `readFile()`?
18. What is backpressure?
19. Readable vs Writable vs Duplex vs Transform.
20. How would you stream a 5 GB file through an API?

### Processes

21. Worker thread vs child process?
22. When would you use worker threads?
23. When would you use child processes?
24. How does IPC work conceptually?

### Production

25. How do you gracefully shut down a Node service?
26. How do environment variables work?
27. Why should synchronous APIs generally be avoided in request handlers?
28. How would you debug a Node service whose latency suddenly increased?
29. How would you diagnose high memory usage?
30. How would you scale a Node.js application across CPU cores?

---

# 19.42 The One Mental Model I Want You to Retain

Everything we've covered can be reduced to this:

```text
                    NODE.JS
                       │
                       ▼
                      V8
                       │
              Executes JavaScript
                       │
                       ▼
                 Main Thread
                       │
                 Event Loop
                       │
          ┌────────────┼────────────┐
          │            │            │
        HTTP          I/O         Timers
          │            │            │
          └────────────┼────────────┘
                       │
                    Callbacks
                       │
                       ▼
                 Your application
```

When work is I/O-bound:

```text
JS
 ↓
start I/O
 ↓
event loop continues
 ↓
other requests execute
 ↓
I/O completes
 ↓
callback/Promise continuation
```

When work is CPU-bound:

```text
JS
 ↓
CPU computation
 ↓
MAIN THREAD BLOCKED
 ↓
other requests suffer
```

Solution:

```text
CPU work
   ↓
Worker Thread / Child Process
```

And for huge data:

```text
Huge data
   ↓
Stream
   ↓
Chunks
   ↓
Backpressure
   ↓
Consumer
```

That is the **Node.js SDE-II mental model**.

The next useful step for this chapter is **not another definition dump**. We should drill into the hardest Node runtime behavior with code: **event-loop ordering → `process.nextTick` → Promise microtasks → timers → `setImmediate` → I/O callbacks**, followed by **a real Node API scenario where we identify exactly what blocks the server and why**.
