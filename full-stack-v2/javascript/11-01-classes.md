# 9. Classes & Object-Oriented JavaScript

This is the natural continuation of **Objects & Object Model**.

The important thing for SDE-2 is that we should **not study `class` as an isolated syntax feature**. We already learned that JavaScript's object model is fundamentally prototype-based. Classes provide a cleaner abstraction over that model.

The mental progression is:

```text
Objects
   ↓
Prototypes
   ↓
Constructor functions
   ↓
new
   ↓
class syntax
   ↓
Inheritance
   ↓
Encapsulation / Polymorphism
   ↓
Composition vs Inheritance
```

---

# 9.1 What Is a Class?

A class is a mechanism for defining how objects should be created and what behavior they should have.

Example:

```js
class User {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }

    greet() {
        console.log(`Hello, I'm ${this.name}`);
    }
}
```

We can create instances:

```js
const user1 = new User("Danishan", 25);
const user2 = new User("Alex", 30);
```

Conceptually:

```text
User
 │
 └── prototype
       └── greet()

user1
 ├── name → "Danishan"
 └── age  → 25
       │
       ↓
 User.prototype
       │
       ↓
 Object.prototype
       │
       ↓
 null
```

So even though we write:

```js
class User {}
```

the resulting objects still participate in the **prototype chain**.

---

# 9.2 Class Declaration

Basic syntax:

```js
class User {
    constructor(name) {
        this.name = name;
    }
}
```

Then:

```js
const user = new User("Danishan");
```

The class itself is a value:

```js
console.log(typeof User);
```

Result:

```text
"function"
```

This surprises people initially.

A JavaScript class is not a completely separate runtime category from functions.

However, classes have special semantics compared with ordinary functions.

---

# 9.3 Constructor

The `constructor()` method runs when an instance is created with `new`.

```js
class User {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
}
```

Then:

```js
const user = new User("Danishan", 25);
```

The constructor receives:

```text
name → "Danishan"
age  → 25
```

and initializes:

```text
user
 ├── name → "Danishan"
 └── age  → 25
```

A class can have only one constructor definition.

---

# 9.4 Constructor Is Not the Prototype

This distinction is important.

Consider:

```js
class User {
    constructor(name) {
        this.name = name;
    }

    greet() {
        console.log("Hello");
    }
}
```

There are two different things:

```text
User
 │
 ├── constructor behavior
 │
 └── prototype
       └── greet
```

When:

```js
const user = new User("Danishan");
```

the instance gets its own:

```text
name
```

while `greet` is available through:

```text
User.prototype
```

---

# 9.5 Instance Properties

Properties assigned through `this` belong to the instance.

```js
class User {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
}
```

For:

```js
const user = new User("Danishan", 25);
```

the object contains:

```text
user
 ├── name → "Danishan"
 └── age  → 25
```

These are instance properties.

Each instance gets its own values.

```js
const a = new User("A", 20);
const b = new User("B", 30);
```

Conceptually:

```text
a
 ├── name → "A"
 └── age  → 20

b
 ├── name → "B"
 └── age  → 30
```

---

# 9.6 Instance Methods

Methods declared inside a class body are generally placed on the class's prototype.

```js
class User {
    greet() {
        console.log("Hello");
    }
}
```

Conceptually:

```text
User.prototype
 └── greet → function
```

Therefore:

```js
const user1 = new User();
const user2 = new User();
```

both can use:

```js
user1.greet();
user2.greet();
```

without requiring a separate `greet` function to be created for each instance.

This is an important connection to our previous prototype discussion.

---

# 9.7 Why Not Define Methods in the Constructor?

You could technically do:

```js
class User {
    constructor(name) {
        this.name = name;

        this.greet = function () {
            console.log(`Hello ${this.name}`);
        };
    }
}
```

But now every instance receives its own function:

```text
user1
 └── greet → Function A

user2
 └── greet → Function B
