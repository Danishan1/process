# 5. Control Flow & Iteration

Control flow is about **controlling what executes, when it executes, and whether execution continues, skips, or terminates**.

The important mental model is:

```text
JavaScript normally executes
        ↓
     top → bottom
        ↓
Control-flow construct changes the path
        ↓
 ┌──────┼──────────┐
 ↓      ↓          ↓
branch  repeat    terminate/skip
```

For SDE-2 interviews, don't study `if`, `for`, `while`, etc. as isolated syntax. The deeper topic is understanding **execution paths, conditions, iteration, and control transfer**.

# 5.1 `if`

The `if` statement conditionally executes a block.

```js
if (condition) {
    // executes when condition is truthy
}
```

Example:

```js
const age = 20;

if (age >= 18) {
    console.log("Adult");
}
```

JavaScript evaluates:

```js
age >= 18
```

which produces:

```text
true
```

Therefore the block executes.

The important point is that JavaScript doesn't require the condition itself to literally be `true` or `false`.

It applies **Boolean coercion**.

For example:

```js
if ("hello") {
    console.log("runs");
}
```

because `"hello"` is truthy.

And:

```js
if (0) {
    console.log("doesn't run");
}
```

because `0` is falsy.

So:

```text
if (expression)
       ↓
evaluate expression
       ↓
Boolean interpretation
       ↓
truthy? ──→ execute block
falsy?  ──→ skip block
```

# 5.2 `else`

`else` provides an alternative execution path.

```js
if (condition) {
    // path A
} else {
    // path B
}
```

Example:

```js
const age = 16;

if (age >= 18) {
    console.log("Adult");
} else {
    console.log("Minor");
}
```

Only **one** of the two branches executes.

Conceptually:

```text
condition
   │
   ├── truthy → if block
   │
   └── falsy  → else block
```

# 5.3 `else if`

Multiple conditions can be evaluated sequentially.

```js
if (condition1) {
    ...
} else if (condition2) {
    ...
} else {
    ...
}
```

Example:

```js
const score = 75;

if (score >= 90) {
    console.log("A");
} else if (score >= 75) {
    console.log("B");
} else if (score >= 60) {
    console.log("C");
} else {
    console.log("D");
}
```

The conditions are checked **top to bottom**.

Once a condition is truthy, its corresponding block executes and the remaining branches are skipped.

So if:

```text
score = 75
```

the evaluation is:

```text
90? → false
75? → true
       ↓
     "B"
       ↓
stop checking
```

This is an important distinction from writing several independent `if` statements.

# 5.4 Multiple `if` vs `else if`

Compare:

```js
if (x > 0) {
    console.log("positive");
}

if (x > 10) {
    console.log("large");
}
```

with:

```js
if (x > 0) {
    console.log("positive");
} else if (x > 10) {
    console.log("large");
}
```

For:

```js
x = 20;
```

the first version executes **both** blocks:

```text
positive
large
```

The second executes only the first matching branch:

```text
positive
```

Why?

Because the first version contains two independent control-flow statements.

The second creates a single mutually exclusive branch chain.

# 5.5 Nested `if`

An `if` can appear inside another control-flow block.

```js
if (user) {
    if (user.isAdmin) {
        console.log("Admin");
    }
}
```

This creates nested execution paths.

Conceptually:

```text
user?
 │
 ├── false → stop
 │
 └── true
       │
       ↓
   isAdmin?
       │
       ├── false → stop
       │
       └── true → execute
```

Deeply nested conditionals can become difficult to reason about.

In production code, developers often reduce nesting using early returns:

```js
if (!user) {
    return;
}

if (!user.isAdmin) {
    return;
}

performAdminOperation();
```

This is not a different control-flow mechanism; it is a different **control-flow structure**.

# 5.6 `switch`

`switch` is useful when comparing one expression against multiple possible values.

```js
switch (expression) {
    case value1:
        ...
        break;

    case value2:
        ...
        break;

    default:
        ...
}
```

Example:

```js
const role = "admin";

switch (role) {
    case "admin":
        console.log("Full access");
        break;

    case "user":
        console.log("Limited access");
        break;

    default:
        console.log("Unknown role");
}
```

The switch expression is evaluated once.

Then JavaScript looks for a matching `case`.

# 5.7 `switch` Uses Strict Matching

A very important detail:

`switch` case matching uses **strict equality semantics**.

For example:

```js
const value = 1;

switch (value) {
    case "1":
        console.log("string");
        break;

    case 1:
        console.log("number");
        break;
}
```

