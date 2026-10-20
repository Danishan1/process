# 7. Scope, Lexical Environment & Closures

This is one of the **most important JavaScript topics for SDE-2**.

In the previous section, we studied functions mainly as callable values:

```text
function
 ├── parameters
 ├── arguments
 ├── return
 ├── callback
 ├── higher-order function
 ├── this
 └── call/apply/bind
```

Now we go one level deeper:

> **When a function uses a variable, where does JavaScript find that variable? And what happens to that variable after the function that created it has finished executing?**

That leads us to:

```text
Scope
   ↓
Lexical Environment
   ↓
Environment Records
   ↓
Scope Chain
   ↓
Variable Resolution
   ↓
Closures
```

---

# 7.1 What Is Scope?

**Scope determines where a variable can be accessed.**

For example:

```js
const globalValue = 10;

function test() {
    const localValue = 20;

    console.log(globalValue);
    console.log(localValue);
}
```

Inside `test()`:

```text
globalValue → accessible
localValue  → accessible
```

But outside:

```js
console.log(localValue);
```

`localValue` isn't accessible.

So we can think:

```text
Scope = region in which a binding is accessible
```

The word **binding** is important.

A variable declaration creates a binding between a name and a value.

```js
const x = 10;
```

Conceptually:

```text
"x" ─────→ 10
```

Scope determines where the name `x` can be resolved.

---

# 7.2 Global Scope

A variable declared at the top level can belong to the global scope, depending on the execution environment and whether the code is a script or module.

For example:

```js
const x = 10;

function test() {
    console.log(x);
}
```

`x` is accessible from `test()` because `test` can resolve names through its outer lexical environment.

Conceptually:

```text
Global Scope
     │
     ├── x
     │
     └── test()
             │
             ↓
       can access x
```

Global state should generally be minimized in application design because many unrelated parts of an application can depend on it.

---

# 7.3 Function Scope

A function creates its own scope.

```js
function test() {
    const x = 10;

    console.log(x);
}

console.log(x); // ReferenceError
```

`x` exists within the function's lexical scope.

This is particularly important for `var`.

```js
function test() {
    var x = 10;
}

console.log(x);
```

`x` is function-scoped.

---

# 7.4 Block Scope

`let` and `const` are block-scoped.

A block is generally created by `{ ... }`.

```js
if (true) {
    let x = 10;
    const y = 20;

    console.log(x);
}

console.log(x); // ReferenceError
console.log(y); // ReferenceError
```

The block creates a lexical scope.

Conceptually:

```text
Outer Scope
    │
    └── if block
          ├── x
          └── y
```

Once execution leaves the block, those bindings aren't accessible from the outer scope.

---

# 7.5 `var` vs `let` in Blocks

This is a classic interview question.

```js
if (true) {
    var a = 10;
    let b = 20;
}

console.log(a);
console.log(b);
```

Result:

```text
10
ReferenceError
```

Why?

Because:

```text
var
 ↓
function scoped

let / const
 ↓
block scoped
```

So the `if` block does not create a separate `var` scope.

---

# 7.6 Module Scope

JavaScript modules have their own top-level lexical scope.

For example:

```js
// math.js

const secret = 42;

export function add(a, b) {
    return a + b;
}
```

`secret` isn't automatically exposed globally.

Another module cannot simply do:

```js
console.log(secret);
```

It must be explicitly exported/imported if it needs access.

Conceptually:

```text
Module A
 ├── secret
 └── add
       │
       ↓ export

Module B
       ↓ import
      add
```

This gives modules a useful boundary for encapsulation.

---

# 7.7 Lexical / Static Scoping

JavaScript uses **lexical scoping**.

This means variable resolution is primarily determined by **where code is written**, not where a function happens to be called.

Consider:

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

Because `inner` was defined inside `outer`.

Its lexical surroundings are:

```text
inner
 ↓
outer
 ↓
global
```

Therefore when `inner` resolves `x`:

```text
inner scope → no x
     ↓
outer scope → x = "outer"
```

It stops there.

---

# 7.8 Lexical Scoping Is About Source Structure

Imagine:

```js
function outer() {
    const message = "hello";

    function inner() {
        console.log(message);
    }

    return inner;
}
```

The important relationship is visible directly from the source:

```text
outer
 └── inner
      └── message is reachable
```

This is why it is called **lexical** scoping.

The structure of the source code establishes the scope relationship.

---

