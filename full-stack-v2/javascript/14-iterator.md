
# 12. Iterators, Iterables & Generators

Before learning the individual APIs, we need one mental model:

```text
                 ITERATION
                     │
          ┌──────────┴──────────┐
          │                     │
       Iterable              Iterator
          │                     │
   "Can I start              "Give me the
    iterating?"               next value"
          │                     │
 Symbol.iterator()            next()
          │                     │
          └──────────┬──────────┘
                     │
                  for...of
                     │
              values one by one
```

The **most important distinction** is:

> **Iterable = something that can provide an iterator.**
> **Iterator = object that produces values one at a time.**

And then:

> **Generator = an easy way to create an iterator.**

---

# 1. Why do we even need Iterators?

Suppose we have:

```js
const arr = [10, 20, 30];
```

We can obviously do:

```js
for (const value of arr) {
    console.log(value);
}
```

But `for...of` needs some standardized way to answer:

> "How do I get the next value from this particular object?"

For an array:

```text
Array
  ↓
Iterator
  ↓
10
  ↓
20
  ↓
30
  ↓
done
```

JavaScript solves this through the **iteration protocols**.

---

# 2. Iterable Protocol

An object is **iterable** if it provides:

```js
obj[Symbol.iterator]()
```

and that method returns an **iterator**.

For example:

```js
const arr = [10, 20, 30];

const iterator = arr[Symbol.iterator]();

console.log(iterator);
```

Conceptually:

```text
array
  │
  │ Symbol.iterator()
  ↓
iterator
```

So:

```js
arr[Symbol.iterator]
```

is a function that knows how to create an iterator for the array.

### Interview definition

An **iterable** is an object that implements the iterable protocol by having a `[Symbol.iterator]()` method that returns an iterator.

---

# 3. Iterator Protocol

Now we have the iterator.

The iterator must provide:

```js
next()
```

Let's see:

```js
const arr = [10, 20, 30];

const iterator = arr[Symbol.iterator]();

console.log(iterator.next());
console.log(iterator.next());
console.log(iterator.next());
console.log(iterator.next());
```

Result:

```js
{ value: 10, done: false }
{ value: 20, done: false }
{ value: 30, done: false }
{ value: undefined, done: true }
```

This is the **Iterator Protocol**.

Every call to:

```js
iterator.next()
```

asks:

> "Give me the next value."

---

# 4. `next()` Return Object

`next()` returns an object containing:

```js
{
    value: ...,
    done: ...
}
```

For example:

```js
{
    value: 10,
    done: false
}
```

means:

```text
value → 10
done  → false
```

And:

```js
{
    value: undefined,
    done: true
}
```

means:

```text
iteration is finished
```

So we can manually reproduce `for...of`:

```js
const arr = [10, 20, 30];

const iterator = arr[Symbol.iterator]();

let result = iterator.next();

while (!result.done) {
    console.log(result.value);
    result = iterator.next();
}
```

Output:

```text
10
20
30
```

This is basically what `for...of` is abstracting away from us.

---

# 5. `for...of`

Now this should make sense:

```js
for (const value of arr) {
    console.log(value);
}
```

Conceptually:

```text
arr
 ↓
arr[Symbol.iterator]()
 ↓
iterator
 ↓
next()
 ↓
value
 ↓
next()
 ↓
value
 ↓
...
```

So `for...of` works with **iterables**, not simply "objects that have values."

---

# 6. Which Built-ins are Iterable?

Common examples:

```js
Array
String
Map
Set
TypedArray
```

For example:

### Array

```js
for (const x of [1, 2, 3]) {
    console.log(x);
}
```

### String

```js
for (const char of "hello") {
    console.log(char);
}
```

### Set

```js
for (const value of new Set([1, 2, 3])) {
    console.log(value);
}
```

### Map

```js
for (const entry of new Map([
    ["a", 1],
    ["b", 2]
])) {
    console.log(entry);
}
```

All of these provide:

```js
obj[Symbol.iterator]
```

---