The second case matches.

The string `"1"` is not considered equal to the number `1`.

This is different from `==`.

# 5.8 The Importance of `break` in `switch`

Consider:

```js
const role = "admin";

switch (role) {
    case "admin":
        console.log("Admin");
    case "user":
        console.log("User");
}
```

Output:

```text
Admin
User
```

Why?

Because after matching the `"admin"` case, execution **falls through** into the next case.

Adding:

```js
break;
```

changes the control flow:

```js
switch (role) {
    case "admin":
        console.log("Admin");
        break;

    case "user":
        console.log("User");
        break;
}
```

Now only:

```text
Admin
```

is printed.

# 5.9 Intentional Fall-Through

Fall-through isn't necessarily a bug.

It can be intentional:

```js
switch (status) {
    case "pending":
    case "processing":
        showLoading();
        break;

    case "completed":
        showSuccess();
        break;

    case "failed":
        showError();
        break;
}
```

Here both:

```text
pending
processing
```

share the same behavior.

The first case falls through intentionally to the second.

# 5.10 `default`

`default` executes when no case matches.

```js
switch (value) {
    case 1:
        ...
        break;

    case 2:
        ...
        break;

    default:
        ...
}
```

Conceptually:

```text
expression
    ↓
case 1? ──→ execute
    ↓ no
case 2? ──→ execute
    ↓ no
default
```

`default` is optional.

# 5.11 `for` Loop

The traditional `for` loop has three parts:

```js
for (initialization; condition; update) {
    // body
}
```

Example:

```js
for (let i = 0; i < 5; i++) {
    console.log(i);
}
```

The execution flow is:

```text
initialization
      ↓
condition
      ↓
   true?
   /   \
 yes    no
  ↓      ↓
body    stop
  ↓
update
  ↓
condition
```

Output:

```text
0
1
2
3
4
```

# 5.12 Understanding the Three Parts

Consider:

```js
for (let i = 0; i < 3; i++) {
    console.log(i);
}
```

### Step 1

Initialization:

```js
let i = 0;
```

### Step 2

Condition:

```js
i < 3
```

### Step 3

Body:

```js
console.log(i);
```

### Step 4

Update:

```js
i++;
```

Then return to the condition.

So the actual sequence is:

```text
let i = 0

i < 3 → true
print 0
i++

i < 3 → true
print 1
i++

i < 3 → true
print 2
i++

i < 3 → false
stop
```

This execution trace is much more important than memorizing loop syntax.

# 5.13 `for` Parts Are Optional

JavaScript allows:

```js
for (;;) {
    // infinite loop
}
```

Because all three components are optional.

You can also write:

```js
let i = 0;

for (; i < 5; ) {
    console.log(i);
    i++;
}
```

The traditional `for` loop is therefore essentially a flexible control-flow construct.

# 5.14 `while`

A `while` loop executes while its condition remains truthy.

```js
while (condition) {
    // body
}
```

Example:

```js
let i = 0;

while (i < 5) {
    console.log(i);
    i++;
}
```

Execution:

```text
condition
    ↓
truthy?
 /     \
yes     no
 ↓       ↓
body    stop
 ↓
condition again
```

# 5.15 `while` vs `for`

These are often interchangeable.

This:

```js
for (let i = 0; i < 5; i++) {
    console.log(i);
}
```

can conceptually be represented as:

```js
let i = 0;

while (i < 5) {
    console.log(i);
    i++;
}
```

The distinction is mostly about **how the loop's control structure is expressed**.

A `for` loop commonly groups:

```text
initialization
condition
update
```

together.

A `while` loop emphasizes:

```text
while condition remains true
    execute body
```

# 5.16 `do...while`

`do...while` is different because the body executes **at least once**.

```js
do {
    // body
} while (condition);
```

Example:

```js
let i = 10;

do {
    console.log(i);
    i++;
} while (i < 5);
```

Even though:

```js
i < 5
```

is initially false, the output is:

```text
10
```

because the body executes before the condition is checked.

Compare:

```js
let i = 10;

while (i < 5) {
    console.log(i);
}
```

This produces no output.

Therefore:

```text
while
    ↓
condition → body

do...while
    ↓
body → condition
```

That's the fundamental difference.

# 5.17 `for...in`

`for...in` iterates over **enumerable property keys** of an object.

Example:

```js
const user = {
    name: "Danishan",
    age: 25
};

for (const key in user) {
    console.log(key);
}
```

Output:

```text
name
age
```

Notice what we get:

```text
key
```

not the property value.

