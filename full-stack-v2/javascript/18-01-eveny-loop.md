# 16. Event Loop & Concurrency Model — SDE-II Complete Notes

This is one of the highest-priority Node.js/JavaScript topics.

The previous topic, **Asynchronous JavaScript**, answers:

> **How do we write asynchronous code?**

This topic answers:

> **What actually happens after we write that code?**

The SDE-II goal is to understand:

```text
Call Stack
     ↓
Host APIs / libuv
     ↓
Queues
     ↓
Event Loop
     ↓
Microtasks / nextTick
     ↓
More JavaScript
```

and be able to answer:

* What executes first?
* Why?
* Is the event loop blocked?
* Is something starving another queue?
* Is the work concurrent or parallel?
* Is Node using the OS or the libuv thread pool?
* How would I detect this problem in production?
* How would I fix it?

---

# 1. First: The Complete Mental Model

A simplified Node.js architecture:

```text
                    Node.js Process
                          │
             ┌────────────┴────────────┐
             │                         │
             ▼                         ▼
            V8                    Node.js Runtime
             │                         │
       JavaScript                     │
             │                    ┌────┴─────┐
             ▼                    │          │
        Call Stack             libuv      Node APIs
             │                    │
             │              ┌─────┴─────┐
             │              │           │
             │           Event Loop   Worker Pool
             │              │
             │              │
             │        OS / Network I/O
             │
             ▼
       Microtask Queues
             │
             ▼
        More JS execution
```

A more accurate interview model is:

```text
                 JavaScript
                     │
                     ▼
                Call Stack
                     │
                     ▼
          Current JS execution
                     │
                     ▼
       ┌─────────────────────────┐
       │ Microtask processing    │
       │                         │
       │ Promise callbacks       │
       │ queueMicrotask()        │
       │                         │
       └─────────────────────────┘
                     │
                     ▼
             Node Event Loop
                     │
       ┌─────────────┼──────────────┐
       │             │              │
     timers         poll          check
       │             │              │
       │             │              │
 setTimeout       I/O         setImmediate
       │
       ▼
   More JS
```

But Node has one particularly important additional mechanism:

```text
process.nextTick()
       ↓
Next Tick Queue
```

And this queue has **special priority** in Node.

---

# 2. Call Stack

The call stack tracks the JavaScript currently being executed.

```js
function a() {
    b();
}

function b() {
    c();
}

function c() {
    console.log("Hello");
}

a();
```

Conceptually:

```text
c()
│
b()
│
a()
```

The stack is:

```text
┌─────────────┐
│     c()     │
├─────────────┤
│     b()     │
├─────────────┤
│     a()     │
└─────────────┘
```

Functions are pushed onto the stack when called and removed when they return.

Therefore:

> **Call Stack = currently executing JavaScript.**

It follows:

```text
LIFO
Last In → First Out
```

---

# 3. Heap

The heap is the runtime's dynamically allocated memory area.

For example:

```js
const user = {
    name: "Danishan",
    age: 25
};
```

Conceptually:

```text
JavaScript
    │
    │ reference
    ▼
   Heap

┌─────────────────┐
│ name: Danishan  │
│ age: 25         │
└─────────────────┘
```

Don't rely on the oversimplification:

> primitives = stack, objects = heap

That isn't a JavaScript specification guarantee.

For interviews, remember:

```text
Call Stack → execution
Heap       → dynamically allocated memory
```

---

# 4. Synchronous JavaScript Always Runs to Completion

This is fundamental.

Consider:

```js
console.log("A");

function heavyWork() {
    for (let i = 0; i < 1e9; i++) {}
}

heavyWork();

console.log("B");
```

JavaScript cannot suddenly stop:

```text
heavyWork()
```

halfway through and execute another callback.

The event loop does **not preempt synchronous JavaScript**.

So:

```text
A
↓
heavyWork()
↓
B
```

Only after the current synchronous execution finishes can other scheduled work run.

This is the foundation of event-loop blocking.

---

# 5. What Is the Event Loop?

The event loop is a **host/runtime mechanism**, not a feature defined completely by the ECMAScript language.

Its job is broadly to coordinate:

```text
JavaScript execution
        +
asynchronous operations
        +
queued callbacks
```

In Node.js, this involves Node's runtime and **libuv**.

In browsers, the browser provides the host environment.

---

# 6. The Simplest Example

```js
console.log("A");

setTimeout(() => {
    console.log("B");
}, 0);

console.log("C");
```

Output:

```text
A
C
B
```

Why?

Because:

```text
console.log("A")
        ↓
setTimeout registered
        ↓
console.log("C")
        ↓
current JS finishes
        ↓
timer becomes eligible
        ↓
callback eventually executes
        ↓
B
```

`0` does **not** mean:

> Execute immediately.

It means roughly:

> Don't execute before the specified delay; once eligible, execute according to the runtime's scheduling rules.

---

# 7. Tasks / Macrotasks

You'll often hear:

```text
Task
Macrotask
```

The terminology varies depending on the environment.

Examples include:

```text
timers
I/O callbacks
message events
some user interaction events
```

Don't obsess over the word "macrotask."

The important concept is:

```text
A task is a unit of work that the host schedules for JavaScript execution.
```

---

# 8. Microtasks

Microtasks are a special class of queued asynchronous work.

Common examples:

```js
Promise.resolve().then(...)
Promise.catch(...)
Promise.finally(...)
queueMicrotask(...)
```

Example:

```js
console.log("A");

Promise.resolve().then(() => {
    console.log("B");
});

console.log("C");
```

Output:

```text
A
C
B
```

Because:

```text
A
C
↓
microtask checkpoint
↓
B
```

---

# 9. Microtasks Are Drained

This is extremely important.

Consider:

```js
Promise.resolve().then(() => {
    console.log("A");

    Promise.resolve().then(() => {
        console.log("B");
    });
});

setTimeout(() => {
    console.log("C");
}, 0);
```

Output:

```text
A
B
C
```

