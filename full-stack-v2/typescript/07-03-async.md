# Summary: Async JavaScript → Promises → `async/await` → Event Loop

| Concept                        | What it means                                   | Key point / Example                                        |
| ------------------------------ | ----------------------------------------------- | ---------------------------------------------------------- |
| **Synchronous JS**             | Code executes one statement at a time           | `A → B → C`                                                |
| **Call Stack**                 | Tracks currently executing functions            | **LIFO** — Last In, First Out                              |
| **Single-threaded JS**         | One main JS execution stack                     | One piece of JS runs at a time on that thread              |
| **Runtime / Host Environment** | Browser/Node provides APIs around JS            | Timers, network, I/O, etc.                                 |
| **Callback**                   | Function passed to another function             | `map(x => x * 2)`                                          |
| **Asynchronous API**           | Starts work that finishes later                 | `setTimeout`, network requests, I/O                        |
| **`setTimeout`**               | Schedules a timer callback                      | `0ms` means *not before 0ms*, not immediately              |
| **Task**                       | Host-scheduled unit of work                     | Timer/event callbacks are commonly tasks                   |
| **Promise**                    | Represents an eventual result                   | `Promise<T>`                                               |
| **Pending**                    | Promise hasn't settled                          | Waiting                                                    |
| **Fulfilled**                  | Promise completed successfully                  | `resolve(value)`                                           |
| **Rejected**                   | Promise failed                                  | `reject(error)`                                            |
| **`.then()`**                  | Handles fulfillment                             | Receives the fulfilled value                               |
| **`.catch()`**                 | Handles rejection                               | Receives the error                                         |
| **`.then()` return value**     | Always a new Promise                            | Enables chaining                                           |
| **Promise chaining**           | Passes results through multiple async steps     | `.then().then().then()`                                    |
| **Promise adoption**           | Returned Promise determines next chain state    | `return anotherPromise`                                    |
| **`async`**                    | Makes a function return a Promise               | `async () => 10` → `Promise<number>`                       |
| **`await`**                    | Suspends the async function's continuation      | Does **not** block the whole JS thread                     |
| **`await` continuation**       | Resumes asynchronously after settlement         | Promise/microtask semantics                                |
| **`throw` in `async`**         | Produces a rejected Promise                     | `async () => { throw Error() }`                            |
| **Microtask**                  | High-priority queued continuation               | Promise reactions, `queueMicrotask`, `await` continuations |
| **Microtask Queue**            | Holds pending microtasks                        | Drained before moving to the next task                     |
| **Event Loop**                 | Coordinates queued work with the call stack     | Moves eligible work toward execution                       |
| **`Promise.all()`**            | Waits for all; rejects if one rejects           | Good for independent tasks                                 |
| **`Promise.allSettled()`**     | Waits for everything                            | Gives every success/failure                                |
| **`Promise.race()`**           | First Promise to settle wins                    | Fulfillment **or** rejection                               |
| **`Promise.any()`**            | First Promise to fulfill wins                   | Rejects only if all reject                                 |
| **Concurrency**                | Multiple operations can be in progress          | `Promise.all([a(), b()])`                                  |
| **Sequential execution**       | Next operation waits for previous               | `await a(); await b();`                                    |
| **Microtask starvation**       | Microtasks continually schedule more microtasks | Can delay timers/rendering                                 |

### ⚡ Execution Priority — Mental Model

| Stage                      | What happens                                       |
| -------------------------- | -------------------------------------------------- |
| **1. Call Stack**          | Synchronous JavaScript executes                    |
| **2. Async work starts**   | Host APIs/timers/network/etc. handle external work |
| **3. Promise settles**     | Its reactions become microtasks                    |
| **4. Stack becomes empty** | Current synchronous execution finishes             |
| **5. Microtasks drain**    | Promise callbacks / `await` continuations run      |
| **6. Next task**           | Timer/event/etc. gets a chance                     |
| **7. Repeat**              | Event loop continues coordinating execution        |

### 🔥 Most Important Output Rule

```ts
console.log("A");

setTimeout(() => console.log("B"), 0);

Promise.resolve().then(() => console.log("C"));

console.log("D");
```

