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

# 5.  Answers

## Conditional Execution

### 1. How does `if` determine whether a block executes?

`if` evaluates its condition and applies **ToBoolean conversion** to the result.

```js
if (value) {
    // executes if value is truthy
}
```

It does **not** require the expression to literally produce `true` or `false`.

```js
if (1) {}        // executes
if ("hello") {}  // executes
if ([]) {}       // executes
if ({}) {}       // executes

if (0) {}        // doesn't execute
if ("") {}       // doesn't execute
if (null) {}     // doesn't execute
```

---

### 2. Does an `if` condition have to be a Boolean?

**No.**

JavaScript converts the expression to Boolean.

```js
const user = { name: "Danishan" };

if (user) {
    console.log("User exists");
}
```

`user` is an object, but objects are truthy.

You can explicitly convert it:

```js
if (Boolean(value)) {}
```

but normally this isn't necessary.

---

### 3. Multiple `if` vs `if...else if`

This is an important distinction.

### Multiple `if`

Every condition is independently checked:

```js
if (x > 0) {
    console.log("positive");
}

if (x > 5) {
    console.log("greater than 5");
}
```

If `x = 10`, **both execute**.

### `if...else if`

Only the **first matching branch** executes:

```js
if (x > 0) {
    console.log("positive");
} else if (x > 5) {
    console.log("greater than 5");
}
```

For `x = 10`, only `"positive"` executes.

### Mental model

```text
multiple if
→ independent decisions

if / else if / else
→ one decision chain
```

---

### 4. How does nested control flow execute?

The inner block is evaluated only if the outer condition allows execution to reach it.

```js
if (user) {
    if (user.isAdmin) {
        console.log("Admin");
    }
}
```

Execution:

```text
user exists?
    ↓ yes
isAdmin?
    ↓ yes
"Admin"
```

If `user` is falsy, the inner condition is never evaluated.

This matters when the inner condition depends on something that may not exist.

---

### 5. When prefer early returns over nested `if`?

Prefer early returns when you can reject invalid cases early and keep the main path flat.

### Nested version

```js
function processOrder(order) {
    if (order) {
        if (order.isPaid) {
            if (order.items.length > 0) {
                return process(order);
            }
        }
    }

    return null;
}
```

### Early-return version

```js
function processOrder(order) {
    if (!order) return null;
    if (!order.isPaid) return null;
    if (order.items.length === 0) return null;

    return process(order);
}
```

The second version is generally easier to read, test, and maintain.

This is often called the **guard clause pattern**.

---

# `switch`

## 6. How does `switch` match cases?

`switch` evaluates the expression once and compares it against each `case`.

```js
switch (status) {
    case "active":
        console.log("Active");
        break;

    case "inactive":
        console.log("Inactive");
        break;
}
```

Conceptually, case matching uses **strict equality (`===`)**.

---

## 7. Does `switch` use `==` or `===`?

**Strict equality semantics (`===`).**

```js
const value = "1";

switch (value) {
    case 1:
        console.log("number");
        break;

    case "1":
        console.log("string");
        break;
}
```

Output:

```text
string
```

Because:

```js
"1" === 1
// false
```

There is no automatic conversion like loose equality.

---

## 8. Why does `switch` fall through?

Once JavaScript finds a matching case, execution continues into subsequent cases until it encounters:

```js
break
```

or the `switch` ends.

```js
const x = 1;

switch (x) {
    case 1:
        console.log("A");

    case 2:
        console.log("B");

    case 3:
        console.log("C");
}
```

Output:

```text
A
B
C
```

This is called **fall-through**.

---

## 9. What does `break` do inside `switch`?

It exits the `switch` statement.

```js
switch (x) {
    case 1:
        console.log("A");
        break;

    case 2:
        console.log("B");
        break;
}
```

Without `break`, execution continues into the next case.

---

## 10. When intentionally use fall-through?

One common legitimate use is grouping multiple values with the same behavior.

```js
switch (role) {
    case "admin":
    case "manager":
    case "lead":
        grantDashboardAccess();
        break;

    case "guest":
        grantReadOnlyAccess();
        break;
}
```

Here:

```text
admin
manager
lead
   ↓
same behavior
```

This is intentional fall-through.

---

# Loops

## 11. Exact execution order of a `for` loop

Consider:

```js
for (initialization; condition; update) {
    body();
}
```

Execution order:

```text
1. initialization
       ↓
2. condition
       ↓
3. body
       ↓
4. update
       ↓
5. condition
       ↓
6. body
       ↓
7. update
       ↓
     repeat
```

Example:

```js
for (let i = 0; i < 3; i++) {
    console.log(i);
}
```

Execution:

```text
i = 0
check 0 < 3 → true
print 0
i++ → 1

check 1 < 3 → true
print 1
i++ → 2

check 2 < 3 → true
print 2
i++ → 3

check 3 < 3 → false
stop
```

---

# 12. `for` vs `while` vs `do...while`

| Loop         | Condition checked     | Guaranteed execution? | Typical use                      |
| ------------ | --------------------- | --------------------- | -------------------------------- |
| `for`        | Before each iteration | No                    | Known/count-controlled iteration |
| `while`      | Before each iteration | No                    | Condition-controlled iteration   |
| `do...while` | After each iteration  | **Yes, once**         | Must execute at least once       |

Example:

```js
for (let i = 0; i < 5; i++) {}
```

```js
let i = 0;

while (i < 5) {
    i++;
}
```

```js
let i = 0;

do {
    i++;
} while (i < 5);
```

---

# 13. Why does `do...while` execute at least once?

Because the condition comes **after the body**.

```js
do {
    console.log("Hello");
} while (false);
```

Output:

```text
Hello
```

Execution:

```text
body
 ↓
condition
 ↓
false
 ↓
stop
```

This is useful for things like retry/input flows where one attempt must happen before checking whether to continue.

---

# 14. What makes an infinite loop?

A loop becomes infinite when its termination condition never becomes false.

```js
while (true) {
    // forever
}
```

Or accidentally:

```js
for (let i = 0; i < 10; ) {
    console.log(i);
}
```

`i` never changes.

Another common bug:

```js
let i = 0;

while (i < 10) {
    console.log(i);
    i--;
}
```

`i` moves away from the termination condition.

---

# 15. What if `for` update expression is omitted?

The loop doesn't automatically update anything.

```js
for (let i = 0; i < 3;) {
    console.log(i);
}
```

This is an infinite loop because `i` remains `0`.

You can manually update it:

```js
for (let i = 0; i < 3;) {
    console.log(i);
    i++;
}
```

The three components are independent:

```js
for (
    initialization;
    condition;
    update
)
```

You can omit any of them.

---

# `for...in` vs `for...of`

## 16. Difference between `for...in` and `for...of`

This is **very important**.

|                    | `for...in`                | `for...of`            |
| ------------------ | ------------------------- | --------------------- |
| Iterates           | **Keys / property names** | **Values**            |
| Arrays             | Indexes                   | Elements              |
| Objects            | Enumerable keys           | Not directly iterable |
| Strings            | Indexes                   | Characters            |
| Sets               | Not useful for values     | Values                |
| Requires iterable? | No                        | **Yes**               |

Example:

```js
const arr = ["a", "b", "c"];

for (const x in arr) {
    console.log(x);
}
```

Output:

```text
0
1
2
```

But:

```js
for (const x of arr) {
    console.log(x);
}
```

Output:

```text
a
b
c
```

### Easy memory trick

> **in → index/key**
> **of → object/value**

---

# 17. Why avoid `for...in` for array values?

Because `for...in` iterates **enumerable property keys**, not array values.

```js
const arr = ["a", "b"];

arr.custom = "hello";

for (const key in arr) {
    console.log(key);
}
```

Potential output:

```text
0
1
custom
```

That's usually not what you want when processing an array.

Use:

```js
for (const value of arr) {
    console.log(value);
}
```

instead.

---

# 18. What does `for...of` rely on?

`for...of` relies on the object's **iterator protocol**.

An object is iterable if it provides:

```js
Symbol.iterator
```

For example:

```js
const arr = [1, 2, 3];

console.log(typeof arr[Symbol.iterator]);
```

The array provides an iterator.

