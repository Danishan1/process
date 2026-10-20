# 1. JavaScript Language Fundamentals

### What we should be able to answer after this section

At SDE-2 level, we shouldn't merely know definitions such as:

> "JavaScript is dynamically typed."

We should be able to explain **why**, demonstrate it with code, distinguish related concepts, and understand their production implications.

For example:

- What exactly is JavaScript?
- What is ECMAScript?
- Is V8 JavaScript?
- Is Node.js JavaScript?
- Does the browser execute JavaScript directly?
- Is JavaScript interpreted or compiled?
- Why can a variable change from a number to a string?
- Is JavaScript weakly typed or strongly typed?
- What is the difference between a value and a reference?
- Why does modifying one object sometimes modify another variable?
- Why are primitives immutable but objects mutable?
- What does `"use strict"` actually change?
- What happens when JavaScript code executes?
- How has ECMAScript evolved without breaking the language?

Let's build the mental model from the bottom up.

## 1.1 JavaScript as a Language

First, separate **the language** from **the environment that runs it**.

JavaScript is a **programming language**.

It defines things such as:

```js
let x = 10;

function add(a, b) {
  return a + b;
}

const user = {
  name: "Danishan",
};
```

The language defines concepts including:

- syntax
- values
- types
- variables
- functions
- objects
- operators
- control flow
- scope
- classes
- promises
- modules
- etc.

But JavaScript itself does **not** define things such as:

```js
document.querySelector(...)
```

or:

```js
fs.readFile(...)
```

Those belong to the **environment/runtime**.

This distinction is fundamental.

#### Think of it this way

```text
                 JavaScript Language
                         │
             ┌───────────┴───────────┐
             │                       │
          Browser                  Node.js
             │                       │
          Web APIs                Node APIs
             │                       │
           DOM                    File System
           Fetch                    HTTP
           Storage                 Streams
```

The same JavaScript language can exist in different environments.

## 1.2 ECMAScript

This is where terminology often becomes confusing.

**ECMAScript is the standardized specification that defines the JavaScript language.**

Historically:

```text
ECMAScript = specification
JavaScript = implementation/language commonly associated with ECMAScript
```

ECMAScript specifies language behavior such as:

```js
let
const
function
class
Promise
Map
Set
Symbol
async/await
```

It defines what these constructs mean and how they behave.

JavaScript engines then implement that specification.

Examples of JavaScript engines include:

- V8
- SpiderMonkey
- JavaScriptCore

So:

```text
ECMAScript specification
        ↓
JavaScript engines implement it
        ↓
JavaScript programs execute
```

#### Important interview distinction

If an interviewer asks:

> Is ECMAScript the same thing as JavaScript?

A good answer is:

> ECMAScript is the standardized language specification on which JavaScript is based. JavaScript is the language as implemented by engines such as V8, SpiderMonkey, and JavaScriptCore.

## 1.3 JavaScript Language vs JavaScript Runtime

This is one of the **most important distinctions in this entire section**.

Consider:

```js
const result = await fetch("/users");
```

Is `fetch()` part of JavaScript?

**No—not historically as a core ECMAScript language construct.**

It's an API provided by the environment.

Similarly:

```js
document.querySelector("#app");
```

`document` is not provided by ECMAScript.

It's provided by the browser environment.

And:

```js
const fs = require("fs");
```

`fs` is provided by Node.js.

So:

#### Language

Defines things like:

```js
let
const
if
for
function
class
Promise
Map
Set
Symbol
```

#### Runtime/environment

Provides things like:

```text
Browser
├── DOM
├── fetch
├── localStorage
├── WebSocket
├── setTimeout
└── Web APIs

Node.js
├── fs
├── http
├── process
├── Buffer
├── streams
└── networking APIs
```

This explains something very important:

> **JavaScript is not synonymous with the browser.**

JavaScript existed as a language; the browser is merely one environment in which JavaScript runs.

