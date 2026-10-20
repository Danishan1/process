
# 8.61 High-Probability SDE-2 Questions

I would prioritize these heavily.

### Object fundamentals

1. What is an object in JavaScript?
2. How are objects created?
3. What are object literals?
4. What are property keys?
5. Can property keys be numbers?
6. What are Symbols as property keys?
7. Difference between dot and bracket notation.
8. What are computed property names?
9. How do you add, update, and delete properties?
10. Difference between deleting a property and assigning `undefined`.

### References

11. Why are objects compared by reference?
12. Why does assigning one object to another not create a copy?
13. What is object identity?
14. Why does `{}` === `{}` return `false`?
15. How do you copy an object?

### Copying

16. What is a shallow copy?
17. Why does object spread create only a shallow copy?
18. Explain nested-object behavior after a spread copy.
19. What is a deep copy?
20. Difference between `structuredClone()` and JSON-based cloning.
21. What values cannot be safely represented through JSON cloning?

### Descriptors

22. What is a property descriptor?
23. Difference between data and accessor properties.
24. What do `writable`, `enumerable`, and `configurable` mean?
25. What is a getter?
26. What is a setter?
27. How do you inspect a property's descriptor?

### Ownership

28. What is an own property?
29. What is an inherited property?
30. How do you check whether an object owns a property?
31. Difference between `Object.hasOwn()` and `in`.

### Prototypes

32. What is a prototype?
33. What is the prototype chain?
34. How does property lookup work through the prototype chain?
35. What is `Object.prototype`?
36. What is `__proto__`?
37. Difference between `__proto__` and `prototype`.
38. What does `Object.create()` do?
39. What happens with `Object.create(null)`?
40. How do you get an object's prototype?

### Constructors / `new`

41. What is a constructor function?
42. What does the `new` operator actually do?
43. What is `User.prototype`?
44. How does an instance relate to `User.prototype`?
45. Why put methods on the prototype instead of creating them for every instance?
46. Explain prototype inheritance.
47. How do JavaScript classes relate to prototypes?

### Immutability

48. What does `Object.freeze()` do?
49. Is `Object.freeze()` deep or shallow?
50. Difference between `freeze`, `seal`, and `preventExtensions`.


Absolutely. Let's turn the 50 questions into an **SDE-II interview revision sheet**. I'll keep each answer concise but include the important mental model and examples.

# 8.61 — Objects & Object Model: 50 SDE-II Questions

## Object Fundamentals

### 1. What is an object in JavaScript?

An object is a collection of **properties**, where each property has a key and a value.

```js
const user = {
    name: "Danishan",
    age: 25
};
```

Conceptually:

```text
user
 ├── name → "Danishan"
 └── age  → 25
```

The value can itself be an object, array, function, primitive, etc.

---

### 2. How are objects created?

Common ways:

```js
// Object literal
const user = { name: "Danishan" };

// Object constructor
const user = new Object();

// Object.create
const user = Object.create(null);

// Constructor function
function User(name) {
    this.name = name;
}

const user = new User("Danishan");

// Class
class User {
    constructor(name) {
        this.name = name;
    }
}

const user = new User("Danishan");
```

For modern application code, **object literals and classes** are the most common.

---

### 3. What are object literals?

An object literal is the `{}` syntax used to directly create an object.

```js
const user = {
    name: "Danishan",
    age: 25
};
```

It creates a new object with those own properties.

---

### 4. What are property keys?

Property keys are identifiers used to access properties.

JavaScript property keys are:

```text
String
Symbol
```

Example:

```js
const user = {
    name: "Danishan"
};
```

The key is:

```text
"name"
```

Numeric-looking keys are converted to strings:

```js
const obj = {
    1: "hello"
};

console.log(obj[1]);
console.log(obj["1"]);
```

Both access the same property.

---

### 5. Can property keys be numbers?

Not as a distinct numeric property-key type for ordinary objects.

```js
const obj = {
    10: "hello"
};
```

The key effectively becomes:

```text
"10"
```

Therefore:

```js
obj[10]
obj["10"]
```

refer to the same property.

---

### 6. What are Symbols as property keys?

A `Symbol` is a primitive that can be used as a unique property key.

```js
const id = Symbol("id");

const user = {
    [id]: 123
};
```

Another Symbol with the same description is different:

```js
Symbol("id") === Symbol("id");
// false
```

This makes Symbols useful when you want property keys that won't collide with ordinary string keys.

---

### 7. Difference between dot and bracket notation?

```js
user.name
```

means:

> Access the property literally named `"name"`.

Whereas:

```js
user[key]
```

means:

> Evaluate `key`, then use its value as the property key.

Example:

```js
const key = "name";

user.key;   // looks for "key"
user[key];  // looks for "name"
```

This is a very common interview question.

---

### 8. What are computed property names?

They allow an expression to determine an object's property key.

```js
const key = "name";

const user = {
    [key]: "Danishan"
};
```

Result:

```js
{
    name: "Danishan"
}
```

Another example:

```js
const prefix = "user";

const obj = {
    [`${prefix}Name`]: "Danishan"
};
```

Creates:

```js
{
    userName: "Danishan"
}
```

---

### 9. How do you add, update, and delete properties?

### Add

```js
user.age = 25;
```

### Update

```js
user.age = 26;
```

### Delete

```js
delete user.age;
```

Important:

```js
user.age = undefined;
```

does **not** remove the property.

---

### 10. Difference between deleting a property and assigning `undefined`?

These are different:

```js
const user = {
    age: 25
};

user.age = undefined;
```

The property still exists.

```js
"age" in user;
// true
```

But:

```js
delete user.age;
```

removes the property.

```js
"age" in user;
// false
```

This can matter for enumeration, property existence checks, and serialization.

---

# References

### 11. Why are objects compared by reference?

Objects have **identity**.

Consider:

```js
const a = { x: 1 };
const b = { x: 1 };

console.log(a === b);
```

Result:

```text
false
```

Because:

```text
a → Object A

b → Object B
```

They contain the same data but are different objects.

---

### 12. Why does assigning one object to another not create a copy?

Because assignment copies the **reference/value pointing to the object**, not the object's contents.

```js
const a = {
    name: "Danishan"
};

const b = a;
```

Conceptually:

```text
a ──┐
    ├──→ Object
b ──┘
```

Therefore:

```js
b.name = "Alex";

console.log(a.name);
```

prints:

```text
Alex
```

Both refer to the same object.

---

### 13. What is object identity?

Object identity means the **specific object instance**.

```js
const a = {};
const b = {};
```

Even though both are empty:

```js
a === b;
// false
```

They have different identities.

But:

```js
const c = a;

a === c;
// true
```

Same identity.

---

### 14. Why does `{}` === `{}` return false?

Because every object literal creates a **new object**.

```js
{} === {}
```

is effectively:

```text
Object A === Object B
```

Therefore:

```text
false
```

The contents don't matter for `===`; object identity does.

---

### 15. How do you copy an object?

Common shallow-copy approaches:

```js
const copy = { ...original };
```

or:

```js
const copy = Object.assign({}, original);
```

For supported data structures, a deep clone can be:

```js
const copy = structuredClone(original);
```

Important distinction:

```text
spread / Object.assign
        ↓
shallow copy

structuredClone
        ↓
deep clone for supported values
```

---

# Copying

### 16. What is a shallow copy?

A shallow copy creates a new top-level object but doesn't recursively clone nested objects.

```js
const original = {
    name: "Danishan",
    address: {
        city: "Delhi"
    }
};

const copy = { ...original };
```

Now:

```text
original ──→ Object A
copy     ──→ Object B

Object A ──┐
           ├──→ address Object C
Object B ──┘
```

The `address` object is shared.

---

### 17. Why does object spread create only a shallow copy?

Because spread copies the object's **own enumerable properties** into a new object.

It doesn't recursively clone values.

```js
const copy = {
    ...original
};
```

For:

```js
original.address
```

the reference/value associated with `address` is copied.

