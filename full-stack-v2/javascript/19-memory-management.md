Yes. We’ll continue with **Point 17 — Memory Management** using the same SDE-2 revision style we’ve been following.

The key distinction from the earlier topics is:

> **Earlier we focused on how JavaScript executes and behaves. Now we focus on what happens to the data created during that execution, how long it stays alive, and why memory can leak.**

We’ll keep this at **SDE-2 / production depth**, not V8-engine-internals depth.

---

# 17. JavaScript Memory Management

## 17.1 The Mental Model

Before individual concepts, keep this model in mind:

```text
JavaScript Program
       │
       ├── creates values
       │      │
       │      ├── primitives
       │      └── objects/functions
       │
       ├── references values
       │
       └── values remain in memory
              │
              └── while they are reachable
                         │
                         ▼
                  Garbage Collector
                         │
                  unreachable objects
                         │
                         ▼
                       cleanup
```

So the fundamental question is:

> **"Can the program still reach this value?"**

If yes → it generally needs to remain alive.

If no → it becomes eligible for garbage collection.

That concept of **reachability** is more important than memorizing "stack vs heap."

---

# 17.2 Stack vs Heap — Conceptual Model

You will commonly hear:

```text
Stack → primitives / local variables
Heap  → objects
```

This is useful as a **conceptual model**, but don't treat it as an exact JavaScript specification rule.

For example:

```js
let x = 10;

let user = {
    name: "Danish"
};
```

Conceptually:

```text
Stack
┌───────────────┐
│ x = 10        │
│ user ─────────┼──────┐
└───────────────┘      │
                       ▼
                     Heap
                ┌──────────────┐
                │ {            │
                │   name: ...  │
                │ }            │
                └──────────────┘
```

`user` contains a reference to the object.

The important interview point:

> JavaScript variables hold **values**. For objects, that value is a **reference to an object**.

Don't say:

> "Objects are stored in heap and variables are stored in stack."

as an absolute language rule.

A better SDE-2 answer is:

> "Stack and heap are useful conceptual models for understanding execution and allocation, but ECMAScript itself doesn't prescribe a particular memory layout."

---

# 17.3 Object Allocation

Whenever you create an object:

```js
const user = {
    name: "John",
    age: 30
};
```

JavaScript needs memory for that object.

Similarly:

```js
const arr = [1, 2, 3];

const fn = () => {};

const map = new Map();
```

These create dynamically allocated values.

You don't manually do:

```cpp
delete user;
```

like C++.

JavaScript uses **automatic memory management**.

---

# 17.4 References

This is extremely important.

Consider:

```js
const user = {
    name: "John"
};

const anotherUser = user;
```

Conceptually:

```text
user ──────────┐
               │
               ▼
          ┌─────────────┐
          │ { name: ... │
          └─────────────┘
               ▲
               │
anotherUser ───┘
```

There is **one object**, but two references.

Therefore:

```js
anotherUser.name = "Alex";

console.log(user.name);
```

Output:

```text
Alex
```

Because both variables refer to the same object.

This connects directly to the **values vs references** discussion from Point 3.

---

# 17.5 Reachability

This is probably the **single most important concept in memory management**.

An object is generally considered alive if it is **reachable** from a set of GC roots.

For example:

```js
let user = {
    name: "John"
};
```

As long as:

```text
user → object
```

the object is reachable.

Now:

```js
user = null;
```

If there are no other references:

```text
user

       X

object
```

the object is no longer reachable.

It becomes **eligible for garbage collection**.

Important distinction:

> **Eligible for garbage collection ≠ immediately deleted.**

The garbage collector decides when/how to reclaim it.

---

# 17.6 Garbage Collection

JavaScript automatically manages memory using garbage collection.

Conceptually:

```text
Allocate memory
      ↓
Use objects
      ↓
Objects become unreachable
      ↓
Garbage Collector identifies them
      ↓
Memory can be reclaimed
```

You don't normally manually free memory.

Example:

```js
function createUser() {
    const user = {
        name: "John"
    };

    return user;
}

let user = createUser();

user = null;
```

Once there are no remaining references to the object, it can eventually be collected.

---

# 17.7 Mark-and-Sweep

For SDE-2, you should understand the basic idea.

Imagine:

```text
GC Roots
   │
   ├──────► A
   │        │
   │        ▼
   │        B
   │
   └──────► C

D ───► E
```

