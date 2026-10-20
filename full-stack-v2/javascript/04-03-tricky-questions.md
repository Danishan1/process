Exactly. At this point, **recognizing the rules isn't enough**. We should test whether you can predict behavior from the mechanism.

Let's do it as an **execution-trace exercise**.

## Round 1 — Predict the output

For each snippet, **don't run it**. Trace the bindings first.

### Question 1

```js
console.log(a);

var a = 10;
```

What happens?

---

### Question 2

```js
console.log(a);

let a = 10;
```

What happens?

---

### Question 3

```js
var a = 10;

function test() {
    console.log(a);
    var a = 20;
}

test();
```

What happens?

---

### Question 4

```js
let a = 10;

function test() {
    console.log(a);
    let a = 20;
}

test();
```

What happens?

---

### Question 5

```js
var a = 10;

{
    var a = 20;
}

console.log(a);
```

What happens?

---

### Question 6

```js
let a = 10;

{
    let a = 20;
}

console.log(a);
```

What happens?

---

### Question 7

```js
const a = 10;

a = 20;

console.log(a);
```

What happens?

---

### Question 8

```js
const user = {
    name: "A"
};

user.name = "B";

console.log(user.name);
```

What happens?

---

## Round 2 — The important one

Now combine **scope + TDZ + shadowing**:

### Question 9

```js
let x = 10;

{
    console.log(x);

    let x = 20;
}
```

Does it print `10`, `20`, `undefined`, or throw an error?

And **why doesn't JavaScript simply go to the outer `x`?**

---

## Round 3 — `var` vs `let` in loops

### Question 10

```js
for (var i = 0; i < 3; i++) {
    setTimeout(() => {
        console.log(i);
    }, 0);
}
```

Predict the output and explain **which binding each callback captures**.

---

### Question 11

```js
for (let i = 0; i < 3; i++) {
    setTimeout(() => {
        console.log(i);
    }, 0);
}
```

Predict the output.

More importantly:

> **How many conceptual `i` bindings are captured by the callbacks?**

---

## Round 4 — SDE-2 level

### Question 12

What does this print?

```js
var x = 1;

function test() {
    console.log(x);

    if (true) {
        var x = 2;
    }

    console.log(x);
}

test();
```

Trace the binding **before executing the first `console.log`**.

---

### Question 13

What does this print?

```js
let x = 1;

function test() {
    console.log(x);

    if (true) {
        let x = 2;
        console.log(x);
    }

    console.log(x);
}

test();
```

Identify exactly **which `x` each `console.log` resolves to**.

---

### Question 14 — `typeof` trap

```js
var x = 10;

function test() {
    console.log(typeof x);

    let x = 20;
}

test();
```

Does it print:

```text
number
```

or throw:

```text
ReferenceError
```

Explain using **name resolution + TDZ**, not "`let` is not hoisted."

---

### Question 15 — Redeclaration

What happens?

```js
var x = 10;
var x = 20;

console.log(x);
```

And compare with:

```js
let x = 10;
let x = 20;

console.log(x);
```

Why is the language behavior different?

---

## The rule I want you to use

For **every question**, trace it in this order:

```text
1. What scope am I in?
        ↓
2. What bindings are established?
        ↓
3. How is each binding initialized?
        ↓
4. Is the binding currently in TDZ?
        ↓
5. Which binding does name resolution find?
        ↓
6. What operation is performed?
```

Don't start with:

> "`var` is hoisted."

Start with:

> **"What binding exists here, where does it exist, and what is its current state?"**

That's the level at which `var`, `let`, `const`, TDZ, shadowing, and closures stop being separate rules and become **one coherent model**.