The nested object itself isn't recursively duplicated.

---

### 18. Explain nested-object behavior after a spread copy.

```js
const a = {
    profile: {
        name: "Danishan"
    }
};

const b = { ...a };

b.profile.name = "Alex";

console.log(a.profile.name);
```

Result:

```text
Alex
```

Because:

```text
a.profile ──┐
            ├──→ same nested object
b.profile ──┘
```

Top-level:

```text
a !== b
```

Nested:

```text
a.profile === b.profile
```

---

### 19. What is a deep copy?

A deep copy creates independent copies of nested objects as well.

Conceptually:

```text
Original
 └── address → Object A

Deep Copy
 └── address → Object B
```

Therefore:

```js
copy.address.city = "Mumbai";
```

doesn't affect:

```js
original.address.city;
```

---

### 20. Difference between `structuredClone()` and JSON-based cloning?

`structuredClone()` is designed to clone many JavaScript data structures while preserving more types and relationships than JSON serialization.

```js
const copy = structuredClone(original);
```

JSON cloning:

```js
const copy = JSON.parse(
    JSON.stringify(original)
);
```

is really:

```text
JavaScript object
       ↓
JSON serialization
       ↓
JSON parsing
       ↓
new object
```

Therefore JSON has limitations.

For example, JSON doesn't represent:

```text
undefined
functions
Symbol
BigInt
```

in the same way as JavaScript.

For general-purpose deep cloning of supported values, prefer:

```js
structuredClone()
```

---

### 21. What values cannot be safely represented through JSON cloning?

Examples include:

```js
undefined
function () {}
Symbol()
BigInt(123)
```

Also, `Date` becomes a string through JSON serialization rather than remaining a `Date`.

Example:

```js
const obj = {
    date: new Date()
};

const copy = JSON.parse(JSON.stringify(obj));

console.log(copy.date instanceof Date);
// false
```

So JSON cloning is appropriate only when your data is intentionally **JSON-compatible**.

---

# Property Descriptors

### 22. What is a property descriptor?

A descriptor defines the behavior/configuration of an object property.

For a data property:

```text
value
writable
enumerable
configurable
```

For an accessor property:

```text
get
set
enumerable
configurable
```

You can inspect one using:

```js
Object.getOwnPropertyDescriptor(
    obj,
    "name"
);
```

---

### 23. Difference between data and accessor properties?

### Data property

Stores an actual value:

```js
const user = {
    name: "Danishan"
};
```

Conceptually:

```text
value
writable
enumerable
configurable
```

### Accessor property

Uses getter/setter functions:

```js
const user = {
    firstName: "Danishan",

    get name() {
        return this.firstName;
    }
};
```

Conceptually:

```text
get
set
enumerable
configurable
```

---

### 24. What do `writable`, `enumerable`, and `configurable` mean?

### `writable`

Can the value be changed?

```js
writable: false
```

means ordinary assignment cannot change the value.

### `enumerable`

Does it appear in common enumeration such as:

```js
Object.keys(obj)
```

### `configurable`

Controls whether the property can generally have its descriptor reconfigured and whether it can be deleted.

Example:

```js
Object.defineProperty(obj, "id", {
    value: 123,
    writable: false,
    enumerable: false,
    configurable: false
});
```

---

### 25. What is a getter?

A getter is a function invoked when a property is read.

```js
const user = {
    firstName: "Danishan",
    lastName: "Farookh",

    get fullName() {
        return `${this.firstName} ${this.lastName}`;
    }
};
```

You access it as:

```js
user.fullName;
```

not:

```js
user.fullName();
```

---

### 26. What is a setter?

A setter is invoked when a property is assigned.

```js
const user = {
    _name: "",

    set name(value) {
        this._name = value.trim();
    }
};
```

Then:

```js
user.name = " Danishan ";
```

invokes the setter.

Setters are useful for:

* validation
* normalization
* controlled mutation

---

### 27. How do you inspect a property's descriptor?

Use:

```js
Object.getOwnPropertyDescriptor(
    user,
    "name"
);
```