## 1.4 Browser vs Node.js Environment

Both can execute JavaScript.

But they provide different host capabilities.

#### Browser

```js
console.log(window);
console.log(document);
fetch("/api/users");
```

The browser provides APIs related to:

- web pages
- DOM
- events
- networking
- storage
- rendering
- workers

#### Node.js

```js
const fs = require("fs");

fs.readFile("data.txt", callback);
```

Node provides APIs related to:

- filesystem
- networking
- processes
- streams
- servers
- operating-system interaction

Therefore:

```js
const x = 10;
```

is JavaScript.

But:

```js
document.querySelector(...)
```

is browser functionality.

And:

```js
fs.readFile(...)
```

is Node.js functionality.

## 1.5 Is JavaScript Interpreted or Compiled?

This is an old interview question where the simplistic answer is usually:

> "JavaScript is interpreted."

That's not a good modern answer.

A better answer is:

> JavaScript engines use sophisticated execution pipelines that can involve parsing, bytecode/intermediate representations, interpretation, and JIT compilation. Therefore, calling modern JavaScript simply "interpreted" or simply "compiled" is an oversimplification.

Conceptually:

```text
JavaScript source
      ↓
    Parsing
      ↓
   Internal representation
      ↓
 Interpretation / execution
      ↓
 JIT optimization/compilation
      ↓
 Optimized machine code
```

The exact architecture depends on the engine.

For example, V8 uses multiple execution/optimization stages.

The important SDE-2 takeaway is:

> **The language itself is not "interpreted." The JavaScript engine determines how the program is executed.**

This connects directly back to our earlier distinction:

```text
JavaScript
   │
   │ language specification
   ↓
Engine
   │
   │ execution strategy
   ↓
Machine
```

## 1.6 Dynamic Typing

JavaScript is **dynamically typed**.

This means the type is associated with the **value at runtime**, rather than being fixed to the variable declaration.

For example:

```js
let value = 10;
```

At this point:

```text
value → number
```

Then:

```js
value = "hello";
```

Now:

```text
value → string
```

Then:

```js
value = true;
```

Now:

```text
value → boolean
```

The variable itself isn't permanently declared as:

```text
value: number
```

in JavaScript.

Instead, the runtime determines the type of the current value.

#### Compare TypeScript

```ts
let value: number = 10;

value = "hello"; // TypeScript error
```

TypeScript introduces static type checking.

JavaScript:

```js
let value = 10;
value = "hello"; // valid
```

## 1.7 Dynamic Typing ≠ No Types

This distinction is extremely important.

JavaScript **has types**.

It is not:

> "JavaScript doesn't have a type system."

It absolutely does.

For example:

```js
typeof 10;
// "number"

typeof "hello";
// "string"

typeof true;
// "boolean"
```

The difference is **when and how type information is enforced**.

JavaScript:

```text
Dynamic typing
        ↓
Type determined during runtime
```

TypeScript:

```text
Static type system
        ↓
Types checked during development/compilation
```

And remember:

> TypeScript ultimately produces JavaScript.

## 1.8 Weak vs Strong Typing

This topic requires more care.

JavaScript is commonly described as **weakly typed**, primarily because it performs extensive implicit type coercion.

For example:

```js
"5" + 2;
```

produces:

```js
"52";
```

while:

```js
"5" - 2;
```

produces:

```js
3;
```

Why?

Because different operators trigger different coercion behavior.

Another example:

```js
if ("hello") {
  console.log("runs");
}
```

The string is converted according to JavaScript's truthiness rules.

#### But avoid saying:

> "JavaScript has no type safety."

That's too broad.

JavaScript has a defined type system and defined coercion rules.

A better interview explanation:

> JavaScript is dynamically typed and is often characterized as weakly typed because it allows implicit type coercion in many operations. The exact meaning of "strong" and "weak" typing is not universally standardized, so those labels should be used carefully.

## 1.9 Expressions vs Statements

