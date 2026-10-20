# JavaScript Execution Model

The goal here is not to memorize "call stack, heap, execution context." We want to understand **what the engine is conceptually doing when JavaScript code runs**.

### 1. The Big Picture

Take this simple program:

```js
const x = 10;

function add(a, b) {
  return a + b;
}

const result = add(x, 20);

console.log(result);
```

At a high level:

```text
JavaScript source code
        ↓
     Parsing
        ↓
Internal representation
        ↓
Execution
        ↓
Runtime state
        ↓
Output
```

The runtime has to keep track of things such as:

- variables
- functions
- scopes
- objects
- function calls
- return values
- references

This is where concepts such as **execution context, lexical environment, call stack, and heap** come in.

## 2. Parsing

Before the engine can execute JavaScript, it needs to understand the source code.

For example:

```js
const x = 10;

function add(a, b) {
  return a + b;
}
```

The engine parses the source and determines its syntactic structure.

Conceptually:

```text
Source Code
    ↓
Lexer / Parser
    ↓
Syntax structure
    ↓
Internal representation
```

If the source contains invalid syntax:

```js
const = 10;
```

the engine cannot normally proceed with execution because the program cannot be parsed as valid JavaScript.

This gives us our first distinction:

```text
Parsing error
     ≠
Runtime error
```

For example:

```js
const x = ;
```

is a syntax problem.

Whereas:

```js
const user = null;
console.log(user.name);
```

can successfully parse but fail during execution.

## 3. Execution Context

An **execution context** is the conceptual environment in which JavaScript code is evaluated.

You can think of it as:

> "The runtime state required to execute a piece of JavaScript code."

There are different kinds of execution contexts, most importantly:

```text
Global execution context
Function execution context
Module execution context
```

For now, focus on global and function execution contexts.

## 4. Global Execution Context

When a JavaScript program starts, it needs a context in which the top-level code can execute.

For:

```js
const x = 10;

function add(a, b) {
  return a + b;
}
```

we can conceptually think:

```text
Global Execution Context
│
├── x
├── add
└── other global/module state
```

Then the engine executes the top-level code.

The exact specification-level mechanics are more nuanced than this diagram, but this is the correct mental direction.

## 5. Function Execution Context

Now:

```js
function add(a, b) {
  return a + b;
}

const result = add(10, 20);
```

When `add()` is called, JavaScript needs a new execution context for that function invocation.

Conceptually:

```text
Global Context
     │
     │ calls add()
     ↓
Function Context
│
├── a = 10
├── b = 20
└── local execution state
```

The function executes:

```js
return a + b;
```

produces:

```text
30
```

and then its execution context is no longer active.

## 6. Call Stack

Now we need somewhere to track **which function is currently executing**.

That's the conceptual role of the **call stack**.

Consider:

```js
function one() {
  two();
}

function two() {
  three();
}

function three() {
  console.log("hello");
}

one();
```

Execution progresses conceptually like this:

#### Initially

```text
Call Stack
──────────
global
```

#### `one()` executes

```text
Call Stack
──────────
one
global
```

#### `two()` executes

```text
Call Stack
──────────
two
one
global
```

#### `three()` executes

```text
Call Stack
──────────
three
two
one
global
```

Then `three()` finishes:

```text
Call Stack
──────────
two
one
global
```

Then `two()` finishes:

```text
Call Stack
──────────
one
global
```

Then `one()` finishes:

```text
Call Stack
──────────
global
```

This is **LIFO**:

> Last In, First Out.

## 7. Why the Call Stack Matters

Consider:

```js
function a() {
  b();
}

function b() {
  c();
}

function c() {
  throw new Error("Something went wrong");
}
```

The stack gives the runtime the chain of active calls.

Conceptually:

```text
c()
↑
b()
↑
a()
↑
global
```

That's why an error stack trace can show something like:

```text
Error: Something went wrong
    at c (...)
    at b (...)
    at a (...)
```

The stack isn't just an interview concept.

It directly helps us understand:

- stack traces
- recursion
- stack overflow
- synchronous execution
- debugging

## 8. Stack Overflow

Because the call stack tracks active function calls, infinitely nested synchronous calls can exhaust it.

Example:

```js
function recursive() {
  recursive();
}

recursive();
```

Conceptually:

```text
recursive()
recursive()
recursive()
recursive()
recursive()
...
```

The stack keeps growing until the engine cannot continue.

Typically you'll see an error such as:

```text
RangeError: Maximum call stack size exceeded
```

This is one reason recursion needs a proper termination condition.

## 9. Heap

The **heap** is commonly used as a conceptual model for dynamically allocated objects and other runtime data.