# 7.9 Dynamic Scoping — Comparison

JavaScript does **not** use dynamic scoping for ordinary variable lookup.

In a dynamically scoped language, the runtime call stack would influence where a variable is found.

Imagine conceptually:

```text
A calls B
B calls C
```

Under dynamic scoping, `C` might search the callers:

```text
C
 ↓
B
 ↓
A
```

rather than following the lexical nesting in the source.

JavaScript instead follows the lexical structure:

```text
C
 ↓
lexical parent
 ↓
lexical parent's parent
 ↓
global/module
```

This distinction becomes critical when reasoning about closures.

---

# 7.10 Lexical Environment

Now we move deeper.

The JavaScript specification describes execution in terms of **Lexical Environments**.

A useful simplified mental model is:

```text
Lexical Environment
 ├── Environment Record
 │      └── bindings
 │
 └── outer environment
```

For example:

```js
const x = 10;

function test() {
    const y = 20;
}
```

Conceptually:

```text
Global Lexical Environment
 ├── x → 10
 └── test → function
             │
             ↓
        Function Lexical Environment
         └── y → 20
```

The function's environment has a relationship to the outer environment.

---

# 7.11 Environment Record

An **Environment Record** is the conceptual structure that stores bindings for an execution context.

For example:

```js
const name = "Danishan";
const age = 25;
```

we can mentally model the environment as:

```text
Environment Record

name → "Danishan"
age  → 25
```

The actual ECMAScript specification has more detailed distinctions between kinds of environment records, but for practical reasoning this model is extremely useful.

---

# 7.12 Lexical Environment vs Scope

These terms are related but aren't identical.

Think:

```text
Scope
 ↓
language-level rule describing
where a binding is accessible

Lexical Environment
 ↓
runtime/specification structure
representing bindings + outer relationship
```

So scope is the **behavioral concept**, while a lexical environment is part of the mechanism used to model that behavior.

---

# 7.13 The Outer Environment Reference

A lexical environment has an outer relationship.

Conceptually:

```text
Environment A
     │
     │ outer
     ↓
Environment B
     │
     │ outer
     ↓
Environment C
```

This creates the structure through which JavaScript resolves variables.

For example:

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
```

The lookup structure is approximately:

```text
inner environment
      │
      ↓
outer environment
      │
      ↓
global environment
```

---

# 7.14 Scope Chain

The **scope chain** is the practical way to think about this lookup path.

Suppose:

```js
const a = 1;

function outer() {
    const b = 2;

    function inner() {
        const c = 3;

        console.log(a + b + c);
    }

    inner();
}
```

When `inner()` evaluates:

```js
a
```

JavaScript conceptually searches:

```text
inner scope
    ↓
a found? No

outer scope
    ↓
a found? No

global scope
    ↓
a found → 1
```

For `b`:

```text
inner scope
    ↓
not found

outer scope
    ↓
found → 2
```

For `c`:

```text
inner scope
    ↓
found → 3
```

That's variable resolution through the scope chain.

---

# 7.15 Variable Resolution

Whenever JavaScript encounters an identifier:

```js
console.log(name);
```

it needs to answer:

> **Which `name` does this refer to?**

The engine searches the relevant lexical environments.

Conceptually:

```text
name
 ↓
current environment?
 ↓ no
outer environment?
 ↓ no
next outer?
 ↓
found
```

If no binding can be resolved:

```js
console.log(doesNotExist);
```

you get a `ReferenceError`.

---

# 7.16 Shadowing

A variable in an inner scope can have the same name as one in an outer scope.

```js
const x = "global";

function test() {
    const x = "local";

    console.log(x);
}

test();
```

Output:

```text
local
```

The inner `x` **shadows** the outer `x`.

The lookup stops at the nearest matching binding.

```text
inner scope
 └── x = "local"  ← found first

outer scope
 └── x = "global"
```

The outer binding still exists; it is simply hidden by the inner binding within that scope.

---

# 7.17 Shadowing Across Multiple Levels

```js
const x = "global";

function outer() {
    const x = "outer";

    function inner() {
        const x = "inner";

        console.log(x);
    }

    inner();
}
```

Output:

```text
inner
```

Lookup:

```text
inner scope
   ↓
x = "inner"
   ↓
stop
```

JavaScript doesn't continue looking after it finds the nearest matching binding.

---

# 7.18 `let` Shadowing

Block scoping makes shadowing particularly common:

```js
let x = 10;