Why?

```text
Microtask A
    ↓
A executes
    ↓
creates Microtask B
    ↓
B executes
    ↓
microtask queue empty
    ↓
timer C
```

The runtime doesn't normally say:

```text
execute one microtask
→ immediately execute a timer
→ return to microtasks
```

Instead, the microtask checkpoint drains queued microtasks.

This leads directly to starvation.

---

# 10. `queueMicrotask()`

You can explicitly schedule a microtask:

```js
queueMicrotask(() => {
    console.log("microtask");
});
```

Example:

```js
console.log("A");

queueMicrotask(() => {
    console.log("B");
});

console.log("C");
```

Output:

```text
A
C
B
```

`queueMicrotask()` is useful when you intentionally want to defer execution until the current synchronous work finishes, while remaining in the microtask mechanism.

---

# 11. `process.nextTick()` — The Important Node.js Concept

This is one of the concepts missing from your original notes.

Node.js has:

```js
process.nextTick()
```

Example:

```js
console.log("A");

process.nextTick(() => {
    console.log("B");
});

console.log("C");
```

Output:

```text
A
C
B
```

At a high level:

```text
Current synchronous JS
        ↓
nextTick queue
        ↓
microtasks
        ↓
event loop continues
```

But there's an important nuance.

Node maintains a **next tick queue**, and `process.nextTick()` callbacks are processed before the event loop continues. Node's documentation explicitly warns that recursively scheduling `process.nextTick()` can create an infinite loop. ([Node.js][1])

---

# 12. `process.nextTick()` vs `queueMicrotask()`

This is an excellent SDE-II interview question.

### `queueMicrotask()`

```js
queueMicrotask(fn);
```

Uses the standard microtask mechanism.

### `process.nextTick()`

```js
process.nextTick(fn);
```

Uses Node's special **next tick queue**.

Conceptually in CommonJS:

```text
Current synchronous execution
            ↓
     process.nextTick
            ↓
      Promise / microtask
            ↓
       Event Loop
```

For example:

```js
process.nextTick(() => {
    console.log("nextTick");
});

Promise.resolve().then(() => {
    console.log("promise");
});

queueMicrotask(() => {
    console.log("microtask");
});
```

In CommonJS, the typical result is:

```text
nextTick
promise
microtask
```

Node's documentation also notes an important ESM nuance: because ESM execution itself occurs through the microtask mechanism, ordering between `nextTick` and microtasks can differ in ESM. ([Node.js][1])

### Interview answer

Don't simply say:

> `nextTick` is always faster than Promise.

Instead say:

> "`process.nextTick()` uses Node's special next-tick queue and is processed before the event loop continues. In CommonJS, it runs before Promise microtasks. There are ESM-specific ordering nuances. For general portable deferral, `queueMicrotask()` is preferred."

Also worth knowing:

> Modern Node documentation marks `process.nextTick()` as Legacy and recommends `queueMicrotask()` for most userland use cases. ([Node.js][1])

---

# 13. Why `process.nextTick()` Can Be Dangerous

Consider:

```js
function loop() {
    process.nextTick(loop);
}

loop();
```

Conceptually:

```text
nextTick
   ↓
nextTick
   ↓
nextTick
   ↓
nextTick
   ↓
...
```

Because the next-tick queue keeps being replenished, Node may never get an opportunity to proceed normally through the event loop.

This is **starvation**.

---

# 14. Promise Microtask Starvation

The same problem can happen with microtasks:

```js
function loop() {
    queueMicrotask(loop);
}

loop();
```

or:

```js
function loop() {
    Promise.resolve().then(loop);
}

loop();
```

Conceptually:

```text
Microtask A
   ↓
creates B
   ↓
B
   ↓
creates C
   ↓
C
   ↓
creates D
   ↓
...
```

The microtask queue never becomes empty.

Therefore:

```text
Timers
I/O callbacks
setImmediate()
other tasks
```

may be delayed indefinitely.

---

# 15. What Exactly Is Starvation?

This distinction is important.

### Blocking

One piece of work takes too long.

```text
Task A
████████████████████████
                       ↓
                     Task B
```

Task A prevents Task B from getting execution time.

---

### Starvation

A particular class of work keeps getting denied execution because another source continuously consumes scheduling opportunities.

```text
Microtask
Microtask
Microtask
Microtask
Microtask
Microtask
...
```

while:

```text
Timer → waiting
I/O   → waiting
```

So:

> **Starvation = a piece/class of work remains waiting because other work continually prevents it from receiving execution time.**

This is different from simply saying:

> "The server is slow."

---

# 16. Blocking vs Starvation

| Problem                | Cause                             | Example                       |
| ---------------------- | --------------------------------- | ----------------------------- |
| Event-loop blocking    | One long synchronous operation    | Huge loop                     |
| Microtask starvation   | Microtasks continuously generated | `queueMicrotask(loop)`        |
| nextTick starvation    | nextTick continuously generated   | `process.nextTick(loop)`      |
| Worker-pool starvation | Worker tasks occupy all workers   | Many expensive `crypto` tasks |
| Resource starvation    | Resource unavailable              | DB connection pool exhausted  |

This distinction is very useful in production debugging.

---

# 17. How Do We Detect Event-Loop Blocking?

This is where your original notes were incomplete.

Knowing:

> "Long synchronous code blocks the event loop"

is theory.

An SDE-II should also know:

> **How do I know it is happening in production?**

There are several signals.

---

# 18. Signal #1 — Event Loop Lag

Imagine:

```js
setTimeout(() => {
    console.log("executed");
}, 100);
```

You expect approximately:

```text
100 ms
```

But if the event loop is blocked:

```text
scheduled
   ↓
100 ms passes
   ↓
event loop blocked
   ↓
500 ms passes
   ↓
callback finally executes
```

Actual delay:

```text
500 ms
```

instead of:

```text
100 ms
```

That difference is **event-loop lag/delay**.

---

# 19. Measuring Event-Loop Delay

Node provides:

```js
perf_hooks
```

including:

```js
monitorEventLoopDelay()
```

Conceptually:

```js
const {
    monitorEventLoopDelay
} = require("node:perf_hooks");

const histogram = monitorEventLoopDelay();

histogram.enable();

setInterval(() => {
    console.log({
        min: histogram.min,
        max: histogram.max,
        mean: histogram.mean,
        p99: histogram.percentile(99)
    });
}, 5000);
```

The important production idea isn't memorizing the API.

It's:

```text
Measure event-loop delay
        ↓
Track p95 / p99
        ↓
Alert when latency becomes abnormal
```

For example:

```text
Event-loop p99 latency
        │
        ├── 5 ms     → healthy
        ├── 20 ms    → investigate
        ├── 100 ms   → serious
        └── 1000 ms  → severe
```

Exact thresholds depend on the application.

---

# 20. Event Loop Utilization

Another important Node concept is:

```text
ELU
Event Loop Utilization
```

Node provides:

```js
performance.eventLoopUtilization()
```

It measures how much time the event loop has been active versus idle. ([Node.js][2])

Conceptually:

```text
ELU = active event-loop time
      ----------------------
      active + idle time
```

Example:

```js
const { performance } = require("node:perf_hooks");

console.log(
    performance.eventLoopUtilization()
);
```

If the event loop spends nearly all its time executing work:

```text
ELU → high
```

that can indicate a heavily loaded event loop.

But:

> **High ELU alone does not prove starvation.**

It tells you the event loop is busy.

You need additional evidence such as:

```text
event-loop delay
request latency
CPU profile
queue behavior
```

to determine the actual cause.

---

# 21. Event Loop Lag vs Event Loop Utilization

Very important distinction:

| Metric                 | Tells you                                           |
| ---------------------- | --------------------------------------------------- |
| Event-loop delay       | How late the event loop is servicing scheduled work |
| Event-loop utilization | How much time the loop is active vs idle            |
| CPU utilization        | How busy the CPU is                                 |
| Request latency        | What users actually experience                      |

For example:

```text
CPU = 90%
ELU = 95%
event-loop lag = high
```

Strong indication of CPU-heavy work.

But:

```text
CPU = 20%
ELU = high
```

could still happen depending on what the process is doing; ELU is not simply CPU utilization. Node specifically notes that ELU measures event-loop activity, not overall CPU usage. ([Node.js][2])

---

# 22. How Do We Detect Starvation Specifically?

This is harder.

There isn't a simple:

```js
isStarving()
```

API.

Instead, we infer starvation from symptoms.

Suppose:

```js
setInterval(() => {
    const start = Date.now();

    setImmediate(() => {
        const delay = Date.now() - start;

        console.log("delay:", delay);
    });
}, 1000);
```

If the callback normally runs quickly:

```text
delay: 1ms
delay: 2ms
delay: 3ms
```

but suddenly:

```text
delay: 100ms
delay: 500ms
delay: 2000ms
```

you know the event loop is not getting timely opportunities.

Then investigate:

```text
CPU-heavy synchronous code?
        ↓
Event-loop blocking

Huge microtask chain?
        ↓
Microtask starvation

Recursive nextTick?
        ↓
nextTick starvation

Huge synchronous JSON parsing?
        ↓
Event-loop blocking

Regex?
        ↓
Possible catastrophic backtracking
```

---

# 23. Production Detection Model

A good SDE-II answer:

> "I wouldn't try to detect starvation from a single metric. I'd monitor event-loop delay/latency, event-loop utilization, CPU, request latency and throughput. If event-loop delay increases while the process remains busy, I'd capture a CPU profile or inspect the code path to determine whether the cause is synchronous CPU work, a microtask/nextTick loop, or another scheduling problem."

That's a much stronger answer than:

> "I would check CPU."

---

# 24. How to Detect a Blocked Event Loop in Production

A practical monitoring stack:

```text
                 Node Service
                      │
          ┌───────────┼────────────┐
          │           │            │
          ▼           ▼            ▼
    Event-loop     CPU          Request
       lag        usage         latency
          │           │            │
          └───────────┼────────────┘
                      ▼
                 Alerting
                      │
                      ▼
                Investigation
                      │
              ┌───────┴────────┐
              ▼                ▼
          CPU profile       Code trace
```

Look for:

```text
High p99 latency
+
High event-loop delay
+
High ELU
```

This is a strong indication that the JavaScript side is overloaded.

---

# 25. Fixing Event-Loop Blocking

There isn't one universal fix.

First determine **what kind of work is causing the problem**.

---

# 26. Fix #1 — Remove Synchronous APIs

Bad:

```js
const data = fs.readFileSync("large.json");
```

Prefer:

```js
const data = await fs.promises.readFile("large.json");
```

The goal is:

```text
synchronous blocking
        ↓
asynchronous operation
```

But remember:

> An async API doesn't automatically mean zero event-loop cost.

For example:

```js
JSON.parse(hugeString)
```

is still synchronous even if the string came from an asynchronous file read.

---

# 27. Fix #2 — Break CPU Work Into Chunks

Suppose:

```js
for (let i = 0; i < 1e9; i++) {
    doWork(i);
}
```

Instead of one enormous operation:

```text
████████████████████████████
```

partition it:

```text
chunk
 ↓
yield
 ↓
chunk
 ↓
yield
 ↓
chunk
 ↓
yield
```

In Node, `setImmediate()` is often useful for yielding between chunks.

Example:

```js
function processChunk(items, index = 0) {
    const end = Math.min(index + 1000, items.length);

    for (let i = index; i < end; i++) {
        processItem(items[i]);
    }

    if (end < items.length) {
        setImmediate(() => {
            processChunk(items, end);
        });
    }
}
```

Now the event loop gets opportunities to process other work.

Node's own guidance describes this technique as **partitioning** CPU work so that execution periodically yields to other pending events. ([Node.js][3])

---

# 28. Fix #3 — Offload CPU Work

