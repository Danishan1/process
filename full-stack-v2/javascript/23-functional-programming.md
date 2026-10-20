# 21. JavaScript Functional Programming — SDE-II Revision

Functional programming in JavaScript is **not about writing everything with arrow functions**.

The real idea is:

> **Treat computation as transformations of values, minimize shared mutable state and side effects, and make functions predictable and composable.**

For SDE-II, you should understand both the **theory** and where functional style helps or hurts in production Node.js/TypeScript code.

---

# 21.1 First-Class Functions

JavaScript treats functions as **first-class values**.

That means a function can be:

* assigned to a variable
* passed as an argument
* returned from another function
* stored in an object/array

Example:

```js
const greet = function (name) {
    return `Hello ${name}`;
};

function execute(fn, value) {
    return fn(value);
}

execute(greet, "Danishan");
```

Here:

```text
greet
  ↓
function value
  ↓
passed into execute()
  ↓
called inside execute()
```

This is the foundation for almost everything else in functional JavaScript.

---

# 21.2 Higher-Order Functions

A **higher-order function** is a function that:

1. accepts another function, or
2. returns another function, or
3. both.

Example:

```js
function applyOperation(a, b, operation) {
    return operation(a, b);
}

const add = (a, b) => a + b;
const multiply = (a, b) => a * b;

applyOperation(10, 20, add);
applyOperation(10, 20, multiply);
```

`applyOperation()` is a higher-order function.

---

## Common JavaScript HOFs

You already use them constantly:

```js
map()
filter()
reduce()
find()
some()
every()
sort()
forEach()
```

For example:

```js
users
    .filter(user => user.active)
    .map(user => user.name);
```

`filter()` and `map()` receive functions.

---

# 21.3 Pure Functions

A pure function has two important properties:

### 1. Same input → same output

### 2. No observable side effects

Example:

```js
function add(a, b) {
    return a + b;
}
```

For:

```js
add(10, 20)
```

the result will always be:

```text
30
```

---

## Impure function

```js
let count = 0;

function increment() {
    count++;
    return count;
}
```

Calling:

```js
increment();
increment();
increment();
```

produces:

```text
1
2
3
```

The result depends on external mutable state.

Therefore it isn't pure.

---

# 21.4 Side Effects

A side effect is an observable interaction beyond simply producing a return value.

Examples:

```js
console.log()
database.write()
file.write()
network request
changing global state
changing an object passed by reference
DOM manipulation
sending an event
updating a cache
```

For example:

```js
function createUser(user) {
    database.insert(user);
}
```

The database write is a side effect.

---

# 21.5 Pure Doesn't Mean "No Real Work"

This distinction is important.

A function can perform expensive computation and still be pure.

```js
function fibonacci(n) {
    // expensive computation
}
```

If it:

* doesn't modify external state
* doesn't depend on changing external state
* returns the same result for the same input

it can still be pure.

---

# 21.6 Why Pure Functions Matter

Pure functions are easier to:

```text
test
debug
cache
reason about
reuse
compose
parallelize conceptually
```

For example:

```js
function calculatePrice(price, tax) {
    return price + price * tax;
}
```

Testing is trivial:

```js
calculatePrice(100, 0.18);
```

No database, filesystem, network, or global state is required.

---

# 21.7 Immutability

Immutability means:

> **Don't modify an existing value; create a new value representing the updated state.**

Instead of:

```js
user.name = "Ali";
```

you can create:

```js
const updatedUser = {
    ...user,
    name: "Ali"
};
```

The original object remains unchanged.

---

# 21.8 Important: `const` ≠ Immutability

This is a classic interview question.

```js
const user = {
    name: "John"
};

user.name = "Ali";
```

This is allowed.

Why?

`const` prevents **rebinding the variable**:

```js
user = anotherObject; // error
```

It doesn't make the referenced object immutable.

So:

```text
const
    ↓
binding cannot change

immutability
    ↓
value/state cannot be modified
```

Different concepts.

---

# 21.9 Shallow Immutability

This:

```js
const updated = {
    ...user
};
```

only creates a shallow copy.

If:

```js
user.address.city
```

