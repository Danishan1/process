# 15. Asynchronous JavaScript

This is one of the **most important JavaScript topics for an SDE-II interview**, especially because you're working with Node.js.

The key is not memorizing `Promise.all()` or `async/await`. You should understand the **execution model underneath them**.

The mental model we'll build is:

```text
JavaScript execution
       │
       ├── Synchronous work
       │      └── executes immediately
       │
       └── Asynchronous work
              │
              ├── Callback
              ├── Promise
              └── async/await
```

And one crucial distinction:

```text
Asynchronous ≠ Non-blocking
```

They are related, but **not the same concept**.

---

# 1. Synchronous vs Asynchronous

### Synchronous

Operations execute one after another.

```js
console.log("A");
console.log("B");
console.log("C");
```

Output:

```text
A
B
C
```

Conceptually:

```text
A
↓
B
↓
C
```

Each operation has to finish before the next one starts.

---

### Asynchronous

An operation can be initiated now, while its completion is handled later.

```js
console.log("A");

setTimeout(() => {
    console.log("B");
}, 1000);

console.log("C");
```

Output:

```text
A
C
B
```

The important point is:

> `setTimeout` does not make JavaScript execute two pieces of JavaScript simultaneously.

Instead, it allows the current execution to continue while the asynchronous operation is waiting.

---

# 2. Blocking vs Non-Blocking

This is a different axis.

### Blocking

The current execution cannot continue until the operation finishes.

```text
JavaScript
   │
   ▼
blocking operation
   │
   │ waiting
   │
   ▼
continue
```

### Non-blocking

The operation is initiated and JavaScript can continue doing other work.

```text
JavaScript
   │
   ├── start operation
   │
   ├── continue other work
   │
   └── handle result later
```

In Node.js, this distinction is particularly important.

For example:

```js
const fs = require("fs");

const data = fs.readFileSync("large.txt");

console.log("Done");
```

`readFileSync()` blocks the current thread.

Whereas:

```js
fs.readFile("large.txt", (err, data) => {
    console.log("Done");
});

console.log("Continue");
```

allows the JavaScript execution to continue while the I/O operation is handled asynchronously.

---

# 3. Callback

A callback is simply a function passed to another function to be called later or when an operation completes.

```js
function fetchUser(callback) {
    setTimeout(() => {
        callback({ id: 1, name: "Danishan" });
    }, 1000);
}

fetchUser(user => {
    console.log(user);
});
```

Mental model:

```text
fetchUser()
     │
     ├── start operation
     │
     └── callback registered
             │
             ▼
        operation completes
             │
             ▼
        callback(user)
```

Callbacks were the original dominant pattern for asynchronous JavaScript APIs.

---

# 4. Callback-Based APIs

Node.js historically uses the **error-first callback pattern**:

```js
fs.readFile("file.txt", (err, data) => {
    if (err) {
        console.error(err);
        return;
    }

    console.log(data);
});
```

The convention is:

```text
callback(error, result)
```

So:

```js
(err, data) => {}
```

means:

```text
err  → failure
data → success
```

This is a convention, not a special JavaScript syntax.

---

# 5. Callback Nesting

Suppose you need:

```text
Get user
   ↓
Get orders
   ↓
Get payment
   ↓
Send email
```

With callbacks:

```js
getUser((err, user) => {
    if (err) return handleError(err);

    getOrders(user.id, (err, orders) => {
        if (err) return handleError(err);

        getPayment(orders[0].id, (err, payment) => {
            if (err) return handleError(err);

            sendEmail(payment, (err) => {
                if (err) return handleError(err);

                console.log("Done");
            });
        });
    });
});
```

Now the control flow becomes deeply nested.

---

# 6. Callback Hell

Callback hell isn't simply "many callbacks."

The real problem is **deeply nested asynchronous control flow that becomes difficult to reason about, maintain, and handle errors in**.

```text
getUser
   │
   └── getOrders
          │
          └── getPayment
                 │
                 └── sendEmail
                        │
                        └── ...
```

Typical problems:

* deeply nested code
* duplicated error handling
* difficult control flow
* difficult composition
* difficult parallel execution

Promises were introduced partly to improve this model.

---

# 7. Promises

A Promise represents the eventual result of an asynchronous operation.

Think:

```text
Promise
   │
   ├── pending
   ├── fulfilled
   └── rejected
```

For example:

```js
const promise = fetchUser();
```

At that moment you may not have the user yet.

You have a **Promise representing the future result**.

---

# 8. Promise States

