# 8. Objects & Object Model

This is another **high-priority SDE-2 JavaScript topic**.

So far, we have built this progression:

```text
1. Language Fundamentals
        ↓
2. Data Types
        ↓
3. Operators & Expressions
        ↓
4. Control Flow
        ↓
5. Functions
        ↓
6. Scope & Closures
        ↓
7. Objects & Object Model
```

Objects are where JavaScript becomes particularly interesting because **objects are not just collections of key-value pairs**.

At the surface level:

```js
const user = {
    name: "Danishan",
    age: 25
};
```

looks like:

```text
user
 ├── name → "Danishan"
 └── age  → 25
```

But underneath, JavaScript objects have:

* properties
* property keys
* property descriptors
* ownership
* prototypes
* prototype chains
* inheritance behavior
* special internal semantics

The most important distinction to keep in mind is:

> **An object's properties and its prototype are two different things.**

---

# 8.1 What Is an Object?

An object is a collection of properties.

A property associates a **key** with a value.

```js
const user = {
    name: "Danishan",
    age: 25
};
```

Conceptually:

```text
user
 │
 ├── "name" → "Danishan"
 └── "age"  → 25
```

But the values themselves can be anything:

```js
const user = {
    name: "Danishan",
    age: 25,
    active: true,
    address: {
        city: "Delhi"
    },
    greet() {
        console.log("Hello");
    }
};
```

So object properties can contain:

```text
primitive values
objects
arrays
functions
etc.
```

---

# 8.2 Object Creation

There are several ways to create objects.

The most common is an object literal:

```js
const user = {
    name: "Danishan",
    age: 25
};
```

You can also use:

```js
const user = new Object();
```

or:

```js
const user = Object.create(null);
```

or constructor functions:

```js
function User(name) {
    this.name = name;
}

const user = new User("Danishan");
```

Modern JavaScript also commonly uses classes:

```js
class User {
    constructor(name) {
        this.name = name;
    }
}

const user = new User("Danishan");
```

Classes are built on JavaScript's prototype-based object model.

We'll get to that.

---

# 8.3 Object Literals

The most straightforward syntax is:

```js
const user = {
    name: "Danishan",
    age: 25
};
```

The keys are:

```text
name
age
```

and the values are:

```text
"Danishan"
25
```

You can also write methods:

```js
const user = {
    name: "Danishan",

    greet() {
        console.log(`Hello ${this.name}`);
    }
};
```

Then:

```js
user.greet();
```

---

# 8.4 Properties

An object property consists conceptually of:

```text
property
 ├── key
 └── value
```

For:

```js
const user = {
    name: "Danishan"
};
```

we have:

```text
key   → "name"
value → "Danishan"
```

A property can later be added:

```js
user.age = 25;
```

Now:

```text
user
 ├── name → "Danishan"
 └── age  → 25
```

---

# 8.5 Property Keys

JavaScript property keys are primarily:

```text
string
symbol
```

For example:

```js
const user = {
    name: "Danishan",
    age: 25
};
```

Although we write:

```js
age
```

the property key is effectively:

```text
"age"
```

Numeric-looking property keys are converted to strings in ordinary object property keys.

```js
const obj = {
    1: "one"
};

console.log(obj[1]);
console.log(obj["1"]);
```

Both access the same property.

```text
1
↓
"1"
```

Symbols are the other property-key type:

```js
const id = Symbol("id");

const user = {
    [id]: 123
};
```

Here `id` is a Symbol property key.

---

# 8.6 Property Values

Property values can be any JavaScript value.

```js
const obj = {
    string: "hello",
    number: 10,
    boolean: true,
    nothing: null,
    fn: function () {},
    nested: {},
    array: []
};
```

So there is no special "object property type."

A property simply points to a JavaScript value.

---

# 8.7 Methods

When a function is stored as an object property, we commonly call it a **method**.

```js
const user = {
    name: "Danishan",

    greet() {
        console.log("Hello");
    }
};
```

Here:

```text
user
 ├── name → "Danishan"
 └── greet → function
```

Calling:

```js
user.greet();
```

invokes the function.

The important connection to our previous section is:

```js
this
```

When called as:

```js
user.greet();
```

the function's `this` is determined by the call expression.

---

# 8.8 Dot Notation

The most common property access syntax is:

```js
user.name
```

This accesses the property whose key is:

```text
"name"
```

Example:

```js
const user = {
    name: "Danishan"
};

console.log(user.name);
```

Result:

```text
Danishan
```

Dot notation is convenient when the property name is known and is a valid identifier.

---

# 8.9 Bracket Notation

You can also use:

```js
user["name"]
```

Example:

```js
const user = {
    name: "Danishan"
};

console.log(user["name"]);
```

Same result.

The important difference appears when the property name is dynamic.

```js
const key = "name";

console.log(user[key]);
```

This means:

```text
key
 ↓
"name"
 ↓
user["name"]
```

---

# 8.10 Dot vs Bracket: A Critical Difference

Consider:

```js
const key = "name";

user.key;
```

This means:

> Look for the property literally called `"key"`.

It does **not** mean:

> Use the value of the variable `key`.

For dynamic access:

```js
user[key];
```

So:

```text
user.key
   ↓
literal property "key"

user[key]
   ↓
evaluate key first
   ↓
property "name"
```

This is a common interview trap.

---

# 8.11 Computed Property Names

JavaScript allows expressions inside object literals:

```js
const key = "name";

const user = {
    [key]: "Danishan"
};
```

The result is effectively:

```js
{
    name: "Danishan"
}
```

You can use arbitrary expressions:

```js
const prefix = "user";
const property = `${prefix}Name`;

const obj = {
    [property]: "Danishan"
};
```

Now:

```js
obj.userName;
```

returns:

```text
Danishan
```

This is called a **computed property name**.

---

# 8.12 Adding Properties

Objects are generally mutable.

```js
const user = {};

user.name = "Danishan";
user.age = 25;
```

Now:

```text
user
 ├── name
 └── age
```

You can also use bracket notation:

```js
user["role"] = "engineer";
```

---

# 8.13 Updating Properties

If a property already exists:

```js
const user = {
    name: "Danishan"
};

user.name = "Engineer Danishan";
```

The binding associated with the property changes:

```text
before:
name → "Danishan"

after:
name → "Engineer Danishan"
```

---

# 8.14 Removing Properties

Use `delete`:

```js
const user = {
    name: "Danishan",
    age: 25
};

delete user.age;
```

Now:

```text
user
 └── name → "Danishan"
```

`delete` removes the property itself.

It is different from:

```js
user.age = undefined;
```

That leaves the property present:

```text
age → undefined
```

Whereas:

```js
delete user.age;
```

removes the property.

This difference matters for:

* property existence
* enumeration
* serialization
* property descriptors

---

# 8.15 Nested Objects

Objects can contain other objects.

```js
const user = {
    name: "Danishan",

    address: {
        city: "Delhi",
        country: "India"
    }
};
```

Access:

```js
user.address.city;
```

Conceptually:

```text
user
 │
 └── address
       │
       ├── city
       └── country
```

Each object is a separate object value.

This becomes important when we discuss **references and copying**.

---

# 8.16 Objects Are Reference Values

Consider:

```js
const user1 = {
    name: "Danishan"
};

const user2 = user1;
```

Now:

```text
user1 ──┐
        ├──→ object
user2 ──┘
```

Both variables refer to the same object.

Therefore:

```js
user2.name = "Changed";

console.log(user1.name);
```

produces:

```text
Changed
```

because `user1` and `user2` refer to the same object.

---

# 8.17 Object Equality

Consider:

```js
const a = {
    x: 1
};

const b = {
    x: 1
};

console.log(a === b);
```

Result:

```text
false
```

Why?

Because they are two different objects.

```text
a → Object A
b → Object B
```

Even though:

```text
Object A:
x → 1

Object B:
x → 1
```

their identities differ.

But:

```js
const a = {
    x: 1
};

const b = a;

console.log(a === b);
```

returns:

```text
true
```

because both references point to the same object.

---

# 8.18 Object Copying

This is one of the most important practical topics.

Suppose:

```js
const original = {
    name: "Danishan"
};
```

