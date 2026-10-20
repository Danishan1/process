
# Summary: Node.js Big Picture

```text
                         NODE.JS
                            │
        ┌───────────────────┼────────────────────┐
        ↓                   ↓                    ↓
       V8                  libuv              Node APIs
        │                   │                    │
        │                   ↓                    ├── fs
        │               Event Loop              ├── http
        │                   │                    ├── path
        │                   │                    ├── crypto
        ↓                   ↓                    ├── streams
 JavaScript            Async I/O                 ├── events
 execution                  │                    └── process
                            ↓
                     Callbacks / Promises
                            ↓
                     Microtasks / Tasks
```

## 🔥 The Node.js Concepts You MUST Know

If you're revising for interviews or backend development, prioritize these:

```text
1. Node.js vs V8
2. Event Loop
3. Non-blocking I/O
4. libuv
5. Callbacks
6. Promises / async-await
7. process.nextTick()
8. EventEmitter
9. Streams
10. Backpressure
11. Buffer
12. fs
13. HTTP
14. Modules: CommonJS vs ESM
15. package.json / npm
16. Environment variables
17. Error handling
18. CPU-bound vs I/O-bound
19. Worker Threads
20. Child Processes
21. Cluster
22. Express / middleware
```

And the **deepest connection** to remember is:

```text
Node.js
   ↓
V8 executes JS
   ↓
JS runs mainly on one main thread
   ↓
Node/libuv handles asynchronous operations
   ↓
Event Loop coordinates callbacks
   ↓
Promises / async-await schedule continuations
   ↓
Streams handle large data efficiently
   ↓
Worker threads/processes handle work
   that shouldn't block the main thread
```

That is the mental model from which most Node.js concepts become much easier.


# 🚀 Node.js — Complete Revision Map

The most important mindset:

> **Node.js is a JavaScript runtime built around Chrome's V8 engine, with APIs and an event-driven architecture that make JavaScript useful outside the browser.**

### 1. What exactly is Node.js?

```text
JavaScript
    ↓
V8 Engine
    ↓
Node.js Runtime
    ↓
OS / Network / Filesystem / Processes
```

Node.js is **not a programming language**.

It lets you execute JavaScript outside the browser.

#### Browser vs Node.js

| Browser          | Node.js                             |
| ---------------- | ----------------------------------- |
| `window`         | `global` / `globalThis`             |
| DOM              | No built-in DOM                     |
| `document`       | No `document`                       |
| `fetch`          | Available in modern Node            |
| `localStorage`   | Not a native Node equivalent        |
| Browser Web APIs | Node APIs                           |
| UI/events        | Server/network/filesystem/processes |

## 2. V8 Engine

V8 is the JavaScript engine used by Node.js.

Its job is roughly:

```text
JavaScript
    ↓
Parsing
    ↓
Bytecode / machine-code generation
    ↓
Execution + optimization
```

But:

> **V8 is not Node.js.**

Node.js adds things around the engine:

```text
              Node.js
        ┌────────┴────────┐
        ↓                 ↓
       V8             Node APIs
        ↓                 ↓
 JavaScript          fs, http,
 execution           streams, etc.
                          ↓
                     OS / libuv
```

## 3. Why Node.js Is Fast

A common misconception:

> "Node.js is fast because JavaScript runs on multiple threads."

That's not the right mental model.

Node.js uses:

```text
JavaScript execution
        ↓
single main JS thread
        ↓
event-driven architecture
        ↓
non-blocking I/O
```

For many I/O-heavy applications, this works extremely well because Node doesn't need to create a JavaScript thread waiting for every network request.

## 4. Event-Driven Architecture

Node.js heavily uses events.

For example:

```ts id="node01"
server.on("request", () => {
  console.log("Request received");
});
```

Something happens:

```text
Request
   ↓
Event
   ↓
Handler
```

This is the **event-driven model**.

## 5. Non-Blocking I/O

Consider:

```ts id="node02"
const data = await fs.promises.readFile("file.txt");
```

While the filesystem operation is happening, Node doesn't need to block the JavaScript thread waiting for the disk.

Conceptually:

```text
JS thread
   ↓
start I/O
   ↓
continue doing other work
   ↓
I/O completes
   ↓
callback / Promise continuation
   ↓
JS thread handles result
```

This is the heart of Node.js.

## 6. libuv ⭐⭐⭐⭐⭐

You will hear this name constantly.