The GC starts from roots and marks everything reachable:

```text
Roots
 ↓
A → B
 ↓
C
```

`D → E` isn't reachable from the roots.

Conceptually:

```text
Reachable:
A
B
C

Unreachable:
D
E
```

Then the collector can reclaim the unreachable objects.

That's the basic idea behind **mark-and-sweep**:

```text
MARK
 ↓
Find reachable objects

SWEEP
 ↓
Reclaim unreachable objects
```

You don't need to know V8's specific GC generations or internal algorithms for this syllabus.

---

# 17.8 Memory Leaks

This is where memory management becomes **production engineering**.

A memory leak occurs when your application unintentionally keeps references to objects that it no longer needs.

Example:

```js
const cache = [];

function processRequest(data) {
    cache.push(data);
}
```

Suppose this runs for every request:

```text
Request 1 → data → cache
Request 2 → data → cache
Request 3 → data → cache
...
Request 1,000,000 → data → cache
```

The data remains reachable through:

```text
cache
```

Therefore GC cannot reclaim it.

Memory keeps increasing.

---

# 17.9 The Most Important Insight About Memory Leaks

A common misconception is:

> "The garbage collector failed to clean the object."

Usually that's **not** what happened.

Instead:

```text
Application accidentally retains reference
              ↓
Object remains reachable
              ↓
GC correctly considers it alive
              ↓
Memory doesn't get reclaimed
```

So:

> **Most JavaScript memory leaks are retention problems, not garbage-collector failures.**

That's a very good SDE-2 interview statement.

---

# 17.10 Detached References

Consider a DOM example:

```js
const element = document.getElementById("button");

const data = {
    hugeData: new Array(1000000).fill("data")
};

element.data = data;
```

Suppose the DOM element is removed:

```js
element.remove();
```

But some other reference still keeps the element or associated object alive.

You can end up with:

```text
Application reference
        ↓
Detached DOM element
        ↓
Huge object
```

The element is no longer part of the document, but it may still be reachable.

That's the basic idea behind a **detached reference / detached DOM tree** memory problem.

This matters especially in long-running browser applications.

---

# 17.11 Closures and Memory

This connects directly to our earlier **Scope & Closures** chapter.

Consider:

```js
function createHandler() {
    const hugeData = new Array(1000000).fill("data");

    return function () {
        console.log(hugeData.length);
    };
}

const handler = createHandler();
```

The returned function closes over:

```text
hugeData
```

Conceptually:

```text
handler
   │
   ▼
closure
   │
   └────► hugeData
```

As long as `handler` remains reachable:

```js
handler
```

the captured data may also remain reachable.

This is not inherently a leak.

Closures are perfectly valid.

The problem occurs when you unintentionally keep closures alive for a very long time.

---

# 17.12 Event Listeners and Memory

One classic browser problem:

```js
const button = document.querySelector("#button");

function handleClick() {
    console.log("clicked");
}

button.addEventListener("click", handleClick);
```

If the associated component/lifecycle ends but the listener or related references remain unnecessarily attached, objects can stay reachable.

A common cleanup pattern is:

```js
button.removeEventListener("click", handleClick);
```

Notice something important:

You need the **same function reference**.

This doesn't work:

```js
button.addEventListener("click", () => {
    console.log("clicked");
});

button.removeEventListener("click", () => {
    console.log("clicked");
});
```

Those are two different function objects.

This connects directly to the earlier topic:

> **Function identity matters.**

---

# 17.13 Timers and Memory

Timers can also keep references alive.

For example:

```js
const hugeData = new Array(1000000).fill("data");

setInterval(() => {
    console.log(hugeData.length);
}, 1000);
```

The callback captures `hugeData`.

And the interval remains active indefinitely.

Conceptually:

```text
Timer
 ↓
Callback
 ↓
Closure
 ↓
hugeData
```

Therefore:

```text
Timer → Callback → hugeData
```

As long as the timer exists, the captured data may remain reachable.

If you no longer need it:

```js
const id = setInterval(() => {
    // ...
}, 1000);

clearInterval(id);
```

Again:

> **Resource cleanup is reference cleanup.**

---

# 17.14 Weak References

Now we reach a more advanced part.

Normally:

```js
const obj = {};

const map = new Map();

map.set(obj, "some data");
```

The `Map` maintains a strong reference to `obj`.

Therefore:

```text
Map
 ↓
obj
```

Even if your original reference disappears:

```js
obj = null;
```

the object may still be reachable through the Map.

This is where `WeakMap` becomes useful.

---

# 17.15 WeakMap

Example:

```js
const weakMap = new WeakMap();

let user = {};

weakMap.set(user, {
    metadata: "some metadata"
});

user = null;
```

The important idea is that the key in a `WeakMap` does not keep the object alive in the same way a normal `Map` does.

Conceptually:

```text
Normal Map

Map ─────► Object
           ↑
           │
        prevents
        collection
```

Whereas:

```text
WeakMap

WeakMap - - -► Object
          weak reference
```

If there are no strong references to the object, it can become collectible.

### Why is this useful?

A classic use case is metadata associated with objects:

```js
const metadata = new WeakMap();

function attachMetadata(object, data) {
    metadata.set(object, data);
}
```

You don't want your metadata store to accidentally keep every object in your application alive forever.

---

# 17.16 WeakSet

`WeakSet` follows the same weak-reference idea.

```js
const weakSet = new WeakSet();

let user = {};

weakSet.add(user);

user = null;
```

Useful when you want to track objects without strongly retaining them.

For example:

```js
const processed = new WeakSet();

function process(object) {
    if (processed.has(object)) {
        return;
    }

    processed.add(object);

    // process
}
```

The `WeakSet` doesn't exist to keep those objects alive.

---

# 17.17 WeakMap vs Map

Remember this interview comparison:

| Feature          | Map                       | WeakMap                                                             |
| ---------------- | ------------------------- | ------------------------------------------------------------------- |
| Keys             | Any value                 | Objects / non-registered symbols, depending on current spec support |
| Strong reference | Yes                       | Weak for keys                                                       |
| Prevents key GC  | Yes                       | No                                                                  |
| Iterable         | Yes                       | No                                                                  |
| `.size`          | Yes                       | No                                                                  |
| Typical use      | General key-value storage | Object-associated metadata                                          |

For SDE-2, the main point is:

> **Use WeakMap when the association should not determine the lifetime of the key object.**

---

# 17.18 WeakRef

`WeakRef` allows you to hold a weak reference to an object.

Conceptually:

```js
const weakRef = new WeakRef(object);
```

You can later attempt to access it:

```js
const value = weakRef.deref();
```

But the object may have already been collected.

Therefore:

```js
const value = weakRef.deref();

if (value) {
    // object is currently available
}
```

The important rule:

> **Never build correctness-critical application logic around an object definitely remaining alive through `WeakRef`.**

It's primarily an advanced memory-management tool.

---

# 17.19 FinalizationRegistry

`FinalizationRegistry` allows you to register cleanup-related callbacks associated with objects that become garbage-collected.

Conceptually:

```js
const registry = new FinalizationRegistry((value) => {
    console.log("Object was collected:", value);
});
```

Then:

```js
registry.register(object, "some-resource");
```

But there is a critical point:

> **Garbage collection timing is nondeterministic.**

You should **not** use `FinalizationRegistry` for normal deterministic resource cleanup.

For example, don't think:

```js
// GC will eventually close my DB connection
```

Instead, explicitly clean up resources:

```js
connection.close();
```

Use deterministic lifecycle management for things such as:

* DB connections
* file handles
* sockets
* subscriptions
* timers

---

# 17.20 Memory Debugging

This is the part that makes the topic **production-level**.

Imagine your Node.js server starts at:

```text
200 MB
```

Then after several hours:

```text
400 MB
```

Then:

```text
700 MB
```

Then:

```text
1.2 GB
```

Eventually:

```text
OOM
```

Your job isn't simply to say:

> "There is a memory leak."

You need to find **what is retaining the memory**.

---

## What you investigate

Typical suspects:

```text
Global variables
      ↓
Caches
      ↓
Closures
      ↓
Event listeners
      ↓
Timers
      ↓
Large arrays
      ↓
Maps/Sets
      ↓
Request-scoped data accidentally retained
      ↓
Buffers
      ↓
Detached objects
```

---

# 17.21 Memory Profiling

In production-oriented debugging, you may inspect:

### Heap snapshots

Take snapshots at different points:

```text
Snapshot 1
   ↓
Run workload
   ↓
Snapshot 2
   ↓
Run workload
   ↓
Snapshot 3
```

Then compare:

```text
What objects are continuously increasing?
```

For Node.js, Chrome DevTools can be used to inspect V8 heap information when connected to a Node process.

