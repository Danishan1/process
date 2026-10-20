# Functions — Complete Revision

| Concept                     | Simple meaning                               | Example / Syntax                       |
| --------------------------- | -------------------------------------------- | -------------------------------------- |
| **Function**                | Reusable block of code                       | `function add(a, b) { return a+b }`    |
| **Parameter**               | Variable received by function                | `function add(a, b)` → `a, b`          |
| **Argument**                | Actual value passed                          | `add(2, 3)` → `2, 3`                   |
| **Return**                  | Sends value back                             | `return a + b`                         |
| **Function Declaration**    | Normal named function                        | `function add() {}`                    |
| **Function Expression**     | Function stored in variable                  | `const add = function() {}`            |
| **Arrow Function**          | Short function syntax                        | `const add = () => {}`                 |
| **Anonymous Function**      | Function without a name                      | `function() {}`                        |
| **First-Class Function**    | Functions can be stored, passed, returned    | `const fn = () => {}`                  |
| **Callback** ⭐              | Function passed to another function          | `arr.map(x => x * 2)`                  |
| **Higher-Order Function** ⭐ | Takes/returns a function                     | `map()`, `filter()`                    |
| **Scope**                   | Where a variable is accessible               | Global / Function / Block              |
| **Closure** ⭐⭐⭐             | Function remembers outer variables           | `return () => count++`                 |
| **`this`**                  | Context determined by how function is called | `obj.greet()` → `this = obj`           |
| **Pure Function**           | Same input → same output, no side effects    | `add(2,3)`                             |
| **Side Effect**             | Function changes something outside itself    | `count++`, API call                    |
| **Async Function**          | Function that returns a Promise              | `async function getData(){}`           |
| **Promise**                 | Represents a future result                   | `fetch(...).then(...)`                 |
| **`async/await`**           | Cleaner way to handle Promises               | `const data = await getData()`         |
| **Recursion**               | Function calls itself                        | `function fact(n){ return fact(n-1) }` |
| **Function Type (TS)**      | Describes a function's type                  | `(a: number) => number`                |
| **Optional Parameter**      | Parameter may be omitted                     | `name?: string`                        |
| **Default Parameter**       | Uses default if no value                     | `name = "Guest"`                       |
| **Rest Parameter**          | Collects arguments into array                | `(...nums: number[])`                  |
| **Method**                  | Function belonging to object/class           | `user.greet()`                         |
| **IIFE**                    | Function executed immediately                | `(function(){})()`                     |
| **Composition**             | Combine functions together                   | `f(g(x))`                              |
| **Currying**                | Multiple arguments → chain of functions      | `add(2)(3)`                            |

#### ⭐ Most important distinctions

| Don't confuse             | Remember                                                             |
| ------------------------- | -------------------------------------------------------------------- |
| **Parameter vs Argument** | Parameter = definition, Argument = actual value                      |
| **Callback vs Promise**   | Callback = function, Promise = future result                         |
| **Callback vs HOF**       | Callback = passed function, HOF = function taking/returning function |
| **Async vs Promise**      | `async` function **returns** a Promise                               |
| **Closure vs Scope**      | Scope = accessibility, Closure = function **remembers** outer scope  |
| **Pure vs Impure**        | Pure = no external effect, Impure = has side effects                 |
| **Function vs Method**    | Method = function attached to an object/class                        |

#### 🧠 One-line mental model

**Function → can be stored/passed → Callback → HOF → can remember outer scope → Closure → can be async → Promise → `await`**.


# Functions — Types

### 1. What is a function?

A function is a **reusable block of code** that can receive input, perform some work, and optionally return output.

```ts
function add(a: number, b: number): number {
  return a + b;
}

add(2, 3); // 5
```

Think:

```text
Input → Function → Output
```

### 2. Parameters vs Arguments

```ts
function greet(name: string) { // name = parameter
  console.log(name);
}

greet("John"); // "John" = argument
```

**Parameter** → variable defined by the function.

**Argument** → actual value passed to the function.

### 3. Return value

```ts
function multiply(a: number, b: number): number {
  return a * b;
}
```

```ts
const result = multiply(2, 5);
// result = 10
```

A function without `return` effectively returns `undefined`.

## 4. Function Declaration

```ts
function add(a: number, b: number) {
  return a + b;
}
```

