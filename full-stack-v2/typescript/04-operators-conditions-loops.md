# Summary - Operator, Conditions, Loops

#### 1. Operators

| Area                | Concepts                                       |
| ------------------- | ---------------------------------------------- |
| Arithmetic          | `+`, `-`, `*`, `/`, `%`, `**`                  |
| Assignment          | `=`, `+=`, `-=`, `*=`, `/=`, `??=`             |
| Comparison          | `==`, `===`, `!=`, `!==`, `>`, `<`, `>=`, `<=` |
| Logical             | `&&`, `\|\|`, `!`                              |
| Nullish             | `??`, `??=`                                    |
| Ternary             | `condition ? a : b`                            |
| Unary               | `++`, `--`, `typeof`, `!`                      |
| Bitwise             | `&`, `\|`, `^`, `~`, `<<`, `>>`                |
| Optional chaining   | `?.`                                           |
| `in` / `instanceof` | Membership / type relationship                 |

#### 2. Conditions

| Concept          | Purpose                         |
| ---------------- | ------------------------------- |
| `if`             | Execute when condition is true  |
| `else`           | Alternative branch              |
| `else if`        | Multiple conditions             |
| Nested `if`      | Condition inside condition      |
| Ternary          | Short conditional expression    |
| `switch`         | Multiple discrete cases         |
| Truthy / Falsy   | How values behave in conditions |
| `===` vs `==`    | Strict vs loose equality        |
| Short-circuiting | `&&` / `\|\|` stop evaluation early|
| Nullish checking | `??` handles `null`/`undefined` |

A key concept we'll dig into is:

```text
condition
   ↓
expression evaluated
   ↓
truthy / falsy
   ↓
branch selected
```

And this is worth visualizing:

genui{"computing_fundamentals_algorithms_learning_block":{"type_id":"IF_STATEMENT_EXECUTION_FLOW"}}

#### 3. Loops

| Loop          | Best understood as                       |
| ------------- | ---------------------------------------- |
| `for`         | Repeat with explicit counter/control     |
| `while`       | Repeat while condition is true           |
| `do...while`  | Run at least once, then check            |
| `for...of`    | Iterate over **values**                  |
| `for...in`    | Iterate over **keys/indexes**            |
| `break`       | Exit loop                                |
| `continue`    | Skip current iteration                   |
| Nested loops  | Loop inside another loop                 |
| Infinite loop | Loop whose condition never becomes false |

The especially important distinction:

```ts
for (const value of array) {}
// values

for (const key in object) {}
// keys
```

# Deep Down

## 🧠 Part 1 — Operators

Operators are symbols/keywords that **perform operations on values**.

Think:

```text
values → operator → result
```

```ts
10 + 5
```

`10` and `5` → operands
`+` → operator
`15` → result

### 1. Arithmetic Operators

```ts
+    // addition
-    // subtraction
*    // multiplication
/    // division
%    // remainder
**   // exponentiation
```

Examples:

```ts
10 + 3   // 13
10 - 3   // 7
10 * 3   // 30
10 / 3   // 3.333...
10 % 3   // 1
2 ** 3   // 8
```

#### `%` is especially important

`%` gives the **remainder**, not the percentage.

```ts
10 % 3 // 1
12 % 4 // 0
7 % 2  // 1
```

That's why we use it for even/odd:

```ts
if (num % 2 === 0) {
  console.log("Even");
}
```

## 2. `+` Has a Special Behavior

This is one of JavaScript's famous quirks.

```ts
1 + 2        // 3
"1" + "2"    // "12"
"1" + 2      // "12"
1 + "2"      // "12"
```

Why?

`+` can mean:

```text
number addition
        OR
string concatenation
```

Compare:

```ts
10 - "5" // 5
```

JavaScript converts `"5"` to a number for `-`.

But:

```ts
10 + "5" // "105"
```

because `+` may become string concatenation.

## 3. Assignment Operators

Basic:

```ts
let x = 10;
```

`=` means **assign**, not "equals" in the mathematical sense.

You also have:

```ts
x += 5; // x = x + 5
x -= 5; // x = x - 5
x *= 5; // x = x * 5
x /= 5; // x = x / 5
x %= 5; // x = x % 5
```

Example:

```ts
let x = 10;

x += 5;

console.log(x); // 15
```

## 4. Comparison Operators

Comparison produces a **boolean**:

```ts
10 > 5    // true
10 < 5    // false
10 >= 10  // true
10 <= 10  // true
10 === 10 // true
10 !== 5  // true
```

Think:

```text
Comparison → true / false
```