For all own properties:

```js
Object.getOwnPropertyDescriptors(user);
```

This is useful when debugging things like:

```text
Why can't I modify this property?
Why doesn't Object.keys() show it?
Why can't I delete it?
```

---

# Ownership

### 28. What is an own property?

An own property is a property directly stored on the object itself.

```js
const user = {
    name: "Danishan"
};
```

Here:

```text
name
```

is an own property.

Check:

```js
Object.hasOwn(user, "name");
// true
```

---

### 29. What is an inherited property?

An inherited property isn't stored directly on the object. It is found through the object's prototype chain.

```js
const user = {};

user.toString;
```

`toString` isn't normally an own property of `user`.

It's inherited from:

```js
Object.prototype
```

Conceptually:

```text
user
 ↓
Object.prototype
 ↓
null
```

---

### 30. How do you check whether an object owns a property?

Modern preferred approach:

```js
Object.hasOwn(obj, "name");
```

Example:

```js
const user = {
    name: "Danishan"
};

Object.hasOwn(user, "name");
// true
```

Don't rely on:

```js
obj.hasOwnProperty(...)
```

because an object may override or lack that method.

---

### 31. Difference between `Object.hasOwn()` and `in`?

This is important.

```js
Object.hasOwn(obj, "name")
```

checks **only own properties**.

Whereas:

```js
"name" in obj
```

checks:

```text
own properties
+
prototype chain
```

Example:

```js
const obj = {};
```

Then:

```js
"toString" in obj;
// true
```

because `toString` is inherited.

But:

```js
Object.hasOwn(obj, "toString");
// false
```

---

# Prototypes

### 32. What is a prototype?

A prototype is the object another object can inherit properties and methods from.

Example:

```js
const parent = {
    greet() {
        console.log("Hello");
    }
};

const child = Object.create(parent);
```

Conceptually:

```text
child
 ↓
parent
```

So:

```js
child.greet();
```

works even though `greet` isn't an own property of `child`.

---

### 33. What is the prototype chain?

The prototype chain is the sequence of objects JavaScript searches during property lookup.

Example:

```text
user
 ↓
User.prototype
 ↓
Object.prototype
 ↓
null
```

When JavaScript can't find a property on `user`, it checks the next object.

---

### 34. How does property lookup work through the prototype chain?

For:

```js
user.name
```

conceptually:

```text
1. Search user
       ↓
2. Found?
       ↓
      yes → return value

      no
       ↓
3. Search user prototype
       ↓
4. Continue upward
       ↓
5. Eventually reach null
       ↓
6. Return undefined
```

This is **property lookup**.

---

### 35. What is `Object.prototype`?

`Object.prototype` is the prototype object from which ordinary objects ultimately inherit.

For:

```js
const user = {};
```

the chain is approximately:

```text
user
 ↓
Object.prototype
 ↓
null
```

Methods such as:

```js
toString()
```

are available through this inheritance chain.

---

### 36. What is `__proto__`?

`__proto__` is a legacy accessor that exposes an object's prototype.

Example:

```js
const user = {};

user.__proto__ === Object.prototype;
// true
```

Modern code should generally use:

```js
Object.getPrototypeOf(user);
```

instead of relying on `__proto__`.

---

### 37. Difference between `__proto__` and `prototype`?

This is one of the **highest-value JavaScript interview questions**.

Suppose:

```js
function User() {}

const user = new User();
```

### `User.prototype`

This is a property on the constructor function:

```js
User.prototype
```

It is the object that instances created with `new User()` normally inherit from.

### `user`'s prototype

The instance has a prototype relationship:

```js
Object.getPrototypeOf(user)
```

which gives:

```js
User.prototype
```

So:

```js
Object.getPrototypeOf(user) === User.prototype;
// true
```

### `__proto__`

```js
user.__proto__
```

is a legacy way of accessing that prototype relationship.

Think:

```text
User
 │
 └── prototype ──→ User.prototype
                       ↑
                       │
user ──────────────────┘
```

---

