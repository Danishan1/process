
# 6.43 High-Probability SDE-2 Questions

I would prioritize these heavily.

### Function fundamentals

1. What is the difference between a function declaration and function expression?
2. What gets hoisted in a function declaration?
3. What is the difference between a normal function and an arrow function?
4. What is the difference between parameters and arguments?
5. What happens when a function doesn't explicitly return anything?
6. What are default parameters?
7. What are rest parameters?
8. Difference between rest and spread.

### First-class / higher-order functions

9. What does it mean that functions are first-class values?
10. What is a higher-order function?
11. What is a callback?
12. Difference between passing `fn` and calling `fn()`.
13. What is function composition?
14. Why are callbacks fundamental to JavaScript APIs?

### Closures

15. What is a closure?
16. What does a closure capture?
17. Explain lexical scoping.
18. Why does a returned inner function still access variables from its outer function?
19. Explain the classic `var` + `setTimeout` loop problem.
20. How does changing `var` to `let` change that behavior?
21. Give a practical use case for closures.

### `this`

22. How is `this` determined for a normal function?
23. How does `this` behave in an arrow function?
24. Why does extracting an object method sometimes change `this`?
25. Why doesn't putting an arrow function inside an object make `this` refer to the object?
26. How does `this` behave when a function is called as a method?
27. How does `this` behave when a function is called normally?

### `call`, `apply`, `bind`

28. Difference between `call`, `apply`, and `bind`.
29. Which of them invokes the function immediately?
30. Why does `bind` return a new function?
31. Can `call`/`apply`/`bind` change an arrow function's `this`?
32. What happens to function identity after `bind`?

### Deeper reasoning

33. Why are functions objects?
34. Why are two identical-looking function expressions not `===`?
35. What happens when a function is passed as an argument?
36. How does lexical scope interact with closures?
37. How do closures and function identity matter in event handlers?
38. What is the difference between a pure and impure function?
39. When can a closure cause unexpected memory retention?
40. How would you debug a complicated `this` problem?
## 6.43 — SDE-2 High-Probability Questions: Answers

### Function fundamentals

| #     | Question                                     | Answer                                                                                                                                                                                                                                                                                                                                 |
| ----- | -------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1** | Function declaration vs function expression? | A **declaration** is declared directly with `function name() {}` and is hoisted with its function value available before its source position. A **function expression** creates a function value as part of an expression, commonly assigned to `let`/`const`; the variable binding follows normal `let`/`const` initialization rules. |
| **2** | What gets hoisted in a function declaration? | The **function binding and its function value** are available during execution before the declaration's textual position. `sayHi(); function sayHi() {}` works.                                                                                                                                                                        |
| **3** | Normal function vs arrow function?           | The biggest semantic difference is `this`. A normal function gets `this` from its **call site**; an arrow function does **not create its own `this`** and captures it lexically. Arrow functions also don't have their own `arguments`, cannot be used with `new`, and don't have a `prototype` property.                              |
| **4** | Parameters vs arguments?                     | Parameters are the variables in the function definition: `function add(a, b)`. Arguments are the actual values supplied: `add(10, 20)`.                                                                                                                                                                                                |
| **5** | Function without explicit return?            | It returns `undefined`.                                                                                                                                                                                                                                                                                                                |
| **6** | Default parameters?                          | A parameter can have a fallback value: `function greet(name = "Guest") {}`. The default is used when the argument is `undefined`, including when the argument is omitted.                                                                                                                                                              |
| **7** | Rest parameters?                             | `...args` collects remaining arguments into a real array: `function sum(...args) {}`.                                                                                                                                                                                                                                                  |
| **8** | Rest vs spread?                              | **Rest collects:** `function f(...args) {}`. **Spread expands:** `f(...args)`. Same syntax, opposite operation.                                                                                                                                                                                                                        |

---

# First-class / Higher-order Functions

### 9. What does it mean that functions are first-class values?

Functions can be treated like ordinary values.

You can:

```js
const fn = () => 10;

const arr = [fn];

function execute(callback) {
    return callback();
}

function createFn() {
    return () => 42;
}
```

So functions can be:

* assigned to variables
* stored in arrays/objects
* passed as arguments
* returned from functions

This capability is fundamental to JavaScript's callback and functional programming patterns.

---

### 10. What is a higher-order function?

A function that **accepts another function or returns a function**.

```js
function execute(fn) {
    return fn();
}
```

`execute` is a higher-order function because it accepts `fn`.

Another example:

```js
function multiplier(x) {
    return y => x * y;
}
```

Here the function returns another function.

---

### 11. What is a callback?

A callback is a function passed to another piece of code so that it can be invoked later or during some operation.

```js
function processUser(user, callback) {
    callback(user);
}

processUser(user, u => {
    console.log(u.name);
});
```

`u => console.log(u.name)` is the callback.

**Important:** callback describes the function's **role**, not its syntax.

---

### 12. Difference between `fn` and `fn()`?

This is extremely important.

```js
execute(fn);
```

means:

> Pass the function itself.

Whereas:

```js
execute(fn());
```

means:

> Execute `fn` now and pass its return value.

Example:

```js
function greet() {
    return "Hello";
}

function execute(fn) {
    console.log(fn());
}

execute(greet);      // Hello
execute(greet());    // Error: "Hello" is not a function
```

---

### 13. What is function composition?

Combining functions so the output of one becomes the input of another.

```js
const double = x => x * 2;
const increment = x => x + 1;

const result = increment(double(5));
```

Execution:

```text
5
 ↓
double
 ↓
10
 ↓
increment
 ↓
11
```

Composition is common in functional programming and middleware pipelines.

---

### 14. Why are callbacks fundamental to JavaScript APIs?

Because JavaScript APIs frequently need to say:

> "Perform this operation, and when you're ready, execute this function."

For example:

```js
setTimeout(() => {
    console.log("Done");
}, 1000);
```

The callback allows the API to control **when** the supplied behavior executes.

This becomes particularly important with:

```text
events
I/O
timers
HTTP requests
streams
database operations
Promises
```

---

# Closures

### 15. What is a closure?

A closure is the behavior where a function retains access to variables from its **lexical environment** even after the surrounding function has finished executing.

```js
function createCounter() {
    let count = 0;

    return function () {
        count++;
        return count;
    };
}

const counter = createCounter();

counter(); // 1
counter(); // 2
counter(); // 3
```

`createCounter()` has already returned, but the returned function still accesses `count`.

Conceptually:

```text
counter
   ↓
function
   ↓
lexical environment
   ↓
count = 0
```

---

### 16. What does a closure capture?

A closure provides access to variables from its surrounding lexical environments.

```js
function outer() {
    const name = "Danishan";

    return function inner() {
        console.log(name);
    };
}
```

`inner` has access to `name`.

A subtle but important point:

> Closures preserve access to the **binding**, not necessarily a frozen copy of its value.

For example:

```js
function createCounter() {
    let count = 0;

    return () => ++count;
}
```

The function accesses the same `count` binding, which can change.

---

### 17. Explain lexical scoping.

Lexical scoping means variable lookup is determined by **where code is written/defined**, rather than where the function is called.

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

fn(); // "outer"
```

`inner` was defined inside `outer`, so its lookup chain includes `outer`.

```text
inner
 ↓
outer
 ↓
global
```

---

### 18. Why does a returned inner function still access outer variables?

Because the function maintains access to its lexical environment.

```js
function outer() {
    const value = 100;

    return function inner() {
        return value;
    };
}
```

Even after:

```js
outer();
```

has returned, the returned function still needs `value`.

The runtime therefore keeps the necessary environment reachable.

---

### 19. Explain `var` + `setTimeout`.

Classic interview question:

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

`var` creates one binding for `i` in the relevant function/global scope.

The callbacks close over that **same binding**.

By the time the timers execute:

```text
loop finished
i = 3
```

Therefore every callback reads:

```text
same i
 ↓
