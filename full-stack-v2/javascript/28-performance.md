# 26. JavaScript Performance

This is an important SDE-II topic because performance is not about memorizing:

> "`Map` is faster than `Object`."

The real skill is:

> **Given a slow system, identify where the time or memory is being spent, understand why, and choose the right optimization without making the system unnecessarily complex.**

A useful mental model:

```text
                    Performance
                        │
        ┌───────────────┼────────────────┐
        │               │                │
     CPU/Time         Memory           I/O/Async
        │               │                │
   ┌────┼────┐      ┌───┼────┐      ┌────┼─────┐
 Arrays Objects     Allocation GC    Network DB
 Map/Set             Leaks            Concurrency
 Algorithms
        │
        ▼
   Event Loop
        │
        ▼
   User Experience
```

---

# 1. First Principle: Measure Before Optimizing

The most important performance rule:

> **Don't optimize based on assumptions. Measure first.**

Suppose an API takes:

```text
1200 ms
```

You might assume:

```text
"Node is slow."
```

But the actual breakdown could be:

```text
Authentication       10 ms
Redis                 5 ms
Business logic       20 ms
PostgreSQL          950 ms   ← bottleneck
External API        200 ms
Serialization         15 ms
```

Optimizing JavaScript might save:

```text
20 ms
```

while optimizing the query saves:

```text
900+ ms
```

So:

```text
Performance engineering
        ↓
Measure
        ↓
Find bottleneck
        ↓
Optimize bottleneck
        ↓
Measure again
```

---

# 2. Time Complexity of Common Operations

You need to understand the **typical complexity**, but also remember that complexity is an abstraction. Real performance depends on data size, engine optimizations, memory locality, implementation details, and workload.

## Array

| Operation    |         Typical complexity |
| ------------ | -------------------------: |
| `arr[i]`     |                       O(1) |
| `push()`     |             O(1) amortized |
| `pop()`      |                       O(1) |
| `shift()`    |                       O(n) |
| `unshift()`  |                       O(n) |
| `slice()`    |                       O(k) |
| `splice()`   |             O(n) generally |
| `indexOf()`  |                       O(n) |
| `includes()` |                       O(n) |
| `find()`     |                       O(n) |
| `map()`      |                       O(n) |
| `filter()`   |                       O(n) |
| `reduce()`   |                       O(n) |
| `sort()`     | roughly O(n log n) typical |

The important one:

```js
arr.shift();
```

If the array is:

```text
[ A B C D E F ]
```

removing A may require remaining elements to be shifted/reorganized.

So repeatedly doing:

```js
while (arr.length) {
    arr.shift();
}
```

can become expensive.

For queue-like workloads, consider a different data structure or an index-based approach.

---

# 3. Array `push()` vs `unshift()`

Compare:

```js
arr.push(item);
```

with:

```js
arr.unshift(item);
```

Appending:

```text
[A B C D]
          ↑
        add
```

usually doesn't require moving all existing elements.

Prepending:

```text
      add
       ↓
[X A B C D]
```

requires more work.

Therefore:

```text
push/pop
→ generally efficient

shift/unshift
→ potentially expensive for large arrays
```

This matters when processing thousands or millions of elements.

---

# 4. Array Memory

Arrays aren't simply "a block of numbers."

JavaScript arrays are flexible objects with engine-specific internal representations.

For example:

```js
const arr = [1, 2, 3];
```

is highly different from relying on a fixed low-level C-style array model.

You should not assume:

```text
JavaScript array = C array
```

Modern JS engines optimize common array patterns heavily.

But unusual operations can make optimization harder.

For example:

```js
const arr = [];

arr[1000000] = "hello";
```

creates a very sparse array.

That's different from:

```js
const arr = ["a", "b", "c"];
```

---

# 5. Objects

Objects are excellent for key-based access:

```js
const users = {
    "101": "Danish",
    "102": "Ali"
};
```

Lookup:

```js
users["101"];
```

