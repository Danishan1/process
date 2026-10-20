# 10. Arrays & Collection Operations

Arrays look simple, but for SDE-2 interviews they are worth studying much deeper than just memorizing `map`, `filter`, and `reduce`.

The important mental model is:

> **An Array is an object with special indexed-property and `length` behavior, plus a large set of collection-oriented methods.**

We should understand both **how arrays behave internally at the language level** and **how to manipulate them correctly in production code**.

---

# 10.1 What Is an Array?

An array is an ordered collection of values.

```js
const numbers = [10, 20, 30];
```

Conceptually:

```text
numbers
 ├── 0 → 10
 ├── 1 → 20
 └── 2 → 30
```

The indexes are property keys:

```js
numbers[0];
```

is accessing the property whose key is effectively:

```text
"0"
```

This is why arrays are technically objects:

```js
typeof [];
```

```text
"object"
```

But arrays have special behavior that ordinary objects don't have.

---

# 10.2 Array Creation

### Array literal

The most common approach:

```js
const numbers = [1, 2, 3];
```

### `Array` constructor

```js
const numbers = new Array(1, 2, 3);
```

creates:

```js
[1, 2, 3]
```

But:

```js
const numbers = new Array(3);
```

creates an array with length `3` but **without three actual elements**.

```js
console.log(numbers);
```

Conceptually:

```text
[ <3 empty items> ]
```

This distinction becomes important when discussing **sparse arrays**.

---

# 10.3 Indexing

Array indexes are zero-based:

```js
const users = ["A", "B", "C"];

users[0]; // "A"
users[1]; // "B"
users[2]; // "C"
```

An out-of-range access doesn't throw an error:

```js
users[10];
```

returns:

```text
undefined
```

That's because there is no property at index `10`.

---

# 10.4 Arrays Are Objects

This is a useful connection to our Objects chapter.

```js
const arr = [10, 20];

arr.name = "numbers";
```

Now:

```js
console.log(arr.name);
```

works.

Arrays can have ordinary properties because they are objects.

But:

```js
arr[2] = 30;
```

has special array semantics because `"2"` is an array index.

So conceptually:

```text
Array
 ├── indexed properties
 ├── length
 └── ordinary object properties
```

---

# 10.5 The `length` Property

Every array has a `length` property.

```js
const arr = ["a", "b", "c"];

arr.length; // 3
```

It represents one greater than the highest array index present under the array's index semantics.

For normal dense arrays this looks like:

```text
number of elements
```

but those concepts aren't always identical.

---

# 10.6 Changing `length`

You can increase the length:

```js
const arr = [1, 2];

arr.length = 5;
```

Now:

```text
[1, 2, empty, empty, empty]
```

You can also reduce it:

```js
arr.length = 1;
```

Now:

```text
[1]
```

Elements at indexes `1` and above are removed.

This is an important property of arrays:

> **Reducing `length` can delete array elements.**

---

# 10.7 Sparse Arrays

A sparse array contains missing indexes.

Example:

```js
const arr = [];

arr[0] = "A";
arr[3] = "D";
```

Conceptually:

```text
index
0 → "A"
1 → missing
2 → missing
3 → "D"
```

Its length is:

```js
arr.length; // 4
```

But it doesn't actually contain four elements.

This is the key distinction:

```text
length = 4

present elements = 2
```

---

# 10.8 Empty Slot vs `undefined`

This is a classic interview trap.

Compare:

```js
const a = [undefined];
const b = new Array(1);
```

Both have:

```js
a.length === 1
b.length === 1
```

But:

```text
a
index 0 → undefined

b
index 0 → missing
```

The property exists in `a`:

```js
0 in a; // true
```

but not in `b`:

```js
0 in b; // false
```

This distinction affects iteration methods.

---

# 10.9 Mutating vs Non-Mutating Methods

This distinction should become automatic.

### Mutating

These modify the original array:

```text
push
pop
shift
unshift
splice
sort
reverse
fill
copyWithin
```

### Non-mutating

These return a new result without modifying the original array:

```text
map
filter
slice
concat
flat
flatMap
find
findIndex
some
every
```

There are also newer non-mutating alternatives to some traditional mutating operations, such as:

```js
toSorted()
toReversed()
toSpliced()
```

Understanding mutation is much more important in production JavaScript than simply remembering method names.

---

# 10.10 `map`

`map()` transforms every present element and returns a new array.

```js
const numbers = [1, 2, 3];

const doubled = numbers.map(n => n * 2);
```

Result:

```js
[2, 4, 6]
```

Original:

```js
[1, 2, 3]
```

remains unchanged.

Mental model:

```text
input
 ↓
transform each element
 ↓
new array
```

---

# 10.11 `map` Must Return the New Value

Correct:

```js
const result = numbers.map(n => n * 2);
```

Potential mistake:

```js
const result = numbers.map(n => {
    n * 2;
});
```

Result:

```js
[undefined, undefined, undefined]
```

Because the callback doesn't explicitly return anything.

---

# 10.12 `filter`

`filter()` keeps elements for which the callback returns truthy.

```js
const numbers = [1, 2, 3, 4, 5];

const even = numbers.filter(n => n % 2 === 0);
```

Result:

```js
[2, 4]
```

Mental model:

```text
element
   ↓
predicate
   ↓
true  → keep
false → discard
```

---

# 10.13 `find`

`find()` returns the **first matching element**.

```js
const users = [
    { id: 1, name: "A" },
    { id: 2, name: "B" },
    { id: 3, name: "C" }
];

const user = users.find(user => user.id === 2);
```

Result:

```js
{ id: 2, name: "B" }
```

If nothing matches:

```js
undefined
```

Important distinction:

```text
find()
    ↓
returns element

filter()
    ↓
returns array
```

---

# 10.14 `findIndex`

`findIndex()` returns the index of the first matching element.

```js
const numbers = [10, 20, 30];

const index = numbers.findIndex(n => n === 20);
```

Result:

```text
1
```

If no element matches:

```text
-1
```

---

# 10.15 `some`

`some()` answers:

> Does at least one element satisfy the condition?

```js
const numbers = [1, 3, 4];

numbers.some(n => n % 2 === 0);
```

Result:

```text
true
```

It can short-circuit.

Once it finds a matching element, it stops.

Conceptually:

```text
1 → false
3 → false
4 → true → STOP
```

---

# 10.16 `every`

`every()` asks:

> Do all elements satisfy the condition?

```js
const numbers = [2, 4, 6];

numbers.every(n => n % 2 === 0);
```

Result:

```text
true
```

It also short-circuits.

```text
2 → true
4 → true
5 → false → STOP
```

---

# 10.17 `forEach`

`forEach()` executes a callback for each present element.

```js
const numbers = [1, 2, 3];

numbers.forEach(n => {
    console.log(n);
});
```

A crucial difference:

```text
map()
    → creates/returns transformed array

forEach()
    → intended for side effects
```

For example:

```js
numbers.forEach(n => {
    console.log(n);
});
```

---

# 10.18 `forEach` Doesn't Break Normally

You cannot do:

```js
numbers.forEach(n => {
    if (n === 2) {
        break;
    }
});
```

That isn't valid.

If you need early termination, consider:

```js
some()
every()
find()
findIndex()
for...of
```

Choosing the correct iteration construct is an important engineering skill.

---

# 10.19 `reduce`

`reduce()` is one of the most powerful array methods.

It combines array elements into a single accumulated result.

Example:

```js
const numbers = [1, 2, 3, 4];

const sum = numbers.reduce(
    (total, current) => total + current,
    0
);
```

Execution:

```text
initial accumulator = 0

0 + 1 → 1
1 + 2 → 3
3 + 3 → 6
6 + 4 → 10
```

Result:

```text
10
```

Mental model:

```text
array
 ↓
accumulator
 ↓
accumulator
 ↓
accumulator
 ↓
final result
```

---

# 10.20 `reduce` Is Not Just for Numbers

For example, counting occurrences:

```js
const words = ["js", "ts", "js", "react"];

const counts = words.reduce((acc, word) => {
    acc[word] = (acc[word] || 0) + 1;
    return acc;
}, {});
```

Result:

```js
{
    js: 2,
    ts: 1,
    react: 1
}
```

Another common use is grouping.

```js
const users = [
    { name: "A", role: "admin" },
    { name: "B", role: "user" },
    { name: "C", role: "admin" }
];
```

You can use `reduce()` to construct:

```js
{
    admin: [...],
    user: [...]
}
```

But an SDE-2 engineer should also recognize when `reduce()` is making code harder to understand. **Powerful doesn't automatically mean preferable.**

---

# 10.21 `sort`

`sort()` is particularly important because it **mutates the array**.

Also, by default it sorts elements as strings.

```js
const numbers = [10, 2, 5];

numbers.sort();
```

Result:

```text
[10, 2, 5]
```

Why?

Because default sorting compares string representations:

```text
"10"
"2"
"5"
```

Lexicographically:

```text
"10" < "2" < "5"
```

---

# 10.22 Numeric Sorting

Use a comparator:

```js
const numbers = [10, 2, 5];

numbers.sort((a, b) => a - b);
```