3
```

---

### 20. How does `let` change it?

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

`let` in a `for` loop provides a distinct per-iteration lexical binding for the loop variable.

Conceptually:

```text
iteration 1 → i = 0
iteration 2 → i = 1
iteration 3 → i = 2
```

Each callback closes over the appropriate iteration's binding.

---

### 21. Practical use case for closures

A very common use is **encapsulation/private state**.

```js
function createRateLimiter(limit) {
    let count = 0;

    return function () {
        if (count >= limit) {
            return false;
        }

        count++;
        return true;
    };
}
```

`count` cannot be directly accessed by the caller.

Closures are also useful for:

* factories
* memoization
* event handlers
* configuration
* middleware
* private state
* maintaining state between calls

---

# `this`

### 22. How is `this` determined for a normal function?

For ordinary functions, `this` is primarily determined by the **invocation form**.

The common cases:

```js
obj.method()
```

→ `this` is `obj`.

```js
fn()
```

→ in strict mode, `this` is `undefined`.

```js
fn.call(obj)
```

→ `this` is explicitly `obj`.

```js
new Fn()
```

→ `this` is the newly created instance.

This is why:

> **For normal functions, look at the call site to determine `this`.**

---

### 23. How does `this` behave in an arrow function?

Arrow functions don't create their own `this`.

```js
const obj = {
    name: "Danishan",

    greet: () => {
        console.log(this.name);
    }
};
```

The arrow does **not** receive `obj` as `this`.

Instead, it captures `this` from its surrounding lexical context.

---

### 24. Why does extracting an object method change `this`?

Consider:

```js
const user = {
    name: "Danishan",

    greet() {
        console.log(this.name);
    }
};

user.greet();
```

Here:

```text
this → user
```

But:

```js
const greet = user.greet;

greet();
```

Now the function is called without the `user.` receiver.

So in strict mode:

```text
this → undefined
```

The function itself didn't change.

**The invocation changed.**

---

### 25. Why doesn't an arrow function inside an object make `this` the object?

Because object literals don't create a `this` lexical scope for the arrow.

```js
const user = {
    name: "Danishan",

    greet: () => {
        console.log(this.name);
    }
};
```

The arrow looks like an object method syntactically, but it doesn't use the object as its `this`.

Compare:

```js
const user = {
    name: "Danishan",

    greet() {
        console.log(this.name);
    }
};
```

This normal method gets:

```text
user.greet()
     ↓
this = user
```

---

### 26. `this` when called as a method?

```js
const user = {
    name: "Danishan",

    greet() {
        console.log(this.name);
    }
};

user.greet();
```

The member-access call:

```js
user.greet()
```

provides `user` as the receiver.

Therefore:

```text
this → user
```

---

### 27. `this` when called normally?

For an ordinary function:

```js
function test() {
    console.log(this);
}

test();
```

In **strict mode**:

```text
this → undefined
```

In non-strict/sloppy mode, a plain call can result in the global object as `this`.

This distinction matters in Node.js and browser environments because the surrounding execution mode matters.

---

# `call`, `apply`, `bind`

### 28. Difference between `call`, `apply`, and `bind`

| Method  | Invokes immediately? | Arguments           | Returns         |
| ------- | -------------------: | ------------------- | --------------- |
| `call`  |                ✅ Yes | Individual          | Function result |
| `apply` |                ✅ Yes | Array/array-like    | Function result |
| `bind`  |                 ❌ No | Individual / preset | New function    |

```js
fn.call(obj, 1, 2);

fn.apply(obj, [1, 2]);

const bound = fn.bind(obj, 1, 2);
bound();
```

---

### 29. Which invokes immediately?

```text
call  → Yes
apply → Yes
bind  → No
```

Example:

```js
function greet() {
    console.log("Hello");
}

greet.call(obj);   // executes now
greet.apply(obj);  // executes now

const fn = greet.bind(obj); // doesn't execute
fn();                       // executes now
```

---

### 30. Why does `bind` return a new function?

Because `bind` creates a new callable function whose invocation behavior has been configured with:

```text
this value
+
possibly some preset arguments
```

```js
const bound = greet.bind(user);
```

Conceptually:

```text
greet
  ↓
bind(user)
  ↓
new function
  ↓
bound()
  ↓
greet executes with bound this
```

---

### 31. Can `call`/`apply`/`bind` change an arrow function's `this`?

**No.**

```js
const fn = () => {
    console.log(this);
};