**libuv** is a library Node.js uses for its event-driven asynchronous I/O infrastructure.

Simplified:

```text
JavaScript
    ↓
Node.js APIs
    ↓
libuv
    ↓
OS / networking / filesystem
```

libuv also provides a **thread pool** used for certain operations.

## 7. Important: Node.js Isn't "Single Threaded" in Every Sense

Better statement:

> **JavaScript execution is primarily single-threaded, but Node.js can use additional threads internally.**

For example, libuv has a thread pool for certain operations.

Also Node provides:

```text
worker_threads
cluster
child_process
```

for different forms of parallelism/process isolation.

## 8. Event Loop

The Node.js event loop coordinates asynchronous work.

Simplified:

```text
       JavaScript
           ↓
       Call Stack
           ↓
       Event Loop
           ↓
 ┌─────────┴─────────┐
 ↓                   ↓
Timers            I/O callbacks
 ↓                   ↓
Poll              Check
 ↓                   ↓
Close callbacks ← setImmediate
```

You don't need to memorize every phase immediately, but you should understand that Node's event loop has **multiple phases**.

## 9. Node Event Loop Phases

A simplified ordering is:

```text
timers
   ↓
pending callbacks
   ↓
idle / prepare
   ↓
poll
   ↓
check
   ↓
close callbacks
```

Important APIs:

| API              | Associated concept    |
| ---------------- | --------------------- |
| `setTimeout()`   | timers                |
| `setImmediate()` | check                 |
| I/O callbacks    | poll / related phases |
| close handlers   | close callbacks       |

## 10. `process.nextTick()` ⭐⭐⭐⭐⭐

Node has a special mechanism:

```ts id="node03"
process.nextTick(() => {
  console.log("next");
});
```

`process.nextTick()` is **not simply another event-loop phase**.

Its callbacks are processed after the current operation completes, before the event loop continues normally.

This makes it very high priority.

Compare conceptually:

```text
Current JS
   ↓
process.nextTick queue
   ↓
microtasks
   ↓
event-loop phases
```

The exact interaction has nuances, but the important thing is:

> **Don't treat `process.nextTick()` as equivalent to `setTimeout(..., 0)`.**

## 11. Node Microtasks

Node also uses Promise microtasks:

```ts id="node04"
Promise.resolve().then(() => {
  console.log("promise");
});
```

And:

```ts id="node05"
queueMicrotask(() => {
  console.log("microtask");
});
```

These are part of the broader microtask model.

So Node async behavior involves:

```text
process.nextTick()
        ↓
microtasks
        ↓
event-loop phases
```

## 12. CommonJS Modules

Historically, Node's module system is CommonJS.

```ts id="node06"
const fs = require("node:fs");
```

Export:

```ts id="node07"
module.exports = {
  add,
  subtract
};
```

Import:

```ts id="node08"
const math = require("./math");
```

## 13. ES Modules

Modern Node also supports ES modules.

```ts id="node09"
import fs from "node:fs";
```

Export:

```ts id="node10"
export function add(a: number, b: number) {
  return a + b;
}
```

Import:

```ts id="node11"
import { add } from "./math.js";
```

So:

```text
CommonJS
→ require()
→ module.exports

ESM
→ import
→ export
```

## 14. `package.json`

One of the most important Node files.

Example:

```json id="node12"
{
  "name": "my-app",
  "version": "1.0.0",
  "type": "module",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {},
  "devDependencies": {}
}
```

It describes project metadata, scripts, dependencies, and module configuration.

## 15. Dependencies

Two major categories:

```text
dependencies
    ↓
required by application at runtime

devDependencies
    ↓
mainly required during development/build/testing
```

Example:

```bash
npm install express
```

typically adds to:

```json
"dependencies"
```

Whereas:

```bash
npm install -D typescript
```

adds to:

```json
"devDependencies"
```

## 16. npm

`npm` is the Node Package Manager.

Common commands:

```bash
npm init
npm install
npm install express
npm install -D typescript
npm uninstall express
npm run dev
```

Important distinction:

```text
Node.js
→ runtime

npm
→ package manager / registry tooling
```

## 17. `node_modules`

When you install dependencies:

```text
project
 ├── package.json
 ├── package-lock.json
 └── node_modules/
```

`node_modules` contains installed packages.

Usually:

```text
node_modules/
```

should **not** be committed to Git.

Instead commit:

```text
package.json
package-lock.json
```

and recreate dependencies with:

```bash
npm install
```

## 18. `package-lock.json`

It locks the dependency tree to reproducible versions.

Think:

```text
package.json
→ what dependencies I want

package-lock.json
→ exact dependency resolution
```

This helps ensure different environments install compatible dependency trees.

## 19. Built-in Node Modules

Node gives you many modules without installing packages.

Examples:

```ts id="node13"
import fs from "node:fs";
import path from "node:path";
import http from "node:http";
import os from "node:os";
import crypto from "node:crypto";
import events from "node:events";
import stream from "node:stream";
```

The `node:` prefix is a useful modern convention:

```ts
import fs from "node:fs";
```

## 20. `fs` — File System

Read a file asynchronously:

```ts id="node14"
import { readFile } from "node:fs/promises";

const data = await readFile("file.txt", "utf8");
```

Write:

```ts id="node15"
import { writeFile } from "node:fs/promises";

await writeFile("file.txt", "Hello");
```

Node supports both:

```text
synchronous APIs
asynchronous APIs
```

For server code, asynchronous APIs are generally preferred when blocking would hurt concurrency.

## 21. Sync vs Async

```ts id="node16"
readFileSync(...)
```

blocks the current JavaScript execution.

Whereas:

```ts id="node17"
await readFile(...)
```

uses asynchronous I/O.

Think:

```text
Sync
→ wait here

Async
→ start operation
→ continue
→ resume later
```

## 22. `path`

Never manually construct filesystem paths like:

```ts
"folder/" + file
```

Use `path`:

```ts id="node18"
import path from "node:path";

const filePath = path.join("folder", "file.txt");
```

Useful:

```ts
path.join()
path.resolve()
path.basename()
path.dirname()
path.extname()
```

## 23. `process`

Node exposes information/control through:

```ts id="node19"
process
```

Examples:

```ts
process.env
process.argv
process.cwd()
process.exit()
process.pid
```

#### Environment variables

```ts id="node20"
const port = process.env.PORT;
```

Very common in backend applications.

## 24. `process.argv`

Command-line arguments:

```bash
node app.js hello
```

Then:

```ts id="node21"
console.log(process.argv);
```

contains command-line arguments.

Conceptually:

```text
process.argv
→ arguments supplied to the Node process
```

## 25. Environment Variables

Instead of hardcoding:

```ts id="node22"
const dbPassword = "secret123";
```

you typically use:

```ts id="node23"
const dbPassword = process.env.DB_PASSWORD;
```

In production, secrets should be supplied through appropriate secret/environment configuration rather than committed to source control.

## 26. HTTP Server

Node can create an HTTP server without Express:

```ts id="node24"
import http from "node:http";

const server = http.createServer((req, res) => {
  res.end("Hello");
});

server.listen(3000);
```

Conceptually:

```text
Client
  ↓
HTTP request
  ↓
Node HTTP server
  ↓
request handler
  ↓
response
```

## 27. Request vs Response

In:

```ts id="node25"
http.createServer((req, res) => {
});
```

`req` contains information about the incoming request.

Examples:

```ts
req.method
req.url
req.headers
```

`res` controls the outgoing response.

Examples:

```ts
res.statusCode = 200;
res.setHeader(...)
res.end(...)
```

## 28. Streams ⭐⭐⭐⭐⭐

Streams are one of the most important Node concepts.

Instead of loading an entire file:

```text
10 GB file
   ↓
RAM
```

you can process it in chunks:

```text
10 GB file
 ↓
chunk
 ↓
chunk
 ↓
chunk
 ↓
...
```

Node streams are useful for:

```text
files
HTTP
compression
encryption
data transformation
```

## 29. Types of Streams

Four main types:

| Stream    | Purpose                  |
| --------- | ------------------------ |
| Readable  | Read data                |
| Writable  | Write data               |
| Duplex    | Read + write             |
| Transform | Read + transform + write |

Example:

```ts id="node26"
import { createReadStream } from "node:fs";

const stream = createReadStream("large.txt");
```

## 30. Pipes

A very important pattern:

```ts id="node27"
readStream.pipe(writeStream);
```

Conceptually:

```text
Readable
   ↓
 pipe()
   ↓
Writable
```

This is especially useful for large data because it avoids loading everything into memory at once.

## 31. Backpressure ⭐⭐⭐⭐

Suppose:

```text
Producer
   ↓
VERY FAST
   ↓
Consumer
   ↓
SLOW
```