To get the value:

```js
for (const key in user) {
    console.log(user[key]);
}
```

# 5.18 `for...in` Is About Property Keys

Consider:

```js
const user = {
    name: "Danishan",
    age: 25
};
```

The loop:

```js
for (const key in user)
```

produces:

```text
"name"
"age"
```

Then:

```js
user[key]
```

retrieves:

```text
"Danishan"
25
```

So the mental model is:

```text
for...in
    ↓
enumerable property keys
```

This is why `for...in` is generally intended for objects rather than iterating array values.

# 5.19 `for...in` and Inherited Properties

`for...in` can enumerate enumerable properties inherited through the prototype chain as well.

For example, if an object inherits an enumerable property, that property can appear during `for...in`.

This is one reason that when you specifically want an object's own properties, you may use:

```js
Object.keys(obj)
```

or explicitly check ownership.

So don't simplify `for...in` to:

> "Loop through the object's own keys."

The more accurate description is:

> **Enumerate enumerable property keys, including inherited enumerable properties.**

# 5.20 `for...of`

`for...of` is fundamentally different.

It iterates over the **values produced by an iterable**.

Example:

```js
const numbers = [10, 20, 30];

for (const value of numbers) {
    console.log(value);
}
```

Output:

```text
10
20
30
```

So:

```text
for...in
→ keys

for...of
→ values from an iterable
```

This distinction is one of the most frequently tested JavaScript loop concepts.

# 5.21 Why `for...of` Works on Arrays

Arrays are iterable.

So:

```js
const numbers = [10, 20, 30];

for (const value of numbers) {
    console.log(value);
}
```

uses the array's iteration behavior to obtain:

```text
10
20
30
```

It isn't simply "array syntax."

The deeper concept is:

```text
for...of
    ↓
Iterable protocol
    ↓
Iterator
    ↓
next()
    ↓
value
```

This becomes very important later when studying **iterators, generators, and the iterable protocol**.

# 5.22 `for...of` Works With Other Iterables

For example:

```js
const text = "hello";

for (const char of text) {
    console.log(char);
}
```

Strings are iterable.

Similarly:

```js
const set = new Set([1, 2, 3]);

for (const value of set) {
    console.log(value);
}
```

works because `Set` is iterable.

So the correct abstraction is:

```text
for...of
→ anything implementing the iterable protocol
```

not:

```text
for...of
→ arrays only
```

# 5.23 `for...in` vs `for...of`

This should be crystal clear:

```js
const numbers = [10, 20, 30];
```

Using:

```js
for (const x in numbers) {
    console.log(x);
}
```

produces keys:

```text
"0"
"1"
"2"
```

Whereas:

```js
for (const x of numbers) {
    console.log(x);
}
```

produces values:

```text
10
20
30
```

Therefore:

| Loop         | Iterates over                   |
| ------------ | ------------------------------- |
| `for`        | whatever you explicitly control |
| `while`      | repeated condition              |
| `do...while` | body then condition             |
| `for...in`   | enumerable property keys        |
| `for...of`   | iterable values                 |

# 5.24 `break`

`break` terminates the nearest applicable loop or `switch`.

Example:

```js
for (let i = 0; i < 10; i++) {
    if (i === 5) {
        break;
    }

    console.log(i);
}
```

Output:

```text
0
1
2
3
4
```

Execution:

```text
i = 5
 ↓
condition true
 ↓
break
 ↓
leave loop
```

It doesn't merely skip the current iteration.

It **terminates the loop**.

# 5.25 `continue`

`continue` skips the remainder of the current iteration and proceeds with the next iteration.

```js
for (let i = 0; i < 5; i++) {
    if (i === 2) {
        continue;
    }

    console.log(i);
}
```

Output:

```text
0
1
3
4
```

At `i === 2`:

```text
continue
   ↓
skip remaining body
   ↓
next iteration
```

For a traditional `for` loop, the update expression still occurs.

So:

```js
for (let i = 0; i < 5; i++) {
    if (i === 2) {
        continue;
    }

    console.log(i);
}
```

does not get stuck at `2`.

The `i++` update executes after the `continue`.

# 5.26 `break` vs `continue`

Very important:

```text
break
 ↓
leave loop completely
```

while:

```text
continue
 ↓
skip current iteration
 ↓
continue looping
```

Think:

```text
break    → "I'm done with this loop."

continue → "I'm done with this iteration."
```

# 5.27 `return`

`return` terminates the execution of the **current function** and optionally provides a value to the caller.

Example:

```js
function add(a, b) {
    return a + b;
}
```

The expression:

```js
a + b
```

is evaluated and returned.

```js
const result = add(2, 3);
```

produces:

```text
5
```

# 5.28 `return` Is Stronger Than `break`

Consider:

```js
function findUser(users) {
    for (const user of users) {
        if (user.id === 10) {
            return user;
        }
    }

    return null;
}
```

When the matching user is found:

```text
return
 ↓
leave loop
 ↓
leave function
 ↓
send value to caller
```

So `return` doesn't just terminate the loop.

It terminates the **entire current function execution**.

# 5.29 `return` Inside Nested Loops

Consider:

```js
function search(matrix) {
    for (const row of matrix) {
        for (const value of row) {
            if (value === 10) {
                return value;
            }
        }
    }
}
```

The `return` exits:

```text
inner loop
   ↓
outer loop
   ↓
function
```

all at once.

This becomes important when comparing `break`, `continue`, and `return`.

# 5.30 Labeled Statements

JavaScript supports labels.

Syntax:

```js
labelName:
statement
```

A label can be used with `break` and `continue`.

The most practical use is escaping from nested loops.

Example:

```js
outer:
for (let i = 0; i < 5; i++) {
    for (let j = 0; j < 5; j++) {
        if (i === 2 && j === 2) {
            break outer;
        }
    }
}
```

Here:

```js
break outer;
```

doesn't break only the inner loop.

It breaks the loop associated with the `outer` label.

# 5.31 Why Labels Exist

Without labels:

```js
for (...) {
    for (...) {
        if (condition) {
            break;
        }
    }
}
```

`break` only affects the nearest loop.

A label allows:

```js
break outer;
```

to target a specific enclosing loop.

Conceptually:

```text
outer loop
    │
    └── inner loop
           │
           └── break outer
                    ↓
              exit outer loop
```

Labels are valid JavaScript but relatively uncommon in everyday application code.

# 5.32 Labeled `continue`

Labels can also target `continue`.

```js
outer:
for (let i = 0; i < 3; i++) {
    for (let j = 0; j < 3; j++) {
        if (j === 1) {
            continue outer;
        }

        console.log(i, j);
    }
}
```

Here:

```js
continue outer;
```

means:

> Skip the current iteration of the **outer** loop and proceed to its next iteration.

This is different from:

```js
continue;
```

which targets the nearest loop.

# 5.33 Conditional Execution

Conditional execution means that code executes only when some condition is satisfied.

This can happen through:

```text
if
else
else if
switch
ternary
&&
||
??
?.
```

For example:

```js
if (user) {
    processUser(user);
}
```

or:

```js
user && processUser(user);
```

or:

```js
const name = user?.name ?? "Guest";
```

These are different language constructs, but all influence **whether and when expressions execute**.

# 5.34 Nested Control Flow

Real applications frequently combine multiple control-flow constructs.

For example:

```js
for (const user of users) {
    if (!user.active) {
        continue;
    }

    if (user.role === "admin") {
        processAdmin(user);
    } else {
        processUser(user);
    }
}
```

Execution can be visualized as:

```text
for each user
      ↓
active?
 ┌────┴────┐
no         yes
 ↓          ↓
continue   role?
            │
       ┌────┴────┐
       ↓         ↓
     admin      user
       ↓         ↓
 processAdmin processUser
```

Understanding these execution paths is more important than knowing syntax individually.

# 5.35 Nested Loops and Complexity

Control flow also connects directly to algorithmic complexity.

Consider:

```js
for (const user of users) {
    for (const order of orders) {
        // ...
    }
}
```

If:

```text
users = N
orders = M
```

the body may execute:

```text
N × M
```

times.

So:

```text
nested loops
     ↓
potentially multiplicative complexity
```

This is why SDE-2 interviews often connect JavaScript control flow with **time complexity and performance reasoning**.

# 5.36 Common Iteration Patterns

### Pattern 1 — Iterate all values

```js
for (const user of users) {
    process(user);
}
```

### Pattern 2 — Search

```js
for (const user of users) {
    if (user.id === targetId) {
        return user;
    }
}
```

### Pattern 3 — Filter manually

```js
const result = [];

for (const user of users) {
    if (user.active) {
        result.push(user);
    }
}
```

### Pattern 4 — Skip unwanted values

```js
for (const user of users) {
    if (!user.active) {
        continue;
    }

    process(user);
}
```

### Pattern 5 — Stop early

```js
for (const user of users) {
    if (user.id === targetId) {
        break;
    }
}
```

