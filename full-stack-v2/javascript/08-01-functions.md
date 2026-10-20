Absolutely. **Functions** are one of the most important JavaScript topics for an SDE-2 interview because they connect almost everything else we've studied: values, scope, execution, objects, closures, callbacks, async programming, and `this`.

The key is not to memorize the different syntaxes. We want to understand **what a function actually is, how it is created, how it is called, what it captures, and what context it executes with.**

# 6. Functions

A JavaScript function is fundamentally a **callable value**.

```js
function add(a, b) {
    return a + b;
}
```

Here:

```text
add
 ↓
function value
 ↓
callable
 ↓
add(2, 3)
 ↓
5
```

But functions are also objects.

That means a function participates in two worlds:

```text
Function
 ├── can be called
 │      add()
 │
 └── is an object
        properties
        methods
        prototype-related behavior
```

This dual nature is extremely important in JavaScript.

---

# 6.1 Function Declaration

The traditional syntax is a function declaration:

```js
function add(a, b) {
    return a + b;
}
```

The function is given the name:

```js
add
```

and can be called:

```js
add(2, 3);
```

A declaration is part of the surrounding program structure.

One important property is that function declarations are **hoisted**.

```js
sayHello();

function sayHello() {
    console.log("Hello");
}
```

This works.

Conceptually, the function declaration is available before execution reaches its textual position.

---

# 6.2 Function Expression

A function can also be created as part of an expression:

```js
const add = function (a, b) {
    return a + b;
};
```

Here there are two things:

```text
function value
     ↓
assigned to
     ↓
const add
```

Unlike the declaration:

```js
function add() {}
```

we are creating a function value and assigning it to a variable.

For example:

```js
const add = function (a, b) {
    return a + b;
};

console.log(add(2, 3));
```

---

# 6.3 Declaration vs Expression

Compare:

```js
sayHello();

function sayHello() {
    console.log("Hello");
}
```

with:

```js
sayHello();

const sayHello = function () {
    console.log("Hello");
};
```

The second produces an error because the variable binding exists differently from the function declaration's callable availability.

This connects directly to:

* hoisting
* `let`
* `const`
* temporal dead zone
* execution contexts

So function syntax isn't merely stylistic.

It can affect when the function becomes usable.

---

# 6.4 Arrow Functions

Arrow functions provide a shorter function syntax.

```js
const add = (a, b) => {
    return a + b;
};
```

For a single expression, you can use an implicit return:

```js
const add = (a, b) => a + b;
```

These are equivalent in returned result:

```js
const add = (a, b) => {
    return a + b;
};
```

and:

```js
const add = (a, b) => a + b;
```

But arrow functions are **not simply shorter normal functions**.

The most important semantic difference is their behavior regarding:

```text
this
arguments
super
new
```

In particular, arrow functions **do not have their own `this` binding**.

We'll return to this in detail.

---

# 6.5 Anonymous Functions

A function without a name is an anonymous function.

```js
const fn = function () {
    console.log("Hello");
};
```

The function itself has no explicit name in the source.

Anonymous functions are commonly passed as callbacks:

```js
setTimeout(function () {
    console.log("Done");
}, 1000);
```

The function is created and passed directly.

---

# 6.6 Named Functions

A function expression can also have a name:

```js
const fn = function calculateTotal() {
    return 100;
};
```

Here:

```text
variable name → fn
function name → calculateTotal
```

These are not conceptually the same thing.

The internal function name can be particularly useful for debugging and recursion.

For example:

```js
const factorial = function calculate(n) {
    if (n <= 1) {
        return 1;
    }

    return n * calculate(n - 1);
};
```

The recursive function can refer to its own function name.

---

# 6.7 Parameters vs Arguments

This distinction is frequently asked.

Consider:

```js
function add(a, b) {
    return a + b;
}

add(10, 20);
```

Here:

```text
a, b
↓
parameters
```

and:

```text
10, 20
↓
arguments
```

So:

> **Parameters are variables defined by the function. Arguments are values supplied when calling it.**

---

# 6.8 Default Parameters

JavaScript allows default parameter values.

```js
function greet(name = "Guest") {
    return `Hello ${name}`;
}
```

Then:

```js
greet();
```

returns:

```text
Hello Guest
```

while:

```js
greet("Danishan");
```

returns:

```text
Hello Danishan
```

The default is used when the corresponding argument is `undefined`.