is typically treated as approximately O(1) average-case for normal property access.

But don't think:

> "Object is always O(1), therefore it's always fastest."

JavaScript engines use sophisticated internal representations and optimize based on object shape and access patterns.

---

# 6. Object Shape

Consider:

```js
const user = {
    id: 1,
    name: "Danish"
};
```

Then consistently creating objects with similar properties:

```js
{
    id: 2,
    name: "Ali"
}
```

allows engines to optimize property access.

But constantly changing object structure:

```js
const user = {};

user.id = 1;
user.name = "Danish";
user.age = 25;
```

and creating many objects with wildly different property structures can make optimization harder.

You don't need engine internals for SDE-II, but understand:

> **Consistent object structures are generally friendlier to JS engine optimization.**

---

# 7. Object vs Map

This is a very common interview question.

### Object

```js
const users = {};

users["101"] = "Danish";
```

### Map

```js
const users = new Map();

users.set(101, "Danish");
```

Map is specifically designed for key-value collections.

| Feature                        | Object                            | Map                    |
| ------------------------------ | --------------------------------- | ---------------------- |
| General-purpose object         | Yes                               | No                     |
| Arbitrary key types            | Property keys are strings/symbols | Yes                    |
| `.size`                        | No                                | Yes                    |
| Iteration semantics            | Different                         | Designed for iteration |
| Prototype concerns             | Yes                               | No                     |
| Key-value collection semantics | Okay                              | Better                 |

If you need:

> "A collection where arbitrary values are keys"

`Map` is usually the clearer choice.

---

# 8. Set

`Set` is useful when you need uniqueness.

Instead of:

```js
const users = [];

if (!users.includes(userId)) {
    users.push(userId);
}
```

you can use:

```js
const users = new Set();

users.add(userId);
```

Then:

```js
users.has(userId);
```

is typically O(1) average-case.

For large collections, this can be dramatically better than repeatedly doing:

```js
array.includes()
```

inside a loop.

---

# 9. The Classic Performance Problem

Suppose:

```js
const users = [/* 100000 users */];

for (const id of incomingIds) {
    if (!users.includes(id)) {
        users.push(id);
    }
}
```

If `incomingIds` is also large:

```text
includes → O(n)
inside loop → O(m)
```

Potentially:

```text
O(n × m)
```

Instead:

```js
const userSet = new Set(users);

for (const id of incomingIds) {
    userSet.add(id);
}
```

Now membership checks are typically O(1) average-case.

Overall, this can approach:

```text
O(n + m)
```

This is the kind of optimization you should recognize immediately in interviews.

---

# 10. Memory Allocation

Every time you create objects/arrays/functions, memory has to be allocated.

For example:

```js
function process() {
    const result = {
        name: "Danish",
        values: [1, 2, 3]
    };

    return result;
}
```

This creates objects that eventually become eligible for garbage collection when no longer reachable.

Repeated allocations:

```js
for (let i = 0; i < 10_000_000; i++) {
    const obj = {
        value: i
    };
}
```

can create substantial GC pressure.

The problem isn't:

> "Creating objects is bad."

The problem is:

> **Unnecessary allocation at high frequency can increase memory usage and garbage-collection work.**

---

# 11. Garbage Collection Impact

From Point 17:

```text
object created
      ↓
references exist
      ↓
object reachable
      ↓
object becomes unreachable
      ↓
GC eventually reclaims it
```

Suppose you're creating millions of temporary objects:

```js
for (...) {
    const temp = {
        ...
    };
}
```

Many become unreachable quickly.

The garbage collector has to deal with them.

That can increase:

```text
CPU usage
GC frequency
latency
memory pressure
```

In latency-sensitive systems, GC pauses/work can become visible.

So performance isn't just:

```text
CPU algorithm
```

It's also:

```text
allocation behavior
+
memory retention
+
GC
```

---

# 12. Allocation vs Leak

These are different problems.

### Excessive allocation

```text
create → discard → create → discard
```

