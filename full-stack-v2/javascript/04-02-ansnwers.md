
# 2.32 Questions on Variables

I would prioritize these heavily.

#### Basic but essential

1. What is a variable in JavaScript?
2. What is a binding?
3. What's the difference between declaration and initialization?
4. What's the difference between assignment and initialization?
5. What's the difference between reassignment and redeclaration?
6. Why must `const` be initialized immediately?

#### Scope

7. What is scope?
8. What is the difference between function scope and block scope?
9. Why is `var` function-scoped?
10. Why are `let` and `const` block-scoped?
11. What is module scope?
12. What is global scope?

#### Hoisting

13. What is hoisting really?
14. Is `let` hoisted?
15. Is `const` hoisted?
16. Why does `var` return `undefined` before declaration?
17. Why does `let` throw `ReferenceError` before declaration?

#### TDZ

18. What is the Temporal Dead Zone?
19. When does the TDZ start?
20. When does the TDZ end?
21. Why does this fail?

```js
console.log(x);
let x = 10;
```

22. Why does this behave differently?

```js
console.log(typeof x);
let x = 10;
```

#### Shadowing

23. What is variable shadowing?
24. Can `let` shadow `let`?
25. Can `let` shadow `var`?
26. What is illegal shadowing?
27. Why does this produce `undefined`?

```js
var x = 10;

function test() {
    console.log(x);
    var x = 20;
}
```

28. Why does changing `var` to `let` produce a `ReferenceError`?

#### Practical

29. Explain the difference between:

```js
var x = 10;
let y = 10;
const z = 10;
```

30. Why do modern JavaScript developers generally prefer `const`/`let` over `var`?


## Answers

### 1. What is a variable in JavaScript?

A variable is a **name associated with a value through a binding**.

```js
let x = 10;
```

Here, `x` is the variable name, and the binding associates `x` with the value `10`.

A useful distinction:

> **Variable is the name we use; binding is the association between that name and its value.**

### 2. What is a binding?

A **binding associates an identifier with a value within a particular scope/environment**.

```js
let x = 10;
```

Conceptually:

```text
x ─────→ 10
↑
binding
```

With `const`:

```js
const user = {};
```

The binding `user` cannot be reassigned, but the object can still be mutated.

### 3. What's the difference between declaration and initialization?

**Declaration** establishes the binding.

```js
let x;
```

The binding `x` exists, but it hasn't been initialized with a usable value yet.

**Initialization** gives the binding its initial value.

```js
let x = 10;
```

So:

```text
Declaration
    ↓
binding exists
    ↓
Initialization
    ↓
binding gets initial value
```

### 4. What's the difference between assignment and initialization?

**Initialization** happens when a binding receives its **first value**.

```js
let x = 10; // initialization
```

**Assignment** changes the value of an already initialized binding.

```js
x = 20; // assignment
```

Think:

```text
let x = 10
    ↑
initialization

x = 20
↑
assignment
```

### 5. What's the difference between reassignment and redeclaration?

**Reassignment** changes the value of an existing binding:

```js
let x = 10;

x = 20;
```

**Redeclaration** attempts to declare the same identifier again in the same scope:

```js
let x = 10;
let x = 20; // ❌ SyntaxError
```

With `var`, redeclaration is allowed:

```js
var x = 10;
var x = 20; // ✅
```

### 6. Why must `const` be initialized immediately?

Because a `const` binding **cannot exist in an uninitialized usable state and later receive its first value through assignment**.

```js
const x; // ❌ SyntaxError
```

It must be:

```js
const x = 10;
```

Conceptually:

```text
const
  ↓
binding created
  ↓
must be initialized
  ↓
cannot be reassigned
```

# Scope

### 7. What is scope?

**Scope determines where an identifier can be accessed.**

```js
function test() {
    let x = 10;

    console.log(x); // ✅
}

console.log(x); // ❌
```

`x` is accessible only within its scope.

### 8. What is the difference between function scope and block scope?

**Function scope:** the variable is accessible throughout the function.

```js
function test() {
    if (true) {
        var x = 10;
    }

    console.log(x); // 10
}
```

`var` is function-scoped.

**Block scope:** the variable is accessible only inside the `{ }` block where it is declared.