One important property: function declarations are **hoisted**, so they can generally be called before their declaration.

## 5. Function Expression

A function stored in a variable:

```ts
const add = function (a: number, b: number) {
  return a + b;
};
```

## 6. Arrow Function

Modern JavaScript/TypeScript style:

```ts
const add = (a: number, b: number) => {
  return a + b;
};
```

Short version:

```ts
const add = (a: number, b: number) => a + b;
```

Arrow functions have important differences from normal functions, especially regarding **`this`**.

## 7. Anonymous vs Named Function

Anonymous:

```ts
const fn = function () {
  console.log("hello");
};
```

Named:

```ts
const fn = function greet() {
  console.log("hello");
};
```

The function itself can have a name even when assigned to a variable.

## 8. First-Class Functions ⭐

JavaScript treats functions like values.

That means you can:

#### Store a function

```ts
const greet = () => console.log("Hi");
```

#### Pass a function

```ts
someFunction(greet);
```

#### Return a function

```ts
function createGreeting() {
  return () => console.log("Hi");
}
```

This idea is called **first-class functions**.

## 9. Callback ⭐

A **callback is a function passed to another function**.

```ts
function process(callback: () => void) {
  console.log("Processing...");
  callback();
}

process(() => {
  console.log("Done!");
});
```

Here:

```ts
() => {
  console.log("Done!");
}
```

is the callback.

#### Callback ≠ Promise

```text
Callback = function
Promise  = object representing a future result
```

A callback can be used with a Promise, but they are not the same thing.

## 10. Higher-Order Function ⭐

A function that:

* accepts another function, **or**
* returns another function.

Example:

```ts
function calculate(
  a: number,
  b: number,
  operation: (x: number, y: number) => number
) {
  return operation(a, b);
}

calculate(10, 5, (x, y) => x + y);
```

`calculate` is a **higher-order function**.

The `(x, y) => x + y` is the **callback**.

## 11. Scope

Scope determines **where a variable can be accessed**.

```ts
function test() {
  const x = 10;

  console.log(x);
}

console.log(x); // ❌
```

`x` is inside the function's local scope.

Main concepts:

```text
Global scope
Function scope
Block scope
Lexical scope
```

`let` and `const` are block-scoped.

## 12. Closure ⭐⭐⭐

A closure happens when a function **remembers variables from its surrounding scope**, even after that outer function has finished.

```ts
function counter() {
  let count = 0;

  return () => {
    count++;
    return count;
  };
}

const increment = counter();

increment(); // 1
increment(); // 2
increment(); // 3
```

Why does `count` still exist?

Because the returned function **closed over** `count`.

```text
counter()
   ↓
creates count
   ↓
returns function
   ↓
function remembers count
   ↓
increment() → 1
increment() → 2
increment() → 3
```

Closures are extremely important in JavaScript.

## 13. `this`

`this` refers to a context determined by **how a function is called**.

```ts
const user = {
  name: "John",

  greet() {
    console.log(this.name);
  }
};

user.greet(); // John
```

Arrow functions behave differently:

```ts
const user = {
  name: "John",

  greet: () => {
    console.log(this.name);
  }
};
```

Don't use arrow functions when you specifically need the method's dynamic `this`.

## 14. Pure Function

A pure function:

1. Gives the same output for the same input.
2. Doesn't cause external side effects.

```ts
function add(a: number, b: number) {
  return a + b;
}
```

```text
add(2, 3) → 5
add(2, 3) → 5
```

## 15. Impure Function / Side Effect

```ts
let total = 0;

function addToTotal(value: number) {
  total += value;
}
```

The function changes something outside itself.

That's a **side effect**.

Other side effects include:

```text
Changing global variables
DOM manipulation
API calls
Writing files
Logging
Database operations
```

## 16. Default Parameters

```ts
function greet(name = "Guest") {
  console.log(name);
}

greet();        // Guest
greet("John");  // John
```

## 17. Optional Parameters — TypeScript

```ts
function greet(name?: string) {
  console.log(name);
}
```

`?` means the argument can be omitted.

## 18. Rest Parameters

Collect multiple arguments into an array:

```ts
function sum(...numbers: number[]) {
  return numbers.reduce((total, n) => total + n, 0);
}

sum(1, 2, 3, 4); // 10
```

