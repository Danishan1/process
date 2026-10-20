# 20. JavaScript Standard Library & Modern Features — SDE-II

This chapter is different from Points **17–19**.

Here we aren't learning a new runtime model. We're revising the **modern JavaScript tools you should naturally use in production code and be able to explain in an interview**.

The SDE-II goal is:

> **Don't just know the syntax. Know the behavior, edge cases, and when the feature is appropriate.**

We'll go feature by feature.

---

# 20.1 Optional Chaining `?.`

Optional chaining allows you to safely access a property when an intermediate value may be `null` or `undefined`.

Without it:

```js
const city = user && user.address && user.address.city;
```

With it:

```js
const city = user?.address?.city;
```

If:

```js
const user = null;
```

then:

```js
user?.address?.city
```

returns:

```text
undefined
```

instead of throwing:

```text
TypeError
```

---

## Property access

```js
user?.name
```

## Nested access

```js
user?.address?.city
```

## Array access

```js
users?.[0]
```

## Function calls

```js
user?.getName?.()
```

This checks whether `getName` exists and is callable.

---

## Important: `?.` doesn't mean "ignore every error"

Consider:

```js
user?.getName()
```

If `getName` exists but isn't a function:

```js
user.getName = "hello";
```

you can still get an error.

Optional chaining protects against **nullish intermediate values**, not arbitrary programming errors.

---

# 20.2 Optional Chaining + Nullish Coalescing

These two are frequently used together.

```js
const city = user?.address?.city ?? "Unknown";
```

Meaning:

```text
Try to get city
      ↓
If null/undefined
      ↓
"Unknown"
```

This is extremely common production code.

---

# 20.3 Nullish Coalescing `??`

This is different from `||`.

Consider:

```js
const value = input || 100;
```

If:

```js
input = 0;
```

then:

```js
value = 100;
```

because `0` is falsy.

But:

```js
const value = input ?? 100;
```

only falls back for:

```text
null
undefined
```

Therefore:

```js
0 ?? 100          // 0
false ?? true     // false
"" ?? "default"   // ""
null ?? "default" // "default"
undefined ?? 10   // 10
```

This is an important SDE-II distinction.

### `||`

> Use fallback when the left side is **falsy**.

### `??`

> Use fallback when the left side is **nullish**.

---

# 20.4 Logical Assignment

Modern JavaScript provides:

```js
&&=
||=
??=
```

These combine logical operators with assignment.

---

## `||=`

```js
let name = "";

name ||= "Unknown";
```

Result:

```js
name === "Unknown";
```

Equivalent conceptually to:

```js
name = name || "Unknown";
```

---

## `??=`

```js
let count = 0;

count ??= 10;
```

Result:

```js
count === 0;
```

because `0` isn't nullish.

---

## `&&=`

```js
let user = {
    active: true
};

user.active &&= false;
```

Conceptually:

```js
user.active = user.active && false;
```

These are particularly useful for concise state/configuration updates.

---

# 20.5 Destructuring

Destructuring allows you to extract values from arrays and objects.

### Object

```js
const user = {
    name: "John",
    age: 30
};

const { name, age } = user;
```

Instead of:

```js
const name = user.name;
const age = user.age;
```

---

## Rename

```js
const { name: userName } = user;
```

Now:

```js
userName
```

contains the value.

---

## Default value

```js
const { role = "user" } = user;
```

Important:

The default applies when the property is `undefined`, not merely when it is falsy.

---

# 20.6 Nested Destructuring

```js
const user = {
    name: "John",
    address: {
        city: "Delhi"
    }
};

const {
    address: {
        city
    }
} = user;
```

Now:

```js
city === "Delhi";
```

But be careful with potentially missing objects.

This can throw:

```js
const {
    address: { city }
} = user;
```

if:

```js
user.address === undefined
```

In such cases, optional chaining may be clearer:

```js
const city = user.address?.city;
```

---

# 20.7 Array Destructuring

```js
const numbers = [10, 20, 30];

const [a, b, c] = numbers;
```

Result:

```text
a = 10
b = 20
c = 30
```

Skip values:

```js
const [first, , third] = numbers;
```

---

## Rest with destructuring

```js
const [first, ...rest] = numbers;
```

Result:

```text
first = 10
rest = [20, 30]
```

---

# 20.8 Spread Syntax

Spread expands an iterable/object into another structure.

### Array

```js
const a = [1, 2];
const b = [...a, 3, 4];
```

Result:

```js
[1, 2, 3, 4]
```

---

### Object

```js
const user = {
    name: "John"
};

const updated = {
    ...user,
    age: 30
};
```

---

# 20.9 Spread Is a Shallow Copy

This is extremely important.