```

Whereas:

```js
class User {
    greet() {
        console.log(`Hello ${this.name}`);
    }
}
```

results conceptually in:

```text
user1 ──┐
        ├──→ User.prototype.greet
user2 ──┘
```

This is one reason prototype methods are generally preferable when shared behavior is desired.

---

# 9.8 Static Methods

An instance method is called on an instance:

```js
user.greet();
```

A static method belongs to the class itself.

```js
class User {
    static createGuest() {
        return new User("Guest");
    }
}
```

Now:

```js
const user = User.createGuest();
```

But:

```js
user.createGuest();
```

doesn't work.

The distinction:

```text
instance method
        ↓
user.greet()

static method
        ↓
User.createGuest()
```

---

# 9.9 Why Use Static Methods?

Static methods are useful when the behavior logically belongs to the class rather than to one particular instance.

For example:

```js
class MathUtils {
    static add(a, b) {
        return a + b;
    }
}
```

Usage:

```js
MathUtils.add(10, 20);
```

There is no need to create:

```js
new MathUtils();
```

---

# 9.10 Static Properties

JavaScript also supports static fields.

```js
class User {
    static role = "user";
}
```

Access:

```js
console.log(User.role);
```

But:

```js
const user = new User();

console.log(user.role);
```

doesn't access that static property.

Think:

```text
User
 └── static role

user
 └── instance properties
```

Static members belong to the class constructor object.

---

# 9.11 Instance vs Static

This is worth memorizing conceptually:

```text
                    User
                     │
          ┌──────────┴──────────┐
          ↓                     ↓
      static members       prototype
          │                     │
          ↓                     ↓
      User.create()        greet()
                                │
                                ↓
                              user
                         instance properties
```

So:

```js
User.create();
```

and:

```js
user.greet();
```

operate at different levels.

---

# 9.12 Private Fields

Modern JavaScript supports private class fields using `#`.

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

Now:

```js
const account = new BankAccount();

account.deposit(100);

console.log(account.getBalance());
```

works.

But:

```js
account.#balance;
```

is a syntax error outside the class body.

The `#` is important.

This is not merely a naming convention.

Compare:

```js
this._balance
```

with:

```js
this.#balance
```

`_balance` is just a conventional name.

`#balance` is an actual private field.

---

# 9.13 Private Fields vs Closures

There are two different ways to hide internal state.

### Closure

```js
function createCounter() {
    let count = 0;

    return {
        increment() {
            count++;
        },

        getCount() {
            return count;
        }
    };
}
```

`count` is hidden through lexical scoping.

### Private class field

```js
class Counter {
    #count = 0;

    increment() {
        this.#count++;
    }

    getCount() {
        return this.#count;
    }
}
```

Here privacy is provided by the class's private-field semantics.

This connects directly to our previous **Scope & Closures** topic.

---

# 9.14 Getters and Setters in Classes

Classes can define accessors:

```js
class User {
    constructor(firstName, lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
    }

    get fullName() {
        return `${this.firstName} ${this.lastName}`;
    }

    set fullName(value) {
        const [firstName, lastName] = value.split(" ");

        this.firstName = firstName;
        this.lastName = lastName;
    }
}
```

Now:

```js
const user = new User("Danishan", "Farookh");

console.log(user.fullName);
```

The getter executes.

And:

```js
user.fullName = "John Doe";
```

invokes the setter.

Again:

```text
user.fullName
```

looks like ordinary property access, but behavior is implemented through an accessor.

---

# 9.15 Class Inheritance

JavaScript supports inheritance through:

```js
extends
```

Example:

```js
class Animal {
    speak() {
        console.log("Animal sound");
    }
}

class Dog extends Animal {
}
```

Now:

```js
const dog = new Dog();

dog.speak();
```

works.

Conceptually:

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

This is prototype inheritance expressed through class syntax.

---

# 9.16 `extends`

When:

```js
class Dog extends Animal {}
```