This distinction becomes very useful when understanding JavaScript syntax and execution.

### Expression

An expression **produces a value**.

Examples:

```js
10 + 20;
```

```js
user.name;
```

```js
a > b;
```

```js
foo();
```

```js
condition ? "yes" : "no";
```

For example:

```js
const x = 10 + 20;
```

The expression:

```js
10 + 20;
```

produces:

```text
30
```

### Statement

A statement represents an instruction or action.

Examples:

```js
let x;
```

```js
if (x > 10) {
  console.log(x);
}
```

```js
return x;
```

```js
for (...) {
}
```

#### Important nuance

Some constructs can behave in expression-like ways, and JavaScript has many syntactic details that make the distinction less trivial than:

> "Expressions return values; statements don't."

For revision purposes:

```text
Expression → evaluates to a value
Statement  → performs/controls an action
```

is the useful first mental model.

## 1.10 Values and References

This is one of the most important concepts for understanding JavaScript behavior.

Consider:

```js
let a = 10;
let b = a;

b = 20;
```

After this:

```text
a = 10
b = 20
```

Changing `b` doesn't affect `a`.

Now:

```js
const a = {
  name: "Danishan",
};

const b = a;

b.name = "John";
```

Now:

```js
a.name;
```

is also:

```text
"John"
```

Why?

Because objects are accessed through references.

Conceptually:

```text
a ─────┐
       │
       ↓
   ┌──────────────┐
   │ { name: ...} │
   └──────────────┘
       ↑
       │
b ─────┘
```

Both variables refer to the same object.

## 1.11 Primitive Values vs Object Values

JavaScript has primitive values such as:

```text
string
number
bigint
boolean
undefined
null
symbol
```

and objects.

Example:

```js
let a = 10;
let b = a;
```

Conceptually:

```text
a → 10
b → 10
```

Then:

```js
b = 20;
```

doesn't modify `a`.

But:

```js
let a = { count: 10 };
let b = a;

b.count = 20;
```

Both variables refer to the same object.

#### Critical terminology

Don't reduce this to:

> "Primitives are passed by value and objects are passed by reference."

That's a common shortcut, but it creates confusion when discussing function arguments.

A more precise model is:

> **JavaScript always passes argument values. When that value is an object, the value itself is a reference to the object.**

For example:

```js
function update(obj) {
  obj.count = 20;
}

const user = {
  count: 10,
};

update(user);
```

The object is mutated because the function receives a copy of the reference value pointing to the same object.

But:

```js
function replace(obj) {
  obj = { count: 100 };
}

const user = { count: 10 };

replace(user);

console.log(user.count);
```

Output:

```text
10
```

Because reassigning the local parameter doesn't change the caller's variable.

This distinction is **very commonly tested in interviews**.

## 1.12 Mutable vs Immutable Values

Now connect values with mutation.

An object can be mutated:

```js
const user = {
  name: "Danishan",
};

user.name = "John";
```

`const` does **not** make the object immutable.

It prevents reassignment of the variable:

```js
user = {};
// TypeError
```

But this is allowed:

```js
user.name = "John";
```

So:

```text
const
 ↓
binding cannot be reassigned

NOT

object cannot be modified
```

#### Primitive immutability

Strings are immutable:

```js
let name = "Danish";

name[0] = "X";
```

This does not mutate the existing string.

Operations create another string value:

```js
name = name.toUpperCase();
```

Objects, on the other hand, are generally mutable:

```js
const user = {
  name: "Danish",
};

user.name = "John";
```

## 1.13 `const` vs Immutability

This deserves its own mental model because it causes many mistakes.

```js
const user = {
  name: "Danish",
};
```

There are two separate concepts:

#### Binding

```text
user ─────────→ object
```

The `user` binding cannot be changed to point somewhere else.

#### Object

The object itself can still change.

```js
user.name = "John";
```

Therefore:

```text
const ≠ immutable object
```

