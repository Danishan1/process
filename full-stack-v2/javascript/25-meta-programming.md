# 23. Metaprogramming & Reflection — SDE-II

This is one of the more advanced JavaScript topics.

The easiest way to understand it is:

> **Normal programming manipulates data. Metaprogramming manipulates how objects/programs behave.**

For example, normally you write:

```js
user.name
```

With metaprogramming, you can intercept what happens when someone does:

```js
user.name
```

That's where **`Proxy`** comes in.

---

# 23.1 What Is Metaprogramming?

Consider normal code:

```js
const user = {
    name: "John"
};

console.log(user.name);
```

You're interacting with the object.

Metaprogramming lets you control operations such as:

```text
property read
property write
property deletion
property existence checks
function calls
object construction
property enumeration
prototype operations
```

Conceptually:

```text
Normal programming

application
    ↓
object
    ↓
data


Metaprogramming

application
    ↓
operation
    ↓
interception/custom behavior
    ↓
object
```

JavaScript provides several mechanisms for this:

```text
Proxy
Reflect
Symbols
Property descriptors
Getters/setters
Object.create()
Object.defineProperty()
```

---

# 23.2 `Proxy`

`Proxy` allows you to create an object that **intercepts operations performed on another object**.

Basic example:

```js
const user = {
    name: "John"
};

const proxy = new Proxy(user, {
    get(target, property) {
        console.log(`Reading ${property}`);
        return target[property];
    }
});

proxy.name;
```

When:

```js
proxy.name
```

runs:

```text
proxy
  ↓
get trap
  ↓
target[property]
  ↓
"John"
```

The `get` trap intercepted the property access.

---

# 23.3 Target vs Proxy

This distinction is important.

```js
const user = {
    name: "John"
};

const proxy = new Proxy(user, handler);
```

There are now two concepts:

```text
user
 ↓
target object

proxy
 ↓
object through which operations are intercepted
```

The Proxy doesn't necessarily contain a copy of the target.

It acts as an intermediary.

---

# 23.4 Proxy Traps

A Proxy can intercept many operations.

Important traps include:

| Trap                       | Intercepts                 |
| -------------------------- | -------------------------- |
| `get`                      | property reading           |
| `set`                      | property assignment        |
| `has`                      | `in` operator              |
| `deleteProperty`           | `delete`                   |
| `ownKeys`                  | property enumeration       |
| `getOwnPropertyDescriptor` | property descriptor lookup |
| `defineProperty`           | property definition        |
| `getPrototypeOf`           | prototype lookup           |
| `setPrototypeOf`           | prototype changes          |
| `isExtensible`             | extensibility check        |
| `preventExtensions`        | prevent extensions         |
| `apply`                    | function call              |
| `construct`                | `new`                      |

You don't need to memorize every trap.

Understand the pattern:

> **A Proxy lets you intercept fundamental object operations.**

---

# 23.5 `get` Trap

```js
const user = {
    name: "John",
    age: 30
};

const proxy = new Proxy(user, {
    get(target, property) {
        console.log("GET:", property);

        return target[property];
    }
});

proxy.name;
proxy.age;
```

Conceptually:

```text
proxy.name
    ↓
get(target, "name")
    ↓
target.name
```

---

# 23.6 `set` Trap

You can intercept writes:

```js
const user = {};

const proxy = new Proxy(user, {
    set(target, property, value) {
        console.log("SET:", property, value);

        target[property] = value;

        return true;
    }
});

proxy.name = "John";
```

Now the Proxy sees:

```text
SET name John
```

---

# 23.7 Why Does `set` Need to Return `true`?

A `set` trap is expected to indicate whether the assignment succeeded.

```js
set(target, property, value) {
    target[property] = value;
    return true;
}
```

If you incorrectly return `false` in strict-mode code, the assignment can result in a `TypeError`.

So the trap isn't simply a callback.

It participates in the semantics of the operation.

---

# 23.8 Proxy for Validation

A practical example:

```js
const user = {};

const validatedUser = new Proxy(user, {
    set(target, property, value) {
        if (property === "age") {
            if (!Number.isInteger(value) || value < 0) {
                throw new Error("Invalid age");
            }
        }

        target[property] = value;
        return true;
    }
});
```

Now:

```js
validatedUser.age = 30;
```

works.

But:

```js
validatedUser.age = -10;
```

throws.

This gives you runtime enforcement.

---

# 23.9 Proxy for Logging

Another useful example:

```js
const service = {
    createUser() {
        // ...
    },

    deleteUser() {
        // ...
    }
};

const loggedService = new Proxy(service, {
    get(target, property, receiver) {
        console.log(`Accessing ${String(property)}`);

        return Reflect.get(target, property, receiver);
    }
});
```

This can be useful for:

```text
logging
metrics
tracing
debugging
instrumentation
```

But in production, you should be careful about adding Proxy overhead to hot paths.

---

# 23.10 `Reflect`

`Reflect` provides standard functions for performing many low-level object operations.

For example:

```js
Reflect.get(user, "name");
```

is conceptually similar to:

```js
user.name;
```

And:

```js
Reflect.set(user, "name", "Ali");
```

is conceptually similar to:

```js
user.name = "Ali";
```

---

# 23.11 Why `Reflect` Exists

One of the biggest practical reasons:

> **`Reflect` provides a consistent way to perform the operation that a Proxy trap intercepted.**

Consider:

```js
const proxy = new Proxy(user, {
    get(target, property, receiver) {
        return Reflect.get(target, property, receiver);
    }
});
```

This is an extremely common pattern.

Think:

```text
Proxy
  ↓
intercept operation
  ↓
custom logic
  ↓
Reflect
  ↓
perform normal operation
```

---

# 23.12 Why Pass `receiver`?

This is a more advanced SDE-II detail.

Consider:

```js
const proxy = new Proxy(target, {
    get(target, property, receiver) {
        return Reflect.get(target, property, receiver);
    }
});
```

The third argument, `receiver`, matters for correct behavior with:

* getters
* inheritance
* `this`

For example:

```js
const target = {
    get name() {
        return this._name;
    },

    _name: "John"
};
```

Using:

```js
Reflect.get(target, "name", receiver);
```

ensures the getter's `this` can refer to the receiver rather than blindly forcing the target.

You don't need to memorize the mechanics, but understand:

> **`receiver` preserves normal JavaScript property-access semantics when Proxy is involved.**

---

# 23.13 Proxy + Reflect — Standard Pattern

You'll frequently see:

```js
const proxy = new Proxy(target, {
    get(target, property, receiver) {
        // custom behavior

        return Reflect.get(
            target,
            property,
            receiver
        );
    },

    set(target, property, value, receiver) {
        // custom behavior

        return Reflect.set(
            target,
            property,
            value,
            receiver
        );
    }
});
```

This is a very useful pattern to recognize in production code.

---

# 23.14 Dynamic Property Access

JavaScript supports dynamic property access.

Instead of:

```js
user.name
```

you can write:

```js
const property = "name";

user[property];
```

This is called **computed/dynamic property access**.

For example:

```js
function getProperty(object, property) {
    return object[property];
}
```

Now:

```js
getProperty(user, "name");
getProperty(user, "age");
```

---

# 23.15 Dot vs Bracket Notation

### Dot notation

```js
user.name
```

The property is known statically.

### Bracket notation

```js
user[property]
```

The property can be dynamic.

This is important in:

```text
configuration systems
serialization
database mapping
generic utilities
API response transformation
form handling
dynamic schemas
```

---

# 23.16 Computed Property Names

You can also dynamically create properties.

```js
const field = "username";

const user = {
    [field]: "john"
};
```

Equivalent conceptually to:

```js
const user = {};

user[field] = "john";
```

This is another form of runtime object manipulation.

---

# 23.17 Getters

A getter allows property access to execute a function.

```js
const user = {
    firstName: "John",
    lastName: "Doe",

    get fullName() {
        return `${this.firstName} ${this.lastName}`;
    }
};
```