fn.call(obj);
fn.apply(obj);

const bound = fn.bind(obj);
bound();
```

None of these changes the arrow's lexical `this`.

This is because the arrow doesn't have its own `this` for these methods to replace.

---

### 32. What happens to function identity after `bind`?

Every `bind()` call creates a new function object.

```js
const a = greet.bind(user);
const b = greet.bind(user);

console.log(a === b);
```

Result:

```text
false
```

Even though both have the same underlying target and `this`.

This matters for event listeners and subscriptions.

---

# Deeper reasoning

### 33. Why are functions objects?

JavaScript treats functions as callable objects.

Therefore they can have properties:

```js
function greet() {}

greet.description = "Greeting";

console.log(greet.description);
```

So a function has two important aspects:

```text
FUNCTION
├── callable behavior
└── object behavior
```

This is why functions can be passed around while also having properties and object-related behavior.

---

### 34. Why aren't identical function expressions `===`?

Because each evaluation creates a distinct function object.

```js
const a = () => 10;
const b = () => 10;

a === b; // false
```

Think:

```text
a → Function Object A

b → Function Object B
```

Different objects → different identity.

But:

```js
const a = () => 10;
const b = a;

a === b; // true
```

Both variables point to the same object.

---

### 35. What happens when a function is passed as an argument?

The function value is passed to the parameter.

```js
function execute(fn) {
    fn();
}

function greet() {
    console.log("Hello");
}

execute(greet);
```

Conceptually:

```text
greet
  ↓
function value
  ↓
fn parameter
  ↓
fn()
```

No special "callback object" is created merely because the function is being used as a callback.

---

### 36. How does lexical scope interact with closures?

Lexical scope determines **where variables are looked up**.

A closure allows a function to retain access to that lexical environment.

```js
function outer() {
    const x = 10;

    return function inner() {
        return x;
    };
}
```

So:

```text
lexical definition
       ↓
inner knows outer scope
       ↓
closure retains access
       ↓
x remains accessible
```

---

### 37. How do closures and function identity matter in event handlers?

Consider:

```js
function handleClick() {
    console.log("clicked");
}

element.addEventListener("click", handleClick);
element.removeEventListener("click", handleClick);
```

The same function reference is used.

But:

```js
element.addEventListener("click", () => {
    console.log("clicked");
});

element.removeEventListener("click", () => {
    console.log("clicked");
});
```

These are **two different function objects**.

Therefore the second call does not remove the first listener.

Closures can also make handlers retain access to surrounding state.

---

### 38. Pure vs impure function?

A pure function generally:

1. produces the same result for the same relevant inputs
2. doesn't produce observable side effects

```js
function add(a, b) {
    return a + b;
}
```

An impure function may modify or depend on external state:

```js
let count = 0;

function increment() {
    count++;
}
```

Impure functions aren't inherently bad.

Real applications need side effects:

```text
database
network
filesystem
logging
external APIs
state updates
```

Good architecture often means **containing and controlling side effects**, rather than eliminating them.

---

### 39. When can a closure cause unexpected memory retention?

A closure can keep objects reachable as long as the closure itself remains reachable.

For example:

```js
function createHandler(largeObject) {
    return function handler() {
        console.log(largeObject.id);
    };
}

const handler = createHandler(largeObject);
```

If `handler` remains registered somewhere—such as an event system, subscription, timer, or long-lived object—it may keep `largeObject` reachable as well.

Conceptually:

```text
long-lived handler
       ↓
closure
       ↓
largeObject
```

So even if you no longer use `largeObject` directly, it may remain reachable.

This becomes a practical memory-management concern with:

* event listeners
* timers
* subscriptions
* caches
* long-lived services

---

### 40. How would you debug a complicated `this` problem?

Don't guess based on where the function was defined.

**Start at the call site.**

For an ordinary function, classify the invocation:

```text
1. obj.fn()
       ↓
   this = obj

2. fn()
       ↓
   strict mode → undefined

3. fn.call(obj)
       ↓
   this = obj

4. fn.apply(obj, args)
       ↓
   this = obj