You can also create your own iterable:

```js
const obj = {
    *[Symbol.iterator]() {
        yield 1;
        yield 2;
        yield 3;
    }
};

for (const value of obj) {
    console.log(value);
}
```

Output:

```text
1
2
3
```

This is a good SDE-2-level connection:

```text
for...of
   ↓
iterable
   ↓
Symbol.iterator
   ↓
iterator
   ↓
next()
```

---

# 19. Why does `for...of` work with strings and Sets?

Because they implement the **iterable protocol**.

```js
for (const char of "ABC") {
    console.log(char);
}
```

Output:

```text
A
B
C
```

And:

```js
const set = new Set([10, 20, 30]);

for (const value of set) {
    console.log(value);
}
```

Output:

```text
10
20
30
```

The common property isn't "being an array."

It's:

```js
object[Symbol.iterator]
```

---

# 20. What if object isn't iterable?

This:

```js
const user = {
    name: "Danishan"
};

for (const value of user) {
    console.log(value);
}
```

throws a `TypeError`.

Because a normal object doesn't implement the iterable protocol.

If you want its values:

```js
for (const value of Object.values(user)) {
    console.log(value);
}
```

Or keys:

```js
for (const key of Object.keys(user)) {
    console.log(key);
}
```

---

# Control Transfer

## 21. `break` vs `continue` vs `return`

| Keyword    | Effect                   |
| ---------- | ------------------------ |
| `break`    | Exit current loop/switch |
| `continue` | Skip current iteration   |
| `return`   | Exit entire function     |

Example:

```js
for (let i = 0; i < 5; i++) {
    if (i === 2) break;
    console.log(i);
}
```

Output:

```text
0
1
```

---

`continue`:

```js
for (let i = 0; i < 5; i++) {
    if (i === 2) continue;
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

---

`return`:

```js
function test() {
    for (let i = 0; i < 5; i++) {
        if (i === 2) return;
        console.log(i);
    }
}
```

Output:

```text
0
1
```

But unlike `break`, `return` exits the **function itself**.

---

# 22. What happens to `for` update after `continue`?

This is a classic interview trap.

Consider:

```js
for (let i = 0; i < 5; i++) {
    if (i === 2) continue;

    console.log(i);
}
```

When `i === 2`:

```text
continue
   ↓
skip remaining body
   ↓
execute i++
   ↓
check condition
```

So the update expression **does execute**.

This is different from:

```js
while (...)
```

where `continue` jumps to the condition check.

---

# 23. `return` inside nested loops

`return` doesn't merely exit the inner loop.

It exits the **entire function**.

```js
function find() {
    for (let i = 0; i < 10; i++) {
        for (let j = 0; j < 10; j++) {
            if (i === 2 && j === 3) {
                return [i, j];
            }
        }
    }
}
```

Once the condition is reached:

```text
inner loop
    ↓
outer loop
    ↓
function
    ↓
EXIT
```

This makes `return` useful for search functions.

---

# 24. What are labeled statements?

Labels give a name to a statement or loop.

```js
outerLoop:
for (let i = 0; i < 5; i++) {
    for (let j = 0; j < 5; j++) {
        // ...
    }
}
```

You can then do:

```js
break outerLoop;
```

or:

```js
continue outerLoop;
```

---

# 25. When is `break label` useful?

Mainly when you need to exit **multiple nested loops**.

Without labels:

```js
let found = false;