```js
if (true) {
    let x = 10;
}

console.log(x); // ❌
```

`let` and `const` are block-scoped.

### 9. Why is `var` function-scoped?

This is part of JavaScript's language semantics and historical design.

A `var` declaration belongs to the **nearest function scope** rather than the nearest block scope.

```js
function test() {
    if (true) {
        var x = 10;
    }

    console.log(x); // 10
}
```

The `if` block doesn't create a separate scope for `var`.

### 10. Why are `let` and `const` block-scoped?

They were introduced with block-scoping semantics so variables could be limited to the block where they're needed.

```js
if (true) {
    let x = 10;
    const y = 20;
}

console.log(x); // ❌
console.log(y); // ❌
```

This reduces accidental access and name conflicts.

### 11. What is module scope?

When JavaScript is written as an **ES module**, declarations at the top level belong to the module's scope.

```js
// module.js

const x = 10;
```

`x` is not automatically a property of the global object.

Modules therefore provide their own scope and help prevent global namespace pollution.

### 12. What is global scope?

Global scope is the outermost scope of a JavaScript execution environment.

An identifier in global scope can potentially be accessed from many parts of the program, subject to the environment and language rules.

For example:

```js
const x = 10;

function test() {
    console.log(x); // 10
}
```

`test()` can access `x` because scope lookup can move outward.

# Hoisting

### 13. What is hoisting really?

Don't think:

> "JavaScript moves declarations to the top."

That's a useful beginner analogy, but not the actual mechanism.

A better model is:

> **Before executing the code, JavaScript establishes bindings according to the language's execution semantics. Different declarations are initialized differently.**

For example:

```js
console.log(x);

var x = 10;
```

The `var` binding already exists when `console.log` executes, and its initial value is `undefined`.

```text
Declaration
    ↓
Binding established
    ↓
var → initialized to undefined
```

For `let`/`const`, the binding exists but remains uninitialized until its declaration is evaluated.

### 14. Is `let` hoisted?

**Yes, in the sense that its binding is established before execution reaches the declaration.**

But it is **not initialized to `undefined` like `var`**.

```js
console.log(x); // ❌ ReferenceError

let x = 10;
```

The binding exists, but it is uninitialized.

### 15. Is `const` hoisted?

Same principle as `let`.

```js
console.log(x); // ❌ ReferenceError

const x = 10;
```

The `const` binding exists but is uninitialized until its declaration is evaluated.

### 16. Why does `var` return `undefined` before declaration?

Consider:

```js
console.log(x);

var x = 10;
```

The `var` binding has already been established and initialized with `undefined`.

Conceptually:

```text
Before execution:

x → undefined
```

Then:

```js
x = 10;
```

updates it.

Therefore:

```js
console.log(x); // undefined
```

### 17. Why does `let` throw `ReferenceError` before declaration?

Because its binding exists but is **uninitialized**.

```js
console.log(x);

let x = 10;
```

Conceptually:

```text
x → uninitialized
```

Trying to access that binding before initialization causes:

```text
ReferenceError
```

This period is the **Temporal Dead Zone**.

# Temporal Dead Zone

### 18. What is the Temporal Dead Zone?

The **TDZ** is the period between the creation of a `let`/`const` binding and its initialization during execution.

```js
{
    // TDZ for x

    console.log(x); // ❌

    let x = 10;

    // x is usable
}
```

### 19. When does the TDZ start?

It starts when the `let` or `const` binding is **created/established for that scope**, before the declaration is initialized.

Conceptually:

```text
Scope starts
    ↓
binding x exists
    ↓
TDZ
    ↓
let x = 10
    ↓
initialized
```

### 20. When does the TDZ end?

The TDZ ends when execution reaches and evaluates the declaration's initialization.

```js
let x = 10;
```

After this point, `x` can be accessed normally.

### 21. Why does this fail?

```js
console.log(x);

let x = 10;
```

Because `x` exists as a binding but is still **uninitialized** when `console.log(x)` executes.

Therefore:

```text
x → uninitialized
       ↓
access
       ↓
ReferenceError
```

### 22. Why does this behave differently?

```js
console.log(typeof x);

let x = 10;
```