You want another object with the same properties.

One approach:

```js
const copy = {
    ...original
};
```

Now:

```text
original → Object A
copy     → Object B
```

So:

```js
copy.name = "Changed";
```

doesn't change:

```js
original.name;
```

---

# 8.19 Shallow Copy

But object spread only creates a **shallow copy**.

Consider:

```js
const original = {
    name: "Danishan",

    address: {
        city: "Delhi"
    }
};

const copy = {
    ...original
};
```

The top-level objects are different:

```text
original ──→ Object A
copy     ──→ Object B
```

But the nested `address` object is shared:

```text
Object A
 └── address ──┐
               │
Object B       │
 └── address ──┘
```

Therefore:

```js
copy.address.city = "Mumbai";

console.log(original.address.city);
```

returns:

```text
Mumbai
```

This is the classic **shallow-copy trap**.

---

# 8.20 Why Is It Called Shallow?

Because only the first level is copied.

```text
Original

level 1
   ↓
Object A
 ├── name → primitive
 └── address ─────→ Object C
                         ↓
                       city
```

Spread creates:

```text
Copy

level 1
   ↓
Object B
 ├── name → copied value
 └── address ─────→ Object C
```

The nested object `Object C` is shared.

---

# 8.21 Deep Copy

A deep copy attempts to create independent copies throughout the object graph.

Conceptually:

```text
Original
   │
   └── address → Object C

Deep Copy
   │
   └── address → Object D
```

So changing the nested object in the copy doesn't affect the original.

Modern JavaScript provides:

```js
structuredClone(original);
```

for many data structures.

But "deep clone" isn't a universal operation for every possible JavaScript value. Functions, certain host objects, and other special cases have restrictions or different semantics.

So don't memorize:

> `structuredClone()` clones absolutely everything.

It doesn't.

---

# 8.22 `JSON.parse(JSON.stringify())` Is Not a General Deep Clone

You may see:

```js
const copy = JSON.parse(
    JSON.stringify(original)
);
```

This can work for some simple JSON-compatible data.

But it changes or loses values that JSON cannot represent faithfully.

For example:

```js
const obj = {
    value: undefined,
    date: new Date(),
    fn: () => {}
};
```

JSON serialization doesn't preserve these in their original JavaScript form.

So:

```text
JSON clone
≠
general-purpose JavaScript deep clone
```

For modern JavaScript, `structuredClone()` is generally the better built-in when its supported data model matches your needs.

---

# 8.23 Object Destructuring

Destructuring allows properties to be extracted into variables.

```js
const user = {
    name: "Danishan",
    age: 25
};

const { name, age } = user;
```

Now:

```text
name → "Danishan"
age  → 25
```

This is not copying the object itself.

It is extracting property values into bindings.

---

# 8.24 Renaming During Destructuring

You can rename:

```js
const {
    name: userName
} = user;
```

Now:

```text
userName → "Danishan"
```

The syntax:

```text
propertyName: variableName
```

can initially look confusing.

It means:

```text
read property "name"
        ↓
store it in variable "userName"
```

---

# 8.25 Destructuring Defaults

You can provide defaults:

```js
const {
    name,
    age = 18
} = user;
```

If `age` is `undefined`, the default `18` is used.

Again, this connects to the distinction:

```text
undefined
vs
null
```

If:

```js
age: null
```

the default does not apply.

---

# 8.26 Nested Destructuring

Given:

```js
const user = {
    name: "Danishan",
    address: {
        city: "Delhi"
    }
};
```

you can write:

```js
const {
    address: {
        city
    }
} = user;
```

Now:

```text
city → "Delhi"
```

Nested destructuring is powerful but can become difficult to read if overused.

---

# 8.27 Object Spread

Object spread copies enumerable own properties into a new object.

```js
const user = {
    name: "Danishan",
    age: 25
};

const copy = {
    ...user
};
```

You can also override properties:

```js
const updatedUser = {
    ...user,
    age: 26
};
```

The later property wins.

Conceptually:

```text
...user
    ↓
properties copied

age: 26
    ↓
same key appears later
    ↓
later property overwrites earlier one
```

---