for (let i = 0; i < 10 && !found; i++) {
    for (let j = 0; j < 10; j++) {
        if (condition(i, j)) {
            found = true;
            break;
        }
    }
}
```

With a label:

```js
search:
for (let i = 0; i < 10; i++) {
    for (let j = 0; j < 10; j++) {
        if (condition(i, j)) {
            break search;
        }
    }
}
```

Labels are valid, but don't overuse them. Often extracting the search into a function and using `return` produces cleaner code.

---

# Practical Reasoning

## 26. Stop iterating as soon as you find a match

With a normal loop:

```js
for (const user of users) {
    if (user.id === targetId) {
        return user;
    }
}
```

This is excellent when you're inside a function.

Alternatively:

```js
for (const user of users) {
    if (user.id === targetId) {
        break;
    }
}
```

If you need the result, `return` is often cleaner.

You can also use:

```js
const user = users.find(user => user.id === targetId);
```

The important concept is:

> Don't scan the rest of the collection after you've already found what you need.

---

# 27. Why can't `break` be used inside `forEach()`?

Because `break` applies to **loop/switch statements**, not callback functions.

This is invalid:

```js
users.forEach(user => {
    if (user.id === targetId) {
        break; // SyntaxError
    }
});
```

`forEach()` isn't syntactically a loop statement.

It invokes a callback for each element.

If you need early termination:

```js
for (const user of users) {
    if (user.id === targetId) {
        return user;
    }
}
```

Or use:

```js
users.find(...)
```

---

# 28. When use `for...of` instead of `forEach()`?

Use `for...of` when you need **control flow**.

For example:

```js
for (const user of users) {
    if (!user.active) continue;

    if (user.id === targetId) {
        return user;
    }
}
```

You can use:

```text
break
continue
return
await
```

naturally.

With `forEach()`, control flow is restricted because you're inside a callback.

### Particularly important with async code

Avoid:

```js
users.forEach(async user => {
    await processUser(user);
});
```

`forEach()` doesn't await those callbacks.

Instead:

```js
for (const user of users) {
    await processUser(user);
}
```

if you need sequential processing.

---

# 29. How do nested loops affect complexity?

Suppose:

```js
for (const a of A) {
    for (const b of B) {
        process(a, b);
    }
}
```

If:

```text
A = n
B = m
```

then complexity is:

```text
O(n × m)
```

If both are approximately `n`:

```text
O(n²)
```

Three nested loops can become:

```text
O(n³)
```

But don't automatically say:

> "Nested loop = O(n²)."

That's not always true.

Example:

```js
for (const user of users) {
    for (const permission of permissions) {
        // ...
    }
}
```

Complexity is:

```text
O(users × permissions)
```

Only if both grow together as `n` does it become `O(n²)`.

---

# 30. How can early termination improve performance?

Suppose:

```js
for (const user of users) {
    if (user.id === targetId) {
        return user;
    }
}
```

Worst case:

```text
O(n)
```

But if the target is near the beginning:

```text
1–2 iterations
```

instead of scanning all `n`.

This doesn't change the **worst-case Big-O** from `O(n)`, but it improves the **average/practical runtime** when matches are often found early.

For nested searches, early termination can be even more valuable:

```js
search:
for (const user of users) {
    for (const permission of permissions) {
        if (matches(user, permission)) {
            result = permission;
            break search;
        }
    }
}
```

Instead of necessarily examining every pair.

---

# 🧠 SDE-2 Cheat Sheet

| Concept             | Remember                                               |
| ------------------- | ------------------------------------------------------ |
| `if`                | Uses truthiness                                        |
| `if / else if`      | First matching branch                                  |
| Multiple `if`       | Every condition independently evaluated                |
| `switch`            | Strict equality semantics                              |
| `break`             | Exit loop/switch                                       |
| Fall-through        | `switch` continues without `break`                     |
| `for`               | init → condition → body → update                       |
| `while`             | condition → body                                       |
| `do...while`        | body → condition                                       |
| `for...in`          | **keys/indexes**                                       |
| `for...of`          | **values**                                             |
| `for...of`          | Requires iterable / `Symbol.iterator`                  |
| `break`             | Stop loop                                              |
| `continue`          | Skip iteration                                         |
| `return`            | Exit function                                          |
| `continue` in `for` | Update expression still runs                           |
| `forEach`           | Cannot use `break`/`continue`                          |
| nested loops        | Usually multiplicative complexity                      |
| early termination   | Improves practical/average work, not necessarily Big-O |

### The 5 things I'd make absolutely automatic for your SDE-2 interview

```text
1. for...in  → keys
   for...of  → values

2. for...of  → iterable protocol / Symbol.iterator

3. break     → loop
   continue  → iteration
   return    → function

4. continue in a for loop
   → update expression still executes

5. forEach()
   → callback-based; break/continue don't work
   → use for...of when you need control flow
```