{
    let x = 20;

    console.log(x);
}

console.log(x);
```

Output:

```text
20
10
```

There are two separate bindings:

```text
outer x → 10
inner x → 20
```

They are not the same variable.

---

# 7.19 Closures

Now we reach the central concept.

Consider:

```js
function outer() {
    const message = "Hello";

    return function inner() {
        console.log(message);
    };
}

const fn = outer();

fn();
```

At first glance, you might wonder:

> `outer()` has already finished. How can `inner()` still access `message`?

Because `inner` has access to the lexical environment in which it was created.

This is the behavior we call a **closure**.

Conceptually:

```text
outer()
 │
 ├── message → "Hello"
 │
 └── inner function
          │
          │ captures/accesses
          ↓
     outer lexical environment
```

After `outer()` returns, the environment remains reachable because `inner` still needs it.

---

# 7.20 Closure Lifetime

This is where closures become particularly interesting.

Consider:

```js
function createCounter() {
    let count = 0;

    return () => {
        count++;
        return count;
    };
}

const counter = createCounter();
```

The call to:

```js
createCounter()
```

has finished.

But:

```js
counter();
```

still works.

Why?

Because the returned function maintains access to the environment containing:

```text
count → 0
```

After:

```js
counter();
```

the same captured binding becomes:

```text
count → 1
```

Then:

```text
count → 2
```

and so on.

---

# 7.21 Closure Lifetime Is About Reachability

A very important SDE-2 refinement:

Don't think:

> "The outer function's variables live forever."

They don't.

Instead:

> A captured environment remains available as long as it is reachable through live references.

For example:

```js
let counter = createCounter();
```

The returned function references the captured environment.

If eventually:

```js
counter = null;
```

and nothing else references that function/environment, it can become eligible for garbage collection.

So closure lifetime is closely related to **reachability and garbage collection**.

---

# 7.22 Captured Variables

Consider:

```js
function outer() {
    let a = 10;
    let b = 20;

    return () => {
        return a;
    };
}
```

The returned function uses `a`, but not `b`.

From a conceptual perspective, the closure needs access to the lexical state necessary for its execution.

Don't oversimplify this into:

> "The entire outer function is kept alive."

The important thing is the **captured lexical environment/state and reachability**, while actual engine optimizations can be more sophisticated.

---

# 7.23 Closures Capture Bindings, Not Just Values

This is an extremely important distinction.

Consider:

```js
function createCounter() {
    let count = 0;

    return () => {
        count++;
        return count;
    };
}
```

The closure doesn't simply capture:

```text
count = 0
```

as a frozen value.

It has access to the **binding**.

Therefore:

```text
initially:
count → 0

after first call:
count → 1

after second call:
count → 2
```

The same binding is being updated.

This explains why closures can maintain mutable private state.

---

# 7.24 Multiple Closures Can Share State

Consider:

```js
function createCounter() {
    let count = 0;

    return {
        increment() {
            count++;
        },

        getCount() {
            return count;
        }
    };
}
```

Now:

```js
const counter = createCounter();
```

Both:

```text
increment
getCount
```

close over the same lexical environment.

Conceptually:

```text
             createCounter environment
                    │
                 count
                    │
          ┌─────────┴─────────┐
          ↓                   ↓
     increment()          getCount()
       closure              closure
```

Therefore:

```js
counter.increment();
counter.increment();

counter.getCount();
```

returns:

```text
2
```

Both functions see the same `count` binding.

---

# 7.25 Closures as Private State

This gives us one of the classic closure use cases:

```js
function createBankAccount(initialBalance) {
    let balance = initialBalance;

    return {
        deposit(amount) {
            balance += amount;
        },

        getBalance() {
            return balance;
        }
    };
}
```

Usage:

```js
const account = createBankAccount(1000);

account.deposit(500);

console.log(account.getBalance());
```

Result:

```text
1500
```

But:

```js
account.balance
```

doesn't exist as a public property.

The state is encapsulated inside the closure.

---

# 7.26 Closure Use Case: Function Factories

Closures are also useful for generating specialized functions.

```js
function createMultiplier(multiplier) {
    return function (value) {
        return value * multiplier;
    };
}
```

Now:

```js
const double = createMultiplier(2);
const triple = createMultiplier(3);
```

We effectively have:

```text
double
 └── captures multiplier = 2

triple
 └── captures multiplier = 3