If the computation is genuinely expensive, don't just partition it.

Move it away from the main JavaScript thread.

Options:

```text
Worker Threads
Child Processes
Multiple Node processes
External job workers
```

For example:

```text
HTTP Request
     │
     ▼
Node Event Loop
     │
     ▼
Worker Thread
     │
     ▼
Heavy computation
     │
     ▼
Result
     │
     ▼
Node Event Loop
```

This provides actual parallel computation.

---

# 29. Worker Threads

Node provides:

```js
worker_threads
```

Useful for CPU-heavy JavaScript.

Example architecture:

```text
Main Thread
    │
    ├── HTTP
    ├── Redis
    ├── DB
    └── Worker Thread
             │
             └── CPU computation
```

The worker executes JavaScript independently of the main event-loop thread.

Use this for things like:

```text
large computation
image processing
CPU-heavy parsing
complex algorithms
ML inference
```

depending on the workload.

---

# 30. Worker Pool vs Worker Threads

Don't confuse these.

### libuv Worker Pool

Used internally by Node for certain operations such as some:

```text
filesystem
crypto
DNS
zlib
```

operations.

### Worker Threads

Explicitly created by your application:

```js
new Worker(...)
```

for running JavaScript in another thread.

So:

```text
libuv worker pool
        ≠
Worker Threads
```

This distinction is frequently asked in Node interviews.

---

# 31. Fixing Microtask Starvation

Suppose:

```js
function loop() {
    queueMicrotask(loop);
}

loop();
```

The solution is **not**:

```js
queueMicrotask(loop);
```

again.

You need to introduce a yield to a different scheduling mechanism or stop generating unbounded microtasks.

For example:

```js
let count = 0;

function process() {
    count++;

    if (count % 1000 === 0) {
        setImmediate(process);
    } else {
        queueMicrotask(process);
    }
}

process();
```

Conceptually:

```text
1000 microtasks
       ↓
yield
       ↓
event loop gets opportunity
       ↓
1000 microtasks
       ↓
yield
```

But in real applications, even better is usually to redesign the algorithm so you don't create an unbounded microtask chain.

---

# 32. Fixing `process.nextTick()` Starvation

Bad:

```js
function loop() {
    process.nextTick(loop);
}

loop();
```

This can prevent normal event-loop progress.

Avoid recursive unbounded `nextTick()`.

Instead:

```js
setImmediate(loop);
```

or redesign the operation.

Remember:

```text
nextTick
→ defer until current operation finishes

NOT

nextTick
→ give other event-loop phases a chance
```

This distinction is crucial.

---

# 33. `setImmediate()` as a Yield

Suppose you have:

```js
function expensiveTask() {
    // huge work
}
```

Instead of:

```js
expensiveTask();
```

you can partition:

```js
function processNextChunk() {
    doSmallAmountOfWork();

    if (hasMoreWork()) {
        setImmediate(processNextChunk);
    }
}
```

Now:

```text
chunk
 ↓
event loop
 ↓
other callbacks
 ↓
chunk
 ↓
event loop
 ↓
other callbacks
```

This improves fairness.

But:

> `setImmediate()` does not make CPU-heavy work parallel.

It only gives other event-loop work opportunities to execute between chunks.

---

# 34. `setTimeout(0)` vs `setImmediate()`

Both can be used as forms of deferred execution, but they participate differently in Node's event-loop scheduling.

```js
setTimeout(fn, 0);
```

is associated with the **timers** phase.

```js
setImmediate(fn);
```

runs in the **check** phase.

Inside an I/O callback:

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

`setImmediate()` will typically execute first.

But:

```js
setTimeout(...);
setImmediate(...);
```

from the main module does **not** guarantee one universal ordering.

Don't memorize:

```text
setImmediate always wins
```

Instead understand the phases.

---

# 35. Node Event Loop Phases

At a high level:

```text
        ┌─────────────┐
        │   timers    │
        └──────┬──────┘
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
               │
               └──────→ repeat
```

Important phases:

### Timers

```js
setTimeout()
setInterval()
```

### Poll

Handles much of Node's I/O processing.

### Check

```js
setImmediate()
```

### Close callbacks

Certain close-related callbacks.

The exact scheduling behavior is more nuanced than this simplified diagram, and Node versions can change implementation details.

For interviews, understand the conceptual phases rather than memorizing every internal detail.

---

# 36. `async/await` and the Event Loop

Consider:

```js
async function test() {
    console.log("A");

    await Promise.resolve();

    console.log("B");
}

test();

console.log("C");
```

Output:

```text
A
C
B
```

Why?

`await` on the Promise causes the continuation to be scheduled asynchronously.

Conceptually:

```text
test()
 ↓
A
 ↓
await
 ↓
function suspends
 ↓
C
 ↓
microtask
 ↓
B
```

Therefore:

> `await` does not block the entire JavaScript thread.

It suspends the current async function's continuation.

---

# 37. `await` Does NOT Make CPU Work Asynchronous

This is a common interview trap.

Bad assumption:

```js
await heavyCalculation();
```

If:

```js
async function heavyCalculation() {
    for (let i = 0; i < 1e10; i++) {
        // CPU work
    }
}
```

then:

```text
async
```

does not magically make the loop non-blocking.

The function still performs the CPU work synchronously before producing its result.

Therefore:

```text
async function
        ≠
non-blocking function
```

Very important.

---

# 38. Concurrency vs Parallelism

### Concurrency

Multiple operations are in progress during overlapping periods.

```text
Request A ─────────────
Request B ────────
Request C ──────────────
```

### Parallelism

Multiple computations execute simultaneously on different execution resources.

```text
CPU 1 → A A A A
CPU 2 → B B B B
CPU 3 → C C C C
```

Node's event loop primarily provides:

```text
high I/O concurrency
```

not:

```text
automatic parallel JavaScript execution
```

---

# 39. Why Node Handles Many Requests

Imagine:

```text
Request A → DB
Request B → Redis
Request C → HTTP API
Request D → DB
```

