# SDE-2 Interview Questions From This Section

These are the questions I'd expect you to be comfortable answering without memorized definitions.

### Fundamental

1. **What exactly is JavaScript?**
2. **What is ECMAScript?**
3. **What is the difference between JavaScript and ECMAScript?**
4. **What is the difference between JavaScript and a JavaScript runtime?**
5. **Is Node.js JavaScript?**
6. **Is V8 JavaScript?**
7. **Is `fetch()` part of JavaScript?**
8. **Why can JavaScript run both in browsers and on servers?**

### Execution

9. **Is JavaScript interpreted or compiled?**
10. **What happens between JavaScript source code and machine execution?**
11. **What is a JavaScript engine?**
12. **What does V8 actually do?**
13. **Why is saying "JavaScript is interpreted" an oversimplification?**

### Type system

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

### Values and references

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

### Mutability

26. **Are JavaScript objects mutable?**
27. **Are JavaScript primitive values mutable?**
28. **Does `const` make an object immutable?**
29. **What's the difference between a binding and the object it points to?**

### Language semantics

30. **What's the difference between an expression and a statement?**
31. **What does strict mode do?**
32. **Why does JavaScript have strict mode?**
33. **Are ES modules automatically strict?**
34. **Why can't JavaScript simply fix historical behaviors such as `typeof null`?**

## Answers

### 1. What exactly is JavaScript?

JavaScript is a **programming language** that defines syntax, types, values, functions, objects, control flow, modules, and execution semantics.

It is implemented by JavaScript engines such as **V8, SpiderMonkey, and JavaScriptCore**.

> **JavaScript is the language; the browser or Node.js provides the environment in which it runs.**

### 2. What is ECMAScript?

**ECMAScript is the standardized specification of the JavaScript language.**

It defines how language features such as `let`, `const`, functions, classes, promises, modules, etc. should behave.

### 3. What is the difference between JavaScript and ECMAScript?

**ECMAScript is the specification; JavaScript is the language implemented according to that specification.**

Think:

```text
ECMAScript specification
        ↓
JavaScript engines
        ↓
JavaScript execution
```

### 4. What is the difference between JavaScript and a JavaScript runtime?

**JavaScript is the language. A runtime provides the environment and APIs needed to execute it.**

For example:

```text
JavaScript
   ↓
Engine
   ↓
Runtime
   ├── Browser APIs
   └── Node.js APIs
```

`document`, `fs`, networking APIs, etc. come from the runtime, not the core language.

### 5. Is Node.js JavaScript?

**No.**

Node.js is a **JavaScript runtime environment** that uses the V8 engine and provides additional APIs for things like filesystem access, networking, streams, and processes.

> Node.js **runs JavaScript**; it isn't JavaScript itself.

### 6. Is V8 JavaScript?

**No.**

V8 is a **JavaScript engine**.

It takes JavaScript code and processes/executes it.

Examples of engines:

- V8 → Chrome, Node.js
- SpiderMonkey → Firefox
- JavaScriptCore → Safari

### 7. Is `fetch()` part of JavaScript?

Not as a core ECMAScript language construct.

`fetch()` is a **web platform API** provided by browser environments. Modern runtimes such as Node.js also provide `fetch()`.

So:

```js
let x = 10;
```

→ JavaScript language.

```js
fetch("/users");
```

→ API provided by the runtime/environment.

The important distinction is that **the availability of an API doesn't mean the API is part of the core language**.

### 8. Why can JavaScript run both in browsers and on servers?

Because **JavaScript is a language independent of a particular host environment**.

Different environments provide different runtimes:

```text
             JavaScript
                  ↓
          JavaScript Engine
           ↙            ↘
      Browser          Node.js
         ↓                ↓
     Web APIs          Node APIs
```

The same language can therefore execute in different environments.

# Execution

### 9. Is JavaScript interpreted or compiled?

Neither answer is completely accurate by itself.

Modern JavaScript engines use a combination of **parsing, interpretation, compilation, and JIT optimization**.

> JavaScript execution is implementation-dependent; calling it simply "interpreted" or "compiled" is an oversimplification.

### 10. What happens between JavaScript source code and machine execution?

At a high level:

```text
Source code
    ↓
Parsing
    ↓
Internal representation
    ↓
Execution / interpretation
    ↓
JIT optimization / compilation
    ↓
Machine code
```