Can create:

```text
GC pressure
```

### Memory leak

```text
create
 ↓
unintentionally retain
 ↓
never becomes unreachable
```

Causes:

```text
memory continuously grows
```

This distinction is extremely important.

---

# 13. Event-Loop Blocking

For Node.js, this is one of the highest-priority performance concepts.

Consider:

```js
app.get("/heavy", (req, res) => {
    let result = 0;

    for (let i = 0; i < 5_000_000_000; i++) {
        result += i;
    }

    res.json({ result });
});
```

While this synchronous computation runs:

```text
Node JS thread
       │
       ▼
Heavy computation
       │
       ├── request 1 waits
       ├── request 2 waits
       ├── request 3 waits
       └── request 4 waits
```

Node isn't necessarily incapable of handling those requests.

The problem is:

> **The event loop cannot execute other JavaScript while this synchronous work occupies the thread.**

---

# 14. CPU-Bound vs I/O-Bound

This distinction is essential.

### I/O-bound

```text
Database
Redis
HTTP
Filesystem
network
```

Node is generally good at handling many concurrent I/O operations.

### CPU-bound

```text
Huge loops
image processing
video encoding
large compression
cryptography
complex parsing
heavy computation
```

These can block the event loop if executed synchronously on the main JS thread.

Potential solutions include:

```text
worker_threads
child processes
job queues
external services
algorithm optimization
```

---

# 15. Async Does Not Automatically Mean Faster

Consider:

```js
await doSomething();
```

`await` doesn't make the underlying operation magically faster.

It allows the event loop to do other work while the Promise is pending.

For example:

```js
const user = await db.getUser();
```

While DB I/O is happening:

```text
Node
 │
 ├── request A waiting on DB
 │
 ├── request B executes
 │
 └── request C executes
```

That's concurrency.

But if you do:

```js
await hugeCpuCalculation();
```

and that function performs synchronous CPU work before returning a Promise, you can still block the event loop.

---

# 16. Sequential vs Concurrent Async Work

This is a very common performance bug.

Suppose:

```js
const user = await getUser();
const orders = await getOrders();
const notifications = await getNotifications();
```

If they're independent:

```text
User       ────────────>
Orders          ─────────────>
Notifications       ───────────>
```

You're unnecessarily serializing them.

Instead:

```js
const [user, orders, notifications] = await Promise.all([
    getUser(),
    getOrders(),
    getNotifications()
]);
```

Now:

```text
User            ─────>
Orders          ───────>
Notifications   ──────>
```

Total time becomes closer to:

```text
max(user, orders, notifications)
```

rather than:

```text
user + orders + notifications
```

assuming they truly can run concurrently and the downstream systems can handle the concurrency.

---

# 17. Don't Over-Concurrently

`Promise.all()` isn't automatically an optimization.

Imagine:

```js
await Promise.all(
    millionUsers.map(user => sendEmail(user))
);
```

You just created potentially enormous concurrency.

That can overload:

* database
* API
* network
* memory
* connection pools
* external service

Instead use:

```text
bounded concurrency
```

For example:

```text
100 operations
   ↓
process 10
   ↓
wait
   ↓
next 10
```

This is an important SDE-II concept:

> **Optimize latency without destroying system capacity.**

---

# 18. Debouncing

Debouncing means:

> **Wait until activity stops before executing.**

Classic example:

```text
User typing in search box
```

Without debounce:

```text
h
he
hel
hell
hello
```

Potentially:

```text
5 API requests
```

With debounce:

```text
h
he
hel
hell
hello
   ↓
wait
   ↓
one request
```

Example:

```js
function debounce(fn, delay) {
    let timer;

    return function (...args) {
        clearTimeout(timer);

        timer = setTimeout(() => {
            fn(...args);
        }, delay);
    };
}
```

Usage:

```js
const search = debounce(query => {
    fetch(`/search?q=${query}`);
}, 300);
```

---

# 19. Throttling

Throttling means:

> **Allow execution at most once within a specified interval.**

Example:

```text
scroll event
scroll event
scroll event
scroll event
scroll event
```

Instead of executing 100 times per second:

```text
execute
wait 100ms
execute
wait 100ms
execute
```

Typical use cases:

* scroll
* resize
* mouse movement
* continuous telemetry
* rate-limited UI updates

---

# 20. Debounce vs Throttle

Remember this:

```text
Debounce
→ "Wait until things stop."

Throttle
→ "Don't run too frequently."
```

Example:

| Scenario                          | Usually                           |
| --------------------------------- | --------------------------------- |
| Search input                      | Debounce                          |
| Scroll handler                    | Throttle                          |
| Resize calculation                | Throttle/debounce depending on UX |
| Auto-save after user stops typing | Debounce                          |
| Continuous mouse tracking         | Throttle                          |

---

# 21. Memoization

Memoization means caching a function's previous results.

Suppose:

```js
function fibonacci(n) {
    // expensive computation
}
```

Without memoization:

```text
f(10)
 ↓
recalculate repeated subproblems
```

With memoization:

```text
f(10)
 ↓
cache
 ↓
reuse existing result
```

Simple example:

```js
function memoize(fn) {
    const cache = new Map();

    return function (value) {
        if (cache.has(value)) {
            return cache.get(value);
        }

        const result = fn(value);
        cache.set(value, result);

        return result;
    };
}
```

---

# 22. Memoization Tradeoffs

Memoization isn't free.

You trade:

```text
CPU
```

for:

```text
memory
```

And caching can become incorrect if:

```text
function isn't deterministic
```

or its result depends on changing external state.

For example:

```js
function getCurrentTime() {
    return Date.now();
}
```

Memoizing this would produce stale results.

So memoization works best when:

```text
same inputs
+
same relevant state
=
same output
```

This connects directly to functional programming and pure functions.

---

# 23. Lazy Evaluation

Lazy evaluation means:

> **Don't perform work until the result is actually needed.**

Instead of:

```js
const data = expensiveOperation();
```

immediately doing the work, you can represent the computation and execute later.

Generators are a useful JavaScript example:

```js
function* numbers() {
    yield 1;
    yield 2;
    yield 3;
}
```

Values are produced as needed.

This is especially useful for:

* large datasets
* streams
* pipelines
* infinite sequences
* memory-sensitive processing

---

# 24. Eager vs Lazy

Imagine one million values.

Eager:

```text
generate 1M values
        ↓
store 1M values
        ↓
process
```

Lazy:

```text
request next
    ↓
generate
    ↓
process
    ↓
request next
```

Potentially much lower memory usage.

This is one reason Node.js streams are powerful.

---

# 25. DOM Performance

Browser JavaScript introduces another performance dimension.

The browser has to deal with:

```text
JavaScript
DOM
Style calculation
Layout
Paint
Compositing
```

If JavaScript repeatedly modifies the DOM:

```js
element.style.width = "...";
element.style.height = "...";
element.style.top = "...";
```

the browser may need to recalculate rendering-related work.

---

# 26. Layout and Paint

Conceptually:

```text
DOM change
   ↓
Style calculation
   ↓
Layout
   ↓
Paint
   ↓
Composite
```

Not every change triggers every stage, and modern browsers optimize heavily, but the model is useful.

Expensive repeated DOM work can cause:

```text
jank
↓
low FPS
↓
poor UX
```

---

# 27. Batch DOM Updates

Instead of repeatedly updating:

```js
for (const item of items) {
    container.appendChild(createElement(item));
}
```

you can often reduce DOM interaction by building content in memory first or using appropriate batching techniques.

For example:

```js
const fragment = document.createDocumentFragment();

for (const item of items) {
    fragment.appendChild(createElement(item));
}

container.appendChild(fragment);
```

The broader principle is:

> **Minimize expensive DOM interaction and batch work when possible.**

---

# 28. Bundle Size

For frontend applications:

```text
larger JavaScript bundle
        ↓
more bytes transferred
        ↓
more parsing
        ↓
more compilation/execution
        ↓
slower startup
```

So performance isn't just network transfer.

JavaScript also costs CPU to:

```text
download
parse
compile
execute
```

This is particularly important on slower mobile devices.

---

# 29. Code Splitting

Instead of:

```text
app.js = 5 MB
```

you might have:

```text
main.js       500 KB
dashboard.js  700 KB
reports.js    600 KB
admin.js      500 KB
```

The user only downloads what they need.

Dynamic imports:

```js
const reports = await import("./reports.js");
```

can enable this.

This connects directly to Point 24.

---

# 30. Bundle Optimization

Typical frontend optimization pipeline:

```text
Source
  ↓
Bundler
  ↓
Dependency graph
  ↓
Tree shaking
  ↓
Code splitting
  ↓
Minification
  ↓
Compressed assets
  ↓
Browser
```

You want to reduce:

```text
bytes
+
parse cost
+
execution cost
```

not merely file size.

---

# 31. Profiling

Profiling means measuring where the application actually spends resources.

For Node.js, you may investigate:

```text
CPU
memory
GC
event loop
I/O
```

For browsers:

```text
JavaScript
DOM
layout
paint
network
memory
```

A profiler can tell you:

```text
Function A → 42% CPU
Function B → 25%
Function C → 10%
```

Now you know where to investigate.

---

# 32. CPU Profiling

Suppose an endpoint is slow.

CPU profiling might reveal:

```text
processOrders()
   ↓
calculateDiscount()
   ↓
expensiveRegex()
```

Instead of blindly optimizing the whole endpoint, optimize:

```text
expensiveRegex()
```

This is much more effective.

---

# 33. Memory Profiling

Suppose:

```text
Memory:
500 MB
600 MB
700 MB
800 MB
...
```

You investigate heap snapshots.

You might discover:

```text
globalCache
   ↓
Array
   ↓
10 million objects
```

Now the issue isn't algorithmic speed.

It's memory retention.

This connects Point 17 directly to Point 26.

---

# 34. Benchmarking

Profiling asks:

> **Where is time being spent in the real application?**

Benchmarking asks:

> **How fast is this particular operation under controlled conditions?**

For example:

```text
Implementation A
→ 120 ms

Implementation B
→ 40 ms
```

You can benchmark:

```js
const start = performance.now();

runOperation();

const end = performance.now();

console.log(end - start);
```

For serious benchmarking, use dedicated benchmark tooling and statistically meaningful repeated runs rather than trusting one measurement.

---

# 35. Benchmarking Pitfalls

Don't benchmark like:

```text
run once
→ 10ms

therefore it's faster
```

Runtime behavior can be affected by:

* JIT optimization
* warm-up
* garbage collection
* CPU load
* OS scheduling
* input distribution
* caching
* network conditions

So proper benchmarks need:

```text
warm-up
multiple iterations
representative inputs
controlled environment
statistical comparison
```

---

# 36. Bottleneck Identification

This is perhaps the most important SDE-II skill.

Suppose:

```text
API latency = 2 seconds
```

Break it down:

```text
Request
 │
 ├── Node processing       50 ms
 ├── Redis                 10 ms
 ├── PostgreSQL          1200 ms
 ├── Kafka                 50 ms
 ├── external API         600 ms
 └── serialization         20 ms
```

The bottlenecks are obvious:

```text
PostgreSQL
External API
```

Optimizing:

```text
Node processing: 50 → 20ms
```

does almost nothing.

This is an example of the broader principle:

> **The largest bottleneck usually dominates total performance.**

---

# 37. Performance Optimization Decision Tree

When something is slow:

```text
Is it slow?
   │
   ▼
Measure
   │
   ▼
Where is the time spent?
   │
   ├── CPU
   │    └── algorithm / profiling / worker
   │
   ├── Database
   │    └── query / index / connection pool
   │
   ├── Network
   │    └── payload / caching / concurrency
   │
   ├── Event loop
   │    └── remove blocking CPU work
   │
   ├── Memory
   │    └── allocation / retention / GC
   │
   └── Browser
        └── JS / DOM / layout / bundle
```

This is much more valuable than memorizing isolated optimization tricks.

---

# 38. Common SDE-II Performance Questions

### Q: Why is `Map` sometimes better than an array for lookup?

Because array lookup such as `includes()` is generally O(n), while `Map.has()` is typically O(1) average-case.

---

### Q: Is `Map` always faster than Object?

No.

Performance depends on workload, key patterns, engine optimizations, data size, and operation type. Choose based on semantics first and benchmark when performance actually matters.

---

### Q: Why can `shift()` be expensive?

Removing from the beginning of an array can require rearranging/reindexing remaining elements, making it generally O(n).

---

### Q: Why is `Promise.all()` sometimes a performance optimization?

When independent operations can safely run concurrently, it removes unnecessary sequential waiting.

But uncontrolled concurrency can overload downstream systems.

---

### Q: Does async/await make code faster?

No.

It improves the ability to perform concurrent/non-blocking work when the underlying operations are asynchronous. It doesn't reduce the actual CPU cost of synchronous computation.

---

### Q: How do you handle CPU-heavy work in Node?

Depending on the workload:

```text
optimize algorithm
      ↓
worker_threads
      ↓
child process
      ↓
background job
      ↓
separate service
```

The correct choice depends on workload and isolation requirements.

---

### Q: What is the difference between debounce and throttle?

```text
Debounce → execute after activity stops.

Throttle → execute at controlled intervals during activity.
```

---

### Q: What is memoization?

Caching function results so repeated calls with the same relevant inputs can reuse previous results.

Tradeoff:

```text
less CPU
more memory
```

---

### Q: What is the first thing you do when an API is slow?

**Measure and identify the bottleneck.**

Don't immediately add caching or rewrite the code.

---

# 39. Your SDE-II Performance Checklist

You should be **strong** on these:

### Data structures

```text
Array
Object
Map
Set

O(1) vs O(n)
push/pop vs shift/unshift
lookup patterns
```

### Node.js

```text
event-loop blocking
CPU vs I/O
sequential vs concurrent async
Promise.all
bounded concurrency
memory allocation
GC pressure
```

### Browser

```text
DOM operations
rendering
layout/paint
debounce
throttle
bundle size
code splitting
```

### Optimization

```text
memoization
lazy evaluation
caching
algorithmic improvements
```

### Engineering

```text
profiling
benchmarking
metrics
bottleneck identification
measure → optimize → measure
```

---

# 40. The Most Important Mental Model

Don't think:

```text
"Performance = make JavaScript faster."
```

Think:

```text
                     User request
                          │
                          ▼
                    Application
                          │
        ┌─────────────────┼─────────────────┐
        │                 │                 │
       CPU              Memory              I/O
        │                 │                 │
   Algorithm          Allocation        DB/API/Network
        │                 │                 │
       GC              Retention        Concurrency
        │                 │                 │
        └─────────────────┼─────────────────┘
                          │
                     Event Loop
                          │
                          ▼
                      Latency
                          │
                          ▼
                     User impact
```

Then apply:

```text
1. Measure
2. Identify bottleneck
3. Understand why
4. Optimize the actual bottleneck
5. Verify with measurement
6. Check that you didn't damage memory, correctness, or scalability
```

That last point is especially important at SDE-II.

A **faster** implementation isn't necessarily a **better** implementation if it:

* consumes 10× more memory,
* overloads Redis,
* creates too many DB connections,
* blocks the event loop,
* makes the code unmaintainable,
* introduces stale-cache bugs,
* or only performs better on an unrealistic benchmark.

**Performance engineering is therefore an optimization problem across CPU + memory + I/O + concurrency + user experience—not simply "write faster JavaScript."**