A common misconception is:

> "`typeof` never throws."

That's not always true.

Because `x` is a lexical binding in the TDZ, accessing it with `typeof` still causes a `ReferenceError`.

```js
console.log(typeof x);
// ReferenceError
```

Compare with an identifier that doesn't exist at all:

```js
console.log(typeof doesNotExist);
// "undefined"
```

That's the important distinction:

```text
undeclared identifier
→ typeof safely returns "undefined"

declared let/const in TDZ
→ ReferenceError
```

# Shadowing

### 23. What is variable shadowing?

Shadowing happens when an inner scope declares an identifier with the **same name** as one in an outer scope.

```js
let x = 10;

function test() {
    let x = 20;

    console.log(x); // 20
}
```

The inner `x` shadows the outer `x`.

### 24. Can `let` shadow `let`?

**Yes**, when they are in different nested scopes.

```js
let x = 10;

{
    let x = 20;

    console.log(x); // 20
}
```

But this is illegal in the **same scope**:

```js
let x = 10;
let x = 20; // ❌
```

### 25. Can `let` shadow `var`?

**Yes**, in a nested scope.

```js
var x = 10;

{
    let x = 20;

    console.log(x); // 20
}
```

However, there are specific cases of **illegal shadowing** involving `var` crossing certain lexical scopes.

### 26. What is illegal shadowing?

A classic example:

```js
let x = 10;

{
    var x = 20; // ❌ SyntaxError
}
```

Why?

Because `var` is function-scoped. The `var x` effectively conflicts with the existing lexical `x` in the same function/global environment.

This is different from:

```js
var x = 10;

{
    let x = 20; // ✅
}
```

because the `let` is confined to the inner block.

### 27. Why does this produce `undefined`?

```js
var x = 10;

function test() {
    console.log(x);

    var x = 20;
}
```

The inner `var x` creates a **function-scoped binding** that shadows the outer `x`.

Its binding is initialized to `undefined` before execution of the function body.

Conceptually:

```js
function test() {
    var x;           // established + initialized undefined

    console.log(x);  // undefined

    x = 20;
}
```

So the outer `x = 10` is never used.

### 28. Why does changing `var` to `let` produce a `ReferenceError`?

```js
let x = 10;

function test() {
    console.log(x);

    let x = 20;
}
```

The inner `let x` shadows the outer `x`.

Therefore, when `console.log(x)` executes, JavaScript looks in the function's lexical environment first:

```text
inner x → uninitialized
```

It finds the inner `x`, so it **doesn't continue looking for the outer `x`**.

Because the inner `x` is still in its TDZ:

```text
ReferenceError
```

This is a very important scope rule:

> **Once name resolution finds a matching binding in the nearest scope, it doesn't skip that binding just because it is currently uninitialized.**

# Practical

### 29. Explain the difference between:

```js
var x = 10;
let y = 10;
const z = 10;
```

| Feature                     | `var`                                         | `let`                              | `const`                            |
| --------------------------- | --------------------------------------------- | ---------------------------------- | ---------------------------------- |
| Scope                       | Function                                      | Block                              | Block                              |
| Redeclaration               | ✅                                             | ❌                                  | ❌                                  |
| Reassignment                | ✅                                             | ✅                                  | ❌                                  |
| Must initialize immediately | ❌                                             | ❌                                  | ✅                                  |
| TDZ                         | ❌                                             | ✅                                  | ✅                                  |
| Hoisted                     | Binding established + initialized `undefined` | Binding established, uninitialized | Binding established, uninitialized |

The biggest conceptual difference is **how the binding is established and initialized**.

### 30. Why do modern JavaScript developers generally prefer `const`/`let` over `var`?

Because `let` and `const` provide **more predictable scoping and safer variable semantics**.

They:

* avoid function-scope leakage from blocks
* prevent accidental redeclaration
* expose access-before-initialization through the TDZ
* make reassignment explicit
* make intent clearer

Generally:

```js
const user = {};
```

means:

> "This binding should not be reassigned."

And:

```js
let count = 0;
count++;
```

means:

> "This binding is expected to change."

`var` is still valid JavaScript, but `let`/`const` generally provide better control over scope and bindings.