**Output:**

```text
A
D
C
B
```

Think:

```text
Synchronous code
      ↓
   A → D
      ↓
Microtasks
      ↓
     C
      ↓
Next task
      ↓
     B
```

### 🧠 One-Line Mental Models

| If you see...    | Think...                                                     |
| ---------------- | ------------------------------------------------------------ |
| `callback`       | **A function to call later/by another function**             |
| `Promise`        | **A representation of a future result**                      |
| `async`          | **This function returns a Promise**                          |
| `await`          | **Pause this function's continuation, not the whole thread** |
| `.then()`        | **Transform/continue a Promise**                             |
| `setTimeout`     | **Schedule a task for later**                                |
| `queueMicrotask` | **Schedule a microtask**                                     |
| `Promise.all()`  | **Wait for all**                                             |
| `Promise.race()` | **First settlement**                                         |
| `Promise.any()`  | **First success**                                            |
| Event loop       | **Coordinate when queued work gets back onto the stack**     |

### 🎯 The Big Picture

```text
JavaScript
    │
    ▼
Call Stack
    │
    ├── synchronous execution
    │
    ▼
Async work ──────► Host APIs
    │
    ▼
Promises / callbacks
    │
    ├── Microtask Queue
    │
    └── Task Queue
            │
            ▼
        Event Loop
            │
            ▼
        Call Stack
```

**Core distinction to remember:**

> **Callback = function**
> **Promise = eventual result**
> **`async/await` = syntax built around Promises**
> **Microtask = queued continuation**
> **Event loop = mechanism coordinating when queued work executes**



# 🔥 Deep Dive: Async JavaScript → Promises → `async/await` → Event Loop

This is where everything starts connecting.

The key idea:

> **JavaScript executes synchronously by default, but the runtime provides mechanisms that let us handle work that finishes later.**

The concepts form this chain:

```text
Synchronous Code
      ↓
Call Stack
      ↓
Asynchronous APIs
      ↓
Callback / Promise
      ↓
Queues
      ↓
Event Loop
      ↓
Back to Call Stack
```

# 1. Synchronous JavaScript

Consider:

```ts
console.log("A");
console.log("B");
console.log("C");
```

Execution is straightforward:

```text
A
↓
B
↓
C
```

Each statement finishes before the next one executes.

# 2. Call Stack

The **call stack** keeps track of currently executing functions.

```ts
function one() {
  two();
}

function two() {
  console.log("Hello");
}

one();
```

Conceptually:

```text
Global
  ↓
one()
  ↓
two()
  ↓
console.log()
```

Then functions return:

```text
console.log()
   ↑
two()
   ↑
one()
   ↑
Global
```

The stack is **LIFO**:

> Last In, First Out.

# 3. JavaScript Is Single-Threaded

For normal JavaScript execution, think:

```text
one call stack
```

So it cannot execute:

```text
function A
function B
```

at exactly the same moment on that same main JavaScript thread.

But then how can this happen?

```ts
setTimeout(() => {
  console.log("Later");
}, 1000);

console.log("Now");
```

We get:

```text
Now
Later
```

The answer is the **runtime environment + event loop**.

# 4. JavaScript Runtime

Think of the runtime approximately as:

```text
┌─────────────────────────────┐
│        JavaScript           │
│                             │
│       Call Stack            │
│           ↓                 │
│       Event Loop            │
│           ↓                 │
│        Queues               │
│                             │
├─────────────────────────────┤
│ Browser / Node APIs         │
│                             │
│ timers, network, I/O, etc.  │
└─────────────────────────────┘
```

Important:

> The **JavaScript language** itself isn't the same thing as the browser or Node.js runtime.

Things such as timers and many I/O facilities are provided by the host environment.

# 5. `setTimeout`

Consider:

```ts
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

Why isn't `B` immediately executed?

Because:

```text
setTimeout
   ↓
register callback with timer mechanism
   ↓
current JavaScript continues
   ↓
A → C
   ↓
callback becomes eligible later
   ↓
event loop
   ↓
callback enters call stack
   ↓
