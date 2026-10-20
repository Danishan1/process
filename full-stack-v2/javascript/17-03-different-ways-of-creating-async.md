In JavaScript, there are several **ways to perform asynchronous work**. For SDE-II interviews, I would organize them historically and conceptually like this:

## 1. Callbacks

The oldest/common pattern:

```js
fs.readFile("file.txt", (err, data) => {
    if (err) {
        console.error(err);
        return;
    }

    console.log(data);
});
```

The callback runs **when the async operation completes**.

```text
Start I/O
   ↓
Continue doing other work
   ↓
I/O completes
   ↓
callback()
```

**Important:** callback ≠ async. The API using the callback has to be asynchronous.

---

## 2. Promises

Promises represent a value that will be available **now, later, or never**.

```js
fetch("/users")
    .then(response => response.json())
    .then(users => {
        console.log(users);
    })
    .catch(error => {
        console.error(error);
    });
```

Conceptually:

```text
fetch()
  ↓
Promise
  ↓
operation running
  ↓
fulfilled / rejected
  ↓
.then() / .catch()
```

Promises are generally cleaner than nested callbacks.

---

## 3. `async/await`

This is the modern syntax built on top of **Promises**.

```js
async function getUsers() {
    try {
        const response = await fetch("/users");
        const users = await response.json();

        console.log(users);
    } catch (error) {
        console.error(error);
    }
}
```

This looks synchronous:

```text
fetch()
 ↓
await
 ↓
users
```

But the underlying operation is still asynchronous.

### Very important:

```js
async function foo() {
    const result = await something();
}
```

`async/await` **doesn't create asynchronous work by itself**.

It provides a convenient way to consume a Promise.

---

# 4. Timers

JavaScript/Node.js also provides:

```js
setTimeout(() => {
    console.log("Executed later");
}, 1000);
```

and:

```js
setInterval(() => {
    console.log("Every second");
}, 1000);
```

These schedule callbacks to execute later.

---

# 5. Event-based APIs

Node.js heavily uses events.

```js
const EventEmitter = require("events");

const emitter = new EventEmitter();

emitter.on("data", (data) => {
    console.log(data);
});

emitter.emit("data", "Hello");
```

A real-world example is a server receiving connections:

```js
server.on("request", (req, res) => {
    // handle request
});
```

Events allow your code to respond when something happens.

---

# 6. APIs that return Promises

A lot of modern JavaScript APIs are already Promise-based:

```js
fetch(...)
```

```js
fs.promises.readFile(...)
```

```js
someDatabase.query(...)
```

You can then use:

```js
await ...
```

or:

```js
.then(...)
```

---

# The important hierarchy

For your Node.js interview preparation, don't memorize these as completely separate mechanisms.

Think:

```text
                 ASYNCHRONOUS PROGRAMMING
                          │
          ┌───────────────┼────────────────┐
          │               │                │
      Callbacks        Promises          Events
                          │
                          │
                    async / await
                    (syntax over
                     Promises)
```

And timers are another way of **scheduling work for later**:

```text
setTimeout / setInterval
```

---

## One thing that often confuses people

These two are **not equivalent**:

```js
async function foo() {
    return 10;
}
```

and:

```js
setTimeout(() => {
    console.log(10);
}, 1000);
```

`async` means the function **returns a Promise**:

```js
foo().then(value => {
    console.log(value);
});
```

It does **not** mean "run this function on another thread."

That's a crucial Node.js concept.

### For SDE-II, focus most on these 4:

1. **Callbacks**
2. **Promises**
3. **async/await**
4. **Event loop / timers / event-driven APIs**

Once you understand how these interact with the **event loop, microtask queue, callback/task queue, and libuv**, asynchronous JavaScript becomes much less mysterious.