A Promise has three conceptual states:

```text
             pending
            /       \
           /         \
          ▼           ▼
     fulfilled     rejected
```

### Pending

Operation hasn't completed.

### Fulfilled

Operation completed successfully.

### Rejected

Operation failed.

Once settled:

```text
pending → fulfilled
```

or:

```text
pending → rejected
```

It does not go backward.

And a Promise can settle only once.

---

# 9. Promise Creation

You can create a Promise manually:

```js
const promise = new Promise((resolve, reject) => {
    // asynchronous operation

    if (success) {
        resolve("Success");
    } else {
        reject(new Error("Failed"));
    }
});
```

The executor receives:

```text
resolve
reject
```

Calling:

```js
resolve(value);
```

fulfills the Promise.

Calling:

```js
reject(error);
```

rejects it.

---

# 10. Promise Resolution

There is an important subtlety here.

We often simplify:

```js
resolve(value)
```

as:

```text
Promise becomes fulfilled with value
```

But Promise **resolution** is slightly more general.

For example:

```js
const p = new Promise(resolve => {
    resolve(Promise.resolve(10));
});
```

The outer Promise adopts the state of the Promise passed to `resolve`.

So conceptually:

```text
resolve(value)
       │
       ├── normal value → fulfill
       │
       └── Promise/thenable → adopt its eventual state
```

This distinction becomes useful in deeper Promise questions.

---

# 11. Promise Rejection

```js
const promise = new Promise((resolve, reject) => {
    reject(new Error("Database failed"));
});
```

Handle it:

```js
promise.catch(error => {
    console.log(error.message);
});
```

A rejection is the Promise equivalent of an asynchronous failure signal.

---

# 12. `.then()`

`.then()` handles successful fulfillment.

```js
fetchUser()
    .then(user => {
        console.log(user);
    });
```

But the most important thing about `.then()` is:

> **It returns another Promise.**

For example:

```js
fetchUser()
    .then(user => {
        return fetchOrders(user.id);
    })
    .then(orders => {
        console.log(orders);
    });
```

This creates a chain:

```text
Promise<User>
     │
    then
     ↓
Promise<Orders>
     │
    then
     ↓
result
```

---

# 13. Promise Chaining

Suppose:

```js
function getUser() {
    return Promise.resolve({ id: 1 });
}

function getOrders(userId) {
    return Promise.resolve(["order1", "order2"]);
}
```

Then:

```js
getUser()
    .then(user => {
        return getOrders(user.id);
    })
    .then(orders => {
        console.log(orders);
    })
    .catch(error => {
        console.error(error);
    });
```

The key rule:

### Returning a normal value

```js
.then(() => {
    return 10;
})
```

produces a fulfilled Promise with:

```text
10
```

### Returning a Promise

```js
.then(() => {
    return somePromise();
})
```

causes the next `.then()` to wait for that Promise.

### Throwing

```js
.then(() => {
    throw new Error("Failed");
})
```

turns the returned Promise into a rejection.

This is fundamental to Promise composition.

---

# 14. `.catch()`

`.catch()` handles rejection.

```js
fetchUser()
    .then(user => processUser(user))
    .catch(error => {
        console.error(error);
    });
```

You can think of:

```js
.catch(handler)
```

roughly as:

```js
.then(undefined, handler)
```

But `.catch()` is much clearer semantically.

---

# 15. `.finally()`

`finally()` executes after settlement regardless of success or failure.

```js
fetchUser()
    .then(user => {
        console.log(user);
    })
    .catch(error => {
        console.error(error);
    })
    .finally(() => {
        console.log("Cleanup");
    });
```

Typical use:

```text
request
   │
   ├── success ──┐
   │             │
   └── failure ──┤
                 ▼
              finally
```

Examples:

* hide loading state
* release resources
* unlock something
* cleanup temporary state

---

# 16. `async`

An `async` function **always returns a Promise**.

```js
async function getUser() {
    return { id: 1 };
}
```

Even though you wrote:

```js
return { id: 1 };
```

the caller receives:

```text
Promise<{ id: 1 }>
```

Conceptually:

```js
async function getUser() {
    return 10;
}
```

behaves like:

```js
function getUser() {
    return Promise.resolve(10);
}
```

for the return-value behavior.

---

# 17. `await`

`await` waits for a Promise's result **inside an async function**.

```js
async function process() {
    const user = await getUser();

    console.log(user);
}
```

Conceptually:

```text
getUser()
    │
    ▼
 Promise
    │
    │ await
    ▼
user
```