If we actually need immutability, we can use mechanisms such as:

```js
Object.freeze(user);
```

But even `Object.freeze()` has its own depth/shallow-freezing considerations.

We'll cover that properly in the **Objects & Object Model** section.

## 1.14 Strict Mode

Strict mode changes certain JavaScript semantics and enables additional runtime errors for problematic behavior.

Enable it using:

```js
"use strict";
```

For example, in strict mode:

```js
"use strict";

x = 10;
```

throws an error instead of silently creating/using an accidental global binding.

Another important difference concerns `this`.

For example:

```js
"use strict";

function test() {
  console.log(this);
}

test();
```

Here, `this` is:

```js
undefined;
```

rather than the non-strict behavior of substituting the global object in ordinary function calls.

#### Why does strict mode exist?

JavaScript historically accumulated behaviors that could make programs error-prone.

Strict mode provides a stricter execution model that:

- catches certain mistakes
- changes some problematic semantics
- disallows certain legacy constructs
- makes some optimizations easier for engines

#### Modern JavaScript

ES modules are automatically strict mode.

So:

```js
// module.js
console.log("strict mode automatically applies");
```

doesn't require:

```js
"use strict";
```

## 1.15 High-Level JavaScript Execution

Now let's combine everything.

Suppose we have:

```js
const x = 10;

function add(a, b) {
  return a + b;
}

const result = add(x, 20);

console.log(result);
```

At a very high level:

```text
JavaScript source
       ↓
     Parsing
       ↓
Program representation
       ↓
Execution
       ↓
Variables / objects / functions
       ↓
Expressions evaluated
       ↓
Function calls
       ↓
Values produced
```

The engine maintains runtime structures such as:

- execution contexts
- lexical environments
- call stack
- heap/object storage
- internal engine structures

We'll study these much more deeply in:

```text
Functions
       ↓
Scope & Closures
       ↓
Event Loop
       ↓
Memory Management
```

For now, the key idea is:

> **JavaScript source code is processed by an engine, and the engine creates and manages the runtime state required to execute it.**

## 1.16 A Very Important Correction to the "Stack vs Heap" Model

You'll often hear:

> "Primitive values are stored on the stack and objects are stored on the heap."

Don't treat that as a JavaScript language rule.

That's an oversimplification.

The ECMAScript specification doesn't require a particular physical memory layout such as:

```text
primitive → stack
object → heap
```

An engine is free to optimize representation and storage.

For SDE-2:

```text
Stack/heap
```

should be understood as a **useful conceptual runtime model**, not a strict specification-level rule.

We'll go deeper into this in **Memory Management**.

## 1.17 ECMAScript Versions & Language Evolution

JavaScript isn't a frozen language.

ECMAScript evolves through standardized editions.

Historically:

```text
ES5
 ↓
ES2015 (ES6)
 ↓
ES2016
 ↓
ES2017
 ↓
...
```

One major milestone was **ES2015**, commonly called **ES6**.

It introduced or standardized many features that fundamentally changed modern JavaScript development, including:

```js
let
const
class
arrow functions
modules
Promise
Map
Set
Symbol
destructuring
rest/spread
template literals
```

Later editions continued adding features such as:

```js
async/await
optional chaining
nullish coalescing
BigInt
private class fields
logical assignment
etc.
```

The important conceptual point is:

> **JavaScript evolves incrementally while attempting to preserve compatibility with existing code.**

This is one reason JavaScript has some seemingly strange historical behavior.

For example:

```js
typeof null;
```

returns:

```text
"object"
```

This is a famous legacy behavior.

The language can't simply change it because enormous amounts of existing code depend on established behavior.

## 1.18 Putting the Entire Topic Together

Now let's build the complete mental model.