is nested, the nested object may still be shared.

This connects directly to Point 20's:

> Spread vs `structuredClone()`.

---

# 21.10 Referential Transparency

This is a more theoretical concept but useful for understanding functional programming.

An expression is **referentially transparent** when it can be replaced with its result without changing program behavior.

For example:

```js
const result = add(10, 20);
```

Since:

```js
add(10, 20)
```

always means:

```text
30
```

you can conceptually replace:

```js
add(10, 20)
```

with:

```js
30
```

without changing behavior.

---

## Compare this

```js
Date.now()
```

You cannot safely replace it with one fixed value everywhere because the result changes.

Therefore it isn't referentially transparent.

Similarly:

```js
Math.random()
```

is not referentially transparent.

---

# 21.11 Pure Function vs Referential Transparency

They're closely related but not identical concepts.

Think:

```text
Pure function
     ↓
predictable behavior
     ↓
same input → same output
     ↓
no relevant side effects
     ↓
expressions become referentially transparent
```

For interview purposes, explain the distinction rather than treating them as exact synonyms.

---

# 21.12 Function Composition

Composition means combining small functions to create a larger operation.

Suppose:

```js
const double = x => x * 2;

const increment = x => x + 1;
```

We can compose:

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

---

# 21.13 Composition in Real Backend Code

Imagine:

```js
const normalizeEmail = email =>
    email.trim().toLowerCase();

const validateEmail = email =>
    email.includes("@");

const createUserInput = email => ({
    email
});
```

You can build a pipeline:

```js
const normalized = normalizeEmail(input);
const valid = validateEmail(normalized);

if (valid) {
    const user = createUserInput(normalized);
}
```

This makes each transformation independently testable.

---

# 21.14 Composition vs Giant Function

Instead of:

```js
function processUser(user) {
    // validate
    // normalize
    // calculate
    // transform
    // format
    // save
    // notify
}
```

you can separate:

```text
input
 ↓
normalize
 ↓
validate
 ↓
transform
 ↓
persist
 ↓
notify
```

But there's an important production caveat:

> **Not every workflow should be forced into pure function composition.**

Database writes, network calls, logging, events etc. are naturally side-effectful.

A good architecture often isolates them.

---

# 21.15 Currying

Currying transforms a function with multiple arguments:

```js
f(a, b, c)
```

into nested single-argument functions:

```js
f(a)(b)(c)
```

Example:

```js
function multiply(a) {
    return function (b) {
        return a * b;
    };
}
```

Usage:

```js
multiply(5)(10);
```

Result:

```text
50
```

---

# 21.16 Currying with Arrow Functions

```js
const multiply =
    a =>
    b =>
    a * b;
```

Then:

```js
const multiplyByFive = multiply(5);

multiplyByFive(10);
multiplyByFive(20);
```

Results:

```text
50
100
```

Notice something important:

```text
multiply(5)
     ↓
returns function
     ↓
that function remembers a
```

That's where **closures** come in.

---

# 21.17 Currying + Closures

When:

```js
const multiplyByFive = multiply(5);
```

the returned function retains access to:

```text
a = 5
```

even after `multiply()` has returned.

So:

```text
Currying
   +
Closure
   ↓
specialized function
```

This is one reason currying is naturally expressed in JavaScript.

---

# 21.18 Partial Application

Partial application means:

> **Fix some arguments now and provide the remaining arguments later.**

Example:

```js
function createLogger(prefix, message) {
    return `[${prefix}] ${message}`;
}

const errorLogger = message =>
    createLogger("ERROR", message);

errorLogger("Database failed");
```

We've fixed:

```text
prefix = "ERROR"
```

and leave:

```text
message
```

for later.

---

# 21.19 Currying vs Partial Application

This is a common interview question.

### Currying

Transforms:

```js
f(a, b, c)
```

into:

```js
f(a)(b)(c)
```

### Partial application

Creates a new function with some arguments already fixed:

```js
f(a, b, c)

↓ fix a

g(b, c)
```

They are related, but **not the same thing**.

---

# 21.20 `map()`

`map()` transforms every element.

```js
const numbers = [1, 2, 3];

const doubled = numbers.map(x => x * 2);
```