For example:

```js
greet(undefined);
```

uses the default.

But:

```js
greet(null);
```

does not.

`null` is an explicitly supplied value.

This is an excellent connection to our earlier discussion of:

```text
null vs undefined
```

---

# 6.9 Default Parameters Can Reference Earlier Parameters

For example:

```js
function createUser(name, role = "user") {
    return {
        name,
        role
    };
}
```

The parameter environment is established as part of function invocation.

Defaults can also depend on earlier parameters:

```js
function multiply(a, b = a * 2) {
    return b;
}
```

```js
multiply(5);
```

returns:

```text
10
```

But parameter defaults cannot freely reference later parameters.

This is another area where execution order matters.

---

# 6.10 Rest Parameters

Rest parameters collect remaining arguments into an array.

```js
function sum(...numbers) {
    return numbers;
}
```

Calling:

```js
sum(1, 2, 3, 4);
```

makes:

```js
numbers
```

an array:

```js
[1, 2, 3, 4]
```

Example:

```js
function sum(...numbers) {
    let total = 0;

    for (const number of numbers) {
        total += number;
    }

    return total;
}
```

Now:

```js
sum(10, 20, 30);
```

returns:

```text
60
```

The key idea:

```text
...numbers
     ↓
remaining arguments
     ↓
array
```

---

# 6.11 Rest vs Spread

The same `...` syntax appears in two different concepts.

### Rest

Collects values:

```js
function fn(...args) {}
```

### Spread

Expands values:

```js
fn(...args);
```

Think:

```text
rest   → collect
spread → expand
```

This distinction is commonly tested.

---

# 6.12 Return Values

A function can return a value:

```js
function square(x) {
    return x * x;
}
```

```js
const result = square(5);
```

Now:

```text
result = 25
```

`return` immediately terminates the current function invocation.

```js
function test() {
    console.log("A");
    return;
    console.log("B");
}
```

Output:

```text
A
```

The second statement is unreachable during that invocation.

---

# 6.13 What Happens Without `return`?

If a function reaches its end without returning a value:

```js
function test() {
    console.log("Hello");
}
```

then:

```js
const result = test();
```

produces:

```js
undefined
```

So:

```text
no explicit return
        ↓
undefined
```

This is different from returning `null`.

```js
return null;
```

explicitly returns `null`.

---

# 6.14 Functions Are First-Class Values

This is one of the most fundamental JavaScript concepts.

Functions can be:

* assigned to variables
* stored in objects
* stored in arrays
* passed as arguments
* returned from functions

For example:

```js
const greet = function () {
    console.log("Hello");
};
```

A function can be stored in an array:

```js
const operations = [
    () => 10,
    () => 20
];
```

And retrieved:

```js
operations[0]();
```

This works because functions are values.

---

# 6.15 Functions as Arguments

```js
function execute(fn) {
    fn();
}

function greet() {
    console.log("Hello");
}

execute(greet);
```

The function:

```js
greet
```

is passed as a value.

Notice:

```js
execute(greet);
```

versus:

```js
execute(greet());
```

These are completely different.

### `greet`

means:

> Give the function itself.

### `greet()`

means:

> Call the function now and give me its return value.

This distinction is absolutely fundamental for callbacks.

---

# 6.16 Higher-Order Functions

A higher-order function is a function that operates on other functions.

Typically, it either:

1. accepts a function as an argument, or
2. returns a function.

Example:

```js
function execute(fn) {
    return fn();
}
```

`execute` is higher-order because it accepts a function.

Another example:

```js
function createMultiplier(x) {
    return function (y) {
        return x * y;
    };
}
```

This returns a function.

Therefore:

```js
const double = createMultiplier(2);

double(5);
```

returns:

```text
10
```

This leads directly into **closures**.

---

# 6.17 Callback Functions

A callback is simply a function passed somewhere so that another piece of code can invoke it.

Example:

```js
function processUser(user, callback) {
    callback(user);
}
```

Usage:

```js
processUser(
    { name: "Danishan" },
    user => console.log(user.name)
);
```

Here:

```text
user => console.log(user.name)
```

is the callback.

The important point is:

> "Callback" describes the **role** of the function, not a special function type.

A callback can be:

* synchronous
* asynchronous
* named
* anonymous
* arrow function
* normal function

---

# 6.18 Function Composition

Function composition means combining smaller functions so that the output of one becomes the input of another.