```text
                         JAVASCRIPT
                             │
                             │ language
                             ↓
                       ECMASCRIPT
                             │
                    defines semantics
                             │
                             ↓
                    JAVASCRIPT ENGINE
                   ┌─────────┴─────────┐
                   │                   │
                  V8              SpiderMonkey
                   │
                   ↓
              Executes code
                   │
          ┌────────┴────────┐
          │                 │
       Browser            Node.js
          │                 │
      Web APIs           Node APIs
          │                 │
          ↓                 ↓
        DOM              File System
        Fetch            Networking
        Storage          Streams
```

And inside the language itself:

```text
JavaScript
│
├── Dynamically typed
│
├── Primitive values
│
├── Object values
│
├── Expressions
│
├── Statements
│
├── Functions
│
├── Scope
│
├── Objects
│
└── Execution semantics
```

## SDE-2 Interview Questions From This Section

These are the questions I'd expect you to be comfortable answering without memorized definitions.

#### Fundamental

1. **What exactly is JavaScript?**
2. **What is ECMAScript?**
3. **What is the difference between JavaScript and ECMAScript?**
4. **What is the difference between JavaScript and a JavaScript runtime?**
5. **Is Node.js JavaScript?**
6. **Is V8 JavaScript?**
7. **Is `fetch()` part of JavaScript?**
8. **Why can JavaScript run both in browsers and on servers?**

#### Execution

9. **Is JavaScript interpreted or compiled?**
10. **What happens between JavaScript source code and machine execution?**
11. **What is a JavaScript engine?**
12. **What does V8 actually do?**
13. **Why is saying "JavaScript is interpreted" an oversimplification?**

#### Type system

14. **What does dynamically typed mean?**
15. **Does dynamically typed mean JavaScript has no types?**
16. **Why can this happen?**

```js
let x = 10;
x = "hello";
```

17. **Is JavaScript weakly typed or strongly typed?**
18. **What's the difference between dynamic typing and weak typing?**
19. **Why does this produce `"52"`?**

```js
"5" + 2;
```

20. **Why does this produce `3`?**

```js
"5" - 2;
```

#### Values and references

21. **What's the difference between a primitive value and an object value?**
22. **Why does modifying one object variable sometimes affect another variable?**
23. **Is JavaScript pass-by-value or pass-by-reference?**
24. **Explain this:**

```js
function change(obj) {
  obj.name = "B";
}

const user = { name: "A" };

change(user);
```

25. **Explain why this doesn't replace the original object:**

```js
function change(obj) {
  obj = { name: "B" };
}
```

#### Mutability

26. **Are JavaScript objects mutable?**
27. **Are JavaScript primitive values mutable?**
28. **Does `const` make an object immutable?**
29. **What's the difference between a binding and the object it points to?**

#### Language semantics

30. **What's the difference between an expression and a statement?**
31. **What does strict mode do?**
32. **Why does JavaScript have strict mode?**
33. **Are ES modules automatically strict?**
34. **Why can't JavaScript simply fix historical behaviors such as `typeof null`?**

## The Core Mental Model to Remember

If you remember only one diagram from this section, make it this:

```text
                 ECMAScript
              (language spec)
                     │
                     ↓
              JavaScript engine
           (V8 / SpiderMonkey / JSC)
                     │
                     ↓
              Executes JavaScript
                     │
          ┌──────────┴──────────┐
          ↓                     ↓
       Browser                Node.js
       runtime                runtime
          │                     │
       Web APIs              Node APIs
          │                     │
        DOM/etc.            fs/http/etc.
```

And inside JavaScript:

```text
                JavaScript
                    │
        ┌───────────┼───────────┐
        ↓           ↓           ↓
      Types       Values      Syntax
        │           │           │
    dynamic      primitive    expressions
    typing       / objects    statements
                    │
                references
                    │
                mutability
```

This gives us the foundation for everything that follows.

Absolutely. I’ll answer these as **SDE-2 interview answers**: concise enough to say in an interview, but with the important reasoning behind each answer. These questions are the set defined for this JavaScript fundamentals section.