```js
const user = {
    name: "John",
    address: {
        city: "Delhi"
    }
};

const copy = {
    ...user
};
```

The top-level object is new:

```text
user ─────► Object A
copy ─────► Object B
```

But nested `address` can still be shared:

```text
Object A
   │
   └── address ─────► Object C
                         ▲
Object B                │
   └── address ─────────┘
```

Therefore:

```js
copy.address.city = "Mumbai";
```

can affect:

```js
user.address.city
```

This connects directly to our earlier **object references / shallow vs deep copy** topic.

---

# 20.10 Rest Parameters

Don't confuse rest parameters with spread syntax.

```js
function sum(...numbers) {
    return numbers.reduce((a, b) => a + b, 0);
}
```

Calling:

```js
sum(1, 2, 3, 4);
```

means:

```text
numbers = [1, 2, 3, 4]
```

### Mental distinction

```text
Spread
value → individual elements

Rest
individual arguments → array
```

---

# 20.11 Template Literals

Instead of:

```js
const message = "Hello " + name + ", welcome!";
```

use:

```js
const message = `Hello ${name}, welcome!`;
```

They also support multiline strings:

```js
const html = `
    <div>
        <h1>Hello</h1>
    </div>
`;
```

And tagged templates provide a more advanced metaprogramming mechanism.

---

# 20.12 Symbols

A `Symbol` creates a unique primitive value.

```js
const id1 = Symbol("id");
const id2 = Symbol("id");

console.log(id1 === id2);
```

Result:

```text
false
```

Even though both have the same description.

---

## Why are Symbols useful?

One major use is creating unique object keys.

```js
const id = Symbol("id");

const user = {
    name: "John",
    [id]: 123
};
```

The Symbol key doesn't collide with normal string keys.

---

# 20.13 Symbols and Well-Known Symbols

JavaScript uses Symbols to customize language behavior.

For example:

```js
Symbol.iterator
```

connects directly to our Point 12:

> Iterators & Iterables.

For example, an object can define:

```js
const obj = {
    *[Symbol.iterator]() {
        yield 1;
        yield 2;
        yield 3;
    }
};
```

Now:

```js
for (const value of obj) {
    console.log(value);
}
```

works because you've implemented the iterable protocol.

Other important well-known Symbols include:

```text
Symbol.iterator
Symbol.asyncIterator
Symbol.toPrimitive
Symbol.toStringTag
Symbol.hasInstance
```

For SDE-II, understand their purpose rather than memorizing every one.

---

# 20.14 BigInt

JavaScript's normal `number` uses IEEE-754 double precision.

That means integers above:

```js
Number.MAX_SAFE_INTEGER
```

cannot always be represented exactly.

BigInt solves this for arbitrarily large integers within implementation/resource limits.

Example:

```js
const value = 9007199254740993n;
```

Notice:

```text
n
```

at the end.

---

## BigInt arithmetic

```js
const a = 1000000000000000000n;
const b = 2000000000000000000n;

console.log(a + b);
```

Works.

But:

```js
10n + 5
```

throws an error because you generally cannot mix BigInt and Number directly in arithmetic.

You need explicit conversion:

```js
10n + BigInt(5)
```

or:

```js
Number(10n) + 5
```

with the obvious precision caveat when converting large values to Number.

---

# 20.15 BigInt in Backend Systems

This becomes particularly relevant for:

```text
Database BIGINT IDs
Financial calculations involving integer units
Large counters
Cryptographic / mathematical operations
Snowflake-style IDs
```

But there is an important issue:

```js
JSON.stringify({
    id: 123n
});
```

throws because standard JSON doesn't directly support BigInt.

This is an excellent production-level edge case to know.

---

# 20.16 Private Class Fields

Modern JavaScript classes support private fields using:

```js
#
```

Example:

```js
class BankAccount {
    #balance = 0;

    deposit(amount) {
        this.#balance += amount;
    }

    getBalance() {
        return this.#balance;
    }
}
```

Outside the class:

```js
account.#balance;
```

is invalid.

This is stronger encapsulation than simply using a naming convention such as:

```js
_balance
```

---

# 20.17 `#private` vs Convention

Old convention:

```js
this._balance
```

means:

> "Please don't touch this."

But it's still accessible.

Modern private field:

```js
this.#balance
```

means the language enforces private access semantics.

This is useful for encapsulating invariants.

---

# 20.18 Modern Array Methods

You should be very comfortable with:

```text
map
filter
reduce
find
findLast
findIndex
findLastIndex
some
every
includes
flat
flatMap
at
toSorted
toReversed
toSpliced
with
```

The newer non-mutating methods are particularly useful.

---

# 20.19 `toSorted()` vs `sort()`

