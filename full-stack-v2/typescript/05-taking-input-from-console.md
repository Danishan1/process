## JavaScript Console Input — Study Notes

### 1. Basic setup

Use Node.js's `readline/promises` to take input from the console.

```ts
import { createInterface } from 'readline/promises';
import { stdin, stdout } from 'process';

const rl = createInterface({
  input: stdin,
  output: stdout
});
```

> `rl.question()` **always returns a string**.

### 2. Take a String

```ts
const name = await rl.question('Enter your name: ');

console.log(name);
```

Example:

```text
Enter your name: Rahul
Rahul
```

### 3. Take an Integer

Since input comes as a string, convert it using `Number()` or `parseInt()`.

```ts
const age = Number(await rl.question('Enter your age: '));

console.log(age);
```

Or:

```ts
const age = parseInt(await rl.question('Enter your age: '), 10);

console.log(age);
```

For most cases, **`Number()` is simpler**.

### 4. Take a Decimal / Float

JavaScript doesn't have a separate `float` type. All normal numbers are `number`.

```ts
const price = Number(await rl.question('Enter price: '));

console.log(price);
```

Example:

```text
Enter price: 99.99
99.99
```

### 5. Take a Boolean

You need to convert the entered string yourself.

```ts
const answer = await rl.question('Are you a student? (true/false): ');

const isStudent = answer.toLowerCase() === 'true';

console.log(isStudent);
```

Example:

```text
Are you a student? (true/false): true
true
```

### 6. Take Multiple Values

Suppose the user enters:

```text
10 20 30
```

You can split the input into an array.

```ts
const input = await rl.question('Enter numbers: ');

const numbers = input.split(' ').map(Number);

console.log(numbers);
```

Output:

```text
[ 10, 20, 30 ]
```

A slightly safer version handles multiple spaces:

```ts
const numbers = input.trim().split(/\s+/).map(Number);
```

### 7. Take Multiple Strings

Input:

```text
apple banana mango
```

Code:

```ts
const input = await rl.question('Enter fruits: ');

const fruits = input.trim().split(/\s+/);

console.log(fruits);
```

Output:

```text
[ 'apple', 'banana', 'mango' ]
```

### 8. Take an Array of Numbers

This is a very common pattern in coding problems:

```ts
const numbers = (await rl.question('Enter numbers: '))
  .trim()
  .split(/\s+/)
  .map(Number);

console.log(numbers);
```

For:

```text
10 20 30 40 50
```

You get:

```ts
[10, 20, 30, 40, 50]
```

### 9. Take a Single Character

JavaScript doesn't have a separate `char` type.

Take a string and access the first character:

```ts
const ch = (await rl.question('Enter a character: '))[0];

console.log(ch);
```

Input:

```text
A
```

Output:

```text
A
```

## Important Concept

The most important thing to remember is:

```ts
const value = await rl.question('Enter something: ');
```

**`value` is always a `string`.**

So:

```ts
const age = await rl.question('Age: ');
```

gives:

```ts
"25"
```

not:

```ts
25
```

You convert it when necessary:

```ts
Number(value)
```

## Quick Cheat Sheet

| What you want    | Code                                    |
| ---------------- | --------------------------------------- |
| String           | `await rl.question()`                   |
| Number           | `Number(await rl.question())`           |
| Integer          | `parseInt(await rl.question(), 10)`     |
| Decimal          | `Number(await rl.question())`           |
| Boolean          | `answer.toLowerCase() === 'true'`       |
| Character        | `(await rl.question())[0]`              |
| Array of strings | `input.trim().split(/\s+/)`             |
| Array of numbers | `input.trim().split(/\s+/).map(Number)` |

## Complete Example

```ts
import { createInterface } from 'readline/promises';
import { stdin, stdout } from 'process';

const rl = createInterface({
  input: stdin,
  output: stdout
});

const name = await rl.question('Enter your name: ');
const age = Number(await rl.question('Enter your age: '));
const salary = Number(await rl.question('Enter your salary: '));

const isStudent =
  (await rl.question('Are you a student? (true/false): '))
    .toLowerCase() === 'true';

const marks = (await rl.question('Enter your marks: '))
  .trim()
  .split(/\s+/)
  .map(Number);

console.log('\n--- Output ---');

console.log('Name:', name);
console.log('Age:', age);
console.log('Salary:', salary);
console.log('Student:', isStudent);
console.log('Marks:', marks);

rl.close();
```

### One rule to memorize

> **Console input → always String → convert to the type you need.**

```text
Console
   ↓
rl.question()
   ↓
String
   ↓
Number() / split() / custom conversion
   ↓
Your desired value
```

This pattern is especially useful for **DSA/coding-problem practice in TypeScript**, where you'll frequently read a line and convert it into numbers or arrays.

## Third Party Libraries

### 1. `prompt-sync` — probably the simplest

Install:

```bash
npm install prompt-sync
```

Then:

```ts
import promptSync from 'prompt-sync';

const prompt = promptSync();

const name = prompt('Enter your name: ');
const age = Number(prompt('Enter your age: '));

console.log(name);
console.log(age);
```

This feels much closer to Python's `input()`:

```python
name = input("Enter your name: ")
age = int(input("Enter your age: "))
```

You can also make arrays very clean:

```ts
const numbers = prompt('Enter numbers: ')
  .trim()
  .split(/\s+/)
  .map(Number);

console.log(numbers);
```

### 2. `readline-sync`

Another popular option:

```bash
npm install readline-sync
```

Usage:

```ts
import readlineSync from 'readline-sync';

const name = readlineSync.question('Enter your name: ');
const age = readlineSync.questionInt('Enter your age: ');

console.log(name);
console.log(age);
```

It has convenient methods like:

```ts
readlineSync.question()
readlineSync.questionInt()
readlineSync.questionFloat()
readlineSync.keyInYN()
```

### Which one I'd recommend

For **learning JavaScript/TypeScript + DSA**, I'd use **`prompt-sync`** if you want the simplest possible syntax:

```ts
import promptSync from 'prompt-sync';

const prompt = promptSync();

const name = prompt('Name: ');
const age = Number(prompt('Age: '));
const numbers = prompt('Numbers: ').split(' ').map(Number);

console.log(name);
console.log(age);
console.log(numbers);
```

It's much less boilerplate than:

```ts
const rl = createInterface({
  input: stdin,
  output: stdout
});

const value = await rl.question(...);
```

One caveat: `prompt-sync` is **synchronous**, so I would use it mainly for **learning, scripts, and DSA/competitive-programming exercises**. For production Node.js applications, the built-in asynchronous `readline/promises` approach is generally preferable.