```

Then:

```js
double(10); // 20
triple(10); // 30
```

Same function factory, different captured state.

---

# 7.27 Closure Use Case: Configuration

A very practical pattern:

```js
function createLogger(prefix) {
    return function log(message) {
        console.log(`[${prefix}] ${message}`);
    };
}
```

Then:

```js
const dbLogger = createLogger("DB");
const apiLogger = createLogger("API");
```

Now:

```js
dbLogger("Connected");
apiLogger("Request received");
```

Conceptually:

```text
dbLogger → captures "DB"
apiLogger → captures "API"
```

This pattern appears throughout real-world JavaScript libraries and applications.

---

# 7.28 Closure Pitfall: `var` in Loops

One of the most famous examples:

```js
for (var i = 0; i < 3; i++) {
    setTimeout(() => {
        console.log(i);
    }, 0);
}
```

Output:

```text
3
3
3
```

Why?

`var` doesn't create a new binding for each loop iteration.

Conceptually:

```text
one shared i binding
       │
       ├── callback 1
       ├── callback 2
       └── callback 3
```

After the loop:

```text
i = 3
```

All callbacks access the same binding.

---

# 7.29 Why `let` Changes the Result

Now:

```js
for (let i = 0; i < 3; i++) {
    setTimeout(() => {
        console.log(i);
    }, 0);
}
```

Output:

```text
0
1
2
```

The important conceptual difference is that `let` provides per-iteration bindings for this loop behavior.

Conceptually:

```text
iteration 1 → i = 0 → callback captures it
iteration 2 → i = 1 → callback captures it
iteration 3 → i = 2 → callback captures it
```

Therefore each callback observes its corresponding iteration value.

---

# 7.30 Another Closure Pitfall: Accidental Memory Retention

Closures can unintentionally keep objects reachable.

For example:

```js
function createHandler() {
    const hugeData = loadHugeData();

    return function handler() {
        console.log(hugeData.someValue);
    };
}
```

As long as the returned `handler` remains reachable, the captured state may remain reachable too.

This can matter in long-lived systems such as:

* event listeners
* subscriptions
* timers
* caches
* global registries
* long-lived service objects

The problem isn't:

> "Closures are memory leaks."

The accurate statement is:

> **Closures can retain references to state longer than intended if the closure itself remains reachable.**

---

# 7.31 Closures and Event Handlers

Consider:

```js
function setup(element) {
    const config = {
        theme: "dark"
    };

    function handleClick() {
        console.log(config.theme);
    }

    element.addEventListener("click", handleClick);
}
```

`handleClick` closes over `config`.

If the event listener remains registered, the function can remain reachable, and therefore the captured state may remain reachable as well.

This is why cleanup of long-lived subscriptions/listeners matters.

---

# 7.32 Scope vs Closure

These are related but not the same concept.

### Scope

Answers:

> Where can this variable be accessed?

### Closure

Answers:

> How can a function continue accessing its lexical environment after the surrounding execution has finished?

So:

```text
Scope
 ↓
determines visibility / resolution

Closure
 ↓
preserves access to lexical state
through a function
```

---

# 7.33 Scope Chain vs Prototype Chain

This distinction is very important.

JavaScript has both:

```text
Scope Chain
Prototype Chain
```

They solve completely different problems.

### Scope chain

Used for:

```js
console.log(name);
```

It determines where an identifier is found.

### Prototype chain

Used for property lookup:

```js
object.name
```

It determines where an object's property is found.

So:

```text
name
 ↓
scope chain

object.name
 ↓
prototype chain
```

Don't mix these two during interviews.

---

# 7.34 Variable Lookup vs Property Lookup

Consider:

```js
const name = "global";

const user = {
    name: "Danishan"
};

console.log(name);
console.log(user.name);
```

These are fundamentally different lookups.

```js
console.log(name);
```

asks:

> Find the identifier `name`.

JavaScript uses lexical environment / scope resolution.

But:

```js
console.log(user.name);
```

asks:

> Find the property `name` on `user`.

That involves object property lookup and potentially the prototype chain.

---

# 7.35 Lexical Scope Is Not `this`

Another important distinction.

These two mechanisms are different:

```text
Lexical scope
      ↓
where variables are resolved

this
      ↓
function execution context
```

For example:

```js
const name = "global";