Classic:

```js
const numbers = [3, 1, 2];

numbers.sort();
```

mutates the array.

Modern:

```js
const sorted = numbers.toSorted();
```

returns a new array.

So:

```text
sort()
   ↓
mutates original

toSorted()
   ↓
returns new array
```

This is very useful when working with immutable state.

---

# 20.20 `toReversed()`

Instead of:

```js
numbers.reverse();
```

which mutates:

```js
const reversed = numbers.toReversed();
```

creates a new array.

---

# 20.21 `toSpliced()`

Instead of:

```js
array.splice(...)
```

which mutates the original:

```js
const updated = array.toSpliced(...);
```

returns a new array.

---

# 20.22 `with()`

This is useful for immutable updates.

Instead of:

```js
array[2] = "new";
```

you can:

```js
const updated = array.with(2, "new");
```

Original:

```js
array
```

remains unchanged.

This is especially useful in React/state-management-style programming.

---

# 20.23 Modern Object Methods

Important methods include:

```js
Object.keys()
Object.values()
Object.entries()
Object.fromEntries()
Object.hasOwn()
Object.assign()
Object.create()
```

---

## `Object.entries()`

```js
const user = {
    name: "John",
    age: 30
};

Object.entries(user);
```

gives:

```js
[
    ["name", "John"],
    ["age", 30]
]
```

Very useful for transformations.

---

# 20.24 `Object.fromEntries()`

The inverse-style operation:

```js
const entries = [
    ["name", "John"],
    ["age", 30]
];

const user = Object.fromEntries(entries);
```

Result:

```js
{
    name: "John",
    age: 30
}
```

Together:

```text
Object
  ↓
Object.entries()
  ↓
Array
  ↓
transform
  ↓
Object.fromEntries()
  ↓
Object
```

---

# 20.25 `Object.hasOwn()`

Modern approach:

```js
Object.hasOwn(user, "name");
```

instead of relying on:

```js
user.hasOwnProperty("name");
```

Why?

Because objects can:

* shadow `hasOwnProperty`
* have a null prototype
* have unusual prototype structures

So:

```js
Object.hasOwn(obj, property)
```

is a safer modern API.

---

# 20.26 Modern Promise APIs

You already covered Promises in Point 15, but here we're revising the modern APIs from a standard-library perspective.

You should know:

```text
Promise.all()
Promise.allSettled()
Promise.race()
Promise.any()
Promise.resolve()
Promise.reject()
```

---

## `Promise.all()`

```js
const [users, orders] = await Promise.all([
    fetchUsers(),
    fetchOrders()
]);
```

Runs operations concurrently.

Rejects if one rejects.

Use when:

> **All results are required.**

---

# 20.27 `Promise.allSettled()`

```js
const results = await Promise.allSettled([
    fetchUsers(),
    fetchOrders(),
    fetchRecommendations()
]);
```

Waits for all operations.

Returns statuses such as:

```js
{
    status: "fulfilled",
    value: ...
}
```

or:

```js
{
    status: "rejected",
    reason: ...
}
```

Use when:

> **You want the outcome of every operation regardless of individual failures.**

---

# 20.28 `Promise.race()`

Returns the first Promise that settles.

```js
await Promise.race([
    request(),
    timeout()
]);
```

Useful for timeout patterns, although production timeout implementations should carefully handle cancellation/resource cleanup.

---

# 20.29 `Promise.any()`

Returns the first **fulfilled** Promise.

```js
const result = await Promise.any([
    serverA(),
    serverB(),
    serverC()
]);
```

If all reject:

```text
AggregateError
```

Use case:

> Multiple equivalent sources; you need whichever successfully responds first.

---

# 20.30 `Promise.withResolvers()`

A modern Promise API worth knowing.

Conceptually:

```js
const {
    promise,
    resolve,
    reject
} = Promise.withResolvers();
```

This separates creation of the Promise from access to its settlement functions.

It can be useful when integrating callback/event-style APIs with Promise-based code.

But don't automatically use it everywhere.

---

# 20.31 `structuredClone()`

This is a very useful modern API.

```js
const copy = structuredClone(original);
```

It performs a structured clone rather than a shallow spread.

For example:

```js
const user = {
    name: "John",
    address: {
        city: "Delhi"
    }
};

const copy = structuredClone(user);

copy.address.city = "Mumbai";
```

The original nested object is not shared.

---

# 20.32 Why Not `JSON.parse(JSON.stringify())`?

A common old technique:

```js
const copy = JSON.parse(
    JSON.stringify(object)
);
```

But it has many limitations.

It doesn't correctly preserve many JavaScript values/types, such as:

```text
undefined
functions
Map
Set
BigInt
Date semantics
special object types
circular references
```