# 8.28 Spread Is Not Deep Copy

This deserves repetition because it is extremely common in interviews:

```js
const copy = {
    ...original
};
```

means:

> Create a new object and copy the relevant own enumerable properties.

It does **not** mean:

> Recursively clone every nested object.

---

# 8.29 Property Descriptors

Now we move into the deeper object model.

A property isn't conceptually just:

```text
key → value
```

JavaScript properties also have **descriptors**.

For a normal data property, important descriptor fields include:

```text
value
writable
enumerable
configurable
```

Example:

```js
const user = {
    name: "Danishan"
};
```

Conceptually, `name` has something like:

```text
value        → "Danishan"
writable     → true
enumerable   → true
configurable → true
```

These attributes control how the property behaves.

---

# 8.30 Inspecting Property Descriptors

You can inspect them:

```js
Object.getOwnPropertyDescriptor(
    user,
    "name"
);
```

You will get a descriptor object containing the property's attributes.

This is very useful when debugging unusual object behavior.

---

# 8.31 `writable`

`writable` determines whether a data property's value can be changed through ordinary assignment.

```js
const user = {};

Object.defineProperty(user, "name", {
    value: "Danishan",
    writable: false
});
```

Then:

```js
user.name = "Changed";
```

doesn't successfully change the value.

The exact observable behavior depends on strictness.

In strict mode, attempting to write to a non-writable property throws a `TypeError`.

---

# 8.32 `enumerable`

`enumerable` determines whether a property participates in common enumeration mechanisms.

For example:

```js
Object.keys(obj);
```

returns the object's own enumerable string-keyed properties.

So:

```js
Object.defineProperty(obj, "secret", {
    value: 123,
    enumerable: false
});
```

means `secret` won't appear in:

```js
Object.keys(obj);
```

even though:

```js
obj.secret
```

can still access it.

---

# 8.33 `configurable`

`configurable` controls whether the property's descriptor can generally be changed and whether the property can be deleted.

For example:

```js
Object.defineProperty(obj, "id", {
    value: 123,
    configurable: false
});
```

This makes the property non-configurable.

A non-configurable property cannot simply be made configurable again.

This is an area where JavaScript's property model becomes significantly deeper than a basic key-value map.

---

# 8.34 Data Properties vs Accessor Properties

JavaScript has two major property descriptor forms.

### Data property

```text
value
writable
enumerable
configurable
```

### Accessor property

```text
get
set
enumerable
configurable
```

This distinction is fundamental.

---

# 8.35 Getters

A getter defines behavior when a property is read.

```js
const user = {
    firstName: "Danishan",
    lastName: "Farookh",

    get fullName() {
        return `${this.firstName} ${this.lastName}`;
    }
};
```

Now:

```js
user.fullName;
```

looks like property access.

But JavaScript actually invokes the getter.

Conceptually:

```text
user.fullName
      ↓
getter executes
      ↓
computed result
```

You don't write:

```js
user.fullName();
```

because `fullName` is an accessor property, not an ordinary method.

---

# 8.36 Setters

A setter defines behavior when a property is assigned.

```js
const user = {
    _name: "",

    set name(value) {
        this._name = value.trim();
    },

    get name() {
        return this._name;
    }
};
```

Now:

```js
user.name = " Danishan ";
```

invokes the setter.

Then:

```js
console.log(user.name);
```

invokes the getter.

Result:

```text
Danishan
```

This pattern can provide validation or normalization.

---

# 8.37 Property Ownership

Given:

```js
const user = {
    name: "Danishan"
};
```

`name` is an **own property** of `user`.

You can test:

```js
Object.hasOwn(user, "name");
```

which returns:

```text
true
```

But an object can also access properties inherited from its prototype.

That distinction becomes critical.

---

# 8.38 Own vs Inherited Properties

Consider:

```js
const user = {
    name: "Danishan"
};
```

`name` is an own property.

But:

```js
user.toString
```

also exists even though we didn't define it directly.

Why?

Because it is inherited through the prototype chain.

Conceptually:

```text
user
 │
 │ own properties
 ├── name
 │
 ↓ prototype
Object.prototype
 ├── toString
 ├── hasOwnProperty
 └── ...
```