const user = {
    name: "Danishan",

    greet() {
        console.log(name);
        console.log(this.name);
    }
};
```

These two expressions do different things:

```js
name
```

uses lexical variable resolution.

```js
this.name
```

uses `this` followed by property lookup.

This is why understanding closures doesn't automatically mean you've understood `this`.

---

# 7.36 A Complete Execution Example

Let's combine everything:

```js
const x = "global";

function outer() {
    const x = "outer";

    return function inner() {
        const x = "inner";

        return function deepest() {
            return x;
        };
    };
}

const first = outer();
const second = first();

console.log(second());
```

Let's trace it.

### Global environment

```text
x → "global"
outer → function
```

### Execute `outer()`

New environment:

```text
x → "outer"
```

It returns `inner`.

`inner` has lexical access to the environment where it was defined.

### Execute `first()`

New environment:

```text
x → "inner"
```

It returns `deepest`.

### Execute `second()`

`deepest` looks for:

```text
x
```

Its own environment doesn't contain `x`.

Next:

```text
inner environment
x → "inner"
```

Found.

Therefore:

```text
inner
```

is printed.

This single example tests:

* lexical scope
* lexical environments
* scope chain
* shadowing
* closures
* nested functions
* variable resolution

---

# 7.37 The Most Useful Mental Model

For any identifier:

```js
someVariable
```

think:

```text
             someVariable
                   ↓
        current lexical environment
                   ↓
               found?
              /      \
            yes       no
             ↓         ↓
           value    outer environment
                         ↓
                      found?
                         ↓
                        ...
                         ↓
                    global/module
                         ↓
                   not found?
                         ↓
                  ReferenceError
```

This is the fundamental mechanism behind lexical variable resolution.

---

# 7.38 The Complete Architecture

At SDE-2 level, I recommend keeping this mental model:

```text
                         JAVASCRIPT
                             │
                         Lexical Scope
                             │
                             ↓
                   Lexical Environment
                             │
                    ┌────────┴────────┐
                    ↓                 ↓
            Environment Record    Outer Reference
                    │                 │
                    ↓                 ↓
                Bindings          Scope Chain
                    │                 │
                    └────────┬────────┘
                             ↓
                     Variable Resolution
                             │
                             ↓
                         Functions
                             │
                             ↓
                       Lexical Capture
                             │
                             ↓
                          Closures
                             │
                  ┌──────────┼──────────┐
                  ↓          ↓          ↓
               private    factories   callbacks
                state
```

This is the conceptual connection between the entire chapter.

---

# 7.39 Scope Questions You Should Be Able to Answer

### Basic

1. What is scope?
2. What is global scope?
3. What is function scope?
4. What is block scope?
5. What is module scope?
6. Difference between `var`, `let`, and `const` from a scope perspective.

### Lexical scoping

7. What is lexical/static scoping?
8. How is lexical scoping different from dynamic scoping?
9. Why does JavaScript use lexical scoping?
10. How does JavaScript resolve an identifier?

### Lexical environments

11. What is a lexical environment?
12. What is an environment record?
13. What is the outer environment reference?
14. How do lexical environments form a scope chain?
15. What happens when a variable isn't found in the current environment?

### Shadowing

16. What is variable shadowing?
17. What happens when inner and outer scopes contain the same variable name?
18. How does block shadowing work with `let` and `const`?

### Closures

19. What is a closure?
20. What exactly does a closure capture?
21. Why does a closure survive after the outer function returns?
22. Does a closure capture a value or a binding?
23. Can multiple closures share the same captured state?
24. When does a closure's captured environment become eligible for garbage collection?
25. What are practical uses of closures?
26. How can closures cause accidental memory retention?
27. Explain the `var` + loop + callback problem.
28. Why does `let` solve the classic loop closure problem?

---

# 7.40 The Three Questions That Reveal Real Understanding

When an interviewer gives you closure code, don't immediately jump to the output.

Ask these three questions:

### 1. Where was the function defined?

This tells you its **lexical environment**.

### 2. Which bindings does the function need?

This tells you what variables it can access through the scope chain.

### 3. Is that environment still reachable?

If yes, the closure can continue accessing that state even after the original function invocation has finished.

So the complete mental model is:

```text
Function definition
       ↓
Lexical environment
       ↓
Variable lookup
       ↓
Captured bindings
       ↓
Closure
       ↓
Environment remains reachable
       ↓
Function can access captured state later
```

That is the real foundation of JavaScript closures—not simply the phrase **"inner function remembers outer variables."**