Suppose:

```js
const double = x => x * 2;

const increment = x => x + 1;
```

We can compose them:

```js
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

A reusable composition function might be:

```js
function compose(f, g) {
    return x => f(g(x));
}
```

Then:

```js
const doubleThenIncrement =
    compose(increment, double);

doubleThenIncrement(5);
```

returns:

```text
11
```

Composition becomes important in functional programming and middleware-style architectures.

---

# 6.19 Nested Functions

Functions can be defined inside other functions.

```js
function outer() {
    function inner() {
        console.log("Hello");
    }

    inner();
}
```

The inner function has access to the lexical environment of the outer function.

For example:

```js
function outer() {
    const message = "Hello";

    function inner() {
        console.log(message);
    }

    inner();
}
```

`inner()` can access:

```js
message
```

even though `message` is not declared inside `inner`.

This is the foundation of **closures**.

---

# 6.20 Closures

A closure is one of the most important SDE-2 JavaScript concepts.

Consider:

```js
function createCounter() {
    let count = 0;

    return function () {
        count++;
        return count;
    };
}
```

Now:

```js
const counter = createCounter();
```

The outer function has already finished executing.

Yet:

```js
counter();
```

returns:

```text
1
```

and:

```js
counter();
```

returns:

```text
2
```

and:

```js
counter();
```

returns:

```text
3
```

Why does `count` still exist?

Because the returned function maintains access to the lexical environment in which it was created.

Conceptually:

```text
createCounter()
      │
      ├── count = 0
      │
      └── returns function
               │
               ↓
         closure captures
         lexical environment
               │
               ↓
        count remains accessible
```

This is closure.

---

# 6.21 Lexical Capture

JavaScript uses **lexical scoping**.

That means variable resolution is determined by where functions are **defined**, not where they are called.

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

Even if `fn()` is called somewhere else, `inner` was defined inside `outer`.

Therefore it resolves:

```text
x
↓
inner's scope
↓
outer's scope
↓
global scope
```

not based on the location where `fn()` is called.

---

# 6.22 Closure ≠ Only Nested Function

A common oversimplification is:

> "A closure is a nested function."

That's not precise enough.

The important relationship is:

```text
function
+
lexical environment it retains access to
=
closure behavior
```

A nested function often creates a closure, but the important concept is **lexical capture**.

---

# 6.23 Function Scope

Variables declared using `var` inside a function are function-scoped.

```js
function test() {
    var x = 10;
}

console.log(x);
```

This fails because `x` is local to the function.

But `let` and `const` are block-scoped:

```js
function test() {
    if (true) {
        let x = 10;
        const y = 20;
    }

    // x and y unavailable here
}
```

This distinction becomes important when closures capture variables from loops.

---

# 6.24 Classic Closure + Loop Problem

Consider:

```js
for (var i = 0; i < 3; i++) {
    setTimeout(() => {
        console.log(i);
    }, 0);
}
```

Many developers expect:

```text
0
1
2
```

But the output is:

```text
3
3
3
```

Why?

Because `var` creates one function-scoped binding for `i`.

The callbacks close over that same binding.

By the time the callbacks execute:

```text
i = 3
```

So all callbacks observe:

```text
3
```

With `let`:

```js
for (let i = 0; i < 3; i++) {
    setTimeout(() => {
        console.log(i);
    }, 0);
}
```

the behavior is different because each iteration gets its own lexical binding.

This is a **very high-value SDE-2 closure question**.

---

# 6.25 IIFE

IIFE stands for:

> **Immediately Invoked Function Expression**

Example:

```js
(function () {
    console.log("Runs immediately");
})();
```

The function is:

1. created
2. treated as an expression
3. immediately invoked

Another syntax:

```js
(() => {
    console.log("Runs immediately");
})();
```

Historically, IIFEs were commonly used to create private scopes before modern module systems became standard.

Example:

```js
const counter = (() => {
    let count = 0;

    return {
        increment() {
            count++;
        },

        getCount() {
            return count;
        }
    };
})();
```

Here `count` isn't directly accessible from outside.

---

# 6.26 Recursive Functions

A recursive function calls itself.

Example:

```js
function factorial(n) {
    if (n <= 1) {
        return 1;
    }

    return n * factorial(n - 1);
}
```

For:

```js
factorial(4);
```

the conceptual execution is:

```text
factorial(4)
    ↓