The exact pipeline depends on the JavaScript engine.

### 11. What is a JavaScript engine?

A JavaScript engine is a **program that processes and executes JavaScript code**.

Examples:

- V8
- SpiderMonkey
- JavaScriptCore

It handles things such as parsing, execution, optimization, memory management, and runtime structures.

### 12. What does V8 actually do?

V8:

1. Parses JavaScript.
2. Builds internal representations.
3. Executes the code.
4. Optimizes frequently executed code.
5. Compiles optimized code to machine code.
6. Manages runtime memory and other engine internals.

> **V8 is the engine that executes JavaScript; Node.js and Chrome provide different environments around it.**

### 13. Why is saying "JavaScript is interpreted" an oversimplification?

Because **interpretation vs compilation describes an engine's execution strategy, not the JavaScript language itself**.

Modern engines can interpret some code and compile/optimize other code.

So a better statement is:

> **The JavaScript engine determines how JavaScript code is executed, and modern engines use multiple execution techniques.**

# Type System

### 14. What does dynamically typed mean?

It means **types are associated with runtime values rather than being permanently fixed to variables**.

```js
let x = 10; // number

x = "hello"; // string

x = true; // boolean
```

The same variable can hold values of different types.

### 15. Does dynamically typed mean JavaScript has no types?

**No.**

JavaScript absolutely has a type system.

```js
typeof 10; // "number"
typeof "hello"; // "string"
typeof true; // "boolean"
```

"Dynamic" describes **when/how types are checked**, not the absence of types.

### 16. Why can this happen?

```js
let x = 10;

x = "hello";
```

Because JavaScript is **dynamically typed**.

The variable `x` isn't permanently declared as a `number`. Its current value is a number first and a string later.

```text
x → 10       → number
x → "hello"  → string
```

### 17. Is JavaScript weakly typed or strongly typed?

The careful answer is:

> **JavaScript is dynamically typed and is often characterized as weakly typed because it allows implicit type coercion.**

However, **"strong" and "weak" are not universally standardized terms**, so they should be used carefully.

### 18. What's the difference between dynamic typing and weak typing?

They describe **different things**.

**Dynamic typing** → concerns **when type information is determined/enforced**.

```js
let x = 10;
x = "hello";
```

**Weak typing** → commonly refers to **how readily the language performs implicit conversions**.

```js
"5" + 2; // "52"
```

So:

> **Dynamic = type checking model.**
> **Weak = coercion/conversion behavior.**

### 19. Why does this produce `"52"`?

```js
"5" + 2;
```

Because `+` has special behavior.

When one operand is a string, JavaScript performs string concatenation and converts the other operand to a string:

```text
"5" + 2
     ↓
"5" + "2"
     ↓
"52"
```

### 20. Why does this produce `3`?

```js
"5" - 2;
```

`-` is a numeric operator, so JavaScript converts `"5"` to the number `5`:

```text
"5" - 2
 ↓
 5 - 2
 ↓
 3
```

This is **implicit type coercion**.

# Values and References

### 21. What's the difference between a primitive value and an object value?

Primitive values are values such as:

```text
string
number
bigint
boolean
undefined
null
symbol
```

Objects are non-primitive values that can contain properties and behavior.

```js
let x = 10;

let user = {
  name: "Danishan",
};
```

### 22. Why does modifying one object variable sometimes affect another variable?

Because both variables can refer to the **same object**.

```js
const a = { name: "A" };
const b = a;

b.name = "B";

console.log(a.name); // "B"
```

Conceptually:

```text
a ──┐
    ├──→ { name: "B" }
b ──┘
```

Both contain a reference to the same object.

### 23. Is JavaScript pass-by-value or pass-by-reference?

**JavaScript is pass-by-value.**

For objects, the value being passed is a **reference to the object**.

That's why the precise explanation is:

> **JavaScript always passes argument values. When that value is an object, the value itself is a reference to the object.**

This distinction is important.

### 24. Explain this:

```js
function change(obj) {
  obj.name = "B";
}

const user = { name: "A" };

change(user);
```

`user` contains a reference to the object.

When calling:

```js
change(user);
```

the reference value is copied into `obj`.

So both point to the same object:

```text
user ──┐
       ↓
    { name: "A" }
       ↑
       │
      obj
```

Then:

```js
obj.name = "B";
```