## 19. Function Type — TypeScript ⭐

You can describe a function's type:

```ts
type AddFunction = (a: number, b: number) => number;
```

Then:

```ts
const add: AddFunction = (a, b) => a + b;
```

This is very important for callbacks.

```ts
function process(callback: (value: number) => void) {
  callback(10);
}
```

## 20. Async Function

```ts
async function getUser() {
  const response = await fetch("/api/user");
  return response.json();
}
```

An `async` function **always returns a Promise**.

Conceptually:

```text
async function
      ↓
returns Promise
      ↓
await Promise
      ↓
get result
```

## 21. Promise

A Promise represents a result that may be available **later**.

```ts
const promise = fetch("/api/users");
```

You can consume it with:

```ts
promise.then(result => {
  console.log(result);
});
```

Here:

```ts
result => {
  console.log(result);
}
```

is a **callback**.

So:

```text
Promise
   │
   └── .then(callback)
```

## 22. `async/await`

Instead of:

```ts
getUser().then(user => {
  console.log(user);
});
```

you can write:

```ts
const user = await getUser();
console.log(user);
```

inside an async function.

## 23. Error handling

With Promises:

```ts
getUser()
  .then(user => console.log(user))
  .catch(error => console.error(error))
  .finally(() => console.log("Finished"));
```

With `async/await`:

```ts
try {
  const user = await getUser();
} catch (error) {
  console.error(error);
} finally {
  console.log("Finished");
}
```

## 24. IIFE

**Immediately Invoked Function Expression**

A function that runs immediately after being created:

```ts
(function () {
  console.log("Runs immediately");
})();
```

Less common in modern TypeScript because we have modules and other better patterns.

## 25. Recursion

A function calling itself:

```ts
function countdown(n: number) {
  if (n === 0) return;

  console.log(n);
  countdown(n - 1);
}

countdown(3);
```

Output:

```text
3
2
1
```

Every recursive function needs a **base condition** to stop.

## 26. Method

A function belonging to an object/class is called a method.

```ts
const user = {
  name: "John",

  greet() {
    console.log("Hello");
  }
};

user.greet();
```

## 27. Function Returning Function

```ts
function multiplyBy(x: number) {
  return (y: number) => x * y;
}

const double = multiplyBy(2);

double(5); // 10
```

This involves:

```text
Function
   ↓
returns function
   ↓
Higher-order function
   ↓
Closure
```

This one example actually demonstrates **three important concepts**.

## 28. Function Composition

Combining functions:

```ts
const double = (x: number) => x * 2;
const addOne = (x: number) => x + 1;

const result = addOne(double(5));

console.log(result); // 11
```

Conceptually:

```text
5
↓
double
↓
10
↓
addOne
↓
11
```

## 29. Currying

Converting a function with multiple arguments into a chain of functions.

Normal:

```ts
function add(a: number, b: number) {
  return a + b;
}
```

Curried:

```ts
function add(a: number) {
  return (b: number) => a + b;
}

add(2)(3); // 5
```

## 🧠 The entire picture

This is the mental map I'd remember:

```text
                         FUNCTIONS
                             │
             ┌───────────────┼────────────────┐
             │               │                │
          Syntax          Behavior         Advanced
             │               │                │
     ┌───────┼──────┐       │        ┌───────┼────────┐
     │       │      │       │        │       │        │
 declaration expression arrow      closure  currying composition
                            │
                     ┌──────┼───────┐
                     │      │       │
                  callback  HOF   first-class
                     │
                     │
                  Async
                     │
                  Promise
                     │
                async/await
```

#### ⭐ If you're preparing for real TypeScript/React/Node development

Prioritize these:

**Must know**

1. Parameters / arguments
2. Return values
3. Arrow functions
4. Scope
5. `this`
6. Callbacks
7. Higher-order functions
8. Closures
9. Function types
10. Promises
11. `async/await`
12. Error handling

**Good to know**
13. Pure functions
14. Side effects
15. Recursion
16. Composition
17. Currying
18. IIFE

The most important connection to really understand is:

**Function → First-class function → Callback → Higher-order function → Closure → Promise → Async/Await**

Once that chain is clear, a *lot* of JavaScript/TypeScript code starts looking much less mysterious.