Now:

```js
user.fullName;
```

looks like property access but executes code.

This is the important distinction:

```text
user.fullName
     ↓
looks like property
     ↓
actually invokes getter
```

---

# 23.18 Setters

Similarly:

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
user.name = "  John  ";
```

runs the setter.

Then:

```js
user.name;
```

runs the getter.

---

# 23.19 Getter/Setter vs Method

Compare:

```js
user.getFullName();
```

with:

```js
user.fullName;
```

A getter is useful when the operation conceptually behaves like a property.

But don't hide expensive work behind property syntax.

For example, this would be misleading:

```js
get recommendations() {
    // 5 database queries
}
```

A method or explicit service call communicates the cost better.

---

# 23.20 Property Descriptors

Every object property has a descriptor.

You can inspect it:

```js
const user = {
    name: "John"
};

Object.getOwnPropertyDescriptor(user, "name");
```

For a normal data property, you'll see concepts such as:

```text
value
writable
enumerable
configurable
```

---

# 23.21 Data Property Descriptor

Conceptually:

```js
{
    value: "John",
    writable: true,
    enumerable: true,
    configurable: true
}
```

These control property behavior.

---

# 23.22 `writable`

Controls whether the property value can be changed.

```js
Object.defineProperty(user, "name", {
    writable: false
});
```

Then:

```js
user.name = "Ali";
```

cannot successfully modify the property under strict semantics.

---

# 23.23 `enumerable`

Controls whether the property appears in common enumeration mechanisms.

For example:

```js
Object.keys(user);
```

only includes enumerable own string properties.

This is useful when creating internal metadata that shouldn't normally appear in enumeration.

---

# 23.24 `configurable`

Controls whether the property's descriptor can generally be changed and whether the property can be deleted.

```js
Object.defineProperty(user, "id", {
    configurable: false
});
```

This makes the property harder to redefine/delete.

---

# 23.25 Accessor Descriptors

Getters/setters use a different descriptor shape:

```js
{
    get: function,
    set: function,
    enumerable: true,
    configurable: true
}
```

You generally don't combine:

```text
value/writable
```

with:

```text
get/set
```

for the same descriptor.

That's a useful interview edge case.

---

# 23.26 `Object.defineProperty()`

You can create properties with specific behavior.

```js
Object.defineProperty(user, "id", {
    value: 123,
    writable: false,
    enumerable: false,
    configurable: false
});
```

Now:

```js
user.id;
```

works, but:

```js
Object.keys(user);
```

won't normally show `id`.

This is runtime object manipulation.

---

# 23.27 Symbols Again

Symbols belong in this chapter because they enable metaprogramming.

For example:

```js
const metadata = Symbol("metadata");

const user = {
    name: "John",
    [metadata]: {
        source: "internal"
    }
};
```

You can also use well-known Symbols to customize language behavior.

For example:

```js
Symbol.iterator
```

controls how an object participates in iteration.

---

# 23.28 `Symbol.toPrimitive`

JavaScript can ask an object:

> "How should you behave when converted to a primitive?"

Example:

```js
const money = {
    value: 100,

    [Symbol.toPrimitive](hint) {
        if (hint === "number") {
            return this.value;
        }

        return `$${this.value}`;
    }
};
```

Now the object can customize its primitive conversion behavior.

This is advanced metaprogramming.

You should **recognize the mechanism**, not necessarily use it frequently.

---

# 23.29 Runtime Object Manipulation

JavaScript lets you modify object behavior at runtime.

Examples:

```js
Object.defineProperty()
Object.defineProperties()
Object.getOwnPropertyDescriptor()
Object.getOwnPropertyDescriptors()
Object.getPrototypeOf()
Object.setPrototypeOf()
Object.preventExtensions()
Object.seal()
Object.freeze()
```

You should understand the difference between:

```text
extensible
sealed
frozen
```

at a conceptual level.

---

# 23.30 `Object.freeze()` Isn't Deep Freeze

This is another important edge case.

```js
const user = {
    name: "John",
    address: {
        city: "Delhi"
    }
};