is used, JavaScript establishes inheritance relationships between the relevant constructors/prototypes.

Conceptually:

```text
Dog.prototype
      ↓
Animal.prototype
      ↓
Object.prototype
```

and the class constructors themselves also participate in an inheritance relationship.

So `extends` is more than simply:

> Copy all methods from Animal to Dog.

It establishes a prototype-based inheritance structure.

---

# 9.17 `super`

`super` allows a derived class to interact with its parent class.

Example:

```js
class Animal {
    constructor(name) {
        this.name = name;
    }

    speak() {
        console.log(`${this.name} makes a sound`);
    }
}

class Dog extends Animal {
    constructor(name) {
        super(name);
    }
}
```

Here:

```js
super(name);
```

invokes the parent constructor.

---

# 9.18 Why Must `super()` Be Called?

In a derived constructor:

```js
class Dog extends Animal {
    constructor(name) {
        this.name = name;
    }
}
```

this causes an error.

Why?

Because in a derived constructor, `this` isn't initialized until the parent constructor has been invoked.

So:

```js
super(name);
```

must happen before using `this`.

Conceptually:

```text
Dog constructor starts
        ↓
this not initialized yet
        ↓
super(...)
        ↓
parent constructor
        ↓
this becomes usable
```

This is a very common interview question.

---

# 9.19 `super.method()`

`super` isn't only for constructors.

Suppose:

```js
class Animal {
    speak() {
        console.log("Animal sound");
    }
}

class Dog extends Animal {
    speak() {
        super.speak();

        console.log("Woof");
    }
}
```

Now:

```js
const dog = new Dog();

dog.speak();
```

produces:

```text
Animal sound
Woof
```

The child method overrides the parent method while still being able to invoke the parent implementation.

---

# 9.20 Method Overriding

A child class can provide its own implementation:

```js
class Animal {
    speak() {
        console.log("Some sound");
    }
}

class Dog extends Animal {
    speak() {
        console.log("Woof");
    }
}
```

Now:

```js
const dog = new Dog();

dog.speak();
```

uses:

```text
Dog.prototype.speak
```

rather than:

```text
Animal.prototype.speak
```

because property lookup finds the child method first.

This is an excellent example of how **method overriding is actually connected to prototype lookup**.

---

# 9.21 Polymorphism

Polymorphism means that different objects can respond to the same operation differently.

```js
class Animal {
    speak() {
        console.log("Animal sound");
    }
}

class Dog extends Animal {
    speak() {
        console.log("Woof");
    }
}

class Cat extends Animal {
    speak() {
        console.log("Meow");
    }
}
```

Now:

```js
function makeSound(animal) {
    animal.speak();
}
```

We can pass:

```js
makeSound(new Dog());
makeSound(new Cat());
```

Same interface:

```text
animal.speak()
```

Different behavior:

```text
Dog → Woof
Cat → Meow
```

JavaScript's dynamic nature makes this particularly natural.

---

# 9.22 Polymorphism Doesn't Require Classes

This is an important JavaScript-specific insight.

You can have polymorphism without inheritance.

```js
const dog = {
    speak() {
        console.log("Woof");
    }
};

const cat = {
    speak() {
        console.log("Meow");
    }
};

function makeSound(animal) {
    animal.speak();
}
```

Then:

```js
makeSound(dog);
makeSound(cat);
```

works.

This is essentially **behavior-based / structural polymorphism**.

The function doesn't care:

```text
"What class is this?"
```

It cares:

```text
"Does this object provide speak()?"
```

This idea is extremely important in JavaScript and TypeScript.

---

# 9.23 Class vs Constructor Function

Before `class`, developers commonly wrote:

```js
function User(name) {
    this.name = name;
}

User.prototype.greet = function () {
    console.log(`Hello ${this.name}`);
};
```

Then:

```js
const user = new User("Danishan");
```

The equivalent class-oriented syntax is:

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

Both ultimately use:

```text
prototype-based object relationships
```

But class syntax provides cleaner syntax and additional semantics for:

* inheritance
* private fields
* static members
* accessors
* class initialization

---

# 9.24 Important Difference: Classes Are Not Just Syntactic Sugar

It's tempting to say:

> "A class is just syntactic sugar over constructor functions."

That's useful as a first approximation, but **not completely accurate**.

Classes have semantics that differ from ordinary constructor functions.

For example, class bodies are strict-mode code, and class constructors cannot be called without `new`.

```js
class User {}

User();
```

throws:

```text
TypeError
```

Whereas an ordinary function can sometimes be called without `new`, depending on its implementation.

So for SDE-2:

> Classes use the prototype-based object model, but they are not merely a textual rewrite of constructor functions.

---

# 9.25 Class vs Prototype-Based Inheritance

This is another terminology trap.

JavaScript's underlying inheritance model is:

```text
prototype-based
```

Classes provide:

```text
class-based syntax
```

So:

```js
class Dog extends Animal {}
```

doesn't transform JavaScript into a fundamentally class-based inheritance runtime.

The prototype chain still exists.

```text
Dog instance
     ↓
Dog.prototype
     ↓
Animal.prototype
     ↓
Object.prototype
     ↓
null
```

Therefore:

> **JavaScript has class syntax but a prototype-based object model.**

That sentence is worth remembering for interviews.

---

# 9.26 Encapsulation

Encapsulation means keeping an object's internal implementation details controlled behind a public interface.

For example:

```js
class BankAccount {
    #balance = 0;

    deposit(amount) {
        if (amount <= 0) {
            throw new Error("Invalid amount");
        }

        this.#balance += amount;
    }

    getBalance() {
        return this.#balance;
    }
}
```

The caller interacts through:

```js
account.deposit(100);
account.getBalance();
```

rather than directly manipulating:

```text
#balance
```

This gives us:

```text
public API
    ↓
methods
    ↓
private implementation
```

---

# 9.27 Encapsulation Is More Than Private Fields

Private fields are one mechanism for encapsulation.

Encapsulation can also involve:

* closures
* module boundaries
* carefully designed public APIs
* avoiding direct mutation
* getters/setters
* private methods
* abstraction

So:

```text
encapsulation
≠
#private fields
```

Private fields are simply one language feature that helps implement it.

---

# 9.28 Composition vs Inheritance

This is one of the most important design discussions for an experienced engineer.

Inheritance models:

> **is-a**

For example:

```text
Dog is an Animal
```

Composition models:

> **has-a / uses-a**

For example:

```text
Car has an Engine
```

---

# 9.29 Inheritance Example

```js
class Animal {
    eat() {
        console.log("Eating");
    }
}

class Dog extends Animal {
    bark() {
        console.log("Barking");
    }
}
```

Here:

```text
Dog
 ↓
Animal
```

Dog inherits behavior from Animal.

---

# 9.30 Composition Example

Instead of inheritance:

```js
function createLogger() {
    return {
        log(message) {
            console.log(message);
        }
    };
}

function createUserService(logger) {
    return {
        createUser(user) {
            logger.log(`Creating ${user.name}`);
        }
    };
}
```

Now:

```js
const logger = createLogger();

const userService = createUserService(logger);
```

The service **uses** a logger.

There is no inheritance relationship.

```text
UserService
     │
     └── uses → Logger
```

---

# 9.31 Why Composition Is Often Preferred

Imagine:

```text
Animal
 ├── Dog
 ├── Cat
 ├── Bird
 └── Fish
```

As systems grow, inheritance hierarchies can become rigid.

Suppose now you need:

```text
Flying
Swimming
Walking
Logging
Caching
Persistence
Notifications
```

Trying to model every combination through inheritance can produce complicated hierarchies.

Composition lets you combine capabilities:

```text
Dog
 ├── walking behavior
 ├── logging behavior
 └── notification behavior
```