mutates that shared object.

Therefore:

```js
user.name; // "B"
```

### 25. Why doesn't this replace the original object?

```js
function change(obj) {
  obj = { name: "B" };
}
```

Because `obj` is a **local parameter** containing a copy of the reference.

Initially:

```text
user ──→ Object A
           ↑
           │
         obj
```

Then:

```js
obj = { name: "B" };
```

only changes the local `obj` binding:

```text
user ──→ Object A

obj ──→ Object B
```

The caller's `user` variable is unchanged.

# Mutability

### 26. Are JavaScript objects mutable?

**Generally, yes.**

```js
const user = {
  name: "A",
};

user.name = "B";
```

The object's property can be changed.

### 27. Are JavaScript primitive values mutable?

**No. Primitive values are immutable.**

For example, strings cannot be modified in place:

```js
let name = "Danish";

name[0] = "X";
```

This doesn't modify the existing string.

Operations such as:

```js
name.toUpperCase();
```

produce a new string value.

### 28. Does `const` make an object immutable?

**No.**

`const` prevents **reassignment of the binding**, not mutation of the object.

```js
const user = {
  name: "A",
};

user.name = "B"; // ✅

user = {}; // ❌
```

So:

> `const` ≠ immutable object.

### 29. What's the difference between a binding and the object it points to?

A **binding** is the association between a variable name and its current value.

With:

```js
const user = {
  name: "A",
};
```

conceptually:

```text
user ─────→ object
 ↑
binding
```

`const` means the `user` binding cannot be reassigned:

```js
user = {}; // ❌
```

But the object can still be mutated:

```js
user.name = "B"; // ✅
```

So:

> **Binding immutability and object immutability are two different things.**

# Language Semantics

### 30. What's the difference between an expression and a statement?

**Expression → evaluates to a value.**

```js
10 + 20;
user.name;
a > b;
foo();
```

**Statement → represents an instruction/control structure.**

```js
let x;

if (x > 10) {
}

return x;
```

Mental model:

> **Expression → produces/evaluates to a value.**
> **Statement → performs or controls an action.**

There are nuances, but this is the useful interview-level distinction.

### 31. What does strict mode do?

Strict mode enables **stricter JavaScript semantics** and causes certain problematic operations to throw errors.

```js
"use strict";

x = 10;
```

Instead of silently creating an accidental global, this produces an error.

It also changes certain behaviors, such as `this` in ordinary function calls:

```js
"use strict";

function test() {
  console.log(this);
}

test(); // undefined
```

### 32. Why does JavaScript have strict mode?

JavaScript accumulated historical behaviors that could easily cause bugs.

Strict mode was introduced to provide a **safer and more predictable execution model** by:

- catching certain mistakes
- changing problematic semantics
- disallowing some legacy constructs
- making some optimizations easier for engines

### 33. Are ES modules automatically strict?

**Yes.**

Code inside an ES module is automatically executed in **strict mode**.

Therefore, you don't need:

```js
"use strict";
```

inside a module.

### 34. Why can't JavaScript simply fix historical behaviors such as `typeof null`?

Because of **backward compatibility**.

```js
typeof null;
// "object"
```

This is a historical behavior of JavaScript.

Changing it now could break existing programs that depend on the established behavior.

Therefore:

> **JavaScript generally evolves incrementally while preserving compatibility with existing code.**

This is also why modern JavaScript contains some behaviors that look strange from today's perspective.

## The SDE-2 mental model

If the interviewer moves between these questions, keep this hierarchy in your head:

```text
              ECMAScript
           (language spec)
                  ↓
         JavaScript Engine
       ┌──────────┼──────────┐
       ↓          ↓          ↓
      V8    SpiderMonkey    JSC
       ↓
    executes
   JavaScript
       ↓
 ┌─────┴─────┐
 ↓           ↓
Browser     Node.js
 ↓           ↓
Web APIs    Node APIs
```

And inside the language:

```text
JavaScript
    │
    ├── Types
    │    ├── Primitive
    │    └── Object
    │
    ├── Dynamic typing
    │
    ├── Coercion
    │
    ├── Values & references
    │
    ├── Mutability
    │
    ├── Expressions
    │
    └── Statements
```

This is the foundation on which **scope/closures → functions → `this` → event loop → promises → objects → prototypes → memory management** build.