But don't interpret `await` as:

> "Block the entire JavaScript runtime."

That's incorrect.

It suspends the execution of **that async function**, allowing other work to proceed.

This distinction is extremely important.

---

# 18. `await` Does Not Block the Event Loop

Consider:

```js
async function task() {
    console.log("A");

    await something();

    console.log("B");
}

console.log("C");

task();

console.log("D");
```

The async function reaches:

```js
await something();
```

and suspends there.

The rest of the program can continue.

So:

```text
task()
 │
 ├── A
 │
 └── await
       │
       └── function suspended
              │
              ▼
         C / D / other work
              │
              ▼
        Promise settles
              │
              ▼
             B
```

This is one of the most important mental models for Node.js.

---

# 19. Async Functions

Putting it together:

```js
async function getUserAndOrders() {
    const user = await getUser();

    const orders = await getOrders(user.id);

    return orders;
}
```

Errors can be handled naturally:

```js
try {
    const orders = await getUserAndOrders();
} catch (error) {
    console.error(error);
}
```

As discussed in the previous section, an error thrown inside an `async` function results in a **rejected Promise**.

---

# 20. Sequential vs Concurrent Operations

This is one of the **most important practical SDE-II concepts** in this section.

Suppose:

```js
const user = await getUser();
const orders = await getOrders();
const recommendations = await getRecommendations();
```

These execute sequentially:

```text
getUser
  │
  ▼
getOrders
  │
  ▼
getRecommendations
```

If each takes 1 second:

```text
≈ 3 seconds
```

assuming they truly don't overlap and ignoring overhead.

But what if the operations are independent?

```js
const userPromise = getUser();
const ordersPromise = getOrders();
const recommendationsPromise = getRecommendations();

const user = await userPromise;
const orders = await ordersPromise;
const recommendations = await recommendationsPromise;
```

Now they can run concurrently:

```text
getUser           ────────┐
getOrders         ────────┤
getRecommendations ───────┘
                           ↓
                         results
```

Or more cleanly:

```js
const [user, orders, recommendations] =
    await Promise.all([
        getUser(),
        getOrders(),
        getRecommendations()
    ]);
```

This is a major performance distinction.

---

# 21. `Promise.all()`

`Promise.all()` is used when:

> **I need all operations to succeed.**

```js
const [user, orders, products] = await Promise.all([
    getUser(),
    getOrders(),
    getProducts()
]);
```

Conceptually:

```text
           Promise.all
          /     |      \
         /      |       \
      user    orders   products
         \      |       /
          \     |      /
             results
```

If all succeed:

```text
fulfilled
```

If one rejects:

```text
rejected
```

Example:

```js
await Promise.all([
    Promise.resolve("A"),
    Promise.reject(new Error("B")),
    Promise.resolve("C")
]);
```

The combined Promise rejects.

### Important

`Promise.all()` **doesn't cancel the other underlying operations** when one rejects.

That's a common interview trap.

---

# 22. `Promise.allSettled()`

Use this when:

> **I want the result of every operation, regardless of success or failure.**

```js
const results = await Promise.allSettled([
    getUser(),
    getOrders(),
    getRecommendations()
]);
```

You get something like:

```js
[
    {
        status: "fulfilled",
        value: user
    },
    {
        status: "rejected",
        reason: error
    },
    {
        status: "fulfilled",
        value: recommendations
    }
]
```

This is useful when operations are independent.

For example:

```text
Send email
Send SMS
Push notification
```

You might want to know which ones succeeded without making one failure discard the others.

---

# 23. `Promise.race()`

`Promise.race()` settles when the **first Promise settles**.

Important:

> First **settled**, not first successful.

```js
const result = await Promise.race([
    requestA(),
    requestB()
]);
```

If:

```text
A → fulfilled after 2s
B → rejected after 1s
```

then:

```text
race → rejected after 1s
```

because rejection is also settlement.

Mental model:

```text
A ──────────────── success

B ───── failure
       │
       ▼
     race
```

---

# 24. `Promise.any()`

`Promise.any()` is different from `race()`.

It waits for the **first successful fulfillment**.

```js
const result = await Promise.any([
    serverA(),
    serverB(),
    serverC()
]);
```

Suppose:

```text
A → rejected at 100ms
B → fulfilled at 500ms
C → fulfilled at 700ms
```

Result:

```text
B
```

The first rejection doesn't matter.

It only rejects if **all Promises reject**.

Then you get an `AggregateError`.

