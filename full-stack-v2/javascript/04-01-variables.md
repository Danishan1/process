# 2. Variables, Declarations & Keywords

The central question is:

> **What exactly happens when JavaScript encounters `var`, `let`, or `const`?**

To answer that properly, we need to separate several concepts that beginners often treat as the same thing:

```text
Declaration
    ↓
Initialization
    ↓
Assignment
    ↓
Reassignment
    ↓
Redeclaration
```

Then we'll connect them to:

```text
Scope
  ↓
Hoisting
  ↓
Temporal Dead Zone
  ↓
Shadowing
  ↓
Global bindings
```

## 2.1 What Is a Variable?

Consider:

```js
let age = 25;
```

We casually say:

> "age is a variable containing 25."

That's useful, but technically there is a more precise mental model.

A variable is essentially a **binding between an identifier and a value**.

Conceptually:

```text
age ───────→ 25
```

The identifier is:

```text
age
```

and the current value associated with that binding is:

```text
25
```

Now:

```js
age = 30;
```

The binding changes:

```text
age ───────→ 30
```

This distinction becomes extremely important when we discuss objects:

```js
const user = {
    name: "Danishan"
};
```

Here:

```text
user ───────→ object
```

`const` prevents changing the **binding**, not necessarily modifying the object.

## 2.2 Declaration

A **declaration** introduces a binding.

For example:

```js
let age;
```

We've declared `age`.

At this point:

```text
age
```

exists as a binding, but we haven't explicitly provided its value.

For `let`, its value is `undefined` once execution reaches the declaration.

```js
let age;

console.log(age);
// undefined
```

Similarly:

```js
var age;
```

creates a variable whose initial value is `undefined`.

## 2.3 Initialization

Initialization means giving a binding its initial value.

```js
let age = 25;
```

Conceptually:

```text
Declaration + initialization
```

The declaration introduces:

```text
age
```

and initialization establishes:

```text
age → 25
```

Compare:

```js
let age;     // declaration
age = 25;    // assignment
```

versus:

```js
let age = 25; // declaration + initialization
```

These are not conceptually identical operations even though they often produce the same final state.

## 2.4 Assignment

Assignment changes the current value associated with an existing mutable binding.

```js
let age = 25;

age = 30;
```

Initially:

```text
age → 25
```

After assignment:

```text
age → 30
```

Assignment is:

```js
age = 30;
```

It is **not** a declaration.

Therefore:

```js
age = 30;
```

doesn't introduce a new local variable in the same way:

```js
let age = 30;
```

does.

## 2.5 Reassignment

Reassignment simply means assigning a new value to an existing binding.

```js
let x = 10;

x = 20;
x = 30;
```

The binding is reused:

```text
x → 10
 ↓
x → 20
 ↓
x → 30
```

This is allowed for `let` and `var`.

```js
let x = 10;
x = 20; // valid
```

```js
var x = 10;
x = 20; // valid
```

But:

```js
const x = 10;
x = 20;
```

throws:

```text
TypeError
```

because a `const` binding cannot be reassigned.

## 2.6 `const` Still Requires Initialization

This is an important rule.

This is invalid:

```js
const x;
```

Why?

Because `const` bindings must be initialized when declared.

Valid:

```js
const x = 10;
```

Invalid:

```js
const x;
x = 10;
```

So:

```text
var
  declaration → initialization may happen later

let
  declaration → initialization may happen later

const
  declaration + initialization must happen together
```

## 2.7 Redeclaration

Redeclaration means declaring the same binding again in the same scope.

For example:

```js
var x = 10;
var x = 20;
```

This is allowed.

But:

```js
let x = 10;
let x = 20;
```

in the same scope is a syntax error.

Likewise:

```js
const x = 10;
const x = 20;
```

is invalid.

So a useful first table is:

| Operation                | `var` | `let` | `const` |
| ------------------------ | ----- | ----- | ------- |
| Declaration              | ✅     | ✅     | ✅       |
| Initialize later         | ✅     | ✅     | ❌       |
| Reassignment             | ✅     | ✅     | ❌       |
| Same-scope redeclaration | ✅     | ❌     | ❌       |

But this table isn't enough. **Scope and hoisting are where the interesting behavior starts.**

## 2.8 Scope

Scope answers:

> **Where can this binding be accessed?**

JavaScript has several relevant scopes:

```text
Global scope
Function scope
Block scope
Module scope
```

The biggest difference among `var`, `let`, and `const` is their relationship with scope.

## 2.9 `var` Is Function-Scoped

Consider:

```js
function test() {
    var x = 10;

    if (true) {
        var x = 20;
    }

    console.log(x);
}
```

Output:

```text
20
```

Why?

Because `var` does **not** create a new binding for the `if` block.

`var` is function-scoped.

Conceptually:

```text
function test()
│
└── x
    │
    ├── declared here
    └── accessible throughout function
```

## 2.10 `let` and `const` Are Block-Scoped

Now:

```js
function test() {
    let x = 10;

    if (true) {
        let x = 20;
        console.log(x);
    }

    console.log(x);
}
```

Output:

```text
20
10
```

There are two different bindings.

Conceptually:

```text
function scope
│
└── x = 10
     │
     └── if-block
           │
           └── x = 20
```

The inner `x` shadows the outer `x`.

Same applies to `const`.

## 2.11 Why Block Scope Exists

Before `let` and `const`, JavaScript mainly had `var`.

This could produce surprising behavior:

```js
for (var i = 0; i < 3; i++) {
    // ...
}

console.log(i);
```

Output:

```text
3
```

because `i` is not scoped to the `for` block.

With `let`:

```js
for (let i = 0; i < 3; i++) {
    // ...
}

console.log(i);
```

we get:

```text
ReferenceError
```

because `i` belongs to the loop's block scope.

This difference becomes especially important with closures.

## 2.12 Hoisting

Now we get to one of the most abused terms in JavaScript:

> **Hoisting**

People often say:

> "JavaScript moves declarations to the top."

That's not literally what happens.

JavaScript does **not physically move your source code around**.

A better mental model is:

> During creation/initialization of the relevant execution environment, bindings are established before the corresponding code executes.

Different declaration types behave differently.

## 2.13 `var` and Hoisting

Consider:

```js
console.log(x);

var x = 10;
```

Output:

```text
undefined
```

A useful conceptual transformation is:

```js
var x;

console.log(x);

x = 10;
```

But remember: this is a **mental model**, not literal source-code rewriting.

The important behavior is:

```text
var binding exists
        ↓
initial value = undefined
        ↓
execution reaches assignment
        ↓
value becomes 10
```

So:

```js
console.log(x); // undefined

var x = 10;
```

doesn't throw a `ReferenceError`.

## 2.14 `let` and `const` Are Also Hoisted — But Differently

This is where many interview answers go wrong.

People often say:

> "`let` and `const` are not hoisted."

That's misleading.

Their bindings are established as part of environment setup, but they are **not initialized to `undefined` in the way `var` is**.

Consider:

```js
console.log(x);

let x = 10;
```

This produces:

```text
ReferenceError
```

Likewise:

```js
console.log(x);

const x = 10;
```

also produces a `ReferenceError`.

So the important difference is:

```text
var
│
├── binding established
└── initialized to undefined

let / const
│
├── binding established
└── not initialized yet
       ↓
    TDZ
```

## 2.15 Temporal Dead Zone

This brings us to the **TDZ**.

TDZ = **Temporal Dead Zone**.

Consider:

```js
console.log(x);

let x = 10;
```

Between entering the relevant scope and reaching:

```js
let x = 10;
```

the binding exists but cannot be accessed.

That period is called the **Temporal Dead Zone**.

Conceptually:

```text
Scope begins
     │
     ↓
x binding exists
     │
     │  ← TDZ
     │
     ↓
let x = 10
     │
     ↓
x initialized
     │
     ↓
x can now be accessed
```

Therefore:

```js
let x = 10;
console.log(x);
```

works.

But:

```js
console.log(x);
let x = 10;
```

doesn't.

## 2.16 `typeof` Does Not Always Save You

There is a famous behavior:

```js
console.log(typeof undeclaredVariable);
```

This returns:

```text
"undefined"
```

But:

```js
console.log(typeof x);

let x = 10;
```

throws a `ReferenceError`.

Why?

Because `x` is a lexical binding in its TDZ.

This is an excellent interview question.

#### Undeclared identifier

```js
typeof completelyUnknown
```

→ `"undefined"`

#### Lexical binding in TDZ

```js
typeof x;
let x = 10;
```

→ `ReferenceError`

So:

> `typeof` is not a universal "safe variable check."

## 2.17 Shadowing

Shadowing occurs when an inner scope declares a binding with the same name as a binding from an outer scope.

Example:

```js
let x = "outer";

{
    let x = "inner";

    console.log(x);
}

console.log(x);
```

Output:

```text
inner
outer
```

Conceptually:

```text
Global
└── x = "outer"
      │
      ↓
Block
└── x = "inner"
```