For example:

```js
const user = {
  name: "Danishan",
  age: 25,
};
```

We can visualize:

```text
Call stack
──────────────
user ─────────────┐
                  │
                  ↓
Heap        ┌──────────────┐
            │ name: ...    │
            │ age: ...     │
            └──────────────┘
```

But remember our earlier warning:

> **Do not treat "objects are on the heap and primitives are on the stack" as a JavaScript language rule.**

That's an implementation-level simplification.

JavaScript/ECMAScript specifies behavior, not a mandatory physical memory layout.

Modern engines can optimize representations aggressively.

## 10. Execution Context vs Call Stack

These are related but **not the same thing**.

An execution context represents the state needed to execute code.

The call stack tracks active execution contexts/calls.

Think:

```text
Execution Context
       ↓
"What's the runtime state for this execution?"

Call Stack
       ↓
"Which executions are currently active, and in what order?"
```

So when:

```js
add(10, 20);
```

is called:

```text
Call Stack
────────────
add execution context
global execution context
```

The stack is about **execution order/lifetime**.

The execution context is about **execution state**.

## 11. Lexical Environment

Now we reach an extremely important concept.

Consider:

```js
const x = 10;

function test() {
  const y = 20;

  console.log(x);
  console.log(y);
}
```

Inside `test()`:

```text
y
```

is local.

But:

```text
x
```

is defined outside the function.

How does JavaScript find `x`?

Through its **lexical scoping/environment mechanism**.

Conceptually:

```text
test environment
│
├── y
│
└── outer environment
       │
       └── x
```

When JavaScript encounters:

```js
console.log(x);
```

it searches the appropriate environment chain.

## 12. Scope Chain

This gives us the **scope chain**.

Example:

```js
const a = 1;

function outer() {
  const b = 2;

  function inner() {
    const c = 3;

    console.log(a);
    console.log(b);
    console.log(c);
  }

  inner();
}

outer();
```

Inside `inner()`:

```text
c → current environment
b → outer environment
a → outer environment
```

Conceptually:

```text
inner
  │
  ↓
outer
  │
  ↓
global
```

Variable resolution walks through this lexical structure.

This is the foundation for understanding **closures** later.

## 13. Lexical Scoping

JavaScript uses **lexical/static scoping**.

That means the relationship between code and its surrounding scopes is determined by **where the code is written**, not by who calls it.

Example:

```js
const x = "global";

function outer() {
  const x = "outer";

  function inner() {
    console.log(x);
  }

  return inner;
}

const fn = outer();
fn();
```

Output:

```text
outer
```

Why?

Because `inner` was **defined inside `outer`**.

The later call:

```js
fn();
```

doesn't change where `inner` gets its lexical variables from.

This is the beginning of closures.

## 14. What Happens During a Function Call?

Let's walk through a complete example.

```js
const x = 10;

function add(a, b) {
  const result = a + b;
  return result;
}

const value = add(x, 20);
```

#### Step 1 — Global execution

Conceptually:

```text
Global
│
├── x = 10
├── add = function
└── value
```

#### Step 2 — Call `add`

```text
Global
   ↓
add()
```

A function execution context becomes active.

```text
add context
│
├── a = 10
├── b = 20
└── result
```

#### Step 3 — Evaluate

```js
const result = a + b;
```

produces:

```text
result = 30
```

#### Step 4 — Return

```js
return result;
```

The value:

```text
30
```

is returned to the caller.

#### Step 5 — Function finishes

The function execution is complete.

Control returns to the global code.

So:

```js
const value = add(x, 20);
```

results in:

```text
value = 30
```

## 15. A More Complete Mental Model

Now combine everything:

```text
                    JavaScript Source
                           │
                           ↓
                        Parsing
                           │
                           ↓
                    Execution begins
                           │
                           ↓
                 Global execution context
                           │
                           ↓
                     Call Stack
                           │
             ┌─────────────┴─────────────┐
             ↓                           ↓
       Function call                Current code
             │
             ↓
      Function context
             │
             ↓
       Lexical environment
             │
             ↓
      Variable resolution
             │
             ↓
          Execution
```

And separately:

```text
                 Runtime Memory
                       │
              ┌────────┴────────┐
              ↓                 ↓
          Call Stack           Heap
              │                 │
       active execution      objects/data
       contexts/calls
```

Again, this is a **conceptual model**, not a literal ECMAScript memory specification.

## 16. Where Closures Enter

Now consider:

```js
function createCounter() {
  let count = 0;

  return function () {
    count++;
    return count;
  };
}

const counter = createCounter();

console.log(counter()); // 1
console.log(counter()); // 2
```