---

# 17.22 A Practical Leak Example

Consider:

```js
const requests = [];

function handleRequest(req) {
    requests.push(req);
}
```

Suppose every request object contains lots of data.

After:

```text
100 requests
1000 requests
10000 requests
100000 requests
```

the array continues growing.

Why doesn't GC clean the requests?

Because:

```text
requests
   ↓
request 1
request 2
request 3
...
```

They are still reachable.

The fix isn't:

> "Force garbage collection."

The fix is:

```js
// Don't retain unnecessary request objects.
```

or impose an appropriate bounded cache/data structure.

---

# 17.23 Memory Management in Node.js

This becomes particularly important for backend developers.

A Node.js process is long-lived:

```text
Server starts
    ↓
Request 1
    ↓
Request 2
    ↓
Request 3
    ↓
...
Request 1,000,000
```

A temporary object from one request should normally become unreachable after the request finishes.

But if something accidentally retains it:

```text
Request
  ↓
Closure
  ↓
Global cache
  ↓
Request data
```

then the data can survive far beyond the request lifecycle.

This is why memory leaks in backend services can be particularly dangerous.

---

# 17.24 The SDE-2 Mental Model

You should be able to reason about memory like this:

```text
Object created
     ↓
Who references it?
     ↓
What references those references?
     ↓
Can we reach it from a GC root?
     ↓
YES ──► Object stays alive
     │
     └──► Is that reference intentional?
                 │
            NO ──┴──► Memory leak
```

That is much more valuable than memorizing GC terminology.

---

# 17.25 Important Interview Distinctions

### Memory leak vs garbage collection

**Bad explanation:**

> Garbage collector didn't delete the object.

**Better:**

> The application unintentionally retained a reference, so the object remained reachable and therefore wasn't eligible for collection.

---

### `Map` vs `WeakMap`

```text
Map
 ↓
strong reference
 ↓
can keep object alive
```

```text
WeakMap
 ↓
weak key reference
 ↓
doesn't prevent collection
```

---

### Closure vs memory leak

Closure:

```js
function outer() {
    const data = ...;

    return () => data;
}
```

is **not automatically a leak**.

It becomes problematic when something unnecessarily keeps that closure alive.

---

### `WeakRef` vs normal reference

Normal:

```js
const obj = {};
const ref = obj;
```

Strong reference.

`WeakRef`:

```js
const ref = new WeakRef(obj);
```

Doesn't guarantee the object remains alive.

---

# 17.26 What You Should Be Able to Answer After This Topic

For SDE-2, these should become interview-ready:

### Basic

* What is garbage collection?
* What is reachability?
* What is mark-and-sweep?
* What happens when an object becomes unreachable?
* What is the difference between stack and heap conceptually?

### Intermediate

* How can JavaScript applications leak memory?
* How do closures affect memory?
* How can event listeners cause leaks?
* How can timers cause leaks?
* What are detached DOM references?
* Why doesn't GC simply remove unused objects?

### Advanced SDE-2

* `Map` vs `WeakMap`
* `WeakSet`
* `WeakRef`
* `FinalizationRegistry`
* How would you debug a memory leak in Node.js?
* How would you determine which objects are retaining memory?
* How do you distinguish high memory usage from an actual memory leak?

---

# 17.27 One Final Mental Picture

Keep this picture:

```text
                GC ROOTS
                    │
        ┌───────────┼───────────┐
        ▼           ▼           ▼
      Global     Stack       Active
     references  refs        callbacks
        │           │           │
        └───────────┼───────────┘
                    ▼
              REACHABLE OBJECTS
                    │
                    │
             remain in memory
                    │
                    ▼
             ┌─────────────┐
             │ Application │
             │ accidentally│
             │ retains refs│
             └─────────────┘
                    │
                    ▼
               MEMORY LEAK
```

And:

```text
Unreachable object
       ↓
Eligible for GC
       ↓
Garbage collector
       ↓
Memory reclaimed
```

**The central concept for Point 17 is therefore:**

> ### **Memory lifetime in JavaScript is primarily determined by reachability, not by scope alone.**

That's the connection between **scope → closures → references → garbage collection → memory leaks**.

Next, we can do the **SDE-2 interview-style revision of Point 17**, where I give you scenarios/code snippets and make you reason about **what stays in memory, what gets collected, and where the leak is** rather than just revising definitions.