## 5. `===` vs `==` ⭐⭐⭐

This is extremely important.

#### Strict equality

```ts
5 === 5      // true
5 === "5"    // false
```

Both **value and type** must match.

#### Loose equality

```ts
5 == "5"     // true
```

JavaScript performs type conversion.

Generally, in TypeScript/modern JavaScript:

> Prefer `===` and `!==`.

## 6. Logical Operators

Three major ones:

```ts
&&   // AND
||   // OR
!    // NOT
```

#### AND

```ts
true && true   // true
true && false  // false
```

Both conditions must be true.

#### OR

```ts
true || false  // true
false || false // false
```

At least one must be true.

#### NOT

```ts
!true  // false
!false // true
```

## 7. Short-Circuiting ⭐⭐⭐

This is where logical operators become more interesting.

Consider:

```ts
false && something()
```

JavaScript knows the whole expression must be false.

So it doesn't need to evaluate `something()`.

That's **short-circuiting**.

Similarly:

```ts
true || something()
```

JavaScript already knows the result must be true.

So `something()` isn't evaluated.

### 8. `&&` and `||` Don't Necessarily Return Booleans

This is a very important JavaScript concept.

```ts
const result = "hello" && "world";

console.log(result); // "world"
```

Why?

`&&` returns:

> the first falsy value, otherwise the last value.

Example:

```ts
"hello" && "world"
// "world"

0 && "world"
// 0

null && "world"
// null
```

For `||`:

> returns the first truthy value, otherwise the last value.

```ts
"" || "Guest"
// "Guest"

"John" || "Guest"
// "John"

0 || 100
// 100
```

This is heavily used in real code.

## 9. Nullish Coalescing `??` ⭐

`??` is similar to `||`, but **more specific**.

It only considers:

```text
null
undefined
```

as missing.

```ts
const name = null ?? "Guest";
// "Guest"
```

But:

```ts
const count = 0 ?? 10;
// 0
```

Compare:

```ts
0 || 10
// 10

0 ?? 10
// 0
```

This difference is extremely important.

#### Mental model

```text
||  → "Is this falsy?"
??  → "Is this null or undefined?"
```

## 10. Optional Chaining `?.`

Instead of:

```ts
if (user && user.address && user.address.city) {
  console.log(user.address.city);
}
```

You can write:

```ts
console.log(user?.address?.city);
```

If something along the chain is `null` or `undefined`, it safely stops.

## 11. Increment / Decrement

```ts
x++;
x--;

++x;
--x;
```

But there's an important difference between **prefix** and **postfix**.

```ts
let x = 5;

console.log(x++); // 5
console.log(x);   // 6
```

`x++`:

> use current value, then increment.

But:

```ts
let x = 5;

console.log(++x); // 6
console.log(x);   // 6
```

`++x`:

> increment first, then use the value.

## 🧠 Part 2 — Conditions

Conditions control **which code gets executed**.

## 12. `if`

```ts
if (age >= 18) {
  console.log("Adult");
}
```

JavaScript evaluates:

```ts
age >= 18
```

The result is converted to a boolean context.

## 13. `if / else`

```ts
if (age >= 18) {
  console.log("Adult");
} else {
  console.log("Minor");
}
```

Only **one branch** executes.

## 14. `else if`

```ts
if (score >= 90) {
  console.log("A");
} else if (score >= 80) {
  console.log("B");
} else if (score >= 70) {
  console.log("C");
} else {
  console.log("Fail");
}
```

JavaScript checks from **top to bottom**.

Once a condition is true, it executes that branch and skips the remaining branches.

## 15. Truthy and Falsy ⭐⭐⭐

JavaScript doesn't require the condition to literally be `true` or `false`.

For example:

```ts
if ("hello") {
  console.log("Runs");
}
```

A non-empty string is **truthy**.

These are common falsy values:

```text
false
0
-0
0n
""
null
undefined
NaN
```

Almost everything else is truthy.

Examples:

```ts
if ([]) {
  // runs
}

if ({}) {
  // runs
}
```

Even an empty array/object is truthy.

## 16. Ternary Operator

For simple conditions:

```ts
const result = age >= 18 ? "Adult" : "Minor";
```

Structure:

```text
condition ? valueIfTrue : valueIfFalse
```

It's an **expression**, so you can assign its result.

## 17. `switch`

Useful when comparing one value against multiple specific cases.

```ts
switch (day) {
  case "Monday":
    console.log("Start");
    break;

  case "Friday":
    console.log("Almost weekend");
    break;

  default:
    console.log("Normal day");
}
```