4 × factorial(3)
        ↓
        3 × factorial(2)
                ↓
                2 × factorial(1)
                        ↓
                        1
```

Then results return upward:

```text
1
↓
2
↓
6
↓
24
```

Every recursive algorithm needs a **base case**.

Without one, the calls can continue until the runtime reaches its call-stack limit.

---

# 6.27 Pure Functions

A pure function generally satisfies two important properties:

1. Same input → same output.
2. No observable side effects.

Example:

```js
function add(a, b) {
    return a + b;
}
```

Given:

```text
add(2, 3)
```

it always returns:

```text
5
```

and doesn't modify external state.

---

# 6.28 Impure Functions

An impure function depends on or modifies external state.

```js
let count = 0;

function increment() {
    count++;
}
```

The function changes external state.

Another example:

```js
function getRandomNumber() {
    return Math.random();
}
```

The same inputs don't guarantee the same result because it depends on external state/randomness.

Impurity isn't automatically bad.

Real applications necessarily perform side effects:

```text
database
network
filesystem
logging
state mutation
```

The important engineering skill is knowing where those effects occur and controlling them.

---

# 6.29 Function Objects

Functions are objects.

For example:

```js
function greet() {
    console.log("Hello");
}

greet.description = "Greeting function";
```

Now:

```js
console.log(greet.description);
```

works.

You can attach properties to the function.

```js
function add(a, b) {
    return a + b;
}

add.version = 1;
```

So:

```text
add
 ├── callable
 └── object with properties
```

This is one of the fundamental characteristics of JavaScript.

---

# 6.30 Function Identity

Functions are objects, so each separately created function is a distinct object.

Consider:

```js
const a = () => 10;
const b = () => 10;

console.log(a === b);
```

Output:

```text
false
```

Even though their code looks identical.

Why?

Because:

```text
a → function object A

b → function object B
```

Two different function objects.

But:

```js
const a = () => 10;
const b = a;

console.log(a === b);
```

returns:

```text
true
```

because both variables reference the same function object.

This is **function identity**.

---

# 6.31 Why Function Identity Matters

This becomes very important in real applications.

For example:

```js
element.addEventListener("click", handler);
```

Later:

```js
element.removeEventListener("click", handler);
```

The same function reference is required.

This:

```js
element.removeEventListener(
    "click",
    () => {}
);
```

doesn't refer to the original function.

The newly created arrow function is a different object.

This same identity issue appears with:

* event handlers
* memoization
* React rendering
* caches
* Maps/Sets
* callback registration
* subscriptions

---

# 6.32 Function Context

Now we reach one of the most important parts:

```text
this
call
apply
bind
```

A function's **context** is primarily relevant to how `this` is determined when the function executes.

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
this
 ↓
user
```

Therefore:

```text
Danishan
```

is printed.

But this isn't because the function was physically defined inside the object.

The invocation form matters.

---

# 6.33 `this` Is Not Lexical for Normal Functions

Consider:

```js
const user = {
    name: "Danishan",

    greet: function () {
        console.log(this.name);
    }
};
```

Calling:

```js
user.greet();
```

makes:

```text
this → user
```

But:

```js
const greet = user.greet;

greet();
```

does not preserve the original `user` receiver.

The function was extracted from the object.

This leads to the key rule:

> For ordinary functions, `this` is determined by **how the function is called**, not simply where it was defined.

---

# 6.34 Arrow Functions and `this`

Arrow functions behave differently.

They don't create their own `this`.

Example:

```js
const user = {
    name: "Danishan",

    greet: () => {
        console.log(this.name);
    }
};
```

You should **not** expect:

```text
this → user
```

just because the arrow function appears inside the object.

Instead, the arrow function captures `this` lexically from its surrounding environment.

This is one of the most important differences between normal functions and arrow functions.

---

# 6.35 `call`

`call` invokes a function while explicitly specifying its `this` value.

```js
function greet() {
    console.log(this.name);
}

const user = {
    name: "Danishan"
};

greet.call(user);
```

Now:

```text
this → user
```

and output:

```text
Danishan
```

The first argument to `call` specifies the `this` value.

Additional arguments are passed individually:

```js
function add(a, b) {
    return this.base + a + b;
}

const obj = {
    base: 10
};

add.call(obj, 2, 3);
```

Result:

```text
15
```

---

# 6.36 `apply`

`apply` is similar to `call`.