When JavaScript resolves:

```js
console.log(x);
```

inside the block, it finds the nearest `x`.

## 2.18 Shadowing With Functions

Example:

```js
const name = "global";

function test() {
    const name = "local";

    console.log(name);
}

test();
```

Output:

```text
local
```

The local binding shadows the global binding.

This is a direct application of the lexical environment model we studied previously.

## 2.19 `var` and Block Shadowing

Here's an interesting case:

```js
var x = 10;

{
    var x = 20;
}

console.log(x);
```

Output:

```text
20
```

There isn't a separate block-scoped `x` created by `var`.

Compare:

```js
let x = 10;

{
    let x = 20;
}

console.log(x);
```

Output:

```text
10
```

because the inner block has a separate binding.

## 2.20 Illegal Shadowing

There are cases where JavaScript rejects certain combinations of lexical and `var` declarations.

For example:

```js
let x = 10;

{
    var x = 20;
}
```

This is invalid.

Why?

Because `var x` would belong to the surrounding function/global scope, conflicting with the existing lexical `x`.

But:

```js
var x = 10;

{
    let x = 20;
}
```

is valid because the lexical `x` belongs to the inner block.

This is a good example of why simply memorizing "`let` is block scoped" isn't enough. You need to understand **which environment each declaration belongs to**.

## 2.21 Global Variables Are Special

Now consider:

```js
var x = 10;
```

at the top level of a classic browser script.

Historically, global `var` declarations can become properties of the global object:

```js
window.x
```

may correspond to the global variable.

But:

```js
let y = 20;
const z = 30;
```

do not create global-object properties in the same way.

So conceptually:

```text
Global scope
│
├── var x ─────────→ global object property
│
├── let y ─────────→ lexical global binding
│
└── const z ───────→ lexical global binding
```

However, **modules change the picture** because modules have their own module scope and are not simply classic global scripts.

This is one reason we should never answer:

> "A global variable is always a property of `window`."

That's too simplistic.

## 2.22 `var` vs `let` vs `const`

Now we can build a much more meaningful comparison.

| Characteristic              | `var`       | `let`                  | `const`                |
| --------------------------- | ----------- | ---------------------- | ---------------------- |
| Scope                       | Function    | Block                  | Block                  |
| Redeclaration in same scope | Yes         | No                     | No                     |
| Reassignment                | Yes         | Yes                    | No                     |
| Must initialize immediately | No          | No                     | Yes                    |
| Pre-declaration access      | `undefined` | TDZ / `ReferenceError` | TDZ / `ReferenceError` |
| Legacy behavior             | Yes         | Modern                 | Modern                 |
| Recommended for new code    | Usually no  | Yes                    | Usually preferred      |

The final row is a **style/practice recommendation**, not a language rule.

## 2.23 Why `const` Is Usually Preferred

Consider:

```js
const user = getUser();
```

If we don't intend to reassign the binding:

```js
user = anotherUser;
```

there's no reason to allow reassignment.

Using:

```js
const
```

communicates that intent.

But again:

```js
const user = {
    name: "Danishan"
};

user.name = "John";
```

is valid.

So `const` means:

> **The binding cannot be reassigned.**

It does **not** mean:

> **The referenced value is deeply immutable.**

## 2.24 A Particularly Important Interview Example

Consider:

```js
var a = 1;

function test() {
    console.log(a);
    var a = 2;
}

test();
```

What happens?

Output:

```text
undefined
```

Why?

Because the function has its own `var a`.

Conceptually:

```js
function test() {
    var a;

    console.log(a);

    a = 2;
}
```

The local `a` shadows the outer `a` from the beginning of the function's execution.

Therefore JavaScript does **not** look at the outer `a`.

## 2.25 Same Example With `let`

Now:

```js
let a = 1;

function test() {
    console.log(a);
    let a = 2;
}

test();
```

Output:

```text
ReferenceError
```

Why?

Because the local lexical `a` exists in the function's lexical environment and is in the TDZ when `console.log(a)` executes.

The outer `a` is **not used**.

This is a very important insight:

```text
Local binding exists
        ↓
It shadows outer binding
        ↓
But it is still in TDZ
        ↓
Access → ReferenceError
```

## 2.26 Loop Scoping: `var` vs `let`

This is one of the classic examples where scope and closures meet.

#### `var`

```js
for (var i = 0; i < 3; i++) {
    setTimeout(() => {
        console.log(i);
    }, 0);
}
```

The callbacks typically print:

```text
3
3
3
```