# 7. `Symbol.iterator`

This is the key mechanism behind the whole topic.

For example:

```js
const arr = [1, 2, 3];

console.log(typeof arr[Symbol.iterator]);
```

Output:

```text
function
```

And:

```js
const iterator = arr[Symbol.iterator]();

console.log(typeof iterator.next);
```

Output:

```text
function
```

So:

```text
Iterable
   │
   └── [Symbol.iterator]()
             │
             ↓
          Iterator
             │
             └── next()
```

---

# 8. Iterable vs Iterator

This distinction is **very important for interviews**.

Consider:

```js
const arr = [1, 2, 3];

const iterator = arr[Symbol.iterator]();
```

`arr` is an **iterable**.

`iterator` is an **iterator**.

Check:

```js
console.log(typeof arr[Symbol.iterator]);
// function

console.log(typeof iterator.next);
// function
```

The iterable provides:

```js
Symbol.iterator
```

The iterator provides:

```js
next
```

A useful mental model:

```text
Iterable
"I know how to create an iterator."

Iterator
"I know how to give you the next item."
```

---

# 9. Interesting Detail: An Iterator Can Also Be Iterable

This is an important interview-level detail.

Many iterators implement:

```js
[Symbol.iterator]()
```

that returns themselves.

For example:

```js
const iterator = [1, 2, 3][Symbol.iterator]();

console.log(iterator[Symbol.iterator]() === iterator);
```

Typically:

```text
true
```

Why?

Because an iterator is already the object that represents the current iteration state.

---

# 10. Custom Iterable

Now we can create our own iterable.

Suppose we want:

```js
const numbers = ...
```

to produce:

```text
1
2
3
4
5
```

We can implement:

```js
const numbers = {
    [Symbol.iterator]() {
        let current = 1;

        return {
            next() {
                if (current <= 5) {
                    return {
                        value: current++,
                        done: false
                    };
                }

                return {
                    value: undefined,
                    done: true
                };
            }
        };
    }
};
```

Now:

```js
for (const number of numbers) {
    console.log(number);
}
```

Output:

```text
1
2
3
4
5
```

This is a **custom iterable**.

---

# 11. Understand What Actually Happened

When we write:

```js
for (const number of numbers) {
    console.log(number);
}
```

JavaScript effectively needs to do:

```js
const iterator = numbers[Symbol.iterator]();

let result = iterator.next();

while (!result.done) {
    const number = result.value;

    console.log(number);

    result = iterator.next();
}
```

So there are three layers:

```text
for...of
   ↓
Symbol.iterator
   ↓
next()
```

Remember this chain.

---

# 12. Generator

Now we reach the really useful part.

The custom iterator above is somewhat verbose.

JavaScript gives us **generators**.

```js
function* numbers() {
    yield 1;
    yield 2;
    yield 3;
}
```

Notice:

```js
function*
```

instead of:

```js
function
```

This is a **generator function**.

Call it:

```js
const iterator = numbers();
```

It doesn't immediately execute the entire function.

Instead, it gives us a generator object.

```js
console.log(iterator.next());
```

Result:

```js
{
    value: 1,
    done: false
}
```

Then:

```js
console.log(iterator.next());
```

```js
{
    value: 2,
    done: false
}
```

Then:

```js
console.log(iterator.next());
```

```js
{
    value: 3,
    done: false
}
```

Finally:

```js
console.log(iterator.next());
```

```js
{
    value: undefined,
    done: true
}
```

---

# 13. `yield`

This is the most important generator keyword.

Consider:

```js
function* numbers() {
    console.log("A");

    yield 1;

    console.log("B");

    yield 2;

    console.log("C");
}
```

Now:

```js
const iterator = numbers();
```

**Nothing is printed yet.**

Then:

```js
iterator.next();
```

Output:

```text
A
```

and:

```js
{ value: 1, done: false }
```

Then:

```js
iterator.next();
```

Output:

```text
B
```

and:

```js
{ value: 2, done: false }
```

Then:

```js
iterator.next();
```

Output:

```text
C
```

and:

```js
{ value: undefined, done: true }
```

This gives us the core generator mental model:

> **`yield` pauses generator execution and `next()` resumes it.**

---

# 14. Generator State

This is important.

Consider:

```js
function* test() {
    console.log("A");

    yield 10;

    console.log("B");

    yield 20;

    console.log("C");
}
```

The generator maintains its execution state.

Think:

```text
START
  │
  ↓
console.log("A")
  │
  ↓
yield 10
  │
  ⏸ PAUSED
  │
  │ next()
  ↓
console.log("B")
  │
  ↓
yield 20
  │
  ⏸ PAUSED
  │
  │ next()
  ↓
console.log("C")
  │
  ↓
DONE
```

This is why generators are fundamentally different from normal functions.

A normal function:

```text
call
 ↓
execute
 ↓
return
 ↓
finished
```

Generator:

```text
call
 ↓
generator object
 ↓
next()
 ↓
execute
 ↓
yield
 ↓
pause
 ↓
next()
 ↓
resume
 ↓
yield
 ↓
pause
```

---

# 15. Generators Are Iterators

This is another key relationship.

```js
function* numbers() {
    yield 1;
    yield 2;
}
```

Then:

```js
const generator = numbers();
```

The generator object has:

```js
generator.next()
```

and is also iterable.

Therefore:

```js
for (const value of generator) {
    console.log(value);
}
```

works.

So:

```text
Generator function
       ↓
Generator object
       ↓
   Iterator
       ↓
   Iterable
```

That's why generators are such a convenient abstraction.

---

# 16. Lazy Evaluation

This is one of the most important reasons generators exist.

Consider:

```js
function* numbers() {
    yield 1;
    yield 2;
    yield 3;
}
```

The values aren't generated all at once.

They're generated **when requested**.

```js
const generator = numbers();

generator.next(); // generates 1
generator.next(); // generates 2
generator.next(); // generates 3
```

Compare this with:

```js
const numbers = [1, 2, 3];
```

The array already contains all values.

Generator:

```text
request → produce one value
request → produce one value
request → produce one value
```

This is **lazy evaluation**.

---

# 17. Why Lazy Evaluation Matters

Imagine:

```text
1,000,000,000 numbers
```

Creating:

```js
const numbers = [];
```

and storing all of them could consume huge memory.

A generator could instead produce them one at a time:

```js
function* numbers() {
    let i = 0;

    while (true) {
        yield i++;
    }
}
```

Notice something interesting:

```js
while (true)
```

This does **not** immediately create an infinite loop.

Because:

```js
yield
```

pauses execution.

We can do:

```js
const nums = numbers();

console.log(nums.next().value);
console.log(nums.next().value);
console.log(nums.next().value);
```

Output:

```text
0
1
2
```

The generator hasn't tried to generate infinity upfront.

That's the power of laziness.

---

# 18. Generator Delegation

Your syllabus also includes **generator delegation**. 

This is done using:

```js
yield*
```

Example:

```js
function* first() {
    yield 1;
    yield 2;
}

function* second() {
    yield* first();

    yield 3;
    yield 4;
}
```

Now:

```js
for (const value of second()) {
    console.log(value);
}
```

Output:

```text
1
2
3
4
```

Conceptually:

```text
second()
   │
   ├── yield* first()
   │       │
   │       ├── 1
   │       └── 2
   │
   ├── 3
   └── 4
```

`yield*` means:

> "Delegate iteration to another iterable."

It isn't limited to another generator.

For example:

```js
function* numbers() {
    yield* [1, 2, 3];

    yield 4;
}
```

---

# 19. Iteration vs Enumeration

This distinction is subtle but important.

### Enumeration

Enumeration is essentially:

> "Which properties/keys does this object have?"

Example:

```js
const user = {
    name: "Danish",
    age: 25
};

for (const key in user) {
    console.log(key);
}
```

This deals with **property keys**.

### Iteration

Iteration is:

> "Which values does this object provide one at a time?"

Example:

```js
const arr = [10, 20, 30];

for (const value of arr) {
    console.log(value);
}
```

This uses the **iterable protocol**.

So:

```text
for...in
    ↓
enumeration
    ↓
property keys
```

while:

```text
for...of
    ↓
iteration
    ↓
values
```

This is a very common interview distinction.

---

# 20. `for...in` vs `for...of`

You should be completely comfortable with this:

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

Because `for...in` gives property keys.

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

Because `for...of` performs iteration.

### Mental shortcut

```text
in  → keys
of  → values
```

Not universally "keys vs values" in every imaginable object scenario, but that's the right practical mental model here.

---

# 21. Iterable vs Array-like

Another common interview trap.

An **array-like** object generally has:

```js
{
    0: "a",
    1: "b",
    2: "c",
    length: 3
}
```

For example:

```js
const obj = {
    0: "a",
    1: "b",
    2: "c",
    length: 3
};
```

It looks like an array.

But:

```js
console.log(obj[Symbol.iterator]);
```

may be:

```text
undefined
```

Therefore it is **array-like but not iterable**.

---

# 22. Array-like ≠ Iterable

This distinction is extremely important.

```text
Array-like
    │
    ├── numeric indexes
    └── length
```

Iterable:

```text
Iterable
    │
    └── Symbol.iterator()
```

They are completely different concepts.

An object can be:

```text
array-like + iterable
```

or:

```text
array-like + NOT iterable
```

or:

```text
iterable + NOT array-like
```

For example, a `Set` is iterable but doesn't behave like an array with numeric indexes.

---

# 23. Why This Matters in Real Code

You may encounter:

```js
const values = {
    0: "a",
    1: "b",
    length: 2
};
```

Then:

```js
for (const value of values) {
}
```

throws:

```text
TypeError: values is not iterable
```

Even though:

```js
values[0]
values[1]
values.length
```

exist.

To make something iterable, it needs an appropriate:

```js
Symbol.iterator
```

implementation.

---

# 24. The Complete Mental Model

At this point, I want you to remember this diagram:

```text
                         ITERATION
                             │
             ┌───────────────┴───────────────┐
             │                               │
         ITERABLE                         ITERATOR
             │                               │
      Symbol.iterator()                     next()
             │                               │
             └───────────────┬───────────────┘
                             │
                         for...of
                             │
                         values
```

And:

```text
GENERATOR
    │
    ├── produces iterator
    ├── is iterable
    ├── has next()
    ├── uses yield
    └── supports lazy evaluation
```

---

# 25. SDE-2 Interview Questions You Should Be Able to Answer

For this topic, I would expect you to comfortably answer these:

### Fundamental

1. What is an iterable?
2. What is an iterator?
3. Difference between iterable and iterator?
4. What is `Symbol.iterator`?
5. What does `next()` return?
6. How does `for...of` work internally?
7. Difference between `for...in` and `for...of`?

### Generators

8. What is a generator function?
9. What does `yield` do?
10. Does calling a generator function execute it immediately?
11. How does generator state work?
12. What happens when `next()` is called after completion?
13. What is `yield*`?
14. What is generator delegation?

### Deeper

15. What is lazy evaluation?
16. Why are generators useful for large/infinite sequences?
17. Can an iterator also be iterable?
18. What's the difference between iterable and array-like?
19. Can you create a custom iterable?
20. Why would you use a generator instead of manually implementing an iterator?

---

## The one distinction I want you to master first

Don't memorize all of this yet. Make sure this is crystal clear:

```js
const arr = [10, 20, 30];

const iterator = arr[Symbol.iterator]();
```

Here:

```text
arr
│
├── Iterable
│
└── [Symbol.iterator]()
          │
          ↓
      iterator
          │
          └── next()
```

Then:

```js
iterator.next();
```

gives:

```js
{ value: 10, done: false }
```

And:

```js
for (const value of arr)
```

is essentially consuming that iteration protocol for you.