The major difference is how arguments are supplied.

### `call`

```js
fn.call(thisArg, arg1, arg2, arg3);
```

### `apply`

```js
fn.apply(thisArg, [arg1, arg2, arg3]);
```

Example:

```js
function add(a, b) {
    return this.base + a + b;
}

add.apply({ base: 10 }, [2, 3]);
```

returns:

```text
15
```

So:

```text
call
→ arguments individually

apply
→ arguments as an array-like collection
```

---

# 6.37 `bind`

`bind` is different.

It **doesn't immediately invoke the function**.

Instead, it creates a new function with a permanently bound `this` value.

```js
function greet() {
    console.log(this.name);
}

const user = {
    name: "Danishan"
};

const boundGreet = greet.bind(user);

boundGreet();
```

`bind()` returns a new function.

Conceptually:

```text
greet
 ↓
bind(user)
 ↓
new function
 ↓
this permanently associated with user
```

---

# 6.38 `call` vs `apply` vs `bind`

Memorize the behavior, not just the syntax:

| Method  | Invokes immediately? | How arguments are supplied? |
| ------- | -------------------: | --------------------------- |
| `call`  |                  Yes | Individually                |
| `apply` |                  Yes | Array/array-like            |
| `bind`  |                   No | Creates new function        |

Example:

```js
fn.call(obj, 1, 2);

fn.apply(obj, [1, 2]);

const newFn = fn.bind(obj, 1, 2);
newFn();
```

---

# 6.39 A Major Interview Trap: `bind` Creates a New Function

Consider:

```js
const bound1 = greet.bind(user);
const bound2 = greet.bind(user);

console.log(bound1 === bound2);
```

The result is:

```text
false
```

because every `bind()` call creates a new function object.

This connects our concepts:

```text
bind
 ↓
new function object
 ↓
different identity
```

---

# 6.40 Arrow Functions Cannot Be Rebound in the Same Way

Because arrow functions don't have their own `this`, methods like:

```js
call
apply
bind
```

cannot change their lexical `this` in the way they can for ordinary functions.

For example:

```js
const greet = () => {
    console.log(this.name);
};

greet.call(user);
```

The `call(user)` does not make the arrow function's `this` become `user`.

This is a classic interview question.

---

# 6.41 The Complete Function Mental Model

At SDE-2 level, think about a function as:

```text
                    FUNCTION
                       │
       ┌───────────────┼────────────────┐
       ↓               ↓                ↓
   callable         object          lexical info
       │               │                │
       ↓               ↓                ↓
    invoke          identity         closure
       │               │                │
       ↓               ↓                ↓
     return        properties       captured vars
       │
       ↓
     context
       │
       ↓
      this
       │
       ├── call
       ├── apply
       └── bind
```

This gives you a much stronger understanding than simply memorizing:

```text
function
arrow
callback
closure
this
```

as separate topics.

They are deeply connected.

---

# 6.42 Function Execution: The Question to Ask

Whenever you see a function, ask:

### 1. How was the function created?

```js
function f() {}
```

or:

```js
const f = function () {};
```

or:

```js
const f = () => {};
```

### 2. What variables can it access?

Look at its lexical environment.

### 3. What arguments does it receive?

```js
f(a, b);
```

### 4. What does it return?

```js
return value;
```

### 5. Is it being passed around?

Then it may be a callback or higher-order function.

### 6. Does it capture variables?

Then closures are involved.

### 7. What is its `this`?

For normal functions, inspect the call site.

For arrow functions, inspect the surrounding lexical context.

### 8. Is the function identity important?

For example:

```js
a === b
```

### 9. Is it pure or does it cause side effects?

These questions let you reason about unfamiliar JavaScript code rather than merely recognize syntax.

---

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

---

# 6.44 The Five Concepts You Absolutely Must Master

For SDE-2, I would go especially deep into these five:

```text
                 FUNCTIONS
                     │
       ┌─────────────┼──────────────┐
       ↓             ↓              ↓
   Closures        `this`       Higher-order
       │             │              │
       ↓             ↓              ↓
 lexical scope   call/apply/bind   callbacks
       │
       ↓
 lexical capture
```

And alongside these:

```text
function identity
function objects
function invocation
```

These are the areas where interviewers can stop asking syntax questions and start giving you unfamiliar code and asking:

> **"What exactly happens when this executes?"**

That is the level we should target for SDE-2.