### 38. What does `Object.create()` do?

It creates a new object whose prototype is the object you provide.

```js
const parent = {
    greet() {
        console.log("Hello");
    }
};

const child = Object.create(parent);
```

Now:

```text
child
 ↓
parent
```

Therefore:

```js
child.greet();
```

works.

---

### 39. What happens with `Object.create(null)`?

It creates an object with **no prototype**.

```js
const obj = Object.create(null);
```

Conceptually:

```text
obj
 ↓
null
```

Therefore it does not inherit:

```text
toString
hasOwnProperty
constructor
...
```

from `Object.prototype`.

This can be useful for pure dictionary/map-like objects.

---

### 40. How do you get an object's prototype?

Preferred:

```js
Object.getPrototypeOf(obj);
```

Example:

```js
const user = {};

console.log(
    Object.getPrototypeOf(user) === Object.prototype
);
// true
```

Avoid relying on `__proto__` in modern code.

---

# Constructors / `new`

### 41. What is a constructor function?

A constructor function is a function intended to create and initialize objects when called with `new`.

```js
function User(name) {
    this.name = name;
}

const user = new User("Danishan");
```

The function initializes the new instance.

---

### 42. What does the `new` operator actually do?

For:

```js
const user = new User("Danishan");
```

conceptually:

### 1. Create a new object

```text
newObject
```

### 2. Set its prototype

```text
newObject.[[Prototype]]
        ↓
User.prototype
```

### 3. Call `User` with `this = newObject`

```js
User.call(newObject, "Danishan");
```

which executes:

```js
newObject.name = "Danishan";
```

### 4. Return the object

Conceptually:

```text
user
 ↓
newObject
 ↓
User.prototype
```

There are additional details around explicit constructor returns, but this is the correct interview mental model.

---

### 43. What is `User.prototype`?

For a normal constructor function:

```js
function User() {}
```

JavaScript provides a `prototype` property on the function.

```js
User.prototype
```

is an object that instances created using:

```js
new User()
```

normally inherit from.

You can place shared methods there:

```js
User.prototype.greet = function () {
    console.log("Hello");
};
```

---

### 44. How does an instance relate to `User.prototype`?

```js
function User() {}

const user = new User();
```

Then:

```js
Object.getPrototypeOf(user) === User.prototype;
```

is:

```text
true
```

Conceptually:

```text
user
 ↓
User.prototype
 ↓
Object.prototype
 ↓
null
```

---

### 45. Why put methods on the prototype instead of creating them for every instance?

Consider:

```js
function User(name) {
    this.name = name;

    this.greet = function () {
        console.log(this.name);
    };
}
```

Every instance gets a separate function.

```text
user1 → greet function A
user2 → greet function B
user3 → greet function C
```

Instead:

```js
User.prototype.greet = function () {
    console.log(this.name);
};
```

All instances can share the same function:

```text
user1 ──┐
user2 ──┼──→ User.prototype.greet
user3 ──┘
```

This generally avoids creating a separate method function for every instance.

---

### 46. Explain prototype inheritance.

Suppose:

```js
const animal = {
    eat() {
        console.log("Eating");
    }
};

const dog = Object.create(animal);

dog.bark = function () {
    console.log("Barking");
};
```

The chain:

```text
dog
 ↓
animal
 ↓
Object.prototype
 ↓
null
```

So:

```js
dog.bark();
```

finds `bark` on `dog`.

And:

```js
dog.eat();
```

doesn't find `eat` on `dog`, so JavaScript searches `animal`.

This is prototype-based inheritance.

---

### 47. How do JavaScript classes relate to prototypes?

Classes provide cleaner syntax for JavaScript's prototype-based model.

```js
class User {
    constructor(name) {
        this.name = name;
    }

    greet() {
        console.log(this.name);
    }
}
```

The `greet` method is placed on:

```js
User.prototype
```

So:

```js
const user = new User("Danishan");
```

has approximately:

```text
user
 ├── name
 │
 ↓
User.prototype
 └── greet
      │
      ↓
Object.prototype
      │
      ↓
     null
```

