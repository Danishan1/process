# 14. Error Handling

This section is about **how JavaScript represents failures, how failures move through the program, and how we decide whether to recover or terminate**.

For SDE-II interviews, don't treat error handling as just `try/catch`. The important part is understanding **error propagation across synchronous code, callbacks, Promises, and `async/await`**.

---

## 1. The Core Mental Model

Think of an error as something that can **interrupt normal execution and travel upward until something handles it**.

```text
Normal execution
      │
      ▼
   operation
      │
      │ failure
      ▼
    Error
      │
      ▼
propagate upward
      │
      ├── handled → recover / respond
      │
      └── unhandled → program/request fails
```

For example:

```js
function databaseCall() {
    throw new Error("Database failed");
}

function service() {
    databaseCall();
}

function controller() {
    service();
}

try {
    controller();
} catch (error) {
    console.log("Handled:", error.message);
}
```

The error travels:

```text
controller()
   ↓
service()
   ↓
databaseCall()
   ↓
throw Error
   ↓
service()
   ↓
controller()
   ↓
try/catch
```

This is **error propagation**.

---

# 2. `Error`

`Error` is the fundamental JavaScript error object.

```js
const error = new Error("Something went wrong");

console.log(error.message);
console.log(error.name);
console.log(error.stack);
```

Typically:

```text
name
message
stack
```

Example:

```js
console.log(error.name);
// Error

console.log(error.message);
// Something went wrong
```

The important thing is that an error is an **object**.

You can also attach additional information:

```js
const error = new Error("Payment failed");

error.code = "PAYMENT_FAILED";
error.orderId = 123;

throw error;
```

This becomes particularly useful in backend systems.

---

# 3. Built-in Error Types

JavaScript provides several standard error classes.

The important ones are:

```text
Error
├── TypeError
├── ReferenceError
├── SyntaxError
├── RangeError
├── URIError
├── EvalError
└── AggregateError
```

### TypeError

You performed an operation on an inappropriate value.

```js
const user = null;

user.name;
```

Usually:

```text
TypeError
```

Another example:

```js
const x = 10;

x();
```

---

### ReferenceError

You reference something that doesn't exist.

```js
console.log(username);
```

If `username` isn't defined:

```text
ReferenceError
```

---

### SyntaxError

The JavaScript itself is syntactically invalid.

```js
const = 10;
```

Generally, this occurs during parsing before normal execution.

---

### RangeError

A value is outside an allowed range.

```js
const arr = new Array(-1);
```

---

### AggregateError

Represents multiple errors together.

Commonly encountered with:

```js
Promise.any()
```

when all supplied promises reject.

```js
try {
    await Promise.any([
        Promise.reject(new Error("A")),
        Promise.reject(new Error("B"))
    ]);
} catch (error) {
    console.log(error instanceof AggregateError);
    console.log(error.errors);
}
```

---

# 4. Custom Errors

In real applications, built-in errors often aren't expressive enough.

We can create our own error types.

```js
class ValidationError extends Error {
    constructor(message) {
        super(message);
        this.name = "ValidationError";
    }
}
```

Now:

```js
throw new ValidationError("Email is required");
```

We can distinguish it:

```js
try {
    throw new ValidationError("Invalid email");
} catch (error) {
    if (error instanceof ValidationError) {
        console.log("Validation problem");
    }
}
```

For backend applications, you might have:

```text
AppError
├── ValidationError
├── AuthenticationError
├── AuthorizationError
├── NotFoundError
└── DatabaseError
```

This lets higher layers make decisions based on **error type**, rather than parsing error messages.

---

# 5. `throw`

`throw` explicitly signals a failure.

```js
throw new Error("Something failed");
```

But technically JavaScript allows throwing almost anything:

```js
throw "Something failed";
```

or:

```js
throw 123;
```

or:

```js
throw { message: "Failed" };
```

Don't do this in production code.

Prefer:

```js
throw new Error("Something failed");
```

or a custom `Error` subclass.

### Why?

Because you want:

```js
error.message
error.stack
error.name
error instanceof Error
```

and predictable error handling.

---

# 6. `try / catch`

`try` contains code that may fail.

```js
try {
    riskyOperation();
} catch (error) {
    console.log(error.message);
}
```

If an exception occurs:

```text
try
 │
 ├── operation 1
 ├── operation 2
 ├── operation 3 ← throws
 │
 ▼
catch
```

Execution immediately leaves the `try` block.

```js
try {
    console.log("A");

    throw new Error("Failure");

    console.log("B");
} catch (error) {
    console.log("C");
}
```

Output:

```text
A
C
```

`B` never executes.

---

# 7. `finally`

`finally` executes regardless of whether an error occurred.

```js
try {
    riskyOperation();
} catch (error) {
    console.log("Error");
} finally {
    console.log("Cleanup");
}
```

Think:

```text
try
 │
 ├── success ──────┐
 │                 │
 └── failure ──────┤
                   ▼
                finally
```

Typical use:

```js
const connection = acquireConnection();

try {
    doSomething(connection);
} catch (error) {
    handle(error);
} finally {
    connection.close();
}
```

The key production use is **cleanup**:

* database connections
* file handles
* locks
* temporary resources
* transactions

---

# 8. Error Propagation

This is one of the most important concepts.

Consider:

```js
function a() {
    throw new Error("Failed");
}

function b() {
    a();
}

function c() {
    b();
}

c();
```

The error moves upward through the call stack:

```text
c()
 ↓
b()
 ↓
a()
 ↓
throw
 ↑
b()
 ↑
c()
 ↑
uncaught
```

If nobody catches it, it becomes an **uncaught exception**.

But:

```js
function c() {
    try {
        b();
    } catch (error) {
        console.log("Handled");
    }
}
```

Now propagation stops at `c()`.

---

# 9. Stack Traces

A stack trace tells you **where the error came from and how execution reached that point**.

```js
function a() {
    throw new Error("Database failed");
}

function b() {
    a();
}

function c() {
    b();
}

c();
```

The stack roughly tells you:

```text
Error: Database failed
    at a (...)
    at b (...)
    at c (...)
```

This is extremely important for production debugging.

Think of it as:

> **"What was the execution path that led to this failure?"**

---

# 10. Synchronous Errors

Synchronous errors happen during the current execution flow.

```js
function divide() {
    throw new Error("Invalid operation");
}

try {
    divide();
} catch (error) {
    console.log("Caught");
}
```

Straightforward:

```text
throw
 ↓
call stack
 ↓
catch
```

---

# 11. Asynchronous Errors

This is where JavaScript interviews become more interesting.

Consider:

```js
try {
    setTimeout(() => {
        throw new Error("Failed");
    }, 1000);
} catch (error) {
    console.log("Caught");
}
```

Many developers expect:

```text
Caught
```

But that **doesn't happen**.

Why?

Because the `try` block has already finished before the callback executes.

```text
try
 │
 └── schedule callback
       │
       ▼
try finishes
       │
       ▼
1 second later
       │
       ▼
callback executes
       │
       ▼
throw
```

The `catch` belongs to the **synchronous execution of the `try` block**, not to arbitrary future callbacks.

This distinction is extremely important.

---

# 12. Promise Rejection

Promises have their own failure mechanism.

```js
const promise = Promise.reject(
    new Error("Request failed")
);
```

A rejected Promise isn't the same thing as a synchronous `throw` that automatically jumps into an unrelated `catch`.

You handle it with:

```js
promise.catch(error => {
    console.log(error.message);
});
```

Or:

```js
try {
    await promise;
} catch (error) {
    console.log(error.message);
}
```

---

# 13. `async/await` Makes Async Errors Look Synchronous

This is one of the most useful things to understand.

```js
async function getUser() {
    throw new Error("User not found");
}
```

Calling:

```js
getUser();
```

doesn't synchronously throw to the caller.

Instead, the `async` function returns a **rejected Promise**.

Conceptually:

```text
async function
      │
      ▼
returns Promise
      │
      ▼
throw inside async function
      │
      ▼
Promise rejection
```

Therefore:

```js
try {
    await getUser();
} catch (error) {
    console.log("Caught");
}
```

works.

This is one of the most important distinctions:

```text
Synchronous throw
      ↓
call stack propagation

throw inside async function
      ↓
Promise rejection
```

---

# 14. Error Propagation with Promises

Promise chains propagate rejection.

```js
fetchUser()
    .then(user => processUser(user))
    .then(result => save(result))
    .catch(error => {
        console.log("Something failed");
    });
```

If any of these rejects:

```text
fetchUser()
    ↓
processUser()
    ↓
save()
    ↓
reject
    ↓
catch()
```

The rejection travels down the Promise chain until handled.

---

# 15. `async/await` Error Propagation

The same idea becomes:

```js
async function process() {
    const user = await fetchUser();

    const result = await processUser(user);

    await save(result);
}
```

If `save()` rejects:

```text
save()
 ↓
Promise rejection
 ↓
await
 ↓
process() rejects
 ↓
caller handles it
```

So:

```js
try {
    await process();
} catch (error) {
    console.log(error);
}
```

can handle failures from the whole operation.

---

# 16. A Critical SDE-II Concept: Error Boundaries

You generally don't want every function doing this:

```js
try {
    // something
} catch (error) {
    console.log(error);
}
```

That's often bad architecture.

Instead, errors should propagate until a layer that **knows what to do with them**.

For example:

```text
Database
   ↓
Repository
   ↓
Service
   ↓
Controller
   ↓
Global Error Handler
```

Imagine:

```js
async function createUser(data) {
    return db.users.insert(data);
}
```

The service doesn't necessarily need:

```js
try {
    ...
} catch {
    ...
}
```

if it cannot meaningfully recover.

Let it propagate:

```text
DB error
 ↓
service
 ↓
controller
 ↓
global error middleware
 ↓
HTTP response
```