B
```

# 6. `0ms` Does NOT Mean "Immediately"

This is a common misconception.

```ts
setTimeout(fn, 0);
```

doesn't mean:

> Run `fn` now.

It means approximately:

> **Don't run the callback before the timer's minimum delay has elapsed; it must still wait for the event loop and stack to become available.**

So:

```ts
console.log("A");

setTimeout(() => console.log("B"), 0);

console.log("C");
```

gives:

```text
A
C
B
```

# 7. Callback

A callback is simply a function passed somewhere to be invoked later or by another function.

```ts
function greet(name: string, callback: () => void) {
  console.log(name);
  callback();
}

greet("John", () => {
  console.log("Done");
});
```

The callback is:

```ts
() => {
  console.log("Done");
}
```

### Important:

A callback **does not have to be asynchronous**.

This is a callback:

```ts
[1, 2, 3].map(x => x * 2);
```

The function passed to `map` is a callback.

No Promise is required.

# 8. Callback vs Promise

This directly answers your earlier question.

```text
Callback
→ a function passed to another function

Promise
→ an object representing a future result
```

They are **not the same thing**.

A Promise can use callbacks:

```ts
promise.then(value => {
  console.log(value);
});
```

Here:

```text
Promise
   ↓
.then()
   ↓
callback
```

# 9. Why Promises Exist

Imagine an API:

```ts
getUser((user) => {
  getOrders(user, (orders) => {
    getPayments(orders, (payments) => {
      console.log(payments);
    });
  });
});
```

This can become difficult to manage.

Promises provide a composable abstraction for asynchronous results.

```ts
getUser()
  .then(user => getOrders(user))
  .then(orders => getPayments(orders))
  .then(payments => {
    console.log(payments);
  });
```

# 10. Promise

A Promise represents a result that may be available **now, later, or never successfully**.

A Promise has three states:

```text
              Promise
                 │
        ┌────────┼────────┐
        ↓        ↓        ↓
    Pending   Fulfilled  Rejected
```

### Pending

Still waiting.

### Fulfilled

Successfully completed.

### Rejected

Failed.

Once settled:

```text
Pending
  ↓
Fulfilled
```

or:

```text
Pending
  ↓
Rejected
```

It doesn't go back.

# 11. Creating a Promise

```ts
const promise = new Promise<string>((resolve, reject) => {
  const success = true;

  if (success) {
    resolve("Success");
  } else {
    reject(new Error("Failed"));
  }
});
```

Here:

```text
resolve()
→ fulfillment

reject()
→ rejection
```

# 12. Consuming a Promise

```ts
promise
  .then(value => {
    console.log(value);
  })
  .catch(error => {
    console.error(error);
  });
```

Think:

```text
Promise
   │
   ├── success → .then()
   │
   └── failure → .catch()
```

# 13. `.then()` Returns a Promise ⭐

This is extremely important.

```ts
const result = promise.then(value => {
  return value.toUpperCase();
});
```

`result` is itself a Promise.

Conceptually:

```text
Promise<A>
   ↓
.then()
   ↓
Promise<B>
```

This is why chaining works:

```ts
promise
  .then(...)
  .then(...)
  .then(...);
```

# 14. Promise Chaining

```ts
Promise.resolve(10)
  .then(x => x * 2)
  .then(x => x + 5)
  .then(x => console.log(x));
```

Result:

```text
25
```

Data flows through the chain:

```text
10
 ↓
20
 ↓
25
 ↓
console.log
```

# 15. Returning a Promise From `.then()`

This is where Promise chaining gets interesting.

```ts
getUser()
  .then(user => {
    return getOrders(user);
  })
  .then(orders => {
    console.log(orders);
  });
```

The second `.then()` waits for the Promise returned by the first callback.

Conceptually:

```text
getUser()
   ↓
Promise<User>
   ↓
.then()
   ↓
getOrders()
   ↓
Promise<Order[]>
   ↓
next .then()
```

# 16. Promise Resolution / Adoption

Suppose:

```ts
Promise.resolve()
  .then(() => {
    return Promise.resolve(100);
  })
  .then(value => {
    console.log(value);
  });