At first glance, this seems strange.

`createCounter()` has finished.

Yet `count` still exists.

Why?

Because the returned function retains access to the lexical environment in which it was created.

Conceptually:

```text
counter
   │
   ↓
function
   │
   ↓
lexical environment
   │
   └── count = 0
```

Then:

```js
counter();
```

updates that captured variable.

This is a **closure**.

We will study closures properly in the dedicated **Scope, Lexical Environment & Closures** section.

For now, remember:

> **A function can retain access to variables from its lexical environment even after the outer function has finished executing.**

## 17. Synchronous Execution

Everything we've discussed so far is synchronous.

Consider:

```js
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

The next operation doesn't begin until the current synchronous operation completes.

This is why:

```js
function first() {
  console.log("first");
}

function second() {
  console.log("second");
}

first();
second();
```

produces:

```text
first
second
```

The call stack controls this sequence.

## 18. What About Asynchronous Code?

Now:

```js
console.log("A");

setTimeout(() => {
  console.log("B");
}, 0);

console.log("C");
```

The result is:

```text
A
C
B
```

This is where the **event loop**, **task queues**, and **runtime APIs** enter the picture.

The important thing for now is:

> `setTimeout()` does not simply mean "put this function on the call stack after zero milliseconds."

Instead, the timer is handled by the host environment, and its callback becomes eligible for later execution.

The event loop eventually coordinates when it can run.

We'll study this deeply in:

**Asynchronous JavaScript → Event Loop & Concurrency Model.**

## 19. One Important Boundary

At this point, keep these concepts separate:

#### JavaScript language

```text
Variables
Functions
Objects
Scope
Promises
etc.
```

#### JavaScript engine

```text
Parsing
Execution
Optimization
Garbage collection
etc.
```

#### Host/runtime

```text
Browser
Node.js
Web APIs
Node APIs
I/O
Timers
etc.
```

#### Event loop

```text
Coordinates asynchronous work
and callback execution
```

These layers interact, but they are **not interchangeable**.

## 20. SDE-2 Interview Questions

Now I'd expect you to be able to reason through these.

#### Q1

**What is an execution context?**

A good answer:

> An execution context is the conceptual runtime state in which JavaScript code is evaluated. It contains the information necessary for executing that code, including its lexical/environment information and execution state.

#### Q2

**What is the call stack?**

> The call stack tracks currently active JavaScript execution calls in a LIFO order. Function calls are pushed onto it and removed when they complete.

#### Q3

**What is the difference between call stack and execution context?**

> An execution context represents the state required to execute code, while the call stack is the mechanism used conceptually to track active execution contexts/calls and their order.

#### Q4

What happens here?

```js
function a() {
  b();
}

function b() {
  c();
}

function c() {
  console.log("hello");
}

a();
```

You should be able to draw:

```text
c
b
a
global
```

and explain how each frame disappears as the function returns.

#### Q5

Why does this work?

```js
const x = 10;

function test() {
  console.log(x);
}

test();
```

Because variable resolution follows the lexical environment/scope structure.

#### Q6

What determines lexical scope?

**Where the code is written**, not where the function is called.

#### Q7

Why does this cause stack overflow?

```js
function foo() {
  foo();
}

foo();
```

Because every recursive invocation creates another active call before the previous invocation returns, causing the call stack to grow continuously.

## 21. The Execution Model We Need to Carry Forward

This is the mental model I want us to retain:

```text
                    SOURCE CODE
                         │
                         ↓
                      PARSING
                         │
                         ↓
                  PROGRAM EXECUTION
                         │
                         ↓
              ┌─────────────────────┐
              │ Execution Context   │
              │                     │
              │ Lexical Environment │
              │ Variables           │
              │ Scope information   │
              │ Execution state     │
              └──────────┬──────────┘
                         │
                         ↓
                    CALL STACK
                         │
             tracks active function calls
                         │
                         ↓
                  Runtime execution
                         │
                  ┌──────┴──────┐
                  ↓             ↓
                Values        Objects
                               │
                               ↓
                              Heap
```

And then later:

```text
Execution Model
      │
      ├── Variables
      │
      ├── Scope
      │
      ├── Closures
      │
      ├── Functions
      │
      ├── Memory
      │
      └── Event Loop
```

That is why this topic is worth understanding **before** we jump into `var`, `let`, `const`, hoisting, and TDZ.

The next topic should therefore be **Variables, Declarations & Keywords**, where we'll take this execution model and answer the difficult questions around **declaration vs initialization, `var` vs `let` vs `const`, hoisting, TDZ, scope, shadowing, and global bindings**.