Object.freeze(user);
```

This freezes the top-level object.

But:

```js
user.address.city = "Mumbai";
```

can still modify the nested object because the freeze isn't recursive.

This connects back to:

* references
* immutability
* structured cloning

from earlier chapters.

---

# 23.31 Proxy for Read-Only Objects

You can enforce behavior with Proxy.

```js
const config = {
    port: 3000
};

const readonlyConfig = new Proxy(config, {
    set() {
        throw new Error("Config is read-only");
    }
});
```

Now:

```js
readonlyConfig.port = 4000;
```

throws.

This is an example of metaprogramming enforcing an abstraction.

---

# 23.32 Proxy for Missing Properties

You can provide fallback behavior:

```js
const defaults = new Proxy(
    {},
    {
        get(target, property) {
            return `Missing: ${String(property)}`;
        }
    }
);
```

Then:

```js
defaults.username;
```

returns:

```text
"Missing: username"
```

This is powerful, but it can also hide bugs.

For production systems, **unexpected property access should usually fail clearly rather than silently inventing values**.

---

# 23.33 Proxy for Access Control

Conceptually:

```js
const secured = new Proxy(user, {
    get(target, property, receiver) {
        if (property === "password") {
            throw new Error("Access denied");
        }

        return Reflect.get(target, property, receiver);
    }
});
```

This demonstrates a use case.

But don't confuse this with a complete security boundary.

A Proxy is an application-level mechanism, not a replacement for:

* authentication
* authorization
* database access controls
* process isolation

---

# 23.34 Proxy for Change Tracking

A Proxy can detect mutations:

```js
const state = new Proxy(
    {
        count: 0
    },
    {
        set(target, property, value, receiver) {
            console.log(
                `${String(property)} changed`
            );

            return Reflect.set(
                target,
                property,
                value,
                receiver
            );
        }
    }
);
```

Now:

```js
state.count = 10;
```

can trigger:

```text
count changed
```

This general concept is used by some state-management and reactive systems.

---

# 23.35 Metaprogramming in Frameworks

This topic becomes much more practical when you understand why frameworks use it.

Potential uses include:

```text
ORM models
reactive state
validation
logging
instrumentation
dependency injection
mocking
access control
lazy loading
change detection
API clients
configuration systems
```

For example, an ORM might conceptually intercept:

```js
user.name
```

or:

```js
user.save()
```

to attach behavior around database operations.

The exact implementation varies by framework.

---

# 23.36 Proxy Performance

Proxy isn't free.

Instead of:

```text
property access
    ↓
object
```

you now potentially have:

```text
property access
    ↓
Proxy
    ↓
trap
    ↓
custom logic
    ↓
Reflect
    ↓
object
```

Therefore:

> **Don't put Proxy-based abstractions on extremely hot paths without understanding their performance characteristics.**

For normal application-level usage, it's often perfectly reasonable.

---

# 23.37 Proxy Invariants

This is a more advanced concept worth knowing exists.

A Proxy can't arbitrarily lie about certain properties of the target.

JavaScript enforces **Proxy invariants**.

For example, if the target has a non-configurable, non-writable property with a fixed value, a `get` trap generally cannot report a conflicting value.

Why?

Because otherwise the Proxy could violate fundamental object semantics.

Think:

```text
Target constraints
       ↓
must remain consistent
       ↓
Proxy cannot violate certain invariants
```

You don't need to memorize every invariant for SDE-II.

---

# 23.38 A Very Important Distinction

### Proxy

> **Intercepts operations.**

### Reflect

> **Performs standard object operations programmatically.**

### Property descriptors

> **Define the behavior/attributes of properties.**

### Getters/setters

> **Customize property reads/writes.**

### Symbols

> **Provide unique keys and hooks for language-level behavior.**

That gives you the whole chapter.

---

# 23.39 Putting Them Together

Imagine a configuration system:

```js
const config = {
    port: 3000
};