Node doesn't need:

```text
Thread A → Request A
Thread B → Request B
Thread C → Request C
Thread D → Request D
```

Instead:

```text
Node Event Loop
      │
      ├── Request A → waiting for DB
      ├── Request B → waiting for Redis
      ├── Request C → waiting for HTTP
      └── Request D → waiting for DB
```

While those operations are pending, the JavaScript thread can process other work.

That's the core reason Node is effective for I/O-heavy workloads.

---

# 40. But I/O Itself Can Have Different Implementations

This is a subtle SDE-II point.

Don't say:

> "Every async operation goes to the thread pool."

That's incorrect.

Some asynchronous I/O can be handled through OS facilities such as:

```text
epoll
kqueue
IOCP
```

while certain operations use libuv's worker pool.

Node's own documentation distinguishes the event loop from the Worker Pool and notes that the OS handles much of the non-blocking network I/O, while the Worker Pool handles certain expensive operations. ([Node.js][3])

So the better mental model is:

```text
Async operation
      │
      ├── OS async I/O
      │
      └── libuv Worker Pool
```

depending on the API/operation.

---

# 41. Event-Loop Blocking

Consider:

```js
app.get("/heavy", (req, res) => {
    for (let i = 0; i < 10_000_000_000; i++) {
        // CPU work
    }

    res.send("done");
});
```

While that loop runs:

```text
JavaScript Thread
████████████████████████████
```

Other callbacks cannot execute normally.

Therefore:

```text
Request A
    ↓
CPU-heavy callback
    ↓
Event loop blocked
    ↓
Request B waits
Request C waits
Request D waits
```

This can cause:

```text
latency ↑
throughput ↓
timeouts ↑
```

Node's official guidance emphasizes keeping individual Event Loop callbacks small because a long callback prevents other clients from getting their turn. ([Node.js][3])

---

# 42. Common Event-Loop Blocking Sources

Watch for:

### 1. Huge loops

```js
for (...) {}
```

### 2. Synchronous filesystem APIs

```js
fs.readFileSync()
```

### 3. Huge JSON operations

```js
JSON.parse(hugeString);
JSON.stringify(hugeObject);
```

### 4. CPU-heavy algorithms

```text
sorting
compression
encryption
image processing
ML
large data transformation
```

### 5. Bad regular expressions

Potential:

```text
Catastrophic backtracking
```

### 6. Synchronous crypto APIs

```text
crypto.*Sync()
```

### 7. Large serialization/deserialization

Especially when processing huge payloads.

---

# 43. Starvation: The Full Picture

There are two levels you should understand.

### Level 1 — What is starvation?

```text
Work A keeps consuming execution opportunities
        ↓
Work B continuously waits
        ↓
Work B gets little/no service
```

### Level 2 — How do we detect it?

Look for:

```text
Unexpected event-loop delay
+
scheduled callbacks executing late
+
high request latency
+
persistent queue growth
+
CPU/ELU anomalies
```

Then investigate:

```text
CPU profile
async stack traces
application metrics
event-loop delay
```

### Level 3 — How do we fix it?

Depending on the cause:

```text
Long synchronous work
        ↓
partition / optimize / worker thread

Microtask starvation
        ↓
limit microtask chains / yield

nextTick starvation
        ↓
avoid recursive nextTick / yield with setImmediate

CPU-heavy work
        ↓
Worker Thread / separate process / job queue

Huge payload processing
        ↓
stream / paginate / chunk

Bad algorithm
        ↓
optimize complexity
```

That's the complete production-level understanding.

---

# 44. Starvation vs Backpressure

Another concept worth adding at SDE-II level:

> **Starvation and backpressure are not the same thing.**

### Starvation

Something isn't getting execution time.

```text
Microtasks
████████████████
Timer
WAITING
```

### Backpressure

The producer is producing work faster than the consumer can process it.

```text
Producer
  ↓↓↓↓↓↓↓↓↓
Queue
████████████████████
  ↓
Consumer
  ↓
slow
```

For example:

```text
Kafka producer
     ↓
100,000 events/sec
     ↓
consumer
     ↓
10,000 events/sec
```

The queue grows.

This is **backpressure / overload**, not necessarily event-loop starvation.

---

# 45. Starvation vs Deadlock

Another distinction:

### Starvation

Work keeps waiting because other work keeps getting scheduled.

### Deadlock

Two or more parties wait indefinitely for each other.

Example conceptually:

```text
A waits for B
B waits for A
```

Node event-loop starvation is generally not a traditional lock-based deadlock.

---

# 46. How to Debug a Production Event-Loop Problem

This is an excellent SDE-II scenario question.

Suppose:

> "Our Node.js API suddenly has 2-second latency."

I'd approach it like this:

```text
1. Check request latency
        ↓
2. Check event-loop delay
        ↓
3. Check ELU / CPU
        ↓
4. Check DB/Redis/external service latency
        ↓
5. Check worker-pool saturation
        ↓
6. Capture CPU profile
        ↓
7. Identify blocking function
        ↓
8. Fix / partition / offload
        ↓
9. Load test
        ↓
10. Add monitoring to prevent recurrence
```

Don't immediately blame the event loop.

The latency might actually be:

```text
Database
Redis
Kafka
network
connection pool
external API
```

---

# 47. Production Example

Suppose:

```text
API p99 latency
    ↑
2 sec

Event-loop p99 delay
    ↑
800 ms

CPU
    ↑
95%

ELU
    ↑
98%
```

This strongly suggests:

```text
Node process is spending too much time executing
```

Next step:

```text
CPU profile
```

Suppose we discover:

```js
JSON.stringify(hugeObject)
```

taking:

```text
700 ms
```

Then the problem isn't:

> "Node is slow."

The actual problem is:

```text
Huge synchronous serialization
        ↓
Event loop blocked
        ↓
requests wait
        ↓
p99 latency increases
```

---

# 48. Another Production Example — Starvation

Suppose:

```js
function process() {
    Promise.resolve().then(process);
}

process();
```

Symptoms:

```text
CPU high
event-loop delay high
timers not executing
I/O callbacks delayed
request latency exploding
```

CPU profiling shows:

```text
Promise callbacks
Promise callbacks
Promise callbacks
...
```

Now we identify:

```text
Microtask starvation
```

Fix:

```text
Stop recursive microtask generation
```

or introduce controlled yielding / redesign the processing pipeline.

---

# 49. Another Production Example — `nextTick` Starvation

Bad code:

```js
function process() {
    process.nextTick(process);
}

process();
```

Symptoms:

```text
CPU high
event-loop phases not progressing normally
timers delayed
I/O delayed
```

Root cause:

```text
Unbounded nextTick queue
```

Fix:

```text
Remove recursive nextTick
```

or yield using an appropriate scheduling mechanism.

Node explicitly warns that recursive `process.nextTick()` can create an infinite loop and prevent the event loop from continuing. ([Node.js][1])

---

# 50. A Crucial Concept: Fairness

The event loop doesn't magically guarantee that your application logic is fair.

Suppose:

```text
Client A → 5 seconds CPU work
Client B → 2 ms work
```

If A's callback executes first:

```text
A ███████████████████████
B                        ↓
```

B waits.

This is why Node's architecture puts responsibility on application code to keep individual callbacks/tasks reasonably small. ([Node.js][3])

This concept is called:

> **Cooperative scheduling / fairness**

Your JavaScript should voluntarily yield.

---

# 51. How to Make CPU Work Fair

Instead of:

```js
processEverything();
```

use:

```text
process chunk
      ↓
yield
      ↓
process chunk
      ↓
yield
      ↓
process chunk
```

Possible mechanisms:

```js
setImmediate(...)
```

or move the workload entirely to:

```text
Worker Thread
```

or:

```text
Background Job
```

---

# 52. The Complete Queue Mental Model for Node

For interview purposes, keep this model:

```text
                CURRENT JS
                    │
                    ▼
              Call Stack
                    │
                    ▼
          ┌──────────────────┐
          │ process.nextTick │
          │      queue       │
          └────────┬─────────┘
                   ▼
          ┌──────────────────┐
          │ Microtask Queue  │
          │                  │
          │ Promise.then     │
          │ queueMicrotask   │
          └────────┬─────────┘
                   ▼
             Event Loop
                   │
       ┌───────────┼────────────┐
       ▼           ▼            ▼
    timers        poll        check
       │           │            │
       │           │            │
       │           │      setImmediate
       │           │
       │          I/O
       │
 setTimeout
```

But remember:

> This is a **mental model**, not a literal implementation diagram.

---

# 53. Execution-Order Problem-Solving Strategy

When an interviewer gives you:

```js
console.log("1");

setTimeout(...);

Promise.resolve().then(...);

process.nextTick(...);

console.log("2");
```

Don't guess.

Use this algorithm.

### Step 1

Execute all synchronous code.

```text
1
2
```

### Step 2

Identify:

```text
nextTick
Promise microtask
queueMicrotask
timer
setImmediate
I/O
```

### Step 3

Apply the appropriate Node/browser scheduling rules.

### Step 4

Remember that microtasks can enqueue more microtasks.

### Step 5

Don't assume:

```text
setTimeout(0)
```

means immediately.

### Step 6

Don't assume:

```text
await
```

means blocking.

---

# 54. Classic Example

```js
console.log("1");

setTimeout(() => {
    console.log("2");
}, 0);

Promise.resolve().then(() => {
    console.log("3");
});

process.nextTick(() => {
    console.log("4");
});

console.log("5");
```

In a CommonJS Node context, the typical order is:

```text
1
5
4
3
2
```

Reason:

```text
Synchronous
    ↓
1
5

nextTick queue
    ↓
4

microtask queue
    ↓
3

timer
    ↓
2
```

Node's exact ordering has ESM-specific nuances, so state the context if you're explaining this in an interview. ([Node.js][1])

---

# 55. Another Important Example

```js
setTimeout(() => {
    console.log("A");
}, 0);

setImmediate(() => {
    console.log("B");
});

Promise.resolve().then(() => {
    console.log("C");
});

process.nextTick(() => {
    console.log("D");
});
```

The safe reasoning is:

```text
nextTick
    ↓
microtasks
    ↓
event-loop phases
    ↓
timer / immediate depending on context
```

Don't claim:

```text
A always before B
```

or:

```text
B always before A
```

from the main module.

Instead:

> "`setTimeout` and `setImmediate` belong to different event-loop phases, so their relative ordering can depend on when and where they're scheduled."

That's the interview-quality answer.

---

# 56. `process.nextTick()` — When Should You Use It?

Historically, Node code used:

```js
process.nextTick()
```

to ensure a callback executes asynchronously after the current operation.

But modern Node documentation recommends:

```js
queueMicrotask()
```

for most userland cases and marks `process.nextTick()` as Legacy. ([Node.js][1])

So your practical rule should be:

```text
Need standard microtask?
        ↓
queueMicrotask()

Need Promise continuation?
        ↓
Promise.then()

Need Node-specific next-tick semantics?
        ↓
process.nextTick()

Need to yield back to event loop?
        ↓
setImmediate() / appropriate task mechanism
```

---

# 57. The Difference Between These Four

Memorize this table:

| API                  | Purpose                                 |
| -------------------- | --------------------------------------- |
| `Promise.then()`     | Promise continuation / microtask        |
| `queueMicrotask()`   | Schedule microtask                      |
| `process.nextTick()` | Node-specific next-tick queue           |
| `setImmediate()`     | Schedule callback in Node's check phase |

And:

```text
queueMicrotask()
        ≠
setImmediate()
```

because:

```text
microtask
```

has substantially higher scheduling priority than ordinary later event-loop work.

---

# 58. Event Loop vs Worker Pool

This is another common SDE-II interview area.