```

The second `.then()` receives:

```text
100
```

not the Promise object itself.

The Promise chain **adopts the state/value of the returned Promise**.

This is one of the reasons Promise chains compose nicely.

# 17. `async`

An `async` function **always returns a Promise**.

```ts
async function getValue() {
  return 10;
}
```

This is conceptually similar to:

```ts
function getValue() {
  return Promise.resolve(10);
}
```

So:

```ts
const result = getValue();
```

is a Promise.

Not:

```text
10
```

but:

```text
Promise<number>
```

# 18. `await`

Inside an `async` function:

```ts
const value = await promise;
```

`await` means approximately:

> Pause this async function's continuation until the Promise settles, then resume it with the result/error.

Example:

```ts
async function example() {
  const user = await getUser();

  console.log(user);
}
```

Important:

> `await` does **not block the entire JavaScript thread**.

It suspends the async function's continuation.

# 19. The Mental Model of `await`

Consider:

```ts
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

Execution:

```text
test()
 ↓
"A"
 ↓
await Promise
 ↓
pause test()
 ↓
return control
 ↓
"C"
 ↓
Promise continuation runs later
 ↓
"B"
```

# 20. `await` Is Closely Connected to Promises

This:

```ts
const value = await promise;
```

is conceptually related to:

```ts
promise.then(value => {
  // continue
});
```

Not literally identical in every detail, but useful as a mental model.

So:

```text
async/await
     ↓
Promise abstraction
     ↓
microtask scheduling
```

# 21. Error Handling with `async/await`

Instead of:

```ts
promise
  .then(...)
  .catch(...)
```

you can use:

```ts
async function getData() {
  try {
    const data = await fetchData();
    return data;
  } catch (error) {
    console.error(error);
  }
}
```

Think:

```text
Promise rejection
       ↓
throw-like behavior at await
       ↓
catch
```

# 22. `throw` in an Async Function

```ts
async function test() {
  throw new Error("Oops");
}
```

Does this synchronously throw to the caller?

No.

The async function returns a **rejected Promise**.

Conceptually:

```ts
function test() {
  return Promise.reject(new Error("Oops"));
}
```

# 23. Event Loop ⭐⭐⭐⭐⭐

Now we're at the important part.

Consider:

```ts
console.log("A");

setTimeout(() => {
  console.log("B");
}, 0);

Promise.resolve().then(() => {
  console.log("C");
});

console.log("D");
```

Output:

```text
A
D
C
B
```

Why?

Because there are different kinds of queues.

# 24. Microtasks vs Tasks

The terminology varies by environment/specification, but the important distinction is:

```text
Microtasks
→ Promise reactions
→ queueMicrotask()
→ async/await continuations

Tasks
→ setTimeout callbacks
→ many event callbacks
→ other host-scheduled work
```

After the current synchronous work finishes, the runtime processes pending microtasks before moving on to the next task.

Simplified:

```text
Call Stack
    ↓
Microtask Queue
    ↓
Next Task
```

# 25. The Famous Example

```ts
console.log("1");

setTimeout(() => {
  console.log("2");
}, 0);

Promise.resolve().then(() => {
  console.log("3");
});

console.log("4");
```

Execution:

### Step 1

```text
1
```

### Step 2

Timer is scheduled.

### Step 3

Promise callback is scheduled as a microtask.

### Step 4

```text
4
```

### Step 5

Synchronous code is finished.

Microtasks execute:

```text
3
```

### Step 6

Then the timer callback gets a chance:

```text
2
```

Final:

```text
1
4
3
2
```

# 26. `async/await` and Microtasks

Consider:

```ts
async function test() {
  console.log("A");

  await null;

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

The continuation after `await` is scheduled asynchronously, using Promise-like microtask semantics.

# 27. Nested Microtasks

Now:

```ts
console.log("A");

Promise.resolve().then(() => {
  console.log("B");

  Promise.resolve().then(() => {
    console.log("C");
  });
});

console.log("D");
```

Output:

```text
A
D
B
C
```

Why?

The first microtask runs:

```text
B
```

and schedules another microtask:

```text
C
```

The microtask queue continues draining.

# 28. Microtask Starvation

This can become dangerous.

```ts
function loop() {
  queueMicrotask(loop);
}