This is the beginning of the real JavaScript object model.

---

# 8.39 Prototype

Every ordinary JavaScript object has an internal relationship to another object called its **prototype**.

Think:

```text
object
   │
   ↓
prototype
   │
   ↓
prototype's prototype
   │
   ↓
...
```

This creates the **prototype chain**.

The chain eventually reaches:

```text
null
```

for ordinary objects.

---

# 8.40 Prototype Chain

Consider:

```js
const user = {
    name: "Danishan"
};
```

When you do:

```js
user.toString();
```

JavaScript first checks:

```text
Does user have "toString"?
```

If not, it checks the prototype.

Conceptually:

```text
user
 ↓
Object.prototype
 ↓
null
```

`toString` is found on `Object.prototype`.

Therefore:

```js
user.toString();
```

works even though `user` itself doesn't have an own `toString` property.

---

# 8.41 Property Lookup Through the Prototype Chain

This is crucial.

For:

```js
user.someProperty
```

JavaScript conceptually does:

```text
1. Check user itself
       ↓
2. Not found?
       ↓
3. Check user.prototype
       ↓
4. Not found?
       ↓
5. Continue upward
       ↓
6. Eventually null
       ↓
7. Return undefined
```

This is **property lookup**.

Don't confuse it with variable lookup through the scope chain.

---

# 8.42 Scope Chain vs Prototype Chain

Keep this distinction extremely clear:

### Variable:

```js
console.log(name);
```

uses lexical scope.

```text
current lexical environment
        ↓
outer environment
        ↓
global/module
```

### Property:

```js
user.name;
```

uses object/property lookup.

```text
user
 ↓
prototype
 ↓
prototype's prototype
 ↓
null
```

These are separate mechanisms.

---

# 8.43 `Object.prototype`

Most ordinary objects ultimately inherit from:

```js
Object.prototype
```

For example:

```js
const user = {};
```

Conceptually:

```text
user
 ↓
Object.prototype
 ↓
null
```

`Object.prototype` provides commonly inherited methods such as:

```text
toString
hasOwnProperty
isPrototypeOf
propertyIsEnumerable
```

among others.

---

# 8.44 `__proto__`

You may encounter:

```js
obj.__proto__
```

This is a legacy accessor exposing the object's prototype relationship.

For example:

```js
const user = {};

console.log(user.__proto__ === Object.prototype);
```

For ordinary objects created using `{}`, this is typically `true`.

However, for modern code, prefer:

```js
Object.getPrototypeOf(user);
```

to inspect the prototype, and:

```js
Object.setPrototypeOf(user, prototype);
```

to change it when genuinely necessary.

`__proto__` is important to understand for interviews and existing code, but it should not be your preferred API.

---

# 8.45 `Object.create`

`Object.create()` lets you create an object with a specific prototype.

```js
const personPrototype = {
    greet() {
        console.log("Hello");
    }
};

const person = Object.create(personPrototype);
```

Now:

```text
person
  ↓
personPrototype
  ↓
Object.prototype
  ↓
null
```

Calling:

```js
person.greet();
```

works even though `greet` isn't an own property of `person`.

It is inherited from `personPrototype`.

---

# 8.46 `Object.create(null)`

A particularly interesting case:

```js
const dictionary = Object.create(null);
```

Now the object has no prototype.

Conceptually:

```text
dictionary
   ↓
null
```

Therefore it doesn't inherit:

```text
toString
hasOwnProperty
...
```

from `Object.prototype`.

This can be useful for dictionary-like structures where you want only explicitly stored keys.

But it also means:

```js
dictionary.hasOwnProperty
```

doesn't exist.

Use:

```js
Object.hasOwn(dictionary, key);
```

instead.

---

# 8.47 Constructor Functions

Before classes became common, constructor functions were a standard way to implement reusable object creation.

Example:

```js
function User(name) {
    this.name = name;
}
```

Then:

```js
const user = new User("Danishan");
```

The function is being used as a constructor.

This introduces the `new` operator.

---

# 8.48 What Does `new` Do?

This is a very high-value SDE-2 interview question.