The important optimization here is **early termination**.

You don't necessarily need to inspect every element.

# 5.37 Control Flow and Side Effects

Consider:

```js
for (const user of users) {
    updateDatabase(user);
}
```

The loop isn't merely producing values.

It is causing side effects.

This becomes particularly important when working with:

* database operations
* API calls
* filesystem operations
* logging
* mutations
* asynchronous operations

For example, this:

```js
for (const user of users) {
    await sendEmail(user);
}
```

has very different execution characteristics from:

```js
await Promise.all(
    users.map(user => sendEmail(user))
);
```

The second example introduces concurrency.

That topic belongs to asynchronous JavaScript, but the underlying foundation is still **control flow + iteration**.

# 5.38 A Critical Interview Trap: `forEach` and `break`

Developers sometimes write:

```js
users.forEach(user => {
    if (user.id === targetId) {
        break;
    }
});
```

This doesn't work because `break` cannot be used to terminate the surrounding `forEach` callback iteration.

`forEach` is a method invoking a callback for each element; it is not a JavaScript loop statement that `break` can target.

If you need early termination, use something such as:

```js
for (const user of users) {
    if (user.id === targetId) {
        break;
    }
}
```

or an appropriate array method such as:

```js
users.find(user => user.id === targetId);
```

This distinction is highly useful in practical JavaScript.

# 5.39 The Core Mental Model

All of Part 5 can be reduced to four major operations:

```text
                 CONTROL FLOW
                      │
       ┌──────────────┼──────────────┐
       ↓              ↓              ↓
    Branch          Repeat         Transfer
       │              │              │
   if/switch      for/while       break
       │          do...while       continue
       │          for...of         return
       │          for...in
       ↓
  conditional
  execution
```

And the central question is always:

> **What is the next statement that executes?**

For every piece of control-flow code, trace:

```text
1. What condition is evaluated?
2. What value does it produce?
3. Is it truthy or falsy?
4. Which branch executes?
5. Does execution repeat?
6. Does it skip anything?
7. Does it terminate the loop?
8. Does it terminate the function?
9. What happens on the next iteration?
```

# 5.40 SDE-2 High-Probability Questions

These are the ones I'd prioritize.

### Conditional execution

1. How does `if` determine whether a block executes?
2. Does an `if` condition have to be a Boolean?
3. Difference between multiple `if` statements and `if...else if`.
4. How does nested control flow execute?
5. When would you prefer early returns over nested `if` blocks?

### `switch`

6. How does `switch` match cases?
7. Does `switch` use `==` or `===` semantics?
8. Why does `switch` fall through?
9. What does `break` do inside a `switch`?
10. When would you intentionally use fall-through?

### Loops

11. Explain the exact execution order of a `for` loop.
12. Difference between `for`, `while`, and `do...while`.
13. Why does `do...while` always execute at least once?
14. What makes an infinite loop?
15. What happens if the update expression of a `for` loop is omitted?

### `for...in` vs `for...of`

16. What is the difference between `for...in` and `for...of`?
17. Why should you generally avoid `for...in` for array values?
18. What exactly does `for...of` rely on?
19. Why can `for...of` work with strings and Sets?
20. What happens if an object isn't iterable and you use `for...of` on it?

### Control transfer

21. Difference between `break`, `continue`, and `return`.
22. What happens to a `for` loop's update expression after `continue`?
23. How does `return` behave inside nested loops?
24. What are labeled statements?
25. When would `break label` be useful?

### Practical reasoning

26. How would you stop iterating as soon as you find a matching item?
27. Why can't `break` be used inside `Array.prototype.forEach()`?
28. When would you use `for...of` instead of `forEach()`?
29. How can nested loops affect time complexity?
30. How can early termination improve performance?

# 5.41 The Most Important Distinction

If you remember only one thing from this section, remember this:

```text
if / switch
    ↓
Choose a path

for / while / do...while
    ↓
Repeat a path

for...in
    ↓
Iterate enumerable keys

for...of
    ↓
Iterate iterable values

continue
    ↓
Skip current iteration

break
    ↓
Exit loop / switch

return
    ↓
Exit current function
```

And the deeper SDE-2 perspective is:

```text
Control Flow
     ↓
Execution Path
     ↓
Iteration count
     ↓
Side effects
     ↓
Complexity
     ↓
Performance
```

Once this foundation is solid, **Functions & Scope** is the next major concept because that's where control flow starts interacting with **function execution, lexical scope, closures, parameters, return values, `this`, and execution contexts**.