The producer can generate data faster than the consumer can process it.

That creates:

```text
buffer growth
memory pressure
```

**Backpressure** is the mechanism/pattern for controlling that flow.

Streams are designed to help manage this.

This is a major Node.js interview topic.

## 32. EventEmitter

Node has an `EventEmitter` abstraction.

```ts id="node28"
import { EventEmitter } from "node:events";

const emitter = new EventEmitter();

emitter.on("login", (user) => {
  console.log(user);
});

emitter.emit("login", { name: "John" });
```

Pattern:

```text
emit event
    ↓
listeners
    ↓
callbacks execute
```

## 33. `on()` vs `once()`

```ts id="node29"
emitter.on("event", handler);
```

The handler can run repeatedly.

```ts id="node30"
emitter.once("event", handler);
```

The listener is automatically removed after the first invocation.

Think:

```text
on
→ every occurrence

once
→ first occurrence only
```

## 34. Buffer

Node deals with raw binary data using `Buffer`.

```ts id="node31"
const buffer = Buffer.from("Hello");
```

Useful for:

```text
files
network packets
binary protocols
crypto
streams
```

Think:

```text
string
→ human-readable text

Buffer
→ raw bytes
```

## 35. Child Processes

Node can create other processes.

Examples:

```ts
spawn()
exec()
execFile()
fork()
```

Why?

```text
Run another program
Run CPU-heavy work externally
Execute system commands
```

A child process is a **separate OS process**, unlike a worker thread.

## 36. Worker Threads

For CPU-heavy JavaScript work, Node provides:

```ts
worker_threads
```

Conceptually:

```text
Main JS thread
      │
      ├──── Worker
      │
      └──── Worker
```

Useful for CPU-intensive tasks that would otherwise block the main JS thread.

## 37. Cluster

Node's `cluster` module can create multiple Node processes that can share server ports under supported setups.

Conceptually:

```text
             Server
                │
       ┌────────┼────────┐
       ↓        ↓        ↓
    Worker   Worker   Worker
```

This is different from worker threads:

```text
worker_threads
→ threads within a process

cluster
→ multiple processes
```

## 38. CPU-Bound vs I/O-Bound ⭐⭐⭐⭐⭐

This distinction is crucial.

#### I/O-bound

```text
Database
Network
Filesystem
```

Node is excellent at handling many concurrent I/O operations.

#### CPU-bound

```text
Huge calculations
Image processing
Video encoding
Complex computation
```

If you do heavy CPU work directly on the main JS thread:

```text
CPU-heavy function
       ↓
blocks event loop
       ↓
other requests wait
```

That's dangerous for a Node server.

## 39. Error Handling

Synchronous:

```ts id="node32"
try {
  riskyOperation();
} catch (error) {
  console.error(error);
}
```

Async Promise:

```ts id="node33"
try {
  await riskyOperation();
} catch (error) {
  console.error(error);
}
```

Callback-style APIs historically often use:

```ts id="node34"
callback(error, result);
```

This is called the **error-first callback convention**.

## 40. Error-First Callbacks

Classic Node:

```ts id="node35"
fs.readFile("file.txt", (err, data) => {
  if (err) {
    console.error(err);
    return;
  }

  console.log(data);
});
```

Convention:

```text
callback(error, result)
```

If successful:

```text
error = null/undefined
result = data
```

If failed:

```text
error = Error
result may be undefined
```

## 41. Express Fits on Top of Node

Node can handle HTTP directly.

But frameworks make development easier.

A common stack:

```text
Application
    ↓
Express
    ↓
Node.js HTTP
    ↓
Node runtime
    ↓
OS
```

Express is therefore **not Node.js itself**.

It's a web framework running on Node.

## 42. Middleware

In Express-style frameworks:

```ts id="node36"
app.use((req, res, next) => {
  console.log(req.method);
  next();
});
```

Middleware sits in the request pipeline:

```text
Request
   ↓
Middleware
   ↓
Middleware
   ↓
Route Handler
   ↓
Response
```

`next()` generally passes control to the next middleware/handler.

## 43. Node + TypeScript

TypeScript isn't executed directly by Node in the traditional sense.

Usually:

```text
TypeScript
    ↓
Type checking / transpilation
    ↓
JavaScript
    ↓
Node.js
    ↓
V8
```

Modern tooling can change the exact development workflow, but the core idea remains:

> Node ultimately executes JavaScript-compatible code.