```text
                  Node.js
                     │
          ┌──────────┴──────────┐
          │                     │
      Event Loop            Worker Pool
          │                     │
       JS callbacks        expensive operations
       network I/O         certain fs/crypto/etc.
          │                     │
          ▼                     ▼
     Main execution        Worker threads
```

If the Event Loop is blocked:

```text
JavaScript requests suffer
```

If the Worker Pool is saturated:

```text
operations depending on it may become delayed
```

So:

> **Event-loop starvation and worker-pool starvation are different problems.**

---

# 59. Worker-Pool Starvation

Suppose your application submits many expensive crypto tasks:

```text
Task 1 ─────────────
Task 2 ─────────────
Task 3 ─────────────
Task 4 ─────────────
Task 5 ─────────────
...
```

If all worker threads are occupied:

```text
Worker Pool
┌───────┐
│ busy  │
├───────┤
│ busy  │
├───────┤
│ busy  │
├───────┤
│ busy  │
└───────┘
```

new work waits.

This is not necessarily event-loop blocking.

It's:

> **Worker-pool saturation.**

Node's architecture has both an Event Loop and a Worker Pool, and Node recommends avoiding long-running tasks on either. ([Node.js][3])

---

# 60. The Three Big Performance Problems

At SDE-II level, distinguish:

### 1. Event-loop blocking

```text
Long synchronous JS
```

### 2. Event-loop starvation

```text
Microtask/nextTick source continuously consumes execution opportunities
```

### 3. Worker-pool saturation

```text
Too many expensive worker-pool tasks
```

And separately:

### 4. Backpressure

```text
Producer > Consumer
```

These can produce similar symptoms but require different fixes.

---

# 61. Complete Production Debugging Framework

If someone says:

> "Our Node.js API is becoming slow."

Think:

```text
                 API latency high
                       │
          ┌────────────┼─────────────┐
          ▼            ▼             ▼
      Event Loop      DB/Redis      Network
          │
          ▼
   Event-loop delay?
          │
      ┌───┴────┐
      │        │
     YES       NO
      │
      ▼
   CPU high?
      │
   ┌──┴───┐
   │      │
  YES     NO
   │
   ▼
CPU profile
   │
   ├── huge loop
   ├── JSON
   ├── regex
   ├── crypto
   └── other CPU work
```

If CPU isn't obviously high:

```text
Check:
    microtask chains
    nextTick recursion
    worker-pool saturation
    external I/O
    connection pools
```

---

# 62. How to Fix It — Decision Tree

```text
Event-loop delay high
        │
        ▼
What is consuming the event loop?
        │
 ┌──────┼─────────┬─────────────┐
 ▼      ▼         ▼             ▼
CPU    JSON     Regex      Microtasks
 │       │        │             │
 ▼       ▼        ▼             ▼
Worker  Chunk    Fix regex   Limit/yield
Thread  /stream
 │
 ▼
Offload
```

For I/O:

```text
DB slow?
Redis slow?
External API slow?
Connection pool exhausted?
```

Don't solve an I/O problem with Worker Threads.

---

# 63. What "Non-Blocking" Actually Means

This term is often misunderstood.

Non-blocking does **not** mean:

> "This operation uses another thread."

It means:

> **The current JavaScript execution does not synchronously wait for the operation to complete.**

For example:

```js
const data = await fetch(url);
```

The network operation is pending while the async function's continuation is suspended.

Other JavaScript can execute.

Therefore:

```text
Non-blocking
        ≠
Parallel
```

and:

```text
Asynchronous
        ≠
CPU parallelism
```

---

# 64. Golden Rules — Updated

If you remember these, you're in very good shape for an SDE-II interview.

### Rule 1

> JavaScript execution is sequential on a given JavaScript thread.

### Rule 2

> The event loop does not interrupt long-running synchronous JavaScript.

### Rule 3

> Asynchronous I/O allows the JavaScript thread to work on other tasks while the I/O is pending.

### Rule 4

> `await` suspends the current async function; it does not block the entire event loop.

### Rule 5

> Promise handlers execute through the microtask mechanism.

### Rule 6

> Microtasks can enqueue additional microtasks, so an unbounded chain can starve other work.

### Rule 7

> `process.nextTick()` has special Node.js scheduling semantics and can also cause starvation if recursively abused. ([Node.js][1])

### Rule 8

> `setTimeout(fn, 0)` means "eligible after the timer threshold," not "execute immediately."

### Rule 9

> `setImmediate()` and `setTimeout()` belong to different scheduling mechanisms/phases.

### Rule 10

> Non-blocking I/O provides concurrency, not automatic parallel JavaScript execution.

### Rule 11

> CPU-heavy JavaScript should be optimized, partitioned, or offloaded.

### Rule 12

> Event-loop delay is a practical signal for detecting event-loop responsiveness problems.

### Rule 13

> Event-loop utilization tells you how busy the event loop is, but high utilization alone doesn't identify the root cause. ([Node.js][2])

### Rule 14

> Starvation means some work is continually denied execution opportunities.

### Rule 15

> Detect starvation through symptoms and instrumentation rather than assuming it from one metric.

### Rule 16

> Fix starvation by removing the unbounded scheduling loop, yielding appropriately, partitioning work, or moving expensive work off the event loop.

### Rule 17

> Event-loop starvation, worker-pool saturation, backpressure, and deadlock are different problems.

---

# 65. The SDE-II Mental Model

When you see:

```js
await db.query();
```

think:

```text
DB operation starts
       ↓
async function suspends
       ↓
event loop can process other work
       ↓
DB completes
       ↓
continuation becomes eligible
       ↓
runtime schedules continuation
       ↓
async function resumes
```

When you see:

```js
Promise.resolve().then(fn);
```

think:

```text
Promise settled
       ↓
fn → microtask
       ↓
current synchronous execution finishes
       ↓
microtask processing
       ↓
fn executes
```

When you see:

```js
process.nextTick(fn);
```

think:

```text
current operation finishes
       ↓
Node nextTick queue
       ↓
fn executes
       ↓
microtasks / event-loop progression
```