Why?

There is one function-scoped `i`.

Conceptually:

```text
i ──→ 3
 ↑
 ├── callback 1
 ├── callback 2
 └── callback 3
```

All callbacks access the same binding.

#### `let`

```js
for (let i = 0; i < 3; i++) {
    setTimeout(() => {
        console.log(i);
    }, 0);
}
```

The callbacks print:

```text
0
1
2
```

because the loop semantics for `let` provide per-iteration bindings that callbacks can capture.

This is a **beautiful example of multiple concepts interacting**:

```text
let
 ↓
block/iteration scoping
 ↓
lexical environments
 ↓
closures
 ↓
asynchronous callbacks
```

We'll revisit this when we study closures and the event loop.

## 2.27 Declaration vs Definition

You'll sometimes hear developers say:

```text
declaration
definition
```

as though they're universal JavaScript terminology.

Be careful.

In JavaScript, the most useful distinctions are:

```text
Declaration
Initialization
Assignment
```

For example:

```js
let x;
```

declaration.

```js
let x = 10;
```

declaration + initialization.

```js
x = 20;
```

assignment/reassignment.

The term **definition** can be used informally, but it doesn't carry the same central language-level distinction here that it does in some other languages.

## 2.28 Identifiers

An identifier is a name used to refer to things such as variables, functions, classes, etc.

Valid:

```js
const userName = "Danishan";
const _count = 10;
const $element = {};
```

Identifiers are case-sensitive:

```js
const user = 10;
const User = 20;
```

These are different identifiers.

```text
user ≠ User
```

An identifier cannot arbitrarily be a reserved keyword.

For example:

```js
const let = 10;
```

is invalid.

## 2.29 Keywords and Reserved Words

JavaScript has language keywords such as:

```text
let
const
var
function
return
if
else
for
class
extends
new
this
throw
try
catch
```

They have syntactic meaning and therefore cannot generally be used as ordinary identifiers in the corresponding contexts.

You don't need to memorize every reserved word at this stage.

The important distinction is:

```text
Identifier
    ↓
Name chosen by programmer

Keyword
    ↓
Special syntactic meaning defined by JavaScript
```

## 2.30 The Mental Model We Now Have

We can now connect this topic directly to the execution model.

Suppose:

```js
let x = 10;

function test() {
    let y = 20;

    console.log(x + y);
}
```

Conceptually:

```text
Global Lexical Environment
│
└── x = 10
       │
       ↓
Function execution
       │
       ↓
Function Lexical Environment
│
└── y = 20
       │
       ↓
Variable resolution
       │
       ├── y → found locally
       │
       └── x → search outer environment
```

And for:

```js
let x = 10;

function test() {
    console.log(x);
    let x = 20;
}
```

the process is:

```text
Function environment
│
└── x exists
       │
       ↓
     TDZ
       │
console.log(x)
       │
       ↓
ReferenceError
```

The outer `x` isn't used because the inner `x` shadows it.

## 2.31 The Three Declaration Keywords in One Picture

```text
                    DECLARATIONS
                         │
              ┌──────────┼──────────┐
              ↓          ↓          ↓
             var        let       const
              │          │          │
        function      block       block
         scoped       scoped      scoped
              │          │          │
       initialized    TDZ          TDZ
       to undefined     │            │
              │          │            │
       reassignment  reassignment   no reassignment
          allowed       allowed
              │          │
       redeclaration   no same-scope
          allowed      redeclaration
```

The most important conceptual distinction is:

```text
var
→ binding initialized to undefined

let / const
→ binding exists but remains uninitialized
→ TDZ until initialization
```

## 2.32 High-Probability SDE-2 Questions

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

## The One Thing I Want You to Internalize

Don't memorize:

> "`var` is hoisted, `let` isn't."

Instead memorize the **mechanism**:

```text
        Declaration
             │
             ↓
      Binding established
             │
       ┌─────┴─────┐
       ↓           ↓
      var       let/const
       │           │
       ↓           ↓
undefined       uninitialized
                   │
                   ↓
                  TDZ
                   │
                   ↓
             initialization
                   │
                   ↓
              usable binding
```

Once this model is clear, most `var`/`let`/`const` interview questions become **reasoning problems instead of memorization problems**.

And this topic now connects directly to the previous one:

```text
Execution Context
       ↓
Lexical Environment
       ↓
Bindings
       ↓
Scope
       ↓
Hoisting / Initialization
       ↓
TDZ
       ↓
Variable Resolution
       ↓
Shadowing
       ↓
Closures
```