Result:

```text
[2, 5, 10]
```

Descending:

```js
numbers.sort((a, b) => b - a);
```

This is a very common interview question.

---

# 10.23 `sort()` Mutates

```js
const numbers = [3, 1, 2];

const sorted = numbers.sort((a, b) => a - b);
```

Now:

```js
sorted === numbers;
```

is:

```text
true
```

The same array was modified.

If you need a non-mutating sort:

```js
const sorted = numbers.toSorted((a, b) => a - b);
```

or, in older environments:

```js
const sorted = [...numbers].sort((a, b) => a - b);
```

---

# 10.24 `slice`

`slice()` extracts a portion of an array without modifying the original.

```js
const numbers = [0, 1, 2, 3, 4];

const result = numbers.slice(1, 4);
```

Result:

```text
[1, 2, 3]
```

The end index is exclusive.

```text
start → included
end   → excluded
```

So:

```js
slice(1, 4)
```

means:

```text
1, 2, 3
```

---

# 10.25 `slice` as a Shallow Copy

Calling:

```js
const copy = numbers.slice();
```

creates a new array.

But it is a **shallow copy**.

```js
const users = [
    { name: "A" }
];

const copy = users.slice();

copy[0].name = "B";
```

Now:

```js
users[0].name;
```

is:

```text
"B"
```

because both arrays reference the same object.

```text
users ──────┐
            ↓
          Object
            ↑
copy ───────┘
```

---

# 10.26 `splice`

`splice()` is a mutation operation.

It can:

* remove elements
* insert elements
* replace elements

Example:

```js
const numbers = [1, 2, 3, 4];

numbers.splice(1, 2);
```

Starting from index `1`, remove `2` elements:

```text
[1, 4]
```

---

# 10.27 `splice` for Insertion

```js
const numbers = [1, 4];

numbers.splice(1, 0, 2, 3);
```

Result:

```text
[1, 2, 3, 4]
```

The arguments are conceptually:

```text
splice(start, deleteCount, ...items)
```

---

# 10.28 `slice` vs `splice`

This should be automatic:

| Method     | Mutates? | Purpose               |
| ---------- | -------- | --------------------- |
| `slice()`  | No       | Extract/copy          |
| `splice()` | Yes      | Insert/remove/replace |

A common interview question:

> What is the difference between `slice` and `splice`?

Answer:

> `slice()` creates a shallow copy of a portion of an array without changing the original, while `splice()` modifies the original array by removing, inserting, or replacing elements.

---

# 10.29 `concat`

`concat()` creates a new array.

```js
const a = [1, 2];
const b = [3, 4];

const result = a.concat(b);
```

Result:

```text
[1, 2, 3, 4]
```

Original arrays remain unchanged.

It can also concatenate values:

```js
a.concat(3, 4);
```

---

# 10.30 Spread Syntax

Spread provides another way to construct arrays:

```js
const a = [1, 2];
const b = [3, 4];

const result = [...a, ...b];
```

Result:

```text
[1, 2, 3, 4]
```

It is commonly used for non-mutating updates:

```js
const newArray = [...oldArray, newItem];
```

or:

```js
const newArray = oldArray.filter(...);
```

---

# 10.31 Spread Is a Shallow Copy

This is extremely important.

```js
const original = [
    {
        name: "Danishan"
    }
];

const copy = [...original];
```

The arrays are different:

```js
copy !== original;
```

But:

```js
copy[0] === original[0];
```

is:

```text
true
```

So:

```text
spread
   ↓
new outer array
   ↓
same nested object references
```

---

# 10.32 `flat`

`flat()` removes nesting up to a specified depth.

```js
const numbers = [1, [2, 3], [4, [5]]];

numbers.flat();
```

Result:

```text
[1, 2, 3, 4, [5]]
```

Default depth is:

```text
1
```

You can specify:

```js
numbers.flat(2);
```

Result:

```text
[1, 2, 3, 4, 5]
```

You can use:

```js
Infinity
```

for arbitrary nesting:

```js
numbers.flat(Infinity);
```

---

# 10.33 `flatMap`

`flatMap()` is essentially useful when you want to:

```text
map
+
flatten one level
```

Example:

```js
const numbers = [1, 2, 3];

const result = numbers.flatMap(n => [n, n * 2]);
```

Result:

```text
[1, 2, 2, 4, 3, 6]
```

Conceptually:

```text
map:
[
  [1, 2],
  [2, 4],
  [3, 6]
]

flatten one level:
[
  1, 2, 2, 4, 3, 6
]
```

---

# 10.34 Array Destructuring

Destructuring extracts values by position.