5. fn.bind(obj)
       ↓
   new function with bound this

6. new Fn()
       ↓
   this = new instance

7. arrow function
       ↓
   lexical this
```

Then inspect whether the function was extracted:

```js
const fn = obj.method;
fn();
```

or wrapped:

```js
const fn = () => obj.method();
```

or bound:

```js
const fn = obj.method.bind(obj);
```

A very effective debugging technique is simply:

```js
console.log(this);
```

at the exact function execution point and then trace **how that function was invoked**.

---

# 🔥 SDE-2 Tricky Examples

These are worth practicing because interviewers often stop asking definitions and give you code.

### Example 1 — Function declaration hoisting

```js
sayHello();

function sayHello() {
    console.log("Hello");
}
```

**Output:**

```text
Hello
```

Because the function declaration is available before its textual position.

---

### Example 2 — `const` + function expression

```js
sayHello();

const sayHello = function () {
    console.log("Hello");
};
```

**Result:** `ReferenceError`.

The `sayHello` binding exists because of `const`, but it is in the **Temporal Dead Zone** until initialization.

---

### Example 3 — Default parameter

```js
function test(value = 10) {
    console.log(value);
}

test();
test(undefined);
test(null);
```

Output:

```text
10
10
null
```

Default parameters activate for `undefined`, not arbitrary falsy values.

---

### Example 4 — Rest

```js
function test(a, ...rest) {
    console.log(a);
    console.log(rest);
}

test(1, 2, 3, 4);
```

Output:

```text
1
[2, 3, 4]
```

---

### Example 5 — Function identity

```js
const a = function () {};
const b = function () {};

console.log(a === b);
```

```text
false
```

Different function objects.

---

### Example 6 — Closure

```js
function create() {
    let x = 10;

    return () => {
        x++;
        return x;
    };
}

const fn = create();

console.log(fn());
console.log(fn());
```

Output:

```text
11
12
```

The closure retains access to the same `x` binding.

---

### Example 7 — Method extraction

```js
const user = {
    name: "Danishan",

    greet() {
        console.log(this.name);
    }
};

user.greet();

const fn = user.greet;
fn();
```

First call:

```text
Danishan
```

Second call in strict mode:

```text
undefined
```

The important change is:

```text
user.greet()
     ↓
method call

fn()
     ↓
plain function call
```

---

### Example 8 — Arrow inside object

```js
const user = {
    name: "Danishan",

    greet: () => {
        console.log(this.name);
    }
};

user.greet();
```

Don't say:

> "`this` is user because it's inside user."

That's wrong.

The arrow captures `this` from its surrounding lexical environment.

---

### Example 9 — `bind` identity

```js
function greet() {}

const a = greet.bind({});
const b = greet.bind({});

console.log(a === b);
```

```text
false
```

Every `bind()` creates a new function object.

---

### Example 10 — `call` cannot override arrow `this`

```js
const obj = {
    name: "Danishan"
};

const fn = () => {
    console.log(this);
};

fn.call(obj);
```

`call(obj)` does **not** make the arrow's `this` become `obj`.

---

## The SDE-2 Mental Model

Don't memorize 40 isolated answers. Connect them like this:

```text
                         FUNCTION
                            │
              ┌─────────────┼──────────────┐
              ↓             ↓              ↓
          callable        object       lexical scope
              │             │              │
              ↓             ↓              ↓
           invoke        identity       closure
              │             │              │
              ↓             ↓              ↓
            return       bind creates    captured
                         new identity     variables
              │
              ↓
             this
              │
       ┌──────┼───────┐
       ↓      ↓       ↓
      call   apply   bind
```

And the **most important interview distinction** is:

```text
NORMAL FUNCTION
      ↓
this determined by invocation


ARROW FUNCTION
      ↓
this comes from lexical surrounding scope
```

Then separately:

```text
LEXICAL SCOPE
      ↓
closure
      ↓
retained access to variables
```

And:

```text
FUNCTIONS ARE OBJECTS
      ↓
identity matters
      ↓
callbacks / event handlers / bind / Map / Set
```

If you can reason through those three chains, you have most of the **SDE-2-level function concepts** covered.