Result:

```text
[2, 4, 6]
```

Important property:

> `map()` normally produces a new array with the same number of elements.

---

# 21.21 `filter()`

`filter()` selects elements.

```js
const numbers = [1, 2, 3, 4];

const even = numbers.filter(x => x % 2 === 0);
```

Result:

```text
[2, 4]
```

Think:

```text
map
  → transform

filter
  → select
```

---

# 21.22 `reduce()`

`reduce()` combines multiple values into one accumulated result.

```js
const numbers = [1, 2, 3, 4];

const total = numbers.reduce(
    (sum, value) => sum + value,
    0
);
```

Result:

```text
10
```

But `reduce()` isn't simply "for summing."

It can construct:

```text
objects
arrays
maps
indexes
groupings
aggregations
```

---

# 21.23 `reduce()` Example: Indexing

Suppose:

```js
const users = [
    { id: 1, name: "A" },
    { id: 2, name: "B" }
];
```

You can build:

```js
const byId = users.reduce((result, user) => {
    result[user.id] = user;
    return result;
}, {});
```

Result conceptually:

```js
{
    1: { id: 1, name: "A" },
    2: { id: 2, name: "B" }
}
```

This is a common backend transformation.

---

# 21.24 `map` + `filter` + `reduce`

A common functional pipeline:

```js
const total = orders
    .filter(order => order.status === "paid")
    .map(order => order.amount)
    .reduce((sum, amount) => sum + amount, 0);
```

Conceptually:

```text
orders
   ↓
filter
   ↓
paid orders
   ↓
map
   ↓
amounts
   ↓
reduce
   ↓
total
```

This is declarative.

You're describing **what transformation you want** rather than manually controlling indexes.

---

# 21.25 Declarative vs Imperative

### Imperative

You describe **how** to do something.

```js
const result = [];

for (const user of users) {
    if (user.active) {
        result.push(user.name);
    }
}
```

### Declarative

You describe **what** you want.

```js
const result = users
    .filter(user => user.active)
    .map(user => user.name);
```

---

# 21.26 Declarative Doesn't Mean "Always Better"

This is important at SDE-II.

Functional/declarative code can become worse if overused.

For example:

```js
const result = data
    .filter(...)
    .map(...)
    .filter(...)
    .map(...)
    .reduce(...);
```

For a huge dataset, multiple intermediate arrays may be created.

A simple loop might be:

* easier to debug
* more memory efficient
* faster

So don't blindly apply functional programming.

---

# 21.27 Functional Style in Node.js

A very useful architecture is:

```text
HTTP Request
     ↓
Controller
     ↓
Pure business transformations
     ↓
Repository / external services
     ↓
Database
```

For example:

```js
function calculateOrderTotal(items) {
    return items
        .map(item => item.price * item.quantity)
        .reduce((total, value) => total + value, 0);
}
```

This function can remain pure.

Then separately:

```js
async function createOrder(order) {
    const total = calculateOrderTotal(order.items);

    await repository.save({
        ...order,
        total
    });
}
```

Now:

```text
Pure logic
   ↓
easy testing

Side effects
   ↓
isolated infrastructure
```

This is much more useful than trying to make your entire backend "functional."

---

# 21.28 Closures in Functional Programming

Closures allow functions to retain state without exposing that state directly.

Example:

```js
function createCounter() {
    let count = 0;

    return function () {
        return ++count;
    };
}

const counter = createCounter();

counter(); // 1
counter(); // 2
counter(); // 3
```

`count` isn't globally accessible.

The returned function retains access to it.

This can provide a form of encapsulation.

---

# 21.29 Closures Can Also Create Problems

Remember Point 17 — memory management.

Consider:

```js
function createProcessor(largeData) {
    return function process() {
        console.log(largeData.length);
    };
}
```

As long as the returned function is retained, the closure may keep `largeData` reachable.

Therefore:

```text
Closure
   ↓
captured variable
   ↓
object remains reachable
```

This isn't automatically a leak.

It becomes a problem when the closure is retained longer than necessary.

---

# 21.30 Functional Programming and Async JavaScript