`structuredClone()` supports a much broader structured-clone data model.

---

# 20.33 `structuredClone()` and Circular References

This is another useful difference.

```js
const obj = {};

obj.self = obj;
```

JSON serialization fails because of the circular reference.

But:

```js
const copy = structuredClone(obj);
```

can handle supported circular structures.

---

# 20.34 Modern ECMAScript Features

You don't need to memorize every new ECMAScript feature.

Instead, maintain awareness of modern language capabilities.

Important areas include:

```text
Optional chaining
Nullish coalescing
Logical assignment
Private fields
BigInt
Async iterators
Optional catch binding
Numeric separators
Object spread
Array spread
Promise improvements
Structured cloning
Non-mutating array methods
```

And newer ECMAScript versions continue adding smaller improvements.

For an SDE-II developer, the important skill is:

> **Know how to recognize a modern idiomatic solution instead of writing unnecessarily verbose legacy JavaScript.**

---

# 20.35 A Production Example

Suppose you receive:

```js
const response = await fetch("/api/user");

const user = await response.json();
```

You could write:

```js
const city = user &&
    user.profile &&
    user.profile.address &&
    user.profile.address.city;
```

Modern JavaScript:

```js
const city =
    user?.profile?.address?.city ?? "Unknown";
```

Then:

```js
const updatedUser = {
    ...user,
    profile: {
        ...user.profile,
        city
    }
};
```

And if you need an independent deep clone:

```js
const backup = structuredClone(user);
```

You can see how these features combine.

---

# 20.36 SDE-II Pitfalls

These are more important than syntax memorization.

### `||` vs `??`

```js
0 || 10     // 10
0 ?? 10     // 0
```

---

### Spread is shallow

```js
const copy = { ...original };
```

doesn't recursively clone nested objects.

---

### `sort()` mutates

```js
array.sort();
```

versus:

```js
array.toSorted();
```

---

### BigInt and Number don't mix automatically

```js
10n + 10
```

throws.

---

### BigInt + JSON

```js
JSON.stringify(10n);
```

throws.

---

### Optional chaining isn't a universal error suppressor

```js
user?.foo.bar
```

can still fail if `foo` is `null`/`undefined`, because only `user` is being optionally chained there.

Safer nested form:

```js
user?.foo?.bar
```

---

# 20.37 What I'd Expect From You in an SDE-II Interview

You shouldn't merely say:

> "`??` means null checking."

You should say:

> "`??` performs nullish coalescing, so it falls back only for `null` and `undefined`, unlike `||`, which falls back for any falsy value such as `0`, `false`, or an empty string."

Similarly:

> "`structuredClone` provides deep structured cloning for supported JavaScript values, unlike object spread, which only performs a shallow copy."

And:

> "`toSorted()` is useful when we need a sorted copy without mutating the original array."

That level of explanation is what we're targeting.

---

# 20.38 Priority for Your Revision

I'd rank Point 20 like this:

### 🔴 Must Know

```text
?. 
??
|| vs ??
??=
Destructuring
Spread vs Rest
structuredClone
Modern array methods
Promise.all
Promise.allSettled
Promise.race
Promise.any
Symbols
BigInt
Private fields
```

### 🟡 Should Know

```text
Logical assignment
Object.entries
Object.fromEntries
Object.hasOwn
toSorted
toReversed
toSpliced
with
```

### 🟢 Know Conceptually

```text
Well-known Symbols
Promise.withResolvers
New ECMAScript additions
Advanced tagged templates
```

---

# 20.39 Final Mental Map

```text
MODERN JAVASCRIPT
│
├── Safe Access
│   ├── ?.
│   └── ??
│
├── Concise Assignment
│   ├── ||=
│   ├── &&=
│   └── ??=
│
├── Data Extraction
│   ├── Destructuring
│   ├── Spread
│   └── Rest
│
├── Modern Types
│   ├── Symbol
│   ├── BigInt
│   └── Private Fields
│
├── Modern Collections
│   ├── Array methods
│   ├── Object methods
│   └── Map / Set
│
├── Async
│   ├── Promise.all
│   ├── Promise.allSettled
│   ├── Promise.race
│   ├── Promise.any
│   └── Promise.withResolvers
│
└── Data Cloning
    └── structuredClone()
```

The **SDE-II takeaway** is that Point 20 isn't really about memorizing modern syntax. It's about being able to look at JavaScript code and immediately recognize:

**what is being copied, what is being mutated, what values are considered missing, what happens on `null`/`undefined`, what happens concurrently, and what data types are actually being represented.**

That ties Point 20 back to almost everything we've already revised: **types → references → functions → promises → iterables → objects → memory → browser/Node runtime**.