The boundary that understands the context handles it.

---

# 17. Error Handling Strategies

There isn't one universal strategy.

The right approach depends on the failure.

### Strategy 1 — Recover

If the problem is temporary:

```js
try {
    return await fetchFromCache();
} catch {
    return await fetchFromDatabase();
}
```

---

### Strategy 2 — Retry

For transient failures:

```text
request
   ↓
failure
   ↓
retry
   ↓
failure
   ↓
retry
   ↓
success
```

But retries should generally involve:

* bounded attempts
* backoff
* jitter
* idempotency considerations

Otherwise you can make an outage worse.

---

### Strategy 3 — Fallback

```js
try {
    return await primaryService();
} catch {
    return await fallbackService();
}
```

---

### Strategy 4 — Propagate

If the current layer cannot recover:

```js
async function service() {
    return await repository.getUser();
}
```

Let the error move upward.

---

### Strategy 5 — Convert

Sometimes you don't want to expose a low-level error directly.

For example:

```text
PostgreSQL error
       ↓
repository
       ↓
DatabaseError
       ↓
service
       ↓
UserCreationError
       ↓
HTTP 500
```

This creates cleaner abstraction boundaries.

---

# 18. Fail-Fast vs Recovery

This distinction is especially important at SDE-II level.

## Fail-fast

Stop immediately when continuing would be unsafe or meaningless.

Example:

```js
if (!config.DATABASE_URL) {
    throw new Error("DATABASE_URL is missing");
}
```

Don't start an application with a fundamentally invalid configuration.

```text
invalid configuration
        ↓
fail immediately
        ↓
application doesn't start
```

---

## Recovery

Continue using an alternative path when possible.

Example:

```js
try {
    return await redis.get(key);
} catch {
    return await database.get(key);
}
```

Here Redis is an optimization/cache, so failure doesn't necessarily mean the entire request must fail.

```text
Redis
  ↓ failure
Database
  ↓
continue
```

---

# 19. The Important Question: "Should I Catch It?"

A useful SDE-II decision framework:

```text
Error occurs
    │
    ▼
Can this layer recover?
    │
 ┌──┴──┐
Yes    No
 │      │
 ▼      ▼
recover propagate
 │      │
 ▼      ▼
continue higher layer
```

Don't catch an error merely because you can.

Ask:

> **Does this layer have enough context to make a correct decision?**

If not, propagate it.

---

# 20. Common Interview Traps

### Trap 1

```js
try {
    setTimeout(() => {
        throw new Error("Failed");
    }, 1000);
} catch (e) {
    console.log("Caught");
}
```

❌ The `catch` doesn't catch the asynchronous callback error.

---

### Trap 2

```js
async function test() {
    throw new Error("Failed");
}

try {
    test();
} catch (e) {
    console.log("Caught");
}
```

❌ This doesn't catch the rejection.

Correct:

```js
try {
    await test();
} catch (e) {
    console.log("Caught");
}
```

or:

```js
test().catch(console.error);
```

---

### Trap 3

Catching and swallowing:

```js
try {
    await saveUser();
} catch (error) {
    console.log(error);
}
```

Now the caller may believe the operation succeeded.

Sometimes that's a serious bug.

---

### Trap 4

Throwing strings:

```js
throw "failed";
```

Prefer:

```js
throw new Error("failed");
```

---

### Trap 5

Using error messages as types:

```js
if (error.message === "User not found") {
    ...
}
```

Prefer structured/custom errors:

```js
if (error instanceof NotFoundError) {
    ...
}
```

---

# 21. The Complete Mental Model

For your SDE-II revision, remember this:

```text
                     ERROR HANDLING
                           │
             ┌─────────────┴─────────────┐
             │                           │
         Error object              Error propagation
             │                           │
      ┌──────┴──────┐             ┌─────┴─────┐
      │             │             │           │
 Built-in       Custom          Sync        Async
 errors         errors           │           │
                                  │      Promise rejection
                                  │           │
                              try/catch    .catch()
                                  │           │
                              finally      await
                                  │           │
                                  └─────┬─────┘
                                        │
                                 Error boundary
                                        │
                            ┌───────────┴───────────┐
                            │                       │
                         Recover                 Propagate
                            │                       │
                     retry/fallback            higher layer
```

### The interview-level distinction to internalize

**Synchronous:**

```text
throw
 ↓
call stack
 ↓
catch
```

**Promise-based asynchronous:**

```text
throw/reject
 ↓
Promise rejection
 ↓
.catch / await + try/catch
```

And architecturally:

```text
LOW LEVEL
   │
   │ error
   ▼
SERVICE
   │
   │ error
   ▼
CONTROLLER
   │
   ▼
ERROR BOUNDARY
   │
   ├── recover
   ├── transform
   ├── log
   └── respond
```

For SDE-II, **error propagation + async errors + Promise rejection + error boundaries + fail-fast/recovery** are more important than memorizing the list of built-in error classes.