Promises themselves encourage functional-style composition.

For example:

```js
fetchUser()
    .then(normalizeUser)
    .then(validateUser)
    .then(saveUser);
```

Each stage transforms the previous result.

Similarly:

```js
const user = await fetchUser();

const normalized = normalizeUser(user);

const validated = validateUser(normalized);

await saveUser(validated);
```

The second version can sometimes be easier to read, but the underlying idea is still transformation/composition.

---

# 21.31 Functional Programming and Immutability in React

This is particularly important if you're working across frontend/backend.

Instead of:

```js
state.users.push(user);
```

you typically create new state:

```js
setUsers(prev => [
    ...prev,
    user
]);
```

Why?

Because state-management systems often rely on **reference identity** to detect changes efficiently.

So functional principles aren't merely academic.

They influence real application architecture.

---

# 21.32 Common SDE-II Interview Traps

### Trap 1

> "`const` means immutable."

❌ Wrong.

`const` means the binding cannot be reassigned.

---

### Trap 2

> "Every arrow function is functional programming."

❌ Wrong.

Arrow functions are merely syntax.

---

### Trap 3

> "`map()` modifies the original array."

❌ Normally false.

`map()` returns a new array, although the objects inside it can still be shared references.

---

### Trap 4

> "`reduce()` should always be used instead of loops."

❌ Wrong.

Choose based on readability, performance, and the actual transformation.

---

### Trap 5

> "Pure functions cannot call other functions."

❌ Wrong.

A pure function can call another pure function.

```js
const double = x => x * 2;

const calculate = x =>
    double(x) + 10;
```

Still pure.

---

# 21.33 The Core Mental Model

Think about functional programming as this pipeline:

```text
                 FUNCTIONAL JAVASCRIPT
                         │
          ┌──────────────┴──────────────┐
          │                             │
      Functions                    Data
          │                             │
   first-class values              immutable
          │                             │
   higher-order functions          transformations
          │                             │
       closures                map/filter/reduce
          │                             │
     composition                predictable state
          └──────────────┬──────────────┘
                         │
                   fewer surprises
                         │
                 easier reasoning
                         │
              easier testing/maintenance
```

---

# 21.34 What You Should Be Able to Explain

For SDE-II, I would expect you to comfortably answer:

| Concept                  | You should be able to explain                |
| ------------------------ | -------------------------------------------- |
| First-class functions    | Why functions can be treated as values       |
| HOF                      | Function accepting/returning functions       |
| Pure function            | Same input/output + no relevant side effects |
| Immutability             | Don't mutate existing state                  |
| `const`                  | Binding vs immutability                      |
| Referential transparency | Expression can be replaced by its result     |
| Composition              | Combining small functions                    |
| Currying                 | `f(a)(b)(c)` transformation                  |
| Partial application      | Pre-fixing some arguments                    |
| `map`                    | Transform                                    |
| `filter`                 | Select                                       |
| `reduce`                 | Accumulate/transform into a result           |
| Side effects             | External observable changes                  |
| Closures                 | Captured lexical state                       |
| Declarative              | Describe what                                |
| Imperative               | Describe how                                 |

---

# 21.35 The Most Important SDE-II Perspective

Don't approach functional programming as:

> **"I need to convert my Node.js application into functional programming."**

Instead:

> **"I should isolate pure transformations, minimize unnecessary shared mutable state, compose reusable operations, and keep side effects at clear boundaries."**

For a backend service, a very healthy structure is:

```text
              REQUEST
                 │
                 ▼
          ┌─────────────┐
          │ Controller  │
          └──────┬──────┘
                 │
                 ▼
       ┌───────────────────┐
       │ Pure Business     │
       │ Logic / Functions  │
       └─────────┬─────────┘
                 │
                 ▼
       ┌───────────────────┐
       │ Side-effect       │
       │ Boundary          │
       └───────┬─────┬─────┘
               │     │
              DB   Redis
               │     │
             Kafka  API
```

**Pure logic is where functional programming gives you maximum leverage.**

**Side effects aren't bad — uncontrolled side effects are.**

That distinction is the real SDE-II takeaway from Point 21.