without forcing everything into a single inheritance tree.

---

# 9.32 Composition Doesn't Mean "Never Use Inheritance"

This is an important engineering nuance.

Inheritance is useful when the relationship is genuinely stable and meaningful.

For example:

```text
Error
 ↓
ValidationError
```

or:

```text
HTMLElement
 ↓
specific element types
```

The question shouldn't be:

> "Is inheritance bad?"

Instead ask:

> **"Is inheritance the right relationship for this domain?"**

---

# 9.33 The Most Important Connection: Classes → Prototypes

Let's trace:

```js
class User {
    constructor(name) {
        this.name = name;
    }

    greet() {
        console.log(this.name);
    }
}

const user = new User("Danishan");
```

Conceptually:

```text
                    User
                     │
                     │ prototype
                     ↓
              User.prototype
                     │
                     └── greet()
                     ↑
                     │
                    user
             ┌───────┘
             │
             └── name → "Danishan"
```

So when:

```js
user.greet();
```

is executed:

```text
1. Look for "greet" on user
        ↓
2. Not found
        ↓
3. Look at User.prototype
        ↓
4. Find greet
        ↓
5. Call it with user as this
```

This ties together:

```text
Objects
+
Prototypes
+
Classes
+
this
+
Method lookup
```

---

# 9.34 High-Probability SDE-2 Questions

I would prioritize these heavily.

### Classes

1. What is a class in JavaScript?
2. How does a JavaScript class relate to prototypes?
3. What does `constructor()` do?
4. What are instance properties?
5. Where do class methods actually live?
6. What are static methods?
7. What are static properties?
8. Difference between static and instance members.
9. Can a class have multiple constructors?

### Private state

10. What are private fields?
11. Difference between `#field` and `_field`.
12. How does JavaScript enforce private fields?
13. Difference between private fields and closure-based encapsulation.

### Inheritance

14. What does `extends` do?
15. What does `super()` do?
16. Why must `super()` be called before using `this` in a derived constructor?
17. What does `super.method()` do?
18. What is method overriding?
19. How does method lookup work with inherited methods?
20. Explain the prototype chain of a derived class instance.

### Conceptual

21. Are JavaScript classes really class-based internally?
22. Is `class` just syntactic sugar over constructor functions?
23. Class vs constructor function.
24. Class-based syntax vs prototype-based inheritance.
25. Explain polymorphism in JavaScript.
26. Does polymorphism require inheritance?
27. What is encapsulation?
28. Composition vs inheritance.
29. When would you prefer composition?
30. When is inheritance appropriate?

---

# 9.35 SDE-2 Mental Model

You should leave this topic with this model:

```text
                         CLASS
                           │
              ┌────────────┼────────────┐
              ↓            ↓            ↓
          constructor    static      prototype
              │         members          │
              ↓                          ↓
         instance                    methods
         properties                      │
              │                          │
              └──────────┐      ┌────────┘
                         ↓      ↓
                       INSTANCE
                           │
                           ↓
                    prototype lookup
                           │
                           ↓
                      parent prototype
                           │
                           ↓
                     Object.prototype
                           │
                           ↓
                          null
```

And for inheritance:

```text
class Dog extends Animal

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

And for design:

```text
Inheritance
    ↓
"is-a"
    ↓
Dog is an Animal

Composition
    ↓
"has-a / uses-a"
    ↓
Service uses Logger
```

### The key SDE-2 takeaway

Don't stop at:

> "`class` is used to create objects."

The deeper understanding is:

> **A JavaScript class defines constructor behavior and prototype-based instance behavior, while also providing features such as inheritance, static members, private fields, and accessors.**

And the most important connection to the previous chapter is:

```text
class
  ↓
prototype
  ↓
property lookup
  ↓
method resolution
  ↓
this
```

If you can trace that chain confidently, you've moved beyond syntax-level knowledge into the level expected from an experienced JavaScript engineer.