Consider:

```js
function User(name) {
    this.name = name;
}

const user = new User("Danishan");
```

Conceptually, `new` performs several important steps.

### Step 1 — Create a new object

```text
new object
```

### Step 2 — Connect its prototype

The new object's prototype is set to:

```js
User.prototype
```

### Step 3 — Call the constructor with `this`

Conceptually:

```js
User.call(newObject, "Danishan");
```

### Step 4 — Return the resulting object

Assuming the constructor doesn't explicitly return another suitable object.

Conceptually:

```text
new User("Danishan")

       ↓

new object
       │
       ├── name → "Danishan"
       │
       └── [[Prototype]]
                ↓
           User.prototype
```

---

# 8.49 Constructor Prototype

Every normal constructor function has a `prototype` property.

```js
function User(name) {
    this.name = name;
}
```

You can add methods to:

```js
User.prototype
```

For example:

```js
User.prototype.greet = function () {
    console.log(`Hello ${this.name}`);
};
```

Then:

```js
const user1 = new User("Danishan");
const user2 = new User("Alex");

user1.greet();
user2.greet();
```

Both objects share the same method through the prototype.

Conceptually:

```text
user1 ──┐
        │
user2 ──┼──→ User.prototype
        │          │
        │          └── greet
        │
        └── own name
```

This is a major reason prototypes are useful.

---

# 8.50 `User.prototype` vs Object's Prototype

This distinction causes huge confusion.

Consider:

```js
function User() {}

const user = new User();
```

There are two different things:

```text
User.prototype
```

and:

```js
Object.getPrototypeOf(user)
```

For the instance:

```text
Object.getPrototypeOf(user) === User.prototype
```

is true.

So:

```text
User
 │
 └── prototype → User.prototype
                         ↑
                         │
user ────────────────────┘
```

But `User.prototype` itself is an object, and it has its own prototype:

```text
User.prototype
      ↓
Object.prototype
      ↓
null
```

This distinction is essential.

---

# 8.51 Prototype Property vs Prototype Relationship

Another classic confusion:

```js
User.prototype
```

is a **property on the constructor function**.

Whereas:

```js
Object.getPrototypeOf(user)
```

asks:

> What object does `user` inherit from?

So:

```text
User.prototype
        ↓
property on User function

[[Prototype]] of user
        ↓
internal prototype relationship
```

They are related through `new`, but conceptually they aren't the same thing.

---

# 8.52 Prototype Inheritance

Suppose:

```js
function Animal(name) {
    this.name = name;
}

Animal.prototype.speak = function () {
    console.log(`${this.name} makes a sound`);
};
```

Now:

```js
function Dog(name) {
    Animal.call(this, name);
}
```

To establish prototype inheritance:

```js
Dog.prototype = Object.create(
    Animal.prototype
);
```

Then:

```js
Dog.prototype.constructor = Dog;
```

Now:

```text
dog
 ↓
Dog.prototype
 ↓
Animal.prototype
 ↓
Object.prototype
 ↓
null
```

So a `Dog` instance can access methods from both:

```text
Dog.prototype
Animal.prototype
```

through the prototype chain.

---

# 8.53 Prototype Chain as Inheritance

The key idea is:

```text
child object
     ↓
child prototype
     ↓
parent prototype
     ↓
Object.prototype
     ↓
null
```

When a property isn't found at one level, JavaScript continues upward.

This is how prototype-based inheritance works.

---

# 8.54 Classes Are Still Prototype-Based

Modern JavaScript gives us:

```js
class User {
    constructor(name) {
        this.name = name;
    }

    greet() {
        console.log(`Hello ${this.name}`);
    }
}
```

Then:

```js
const user = new User("Danishan");
```

The syntax looks class-oriented.

But the underlying object model still uses prototypes.

The method:

```js
greet() {}
```

is placed on:

```text
User.prototype
```

rather than copied separately onto every instance.

So conceptually:

```text
user
 ↓
User.prototype
 ├── greet
 ↓
Object.prototype
 ↓
null
```

This is extremely important for understanding JavaScript deeply.

---

# 8.55 Object Immutability