Therefore:

> JavaScript classes do not replace prototypes; they provide class-oriented syntax on top of the prototype system.

---

# Immutability

### 48. What does `Object.freeze()` do?

It makes an object non-extensible and prevents modification/deletion of its existing properties, subject to the property's descriptor semantics.

Example:

```js
const user = {
    name: "Danishan"
};

Object.freeze(user);
```

Then:

```js
user.name = "Alex";
delete user.name;
user.age = 25;
```

will not successfully mutate the frozen object.

In strict mode, prohibited mutations generally throw `TypeError`.

---

### 49. Is `Object.freeze()` deep or shallow?

**Shallow.**

```js
const user = {
    name: "Danishan",

    address: {
        city: "Delhi"
    }
};

Object.freeze(user);
```

This:

```js
user.name = "Alex";
```

is blocked.

But:

```js
user.address.city = "Mumbai";
```

can still modify the nested object.

Because:

```text
user                 ← frozen
 │
 └── address ───────→ nested object
                         ↑
                       NOT frozen
```

To deeply freeze, you would need a recursive strategy or an appropriate immutable-data design.

---

### 50. Difference between `freeze`, `seal`, and `preventExtensions`?

This is worth memorizing as a hierarchy:

| Operation             | Add | Delete | Modify existing values |
| --------------------- | --: | -----: | ---------------------: |
| `preventExtensions()` |   ❌ |      ✅ |                      ✅ |
| `seal()`              |   ❌ |      ❌ |                     ✅* |
| `freeze()`            |   ❌ |      ❌ |                     ❌* |

`*` Subject to property descriptors and the property's nature.

Think:

```text
preventExtensions
        ↓
       ADD ❌

seal
        ↓
    ADD ❌
    DELETE ❌

freeze
        ↓
    ADD ❌
    DELETE ❌
    MODIFY ❌
```

All three are **shallow**.

---

# 🔥 The 10 Questions I Would Prioritize for SDE-II

If you're revising this for an interview, don't give equal weight to all 50.

Focus heavily on these:

| Priority | Question                          | What interviewer is testing |
| -------- | --------------------------------- | --------------------------- |
| 🔥🔥🔥   | 12. Object assignment             | References / identity       |
| 🔥🔥🔥   | 14. `{}` === `{}`                 | Object identity             |
| 🔥🔥🔥   | 16. Shallow copy                  | References                  |
| 🔥🔥🔥   | 18. Nested spread                 | Shared nested references    |
| 🔥🔥🔥   | 31. `Object.hasOwn()` vs `in`     | Property lookup             |
| 🔥🔥🔥   | 34. Prototype lookup              | Prototype chain             |
| 🔥🔥🔥   | 37. `__proto__` vs `prototype`    | Object model                |
| 🔥🔥🔥   | 42. What `new` does               | Constructor mechanics       |
| 🔥🔥🔥   | 47. Classes vs prototypes         | JS internals                |
| 🔥🔥🔥   | 50. freeze/seal/preventExtensions | Object mutability           |

### And the single mental model to remember:

```text
                 OBJECT
                   │
          ┌────────┴────────┐
          ↓                 ↓
    Own Properties       Prototype
          │                 │
     ┌────┴────┐            ↓
     ↓         ↓       Next Prototype
   Data      Accessor       │
 Property    Property       ↓
     │         │        Object.prototype
     │         │             │
     ↓         ↓             ↓
 value       get/set        null
```

When you see:

```js
obj.someProperty
```

think:

```text
Does obj have it?
       ↓ no
Does prototype have it?
       ↓ no
Does next prototype have it?
       ↓
      ...
       ↓
     null
       ↓
  undefined
```

But when you see:

```js
someVariable
```

think:

```text
Current lexical scope
       ↓
Outer scope
       ↓
Outer scope
       ↓
Global/module scope
```

**That is the fundamental connection between your previous “Scope & Closures” section and this “Objects & Object Model” section:**

> **Scope chain resolves identifiers; prototype chain resolves object properties.**