When you see:

```js
setImmediate(fn);
```

think:

```text
defer execution
       ↓
Node check phase
       ↓
fn
```

When you see:

```js
for (...) {
    // huge CPU work
}
```

think:

```text
CALL STACK
    ↓
BLOCKED
    ↓
EVENT LOOP DELAY
    ↓
REQUEST LATENCY
```

When you see:

```js
function loop() {
    queueMicrotask(loop);
}
```

think:

```text
MICROTASK STARVATION
        ↓
other work can't get sufficient opportunity
```

And when you see:

```js
function loop() {
    process.nextTick(loop);
}
```

think:

```text
NEXT-TICK STARVATION
        ↓
event-loop progression is prevented
```

---

# 66. What You Should Be Able to Answer in an SDE-II Interview

I would now divide this topic into **four levels**.

### 🔴 Level 1 — Must Know

You should answer these almost instantly:

1. What is the call stack?
2. What is the event loop?
3. Why is JavaScript single-threaded?
4. What is a microtask?
5. What is a task?
6. Promise vs `setTimeout`
7. What does `await` actually do?
8. Why doesn't `setTimeout(..., 0)` run immediately?
9. What is non-blocking I/O?
10. Concurrency vs parallelism

### 🟠 Level 2 — SDE-II Core

11. Node.js event-loop phases
12. `process.nextTick()`
13. `queueMicrotask()`
14. `setImmediate()`
15. `setTimeout()` vs `setImmediate()`
16. libuv
17. libuv worker pool
18. Worker Threads
19. Event-loop blocking
20. CPU-bound work

### 🟠 Level 3 — Production Understanding

21. What is event-loop lag?
22. How do you measure event-loop delay?
23. What is Event Loop Utilization?
24. How do you diagnose event-loop blocking?
25. How do you diagnose starvation?
26. How do you fix microtask starvation?
27. How do you fix nextTick starvation?
28. How do you partition CPU work?
29. When do you use Worker Threads?
30. Event-loop vs worker-pool saturation

### 🔴 Level 4 — Difficult Interview Questions

31. `process.nextTick()` vs Promise microtasks
32. CJS vs ESM ordering nuances
33. Why recursive `nextTick()` can starve the event loop
34. Why recursive microtasks can starve tasks
35. `setImmediate()` vs `setTimeout(0)` inside I/O
36. Event-loop lag vs ELU
37. How to identify CPU-bound vs I/O-bound latency
38. How to debug a production event-loop issue
39. How to partition CPU work
40. How to distinguish starvation from backpressure
41. How to distinguish event-loop saturation from worker-pool saturation
42. How to design a Node service that remains responsive under CPU-heavy workloads

---

# 67. The One Diagram to Remember

If you want **one diagram in your head for the interview**, use this:

```text
                         NODE.JS
                            │
                            ▼
                    ┌──────────────┐
                    │  JS Execution│
                    └──────┬───────┘
                           │
                           ▼
                      CALL STACK
                           │
                    synchronous JS
                           │
                           ▼
              ┌────────────────────────┐
              │   Scheduling queues    │
              │                        │
              │  1. nextTick           │
              │  2. Microtasks         │
              │     - Promise          │
              │     - queueMicrotask   │
              └────────────┬───────────┘
                           │
                           ▼
                     EVENT LOOP
                           │
            ┌──────────────┼──────────────┐
            │              │              │
            ▼              ▼              ▼
         TIMERS           POLL          CHECK
            │              │              │
       setTimeout          I/O       setImmediate
            │              │              │
            └──────────────┼──────────────┘
                           │
                           ▼
                     More JavaScript


             ┌─────────────────────────┐
             │       libuv / OS        │
             │                         │
             │   Network I/O           │
             │   Worker Pool           │
             │   Filesystem             │
             │   Crypto                 │
             └─────────────────────────┘
```

And surrounding all of this:

```text
                    PERFORMANCE
                         │
        ┌────────────────┼────────────────┐
        ▼                ▼                ▼
 Event-loop lag        ELU              CPU
        │                │                │
        └────────────────┼────────────────┘
                         ▼
                  DIAGNOSE PROBLEM
                         │
        ┌────────────────┼─────────────────┐
        ▼                ▼                 ▼
   Blocking          Starvation       Worker Pool
        │                │             Saturation
        ▼                ▼                 ▼
 Partition /        Stop recursive    Reduce / move
 Worker Thread      scheduling        expensive work
```

## The most important upgrade from your original notes

Your original version taught:

```text
"What is the event loop?"
```

This version should prepare you to answer the much more valuable SDE-II question:

> **"My Node.js service has suddenly become slow. How do you know whether the event loop is blocked or starved, how do you prove it, and how do you fix it?"**

The answer should now flow naturally:

```text
Observe latency
      ↓
Measure event-loop delay
      ↓
Check ELU + CPU
      ↓
Check I/O / DB / Redis
      ↓
Profile if event loop is busy
      ↓
Identify:
 ┌──────────────┬───────────────┬───────────────┐
 │              │               │
Blocking      Starvation      Worker Pool
 │              │               │
CPU/JSON/etc.  nextTick/       expensive
               microtasks      tasks
 │              │               │
 ▼              ▼               ▼
Partition/     stop recursive  reduce/offload
Worker Thread  scheduling      workload
```

That is the **production-level event-loop understanding** I'd expect from a 3–4 year Node.js SDE-II candidate.

[1]: https://nodejs.org/api/process.html?utm_source=chatgpt.com "Process | Node.js v26.8.1 Documentation"
[2]: https://nodejs.org/api/perf_hooks.html?utm_source=chatgpt.com "Performance measurement APIs | Node.js v26.8.1 Documentation"
[3]: https://nodejs.org/en/learn/asynchronous-work/dont-block-the-event-loop?source=post_page---------------------------&utm_source=chatgpt.com "Node.js — Don't Block the Event Loop (or the Worker Pool)"