const validatedConfig = new Proxy(config, {
    get(target, property, receiver) {
        console.log(`Reading ${String(property)}`);

        return Reflect.get(
            target,
            property,
            receiver
        );
    },

    set(target, property, value, receiver) {
        if (property === "port") {
            if (!Number.isInteger(value)) {
                throw new TypeError(
                    "Port must be an integer"
                );
            }
        }

        return Reflect.set(
            target,
            property,
            value,
            receiver
        );
    }
});
```

Now you've combined:

```text
Proxy
  ↓
interception

Reflect
  ↓
normal operation

Dynamic property
  ↓
property variable

Runtime validation
  ↓
custom behavior
```

That's a realistic metaprogramming pattern.

---

# 23.40 When Should You Use Metaprogramming?

Good use cases:

```text
✓ instrumentation
✓ generic libraries
✓ reactive systems
✓ ORM infrastructure
✓ validation infrastructure
✓ controlled abstraction layers
✓ framework internals
```

Be cautious with:

```text
⚠ business logic everywhere
⚠ hidden behavior
⚠ performance-critical loops
⚠ security boundaries
⚠ extremely dynamic APIs
```

The biggest engineering problem isn't that Proxy is complicated.

It's that **metaprogramming can make ordinary code behave in non-obvious ways**.

---

# 23.41 SDE-II Interview Questions

You should be able to answer these.

### 1. What is a Proxy?

> An object wrapper that can intercept fundamental operations performed on a target object through traps such as `get`, `set`, `has`, `deleteProperty`, `apply`, and `construct`.

---

### 2. Why use Reflect inside Proxy traps?

> Reflect provides the standard semantics for the operation and, particularly with the receiver argument, helps preserve normal behavior involving getters, inheritance, and `this`.

---

### 3. Proxy vs getter?

```text
Getter
    ↓
specific property

Proxy
    ↓
can intercept many object operations
```

---

### 4. What are property descriptors?

> Metadata describing how an object property behaves, including value/writability/enumerability/configurability for data properties, or getter/setter behavior for accessor properties.

---

### 5. What is dynamic property access?

```js
object[property]
```

where `property` is evaluated at runtime.

---

### 6. Why use Symbols?

> To create unique property keys and to participate in well-known language protocols such as iteration.

---

# 23.42 Priority

For your SDE-II revision:

### 🔴 Must Know

```text
Proxy
    get
    set

Reflect
    get
    set

Dynamic property access
Getters/setters
Property descriptors
Symbols
Object.defineProperty()
```

### 🟡 Should Know

```text
Proxy:
    has
    deleteProperty
    ownKeys
    apply
    construct

Reflect methods
Symbol.iterator
Symbol.toPrimitive
Object.freeze/seal/preventExtensions
```

### 🟢 Conceptual

```text
Proxy invariants
Advanced well-known Symbols
Framework-level metaprogramming internals
```

---

# 23.43 Final Mental Model

```text
                 METAPROGRAMMING
                       │
          "Control how JS behaves"
                       │
       ┌───────────────┼────────────────┐
       │               │                │
     Proxy          Reflect          Descriptors
       │               │                │
   intercept        perform          property
   operations       operations        behavior
       │               │                │
       └───────────────┼────────────────┘
                       │
              Runtime Object Model
                       │
          ┌────────────┴────────────┐
          │                         │
      Symbols                 Getters/Setters
          │                         │
   language protocols          custom access
          │
          └─────────────┐
                        ↓
               Dynamic Behavior
                        ↓
          Framework / Library Infrastructure
```

The key SDE-II distinction is:

> **`Proxy` changes/intercepts behavior, `Reflect` gives you programmatic access to standard object operations, descriptors define property behavior, getters/setters customize individual properties, and Symbols provide unique keys and language-level hooks.**

And one final engineering principle:

> **Metaprogramming is powerful precisely because it can make ordinary syntax behave differently. Use that power at infrastructure/abstraction boundaries, but avoid making core business logic mysteriously dynamic.**