## 18. `break` in `switch` ⭐

Without `break`:

```ts
switch (x) {
  case 1:
    console.log("One");

  case 2:
    console.log("Two");
}
```

If `x === 1`, both may execute.

This is called **fall-through**.

With `break`, execution exits the switch.

## 🧠 Part 3 — Loops

A loop repeats code.

Think:

```text
initialize
    ↓
condition?
   ↙ ↘
 yes  no
 ↓     ↓
body   exit
 ↓
update
 ↓
condition
```

## 19. `for` Loop

```ts
for (let i = 0; i < 5; i++) {
  console.log(i);
}
```

Three parts:

```ts
for (
  initialization;
  condition;
  update
)
```

Execution:

```text
let i = 0
   ↓
i < 5 ?
   ↓
body
   ↓
i++
   ↓
i < 5 ?
```

Output:

```text
0
1
2
3
4
```

## 20. `while`

```ts
let i = 0;

while (i < 5) {
  console.log(i);
  i++;
}
```

The condition is checked **before** each iteration.

Therefore, it might execute **zero times**.

```ts
while (false) {
  console.log("Hello");
}
```

Nothing happens.

## 21. `do...while`

```ts
let i = 0;

do {
  console.log(i);
  i++;
} while (i < 5);
```

Difference:

```text
while
→ check first
→ execute

do...while
→ execute first
→ check
```

Therefore `do...while` always executes **at least once**.

## 22. `for...of` ⭐⭐⭐

Used to iterate over **values**.

```ts
const numbers = [10, 20, 30];

for (const number of numbers) {
  console.log(number);
}
```

Output:

```text
10
20
30
```

Think:

> **of → values**

## 23. `for...in` ⭐⭐⭐

Used to iterate over **keys/property names**.

```ts
const user = {
  name: "John",
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

Think:

> **in → keys**

For arrays, `for...in` gives indexes, not values:

```ts
const numbers = ["a", "b", "c"];

for (const index in numbers) {
  console.log(index);
}
```

Output:

```text
0
1
2
```

## 24. `break`

Immediately exits the loop.

```ts
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

## 25. `continue`

Skips the current iteration.

```ts
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

Think:

```text
break    → leave loop
continue → skip this round
```

## 26. Nested Loops

A loop inside another loop:

```ts
for (let i = 1; i <= 3; i++) {
  for (let j = 1; j <= 2; j++) {
    console.log(i, j);
  }
}
```

For every `i`, the entire inner loop runs.

```text
i = 1
  j = 1
  j = 2

i = 2
  j = 1
  j = 2

i = 3
  j = 1
  j = 2
```

This concept becomes important for:

* matrices
* grids
* combinations
* sorting algorithms
* nested data

## 🔥 The Most Important Mental Models

### Operators

```text
Arithmetic
   ↓
Comparison → true/false
   ↓
Logical → combine conditions
   ↓
Short-circuit → may stop evaluation
```

### Conditions

```text
Expression
    ↓
Truthy / Falsy
    ↓
if / else / switch
    ↓
one execution path
```

### Loops

```text
Initialization
      ↓
   Condition
      ↓
    true?
    ↙   ↘
  yes    no
   ↓      ↓
 body    exit
   ↓
 update
   ↓
condition again
```

## ⭐ Concepts You REALLY Need to Master

If you're going deep, these deserve special attention:

| Concept                  | Why important           |   |                  |
| ------------------------ | ----------------------- | - | ---------------- |
| `===` vs `==`            | Type coercion           |   |                  |
| Truthy / falsy           | Conditions everywhere   |   |                  |
| `&&` / `                 |                         | ` | Short-circuiting |
| `??`                     | Null/undefined handling |   |                  |
| `?.`                     | Safe property access    |   |                  |
| Prefix vs postfix `++`   | Evaluation order        |   |                  |
| `if / else if`           | Control flow            |   |                  |
| `switch` + `break`       | Fall-through            |   |                  |
| `for` execution order    | Predicting output       |   |                  |
| `while` vs `do...while`  | Condition timing        |   |                  |
| `for...of` vs `for...in` | Values vs keys          |   |                  |
| `break` vs `continue`    | Loop control            |   |                  |
| Nested loops             | Complex iteration       |   |                  |

And there's one **big theme** connecting all three:

> **JavaScript evaluates expressions → gets values → uses those values to make decisions → repeats those decisions through loops.**

That's the foundation we'll use for the **three-level question bank**, where the hard questions will deliberately combine things like **truthiness + short-circuiting + loops + increment operators + scope** rather than testing each concept in isolation.