loop();
```

You continually create more microtasks.

If the microtask queue never empties, the runtime may not get a chance to proceed to other tasks such as timers or rendering.

Conceptually:

```text
Microtask
 ↓
Microtask
 ↓
Microtask
 ↓
Microtask
 ↓
...
```

This is called **microtask starvation**.

# 29. `Promise.all`

Useful when multiple independent operations can happen concurrently.

```ts
const [user, orders] = await Promise.all([
  getUser(),
  getOrders()
]);
```

Instead of:

```ts
const user = await getUser();
const orders = await getOrders();
```

The second version waits for the first before starting the second if those calls themselves initiate the work at that point.

With `Promise.all`, both promises are created/started before waiting:

```text
getUser()   ──────────┐
                      ├── Promise.all()
getOrders() ──────────┘
                      ↓
                 wait for both
```

# 30. `Promise.all` Fails Fast

```ts
await Promise.all([
  task1(),
  task2(),
  task3()
]);
```

If one rejects, the resulting `Promise.all` rejects.

Important nuance:

> The other operations don't magically get cancelled just because `Promise.all` rejects.

They may continue running unless the underlying operations support cancellation and you implement it.

# 31. `Promise.allSettled`

Use when you want **every result**, regardless of success/failure.

```ts
const results = await Promise.allSettled([
  task1(),
  task2(),
  task3()
]);
```

You'll get results like:

```text
fulfilled
rejected
fulfilled
```

Useful when one failure shouldn't discard the other outcomes.

# 32. `Promise.race`

Returns the first Promise to settle:

```ts
await Promise.race([
  request(),
  timeout()
]);
```

The first settled result wins:

```text
request ────────────────┐
                        ↓
                     winner
                        ↑
timeout ────────────────┘
```

Again, the losing operation isn't automatically cancelled.

# 33. `Promise.any`

Returns the first **fulfilled** Promise.

```ts
await Promise.any([
  server1(),
  server2(),
  server3()
]);
```

If:

```text
server1 → reject
server2 → reject
server3 → fulfill
```

you get server3's value.

If all reject, `Promise.any()` rejects with an `AggregateError`.

# 34. Promise Methods — Quick Table

| Method                 | Resolves when                           |
| ---------------------- | --------------------------------------- |
| `Promise.all()`        | All fulfill; rejects on first rejection |
| `Promise.allSettled()` | All settle                              |
| `Promise.race()`       | First settles                           |
| `Promise.any()`        | First fulfills                          |

Memory trick:

```text
all        → everyone
allSettled → everyone finished
race       → first result
any        → first success
```

# 35. Sequential vs Concurrent

This is an important real-world distinction.

### Sequential

```ts
const a = await taskA();
const b = await taskB();
const c = await taskC();
```

Potentially:

```text
A ────→
       B ────→
              C ────→
```

### Concurrent initiation

```ts
const promiseA = taskA();
const promiseB = taskB();
const promiseC = taskC();

const [a, b, c] = await Promise.all([
  promiseA,
  promiseB,
  promiseC
]);
```

Potentially:

```text
A ───────────→
B ───────→
C ─────────────→
```

This distinction matters a lot for performance.

# 36. Don't Do This Accidentally

Suppose the tasks don't depend on each other:

```ts
const user = await getUser();
const products = await getProducts();
const settings = await getSettings();
```

You're potentially unnecessarily serializing them.

Instead:

```ts
const userPromise = getUser();
const productsPromise = getProducts();
const settingsPromise = getSettings();

const [user, products, settings] =
  await Promise.all([
    userPromise,
    productsPromise,
    settingsPromise
  ]);
```

# 37. But Don't Use `Promise.all` Blindly

If task B depends on task A:

```ts
const user = await getUser();

const orders = await getOrders(user.id);
```

You cannot simply parallelize them because you don't have `user.id` yet.

Dependency matters.

# 38. The Full Runtime Picture

Now combine everything we've learned:

```text
                 JavaScript Code
                       │
                       ↓
                 Call Stack
                       │
              synchronous work
                       │
            ┌──────────┴──────────┐
            ↓                     ↓
       Host APIs              Promise
       / timers                  │
            │                    ↓
            │              Microtask Queue
            │                    │
            ↓                    │
       Task Queue                │
            │                    │
            └─────────┬──────────┘
                      ↓
                  Event Loop
                      ↓
                 Call Stack
