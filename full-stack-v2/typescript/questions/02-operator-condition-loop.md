
# 🧠 Operators, Conditions & Loops — Question Bank

## 🟢 Level 1 — Beginner

**Goal: identify and use individual concepts.**

### Operators

1. What is the difference between `=` and `===`?
2. What does `%` return?
3. What is the result of `10 % 3`?
4. What is the difference between `++x` and `x++`?
5. What do `&&`, `||`, and `!` mean?
6. What is the difference between `==` and `===`?
7. What does `??` do?
8. What does `?.` do?
9. What does `+=` mean?
10. Which operators return a boolean?

### Conditions

11. What does an `if` statement do?
12. What is the purpose of `else`?
13. When would you use `else if`?
14. What is a ternary operator?
15. What is the purpose of `switch`?
16. What does `break` do inside a `switch`?
17. What does "truthy" mean?
18. What does "falsy" mean?
19. Name the common falsy values in JavaScript.
20. Is `[]` truthy or falsy? What about `{}`?

### Loops

21. What are the three parts of a `for` loop?
22. What's the difference between `while` and `do...while`?
23. What does `break` do in a loop?
24. What does `continue` do?
25. What's the difference between `for...of` and `for...in`?
26. What does `for...of` give you when iterating an array?
27. What does `for...in` give you when iterating an array?
28. Write a loop that prints numbers from `1` to `10`.
29. Write a loop that prints only even numbers from `1` to `20`.
30. Write a loop that calculates the sum from `1` to `100`.

# 🟡 Level 2 — Intermediate

**Goal: predict behavior and explain WHY.**

## Operators + Conditions

### 31. What is the output?

```ts
console.log(5 == "5");
console.log(5 === "5");
```

Explain the difference.

### 32. What is the output?

```ts
console.log("10" + 5);
console.log("10" - 5);
```

Why does `+` behave differently?

### 33. Predict:

```ts
console.log(0 || "Hello");
console.log(0 ?? "Hello");
```

Why are the results different?

### 34. Predict:

```ts
console.log(false && "Hello");
console.log(true && "Hello");

console.log(false || "Hello");
console.log(true || "Hello");
```

Explain what `&&` and `||` actually return.

### 35. What gets executed?

```ts
false && console.log("A");

true || console.log("B");

true && console.log("C");
```

Explain the short-circuiting.

### 36. Predict:

```ts
let x = 5;

console.log(x++);
console.log(x);
console.log(++x);
console.log(x);
```

## Truthiness

### 37. Which branches execute?

```ts
if ("") console.log("A");
if ("hello") console.log("B");
if (0) console.log("C");
if (1) console.log("D");
if ([]) console.log("E");
if ({}) console.log("F");
```

### 38. What does this print?

```ts
const name = "";

if (name) {
  console.log("Has name");
} else {
  console.log("No name");
}
```

### 39. Why can this be dangerous?

```ts
const count = 0;

const result = count || 10;
```

What if `0` is a valid value?

What's the better alternative?

## Conditions

### 40. What does this output?

```ts
const score = 85;

if (score >= 90) {
  console.log("A");
} else if (score >= 80) {
  console.log("B");
} else {
  console.log("C");
}
```

Why doesn't it also execute the `C` branch?

### 41. Rewrite this using a ternary:

```ts
let message;

if (age >= 18) {
  message = "Adult";
} else {
  message = "Minor";
}
```

### 42. What happens if you remove `break`?

```ts
const x = 1;

switch (x) {
  case 1:
    console.log("One");

  case 2:
    console.log("Two");

  default:
    console.log("Other");
}
```

# Loops

### 43. What is the output?

```ts
for (let i = 0; i < 5; i++) {
  console.log(i);
}
```

Explain the execution order.

### 44. What is the output?

```ts
let i = 0;

while (i < 3) {
  console.log(i);
  i++;
}
```

### 45. What is the output?

```ts
let i = 0;

do {
  console.log(i);
  i++;
} while (i < 0);
```

Why does it execute once?

### 46. Predict:

```ts
for (let i = 0; i < 5; i++) {
  if (i === 2) continue;

  console.log(i);
}
```

### 47. Predict:

```ts
for (let i = 0; i < 5; i++) {
  if (i === 2) break;

  console.log(i);
}
```

### 48. What's the difference?

```ts
for (const x of [10, 20, 30]) {
  console.log(x);
}
```

vs.

```ts
for (const x in [10, 20, 30]) {
  console.log(x);
}
```

# 🔴 Level 3 — Hard

**Goal: combine multiple concepts and reason about execution.**

## Operators + Conditions

### 49. Predict the exact output:

```ts
const a = 10;
const b = 0;

if (a && b) {
  console.log("A");
}

if (a || b) {
  console.log("B");
}
```

Why?

### 50. What does this produce?

```ts
const value = null;

const result =
  value ?? "default";

console.log(result);
```

Now change `value` to:

```ts
0
```

What changes?

### 51. Predict:

```ts
const user = {
  name: "",
  age: 0
};

console.log(user.name || "Unknown");
console.log(user.name ?? "Unknown");

console.log(user.age || 18);
console.log(user.age ?? 18);
```

Explain every result.

### 52. What does this expression evaluate to?

```ts
true && false || true
```

Then explain **operator precedence**.

### 53. Predict:

```ts
console.log(1 + 2 * 3);
console.log((1 + 2) * 3);
```

Why are the results different?

# Loops + Operators

### 54. What is the output?

```ts
for (let i = 0; i < 5; i++) {
  if (i % 2 === 0) {
    console.log(i);
  }
}
```

Explain how `%` and `if` work together.

### 55. What does this print?

```ts
for (let i = 1; i <= 10; i++) {
  if (i % 3 === 0) {
    continue;
  }

  console.log(i);
}
```

### 56. What happens here?

```ts
let i = 0;

while (i < 5) {
  console.log(i);
}
```

Why is this dangerous?

How would you fix it?

# Nested Loops

### 57. Predict the output:

```ts
for (let i = 1; i <= 2; i++) {
  for (let j = 1; j <= 3; j++) {
    console.log(i, j);
  }
}
```

### 58. How many times does `"Hello"` print?

```ts
for (let i = 0; i < 5; i++) {
  for (let j = 0; j < 4; j++) {
    console.log("Hello");
  }
}
```

Don't run it. Calculate it.

### 59. Write nested loops that produce:

```text
*
**
***
****
*****
```

# 🔥 Deep Reasoning Questions

### 60. Why does this work?

```ts
const name = user?.profile?.name;
```

What problem does optional chaining solve?

### 61. Explain the difference between:

```ts
if (value) {}
```

and:

```ts
if (value !== null && value !== undefined) {}
```

Are they always equivalent?

### 62. Why does this behave differently?

```ts
const x = 0;

console.log(x || 100);
console.log(x ?? 100);
```

### 63. Can you explain exactly why this prints `1`?

```ts
console.log(
  false && 1 || 2
);
```

### 64. What is the output?

```ts
let x = 5;

if (x++ > 5) {
  console.log("A");
} else {
  console.log("B");
}

console.log(x);
```

The trick is combining **postfix increment + comparison + condition**.

### 65. Predict the output:

```ts
for (let i = 0; i < 3; i++) {
  console.log("outer");

  for (let j = 0; j < 2; j++) {
    console.log("inner");
  }
}
```

Then calculate:

**How many total lines are printed?**

# 🏆 Challenge Questions

These are the ones I'd use to check whether you've **actually mastered the concepts**.

### 66. Write FizzBuzz

Print numbers `1–100`.

```text
3     → Fizz
5     → Buzz
15    → FizzBuzz
others → number
```

Rules:

* Use a loop.
* Use conditions.
* Use `%`.
* Don't use an array.

### 67. Find the largest number

Without using `Math.max()`:

```ts
const numbers = [10, 4, 25, 7, 18];
```

Find the largest value using a loop.

### 68. Count even and odd numbers

```ts
const numbers = [1, 4, 7, 8, 10, 13, 20];
```

Produce:

```text
Even: 4
Odd: 3
```

### 69. Reverse a number

```text
12345 → 54321
```

Use operators and a loop.

### 70. Find duplicates

Given:

```ts
const numbers = [1, 2, 3, 2, 4, 1, 5];
```

Find the duplicate values.

### 71. Build a number pyramid

Given `5`, produce:

```text
1
12
123
1234
12345
```

Use nested loops.

### 72. Build FizzBuzz without `if`

This forces you to think differently.

Use:

* logical operators
* ternary
* `%`

# 🎯 Mastery Path

Don't just answer them sequentially. Use this approach:

```text
LEVEL 1
   ↓
"I know what this operator/statement means."
   
LEVEL 2
   ↓
"I can predict what the code does."

LEVEL 3
   ↓
"I can explain WHY JavaScript does it."

CHALLENGE
   ↓
"I can use the concepts to solve a new problem."
```

The **most important Level 3 topics** to master are:

**`===` vs `==` → truthy/falsy → `&&`/`||` short-circuiting → `??` → `?.` → prefix/postfix operators → loop execution order → `for...of` vs `for...in` → `break`/`continue` → nested loops.**

Once you can predict those without running the code, your understanding of JavaScript control flow is becoming genuinely strong.