```text
             Promise.any
          /       |       \
         A        B        C
       reject   reject   reject
          \       |       /
              AggregateError
```

---

# 25. The Four Promise Combinators

This table is worth memorizing:

| Method                 | Resolves when      | Rejects when                             |
| ---------------------- | ------------------ | ---------------------------------------- |
| `Promise.all()`        | **all fulfill**    | **one rejects**                          |
| `Promise.allSettled()` | **all settle**     | practically never due to input rejection |
| `Promise.race()`       | **first settles**  | first settlement is rejection            |
| `Promise.any()`        | **first fulfills** | **all reject**                           |

The easiest mental model:

```text
all
→ I need EVERYONE

allSettled
→ Tell me what happened to EVERYONE

race
→ Give me whoever FINISHES FIRST

any
→ Give me whoever SUCCEEDS FIRST
```

---

# 26. A Very Common Performance Bug

Consider:

```js
const user = await getUser();
const settings = await getSettings();
const permissions = await getPermissions();
```

If these don't depend on each other, this unnecessarily serializes them.

Better:

```js
const [user, settings, permissions] = await Promise.all([
    getUser(),
    getSettings(),
    getPermissions()
]);
```

But **don't blindly use `Promise.all()`**.

If:

```js
const user = await getUser();
const orders = await getOrders(user.id);
```

then:

```text
orders depends on user
```

So sequential execution is necessary.

```text
getUser
   ↓
user.id
   ↓
getOrders
```

The optimization rule is:

> **Parallelize independent work; sequence dependent work.**

---

# 27. Another Important Trap

Don't do this:

```js
const p1 = await request1();
const p2 = await request2();

const result = await Promise.all([p1, p2]);
```

You've already waited for both operations.

The concurrency opportunity was lost.

Instead:

```js
const p1 = request1();
const p2 = request2();

const [result1, result2] = await Promise.all([p1, p2]);
```

---

# 28. Complete Async Mental Model

For your revision, keep this architecture in your head:

```text
                 ASYNCHRONOUS JAVASCRIPT
                          │
             ┌────────────┴────────────┐
             │                         │
        Callback                    Promise
             │                         │
       callback hell            ┌──────┴──────┐
                                 │             │
                               then          async/await
                                 │             │
                               catch          try/catch
                                 │
                              finally
                                 │
                     ┌───────────┴───────────┐
                     │                       │
              Sequential                 Concurrent
                                             │
                              ┌──────────────┼──────────────┐
                              │              │              │
                           all()        allSettled()      race()
                              │
                           any()
```

And underneath all of this:

```text
JavaScript execution
       │
       ▼
Current synchronous work
       │
       ▼
Asynchronous operation
       │
       ▼
Promise/callback completion
       │
       ▼
Continuation executes later
```

---

# 29. SDE-II Interview Traps You Should Be Able to Explain

You should be comfortable answering these without memorization:

### Q1. Is asynchronous code always non-blocking?

**No.** Asynchronous and blocking/non-blocking describe different properties.

### Q2. Does `await` block Node.js?

**No.** It suspends the current async function; it doesn't block the event loop.

### Q3. Why is this slow?

```js
await a();
await b();
await c();
```

Because you're serializing the operations.

### Q4. How do you make independent operations concurrent?

```js
await Promise.all([a(), b(), c()]);
```

### Q5. Does `Promise.all()` cancel remaining operations after one rejects?

**No.** The combined Promise rejects, but already-started underlying operations aren't automatically cancelled.

### Q6. Difference between `race()` and `any()`?

```text
race → first settlement
any  → first fulfillment
```

### Q7. Difference between `all()` and `allSettled()`?

```text
all        → fail if one fails
allSettled → report every result
```

### Q8. Why doesn't this catch the error?

```js
try {
    setTimeout(() => {
        throw new Error("Failed");
    }, 1000);
} catch (error) {
    // doesn't catch it
}
```

Because the callback executes after the synchronous `try` block has already completed.

---

## The one mental model I want you to retain

Don't think:

> **"JavaScript executes asynchronous code in parallel."**

Think:

> **"JavaScript can start an operation, continue executing other work, and later resume the relevant continuation when the asynchronous operation completes."**

Then build everything on top of that:

```text
                 START OPERATION
                       │
                       ▼
                Continue execution
                       │
                       ▼
              operation completes
                       │
                       ▼
             schedule continuation
                       │
                       ▼
                 run callback /
                 Promise handler /
                     await
```

Once this is solid, **Promises, `async/await`, concurrency, error propagation, and the Promise combinators become much easier to reason about.**