```

# 🧠 The Most Important Distinctions

| Concept              | Meaning                                                     |
| -------------------- | ----------------------------------------------------------- |
| Callback             | Function passed to another function                         |
| Promise              | Represents eventual completion/result                       |
| `async`              | Makes a function return a Promise                           |
| `await`              | Suspends an async function's continuation until settlement  |
| Call Stack           | Tracks currently executing JS                               |
| Event Loop           | Coordinates when queued work gets executed                  |
| Microtask            | Higher-priority queued continuation, e.g. Promise reactions |
| Task                 | Host-scheduled work such as timers/events                   |
| `Promise.all`        | Wait for all, reject if one rejects                         |
| `Promise.allSettled` | Wait for all regardless of outcome                          |
| `Promise.race`       | First settlement wins                                       |
| `Promise.any`        | First fulfillment wins                                      |

# 🔥 The One Mental Model to Remember

When you see:

```ts
async function getData() {
  console.log("A");

  const data = await fetchData();

  console.log("B");

  return data;
}
```

Think:

```text
getData()
   ↓
"A"
   ↓
fetchData()
   ↓
Promise
   ↓
await
   ↓
getData pauses
   ↓
JavaScript can do other work
   ↓
Promise settles
   ↓
continuation scheduled
   ↓
microtask
   ↓
getData resumes
   ↓
"B"
   ↓
return value
   ↓
async function's Promise fulfills
```

That's the **real mental model**.

# 🔴 Deep Questions to Test Yourself

Before moving on, you should be able to explain these without memorizing.

### Level 1

1. What is a Promise?
2. What are the three Promise states?
3. What does `async` do?
4. What does `await` do?
5. What is the event loop?
6. What is the call stack?
7. What is a microtask?
8. What is a task?

### Level 2

9. Why does this print `A C B`?

```ts
console.log("A");

setTimeout(() => console.log("B"), 0);

console.log("C");
```

10. Why does this print `A C B`?

```ts
async function test() {
  console.log("A");
  await Promise.resolve();
  console.log("B");
}

test();
console.log("C");
```

11. Why does `.then()` return another Promise?

12. What's the difference between `Promise.all()` and `Promise.allSettled()`?

### Level 3 🔴

13. Predict the exact output:

```ts
console.log("1");

setTimeout(() => {
  console.log("2");
}, 0);

Promise.resolve().then(() => {
  console.log("3");

  Promise.resolve().then(() => {
    console.log("4");
  });
});

console.log("5");
```

14. Predict:

```ts
async function foo() {
  console.log("A");

  await Promise.resolve();

  console.log("B");

  await Promise.resolve();

  console.log("C");
}

console.log("D");

foo();

console.log("E");
```

15. Explain the difference between:

```ts
const a = await taskA();
const b = await taskB();
```

and:

```ts
const aPromise = taskA();
const bPromise = taskB();

const [a, b] = await Promise.all([
  aPromise,
  bPromise
]);
```

16. Given:

```ts
console.log("A");

setTimeout(() => console.log("B"), 0);

Promise.resolve().then(() => console.log("C"));

queueMicrotask(() => console.log("D"));

console.log("E");
```

Predict the output and explain **every transition between stack → microtask queue → task queue**.

## 🚀 Next deep layer

We've now covered the **runtime + asynchronous foundation**:

```text
Scope
 ↓
Closures
 ↓
this
 ↓
Prototypes
 ↓
Call Stack
 ↓
Callbacks
 ↓
Promises
 ↓
async/await
 ↓
Microtasks
 ↓
Tasks
 ↓
Event Loop
```

The next major branch is **TypeScript's type system**, where we'll go deep into:

**`any` → `unknown` → `never` → unions → intersections → narrowing → type guards → generics → `keyof` → `typeof` → mapped types → conditional types → `infer` → utility types.**