```js
const numbers = [10, 20, 30];

const [a, b, c] = numbers;
```

Now:

```text
a → 10
b → 20
c → 30
```

You can skip elements:

```js
const [first, , third] = numbers;
```

Result:

```text
first → 10
third → 30
```

---

# 10.35 Rest in Array Destructuring

```js
const numbers = [1, 2, 3, 4];

const [first, ...rest] = numbers;
```

Result:

```text
first → 1
rest  → [2, 3, 4]
```

Notice the distinction:

```text
rest
```

creates a new array containing the remaining elements.

---

# 10.36 Array-Like Objects

An array-like object has indexed properties and a `length`, but isn't necessarily an actual Array.

Example:

```js
const arrayLike = {
    0: "A",
    1: "B",
    length: 2
};
```

It resembles:

```text
["A", "B"]
```

but:

```js
Array.isArray(arrayLike);
```

returns:

```text
false
```

This distinction matters with browser APIs and older JavaScript APIs.

---

# 10.37 `arguments` Is Array-Like

Inside a traditional function:

```js
function test(a, b) {
    console.log(arguments);
}
```

`arguments` has indexed values and a `length`, but it isn't an Array.

So:

```js
Array.isArray(arguments);
```

returns:

```text
false
```

You can convert it:

```js
const args = Array.from(arguments);
```

---

# 10.38 Converting to an Array

### `Array.from`

```js
const arrayLike = {
    0: "A",
    1: "B",
    length: 2
};

const arr = Array.from(arrayLike);
```

Result:

```text
["A", "B"]
```

It also works with iterables such as Sets.

```js
const set = new Set([1, 2, 3]);

const arr = Array.from(set);
```

---

# 10.39 Spread Can Convert Iterables

For an iterable:

```js
const set = new Set([1, 2, 3]);

const arr = [...set];
```

Result:

```text
[1, 2, 3]
```

Important distinction:

> Spread works with **iterables**, while `Array.from()` can work with **iterables and array-like objects**.

That's a very useful SDE-2-level distinction.

---

# 10.40 Arrays and Iteration Protocol

Arrays are iterable.

Therefore:

```js
for (const value of [10, 20, 30]) {
    console.log(value);
}
```

works.

And:

```js
const arr = [...anotherIterable];
```

can consume an iterable.

This connects arrays to the broader JavaScript **iteration protocol**, which becomes especially important when studying:

```text
Iterator
Iterable
Generator
for...of
```

later.

---

# 10.41 `for...in` vs `for...of`

For arrays, prefer:

```js
for (const value of arr) {
}
```

when you want values.

`for...in` iterates enumerable property keys.

```js
for (const key in arr) {
    console.log(key);
}
```

gives keys such as:

```text
"0"
"1"
"2"
```

while:

```js
for (const value of arr) {
}
```

gives:

```text
10
20
30
```

For arrays:

> **`for...of` → values**
> **`for...in` → enumerable keys**

---

# 10.42 Sparse Arrays and Iteration Methods

This is a good SDE-2 trap.

Consider:

```js
const arr = new Array(3);

arr[1] = "A";
```

There are:

```text
index 0 → missing
index 1 → "A"
index 2 → missing
```

Many array iteration methods skip missing elements.

For example:

```js
arr.forEach(value => {
    console.log(value);
});
```

will process the existing element rather than treating missing slots as explicit `undefined` values.

But different operations can treat holes differently, so don't mentally equate:

```text
hole
```

with:

```text
undefined
```

---

# 10.43 A Useful Classification

You should be able to classify the common methods quickly:

```text
TRANSFORMATION
    map
    flatMap

SELECTION
    filter
    find
    findIndex

PREDICATES
    some
    every

ACCUMULATION
    reduce

ITERATION / SIDE EFFECT
    forEach

COPY / EXTRACT
    slice
    concat

STRUCTURAL MUTATION
    splice
    sort
```

This is much more useful than memorizing methods alphabetically.

---

# 10.44 The Most Important Interview Comparison

You should be able to answer this immediately:

```text
map
filter
reduce
forEach
find
some
every
```

| Method      | Main purpose     | Returns               |
| ----------- | ---------------- | --------------------- |
| `map`       | Transform        | New array             |
| `filter`    | Select           | New array             |
| `reduce`    | Accumulate       | Any value             |
| `forEach`   | Side effects     | `undefined`           |
| `find`      | Find first match | Element / `undefined` |
| `findIndex` | Find first match | Index / `-1`          |
| `some`      | Any match?       | Boolean               |
| `every`     | All match?       | Boolean               |

---

# 10.45 A Real-World Pipeline

Suppose we have:

```js
const users = [
    { name: "A", age: 20, active: true },
    { name: "B", age: 17, active: true },
    { name: "C", age: 25, active: false }
];
```

We want names of active adults.

```js
const names = users
    .filter(user => user.active)
    .filter(user => user.age >= 18)
    .map(user => user.name);
```

Result:

```text
["A"]
```

The pipeline is:

```text
users
 ↓
filter active
 ↓
filter adult
 ↓
map to name
 ↓
names
```

This functional style is extremely common in production JavaScript.

---

# 10.46 But Don't Overuse Array Pipelines

For large datasets or performance-sensitive code, blindly chaining:

```js
.filter(...)
.filter(...)
.map(...)
.filter(...)
```

may create multiple intermediate arrays.

Sometimes a single loop is clearer and more efficient:

```js
const result = [];

for (const user of users) {
    if (user.active && user.age >= 18) {
        result.push(user.name);
    }
}
```

The point isn't:

> "Loops are better."

or:

> "Functional methods are better."

The SDE-2 mindset is:

> **Choose the abstraction that gives the right balance of readability, correctness, memory behavior, and performance.**

---

# 10.47 Common Traps

### Trap 1 — `sort()`

```js
[10, 2, 5].sort();
```

doesn't perform numeric sorting.

---

### Trap 2 — `sort()` mutates

```js
const a = [3, 1, 2];
const b = a.sort();

a === b; // true
```

---

### Trap 3 — `slice()` vs `splice()`

```text
slice  → non-mutating
splice → mutating
```

---

### Trap 4 — Spread is shallow

```js
const copy = [...original];
```

doesn't recursively clone nested objects.

---

### Trap 5 — `map()` needs a return value

```js
arr.map(x => {
    x * 2;
});
```

produces an array of `undefined`.

---

### Trap 6 — `forEach()` doesn't produce a transformed array

```js
const result = arr.forEach(...);
```

`result` is:

```text
undefined
```

---

### Trap 7 — Array holes aren't `undefined`

```js
const a = [undefined];
const b = new Array(1);

0 in a; // true
0 in b; // false
```

---

### Trap 8 — Arrays are objects

```js
typeof [] === "object";
```

To check specifically for an Array:

```js
Array.isArray(value);
```

---

# 10.48 SDE-2 Questions

These are the questions I'd prioritize.

### Fundamentals

1. What is an Array in JavaScript?
2. Why does `typeof []` return `"object"`?
3. How does array indexing work?
4. What is special about the `length` property?
5. What happens when you manually increase/decrease `length`?
6. What is a sparse array?
7. Difference between an empty slot and `undefined`.
8. How do you check whether a value is actually an Array?

### Array methods

9. Difference between `map`, `filter`, and `reduce`.
10. Difference between `find` and `filter`.
11. Difference between `some` and `every`.
12. Difference between `map` and `forEach`.
13. Why can't you use `break` directly inside `forEach`?
14. How does `reduce` work internally?
15. When would you avoid using `reduce`?

### Mutation

16. Which array methods mutate the original array?
17. Does `sort()` mutate?
18. Why does `[10, 2, 5].sort()` produce unexpected numeric ordering?
19. Difference between `slice()` and `splice()`.
20. How would you sort an array without mutating the original?

### Copying

21. Is `[...arr]` a deep copy?
22. Why can changing a nested object through a copied array modify the original?
23. Difference between shallow and deep copying of arrays.

### Collections

24. What is an array-like object?
25. Is `arguments` an Array?
26. Difference between iterable and array-like.
27. Difference between `Array.from()` and spread syntax.
28. Why does `for...of` work with arrays?
29. Difference between `for...in` and `for...of` for arrays.

---

# 10.49 Final Mental Model

The most useful way to remember this chapter is:

```text
                         ARRAY
                           │
        ┌──────────────────┼──────────────────┐
        ↓                  ↓                  ↓
    Structure          Transformation      Iteration
        │                  │                  │
    index               map                for...of
    length              filter             forEach
    holes               reduce
                        flat
                        flatMap
        │
        ↓
   Mutation
        │
   push/pop
   splice
   sort
```

And:

```text
map      → transform
filter   → select
find     → first matching value
some     → at least one?
every    → all?
reduce   → accumulate
forEach  → side effect
```

Finally, keep these four distinctions extremely sharp:

```text
slice()   ≠ splice()

map()     ≠ forEach()

find()    ≠ filter()

hole      ≠ undefined
```

Those distinctions, combined with **mutation, references, shallow copying, iteration, and sparse-array behavior**, are what turn basic Array knowledge into strong SDE-2-level JavaScript understanding.