JavaScript provides several levels of restricting object mutation.

The most familiar is:

```js
Object.freeze(obj);
```

Example:

```js
const user = {
    name: "Danishan"
};

Object.freeze(user);
```

Now properties cannot normally be:

* changed
* added
* removed

But remember:

> `Object.freeze()` is shallow.

---

# 8.56 Shallow Freeze

Consider:

```js
const user = {
    name: "Danishan",

    address: {
        city: "Delhi"
    }
};

Object.freeze(user);
```

You cannot normally do:

```js
user.name = "Changed";
```

But:

```js
user.address.city = "Mumbai";
```

can still mutate the nested object.

Why?

Because:

```text
user
 ├── name → primitive
 └── address ──→ nested object
```

`Object.freeze(user)` freezes the top-level object, not recursively every nested object.

---

# 8.57 `Object.seal`

Another option:

```js
Object.seal(obj);
```

A sealed object cannot have properties added or removed, but existing writable data properties can still be changed.

Think:

```text
freeze
 ├── cannot add
 ├── cannot delete
 └── cannot modify values

seal
 ├── cannot add
 ├── cannot delete
 └── can modify existing writable values
```

Both are shallow.

---

# 8.58 `Object.preventExtensions`

The weakest restriction:

```js
Object.preventExtensions(obj);
```

This prevents adding new properties.

Existing properties can generally still be:

* modified
* deleted

subject to their descriptors.

So:

```text
preventExtensions
    ↓
cannot add

seal
    ↓
cannot add/delete

freeze
    ↓
cannot add/delete/modify writable data properties
```

---

# 8.59 A Complete Object Mental Model

At this point, don't think:

```text
object = key-value map
```

Think:

```text
                         OBJECT
                           │
          ┌────────────────┼────────────────┐
          ↓                ↓                ↓
      own properties    descriptors      prototype
          │                │                │
          ↓                ↓                ↓
       key/value      writable/etc.    prototype chain
          │                                 │
          ↓                                 ↓
   ┌──────┴──────┐                    Object.prototype
   ↓             ↓                           │
 data         accessor                       ↓
property      property                      null
   │
   ├── value
   └── writable
```

This is much closer to the actual JavaScript object model.

---

# 8.60 Property Lookup vs Variable Lookup

This deserves a final emphasis because it combines Sections 7 and 8.

Given:

```js
const name = "global";

const user = {
    name: "Danishan"
};

console.log(name);
console.log(user.name);
```

The first:

```js
name
```

uses:

```text
lexical environment
 ↓
scope chain
```

The second:

```js
user.name
```

uses:

```text
user
 ↓
own properties
 ↓
prototype chain
```

So:

```text
identifier lookup
      ≠
property lookup
```

This distinction will save you from many confusing interview questions.

---

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

---

# 8.62 The SDE-2 Mental Model

If you remember only one architecture from this topic, remember this:

```text
                    JavaScript Object
                           │
              ┌────────────┴────────────┐
              ↓                         ↓
       Own Properties               Prototype
              │                         │
       ┌──────┴──────┐                  ↓
       ↓             ↓             Prototype Object
     Data         Accessor              │
   Property       Property              ↓
       │             │             Next Prototype
       │             │                  │
       ↓             ↓                  ↓
 value/writable    get/set           ...
 enumerable
 configurable
```

And property lookup:

```text
obj.property
     │
     ↓
Does obj have own property?
     │
   yes → use it
     │
    no
     ↓
Check prototype
     │
   found → use it
     │
    no
     ↓
Continue prototype chain
     │
     ↓
null
     │
     ↓
undefined
```

While variable lookup is completely different:

```text
variable
   ↓
current lexical environment
   ↓
outer lexical environment
   ↓
outer lexical environment
   ↓
global/module
   ↓
ReferenceError
```

**That distinction—scope chain for variables, prototype chain for properties—is probably the single most important conceptual connection between Sections 7 and 8.**

For SDE-2, the next level of difficulty is not memorizing `Object.create()` or `new`. It is being able to **trace an unfamiliar object through its own properties, descriptors, prototype chain, constructor, and method invocation and predict exactly what JavaScript does.**
