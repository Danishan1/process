Yes. Let's make it **interview-level complex** by nesting `nextTick`, Promises, timers, `setImmediate`, and I/O callbacks inside each other.

## Complex Example

```javascript id="6v0zq1"
const fs = require("fs");

console.log("1");

setTimeout(() => {
  console.log("2 - timer");

  process.nextTick(() => {
    console.log("3 - nextTick inside timer");
  });

  Promise.resolve().then(() => {
    console.log("4 - promise inside timer");
  });

  setImmediate(() => {
    console.log("5 - immediate inside timer");
  });
}, 0);

setImmediate(() => {
  console.log("6 - immediate");

  process.nextTick(() => {
    console.log("7 - nextTick inside immediate");
  });

  Promise.resolve().then(() => {
    console.log("8 - promise inside immediate");
  });
});

fs.readFile(__filename, () => {
  console.log("9 - I/O callback");

  process.nextTick(() => {
    console.log("10 - nextTick inside I/O");
  });

  Promise.resolve().then(() => {
    console.log("11 - promise inside I/O");
  });

  setTimeout(() => {
    console.log("12 - timer inside I/O");
  }, 0);

  setImmediate(() => {
    console.log("13 - immediate inside I/O");
  });
});

Promise.resolve().then(() => {
  console.log("14 - promise");

  process.nextTick(() => {
    console.log("15 - nextTick inside promise");
  });
});

process.nextTick(() => {
  console.log("16 - nextTick");

  Promise.resolve().then(() => {
    console.log("17 - promise inside nextTick");
  });
});

console.log("18");
```

---

# Step 1 — Synchronous execution

Start from the top.

```text id="8g3c4m"
Synchronous
│
├── console.log("1")
│     ↓
│     1
│
├── Register setTimeout
│
├── Register setImmediate
│
├── Register fs.readFile
│
├── Register Promise
│
├── Register process.nextTick
│
└── console.log("18")
      ↓
      18
```

So first:

```text id="w7f1gx"
1
18
```

Nothing asynchronous executes yet.

---

# Step 2 — `process.nextTick()`

We have:

```javascript id="z5h6om"
process.nextTick(() => {
  console.log("16");

  Promise.resolve().then(() => {
    console.log("17");
  });
});
```

So:

```text id="o4bdm5"
nextTick Queue
│
└── 16
     │
     └── schedules Promise
```

Output:

```text id="3yrz76"
16
```

But now there is a Promise microtask:

```text id="q6z0o9"
Promise Microtask
└── 17
```

So:

```text id="1x5wca"
17
```

---

# Step 3 — Existing Promise microtask

We had:

```javascript id="2c5z2g"
Promise.resolve().then(() => {
  console.log("14");

  process.nextTick(() => {
    console.log("15");
  });
});
```

So:

```text id="j8m8qa"
Promise
│
├── 14
│
└── schedules nextTick(15)
```

Output:

```text id="l1p2rt"
14
```

Now `15` is placed into the `nextTick` queue.

Node processes it:

```text id="s2o8k4"
nextTick
└── 15
```

Output:

```text id="t4z7q1"
15
```

---

# Current output

So far:

```text id="kq9p4e"
1
18
16
17
14
15
```

The exact interaction between newly scheduled `nextTick` and Promise microtasks is an important Node-specific detail; don't reduce it to a browser-style single microtask queue.

---

# Step 4 — Event Loop

Now Node continues into the event loop.

We have:

```text id="r8t2vv"
Event Loop
│
├── Timers
│   └── setTimeout()
│
├── Pending callbacks
│
├── Poll
│   └── fs.readFile()
│
├── Check
│   └── setImmediate()
│
└── Close callbacks
```

---

# Step 5 — Timer executes

Timer:

```javascript id="b4n0x1"
setTimeout(() => {
  console.log("2");

  process.nextTick(() => {
    console.log("3");
  });

  Promise.resolve().then(() => {
    console.log("4");
  });

  setImmediate(() => {
    console.log("5");
  });
}, 0);
```

Output:

```text id="0y0n0c"
2
```

But the callback schedules:

```text id="c5u0v6"
nextTick
└── 3

Promise
└── 4

setImmediate
└── 5
```

After the timer callback completes, Node processes the higher-priority queues:

```text id="v9xq2f"
3
↓
4
```

So:

```text id="u5e2c8"
2
3
4
```

Then the event loop continues.

---

# Step 6 — `setImmediate`

The previously registered `setImmediate()` can execute in the **check phase**.

```javascript id="b6d9k4"
setImmediate(() => {
  console.log("6");

  process.nextTick(() => {
    console.log("7");
  });

  Promise.resolve().then(() => {
    console.log("8");
  });
});
```

Output:

```text id="q6h3x8"
6
```

Then:

```text id="2f0h8b"
nextTick
└── 7

Promise
└── 8
```

Therefore:

```text id="1w8jz0"
6
7
8
```

---

# Step 7 — I/O callback

Eventually:

```javascript id="5s9w6q"
fs.readFile(__filename, () => {
```

executes during the poll phase.

Output:

```text id="3a6wq2"
9
```

Inside it, we schedule:

```text id="q5e9y1"
I/O callback
│
├── nextTick
│   └── 10
│
├── Promise
│   └── 11
│
├── Timer
│   └── 12
│
└── Immediate
    └── 13
```

After the I/O callback finishes:

```text id="a7n2k5"
10
11
```

Then the event loop moves toward the check phase:

```text id="p3c8r4"
13
```

Then eventually:

```text id="x2m7v9"
12
```

---

# Mental Execution Tree

This is the important part to memorize:

```text id="n8w4s2"
MAIN
│
├── Sync
│   ├── 1
│   └── 18
│
├── nextTick
│   └── 16
│       └── Promise
│           └── 17
│
├── Promise
│   └── 14
│       └── nextTick
│           └── 15
│
└── EVENT LOOP
    │
    ├── TIMER
    │   └── 2
    │       ├── nextTick → 3
    │       ├── Promise → 4
    │       └── setImmediate → 5
    │
    ├── POLL / I/O
    │   └── 9
    │       ├── nextTick → 10
    │       ├── Promise → 11
    │       ├── setImmediate → 13
    │       └── setTimeout → 12
    │
    └── CHECK
        └── 6
            ├── nextTick → 7
            └── Promise → 8
```

## The key interview rule

Don't memorize one giant fixed sequence.

Instead, use this algorithm:

```text id="e6y1w2"
Execute synchronous code
        ↓
Drain nextTick
        ↓
Drain Promise microtasks
        ↓
Enter event-loop phase
        ↓
Execute callback
        ↓
Drain nextTick + microtasks
        ↓
Continue event loop
```

And remember the crucial distinction:

```text id="x7k4p1"
setTimeout()
    ↓
Timers phase

I/O
    ↓
Poll phase

setImmediate()
    ↓
Check phase
```

**The tricky interview question is not "what runs first?" — it's "where was the callback scheduled?"** That determines the relationship between timers, I/O, and `setImmediate()`.
