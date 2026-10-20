# 🧠 TypeScript OOP — Revision Summary

| Concept                  | What it means                                         | Key syntax / example          | Remember                             |
| ------------------------ | ----------------------------------------------------- | ----------------------------- | ------------------------------------ |
| **Class**                | Blueprint for creating objects                        | `class User {}`               | Defines structure + behavior         |
| **Object**               | Actual instance of a class                            | `new User()`                  | Class → object                       |
| **Property**             | Data/state of an object                               | `name: string`                | What the object **has**              |
| **Method**               | Behavior of an object                                 | `greet() {}`                  | What the object **does**             |
| **Constructor**          | Runs when object is created                           | `constructor() {}`            | Initializes object                   |
| **`new`**                | Creates an instance                                   | `new User()`                  | Creates object + runs constructor    |
| **`this`**               | Refers to the current object                          | `this.name`                   | "This object"                        |
| **`public`**             | Accessible everywhere                                 | `public name`                 | Default visibility                   |
| **`private`**            | Accessible only inside class                          | `private balance`             | Protect internal state               |
| **`protected`**          | Class + subclasses                                    | `protected id`                | More accessible than private         |
| **Encapsulation**        | Control/protect internal state                        | `private balance`             | **Protect state**                    |
| **`extends`**            | Inherits from another class                           | `class Dog extends Animal`    | **is-a** relationship                |
| **`super()`**            | Calls parent constructor                              | `super(name)`                 | Initialize parent                    |
| **`super.method()`**     | Calls parent method                                   | `super.speak()`               | Use parent behavior                  |
| **Overriding**           | Child replaces parent method                          | `speak() {}`                  | Same method, new behavior            |
| **Inheritance**          | Reuse/extend another class                            | `extends`                     | Parent → child                       |
| **Polymorphism**         | Same interface, different behavior                    | `animal.speak()`              | One call → different implementations |
| **Abstraction**          | Hide implementation details                           | `abstract class`              | Focus on **what**, not **how**       |
| **Abstract class**       | Class designed to be extended                         | `abstract class Shape`        | Cannot instantiate directly          |
| **Abstract method**      | Method child must implement                           | `abstract area(): number`     | Defines required behavior            |
| **Interface**            | Defines a contract/type shape                         | `interface User {}`           | Describes what must exist            |
| **`implements`**         | Class follows an interface                            | `class Dog implements Animal` | "I satisfy this contract"            |
| **Composition**          | Object contains another object                        | `Car has Engine`              | **has-a** relationship               |
| **Dependency Injection** | Dependency supplied from outside                      | `constructor(db: DB)`         | Reduces coupling                     |
| **Static**               | Belongs to class, not instance                        | `User.create()`               | `Class.method()`                     |
| **Getter**               | Reads a property through a method                     | `get name()`                  | Looks like property access           |
| **Setter**               | Controls property assignment                          | `set name()`                  | Validate/control updates             |
| **`readonly`**           | Cannot be reassigned after initialization             | `readonly id`                 | One-time assignment                  |
| **Prototype**            | Object from which methods/properties can be inherited | `User.prototype`              | Core JS mechanism                    |
| **Prototype chain**      | Lookup path for properties/methods                    | `object → prototype → ...`    | How JS finds inherited behavior      |

#### ⭐ Four Pillars

| Pillar            | Simple meaning                    | Think                     |
| ----------------- | --------------------------------- | ------------------------- |
| **Encapsulation** | Protect internal state            | 🔒 **Protect**            |
| **Abstraction**   | Hide unnecessary complexity       | 🎭 **Hide**               |
| **Inheritance**   | Reuse/extend behavior             | 🌳 **Reuse**              |
| **Polymorphism**  | Same contract, different behavior | 🔄 **Different behavior** |

#### 🔥 Most Important Comparisons

| Concept A   | Concept B      | Difference                                                         |
| ----------- | -------------- | ------------------------------------------------------------------ |
| Class       | Object         | Blueprint vs actual instance                                       |
| Property    | Method         | Data vs behavior                                                   |
| `extends`   | `implements`   | Inheritance vs contract                                            |
| `private`   | `protected`    | Class only vs class + subclasses                                   |
| Inheritance | Composition    | **is-a** vs **has-a**                                              |
| Interface   | Abstract class | Contract vs contract + shared implementation                       |
| Instance    | Static         | Belongs to object vs belongs to class                              |
| Overloading | Overriding     | Same class/signature variations vs child replacing parent behavior |
| `private`   | `#private`     | TS compile-time restriction vs JS runtime private field            |

#### 🧩 The OOP Mental Model

```text
                CLASS
                  │
                 new
                  ↓
                OBJECT
                  │
          ┌───────┴────────┐
          ↓                ↓
       STATE            BEHAVIOR
     properties         methods
          │                │
          └───────┬────────┘
                  ↓
           ENCAPSULATION
                  │
        ┌─────────┴─────────┐
        ↓                   ↓
   INHERITANCE          COMPOSITION
      is-a                 has-a
        │                   │
        ↓                   ↓
  POLYMORPHISM       DEPENDENCY
        │             INJECTION
        ↓
   ABSTRACTION
        │
        ↓
   INTERFACE / CONTRACT
```

#### 🧠 And underneath JavaScript:

```text
Object
   ↓
Prototype
   ↓
Prototype Chain
   ↓
Method Lookup
```

**If you remember only one thing:**

> **OOP is about modeling objects with state and behavior, controlling their state through encapsulation, defining relationships through inheritance/composition, and allowing different objects to satisfy the same abstraction through polymorphism.**

# 🧠 OOP in TypeScript — Deep Revision

The core idea:

> **OOP = organizing code around objects that contain data + behavior.**

Think:

```text
                OOP
                 │
     ┌───────────┼───────────┐
     │           │           │
   Class       Object      Methods
     │           │           │
     └───────────┼───────────┘
                 │
       ┌─────────┼─────────┐
       │         │         │
 Encapsulation Inheritance Polymorphism
                           │
                       Abstraction
```

## 1. Class

A **class is a blueprint/template** for creating objects.

```ts
class User {
  name: string;

  constructor(name: string) {
    this.name = name;
  }

  greet() {
    console.log(`Hello ${this.name}`);
  }
}
```

The class describes:

```text
data       → name
behavior   → greet()
creation   → constructor
```

## 2. Object

An object is an **instance of a class**.

```ts
const user1 = new User("John");
const user2 = new User("Alice");
```

Think:

```text
Class
 ↓
Blueprint

Object
 ↓
Actual instance
```

One class can create many objects.

## 3. Constructor

A constructor runs when an object is created.

```ts
class User {
  constructor(public name: string) {}
}

const user = new User("John");
```

When you do:

```ts
new User("John")
```

the constructor receives `"John"`.

## 4. `new`

`new` creates an object from a class.

```ts
const user = new User("John");
```

Conceptually:

```text
new
 ↓
create object
 ↓
connect prototype
 ↓
execute constructor
 ↓
return object
```

## 5. Properties

Properties represent **data/state**.

```ts
class User {
  name: string;
  age: number;
}
```

```text
User
 ├── name
 └── age
```

## 6. Methods

Methods represent **behavior**.

```ts
class User {
  greet() {
    console.log("Hello");
  }
}
```

```text
User
 └── greet()
```

## 7. `this`

Inside a class, `this` usually refers to the **current object instance**.

```ts
class User {
  constructor(public name: string) {}

  greet() {
    console.log(this.name);
  }
}
```

```ts
const user = new User("John");

user.greet();
```

Here:

```text
this → user
```

## 8. Encapsulation ⭐⭐⭐

Encapsulation means:

> **Keeping data and the operations on that data together, while controlling access to the internal state.**

TypeScript provides:

```ts
public
private
protected
```

Example:

```ts
class BankAccount {
  private balance = 0;

  deposit(amount: number) {
    this.balance += amount;
  }

  getBalance() {
    return this.balance;
  }
}
```

You can't directly do:

```ts
account.balance = 1000000; // ❌
```

Instead:

```ts
account.deposit(100);
```

This protects the object's internal state.

## 9. Access Modifiers

| Modifier    | Accessible from       |
| ----------- | --------------------- |
| `public`    | Everywhere            |
| `private`   | Only inside the class |
| `protected` | Class + subclasses    |

Example:

```ts
class Person {
  public name = "John";
  private age = 25;
  protected id = 123;
}
```

## 10. Inheritance ⭐⭐⭐

Inheritance allows one class to **reuse/extend another class**.

```ts
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

Now:

```ts
const dog = new Dog();

dog.eat();  // inherited
dog.bark(); // own method
```

Think:

```text
Animal
  ↑
  │ extends
  │
 Dog
```

## 11. `extends`

Used to create inheritance:

```ts
class Dog extends Animal {}
```

The child class gets accessible properties/methods from the parent.

## 12. `super`

Used to access the parent class.

```ts
class Animal {
  constructor(public name: string) {}
}

class Dog extends Animal {
  constructor(name: string) {
    super(name);
  }
}
```

`super()` calls the parent constructor.

You can also call parent methods:

```ts
super.eat();
```

## 13. Method Overriding ⭐

A child class can provide its own implementation of a parent method.

```ts
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
```

The child **overrides** the parent's method.

## 14. Polymorphism ⭐⭐⭐

Polymorphism means:

> **The same interface/method call can produce different behavior depending on the object.**

```ts
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

Then:

```ts
const animals: Animal[] = [
  new Dog(),
  new Cat()
];

animals.forEach(animal => {
  animal.speak();
});
```

Same:

```ts
animal.speak()
```

Different behavior.

```text
Dog → Woof
Cat → Meow
```

That's polymorphism.

## 15. Abstraction ⭐⭐⭐

Abstraction means:

> **Expose what an object can do while hiding unnecessary implementation details.**

For example:

```ts
abstract class Animal {
  abstract speak(): void;

  eat() {
    console.log("Eating");
  }
}
```

A subclass must implement:

```ts
class Dog extends Animal {
  speak() {
    console.log("Woof");
  }
}
```

You cannot directly create:

```ts
new Animal(); // ❌
```

because it's abstract.

## 16. Abstract Class

An abstract class is designed to be **extended**, not directly instantiated.

```ts
abstract class Shape {
  abstract area(): number;
}
```

Child:

```ts
class Circle extends Shape {
  constructor(private radius: number) {
    super();
  }

  area() {
    return Math.PI * this.radius ** 2;
  }
}
```

## 17. Interface ⭐⭐⭐

An interface describes a **contract/shape**.

```ts
interface User {
  name: string;
  age: number;
}
```

Then:

```ts
const user: User = {
  name: "John",
  age: 25
};
```

Interfaces are heavily used in TypeScript.

## 18. Interface vs Class

| Class                                | Interface                       |
| ------------------------------------ | ------------------------------- |
| Can create objects with `new`        | Cannot                          |
| Can contain implementation           | Primarily describes structure   |
| Can have constructor                 | No constructor                  |
| Can have methods with implementation | Describes method shape          |
| Used for behavior + state            | Used mainly for contracts/types |

Example:

```ts
interface Flyable {
  fly(): void;
}

class Bird implements Flyable {
  fly() {
    console.log("Flying");
  }
}
```

## 19. `implements`

Means:

> "This class promises to follow this interface."

```ts
interface Printable {
  print(): void;
}

class Document implements Printable {
  print() {
    console.log("Printing...");
  }
}
```

If the class doesn't implement the required structure, TypeScript complains.

## 20. Composition ⭐⭐⭐

Instead of saying:

> "Class B **is an** A"

we sometimes say:

> "Class B **has an** A."

Example:

```ts
class Engine {
  start() {
    console.log("Engine started");
  }
}

class Car {
  constructor(private engine: Engine) {}

  start() {
    this.engine.start();
  }
}
```

Here:

```text
Car
 └── has an → Engine
```

This is **composition**.

## 21. Inheritance vs Composition

Very important design concept:

```text
Inheritance
"is-a"

Dog is an Animal
Car is a Vehicle
```

Composition:

```text
"has-a"

Car has an Engine
User has an Address
Order has Items
```

Modern application design often favors **composition over deep inheritance hierarchies**.

## 22. Static Members

A static member belongs to the **class itself**, not an individual instance.

```ts
class MathUtil {
  static PI = 3.14;
}
```

Use:

```ts
MathUtil.PI;
```

Not:

```ts
const x = new MathUtil();

x.PI; // ❌
```

## 23. Getters and Setters

Used to control property access.

```ts
class User {
  constructor(private _name: string) {}

  get name() {
    return this._name;
  }

  set name(value: string) {
    this._name = value;
  }
}
```

Then:

```ts
const user = new User("John");

console.log(user.name);

user.name = "Alice";
```

Looks like property access but actually executes methods.

## 24. Readonly

TypeScript can prevent reassignment.

```ts
class User {
  readonly id: number;

  constructor(id: number) {
    this.id = id;
  }
}
```

After initialization:

```ts
user.id = 10; // ❌
```

## 25. OOP's Four Pillars

The classic four:

| Pillar            | Meaning                            |
| ----------------- | ---------------------------------- |
| **Encapsulation** | Protect/control internal state     |
| **Abstraction**   | Hide implementation details        |
| **Inheritance**   | Reuse/extend another class         |
| **Polymorphism**  | Same interface, different behavior |

Remember:

```text
Encapsulation → protect
Abstraction   → hide complexity
Inheritance   → reuse/extend
Polymorphism  → behave differently
```

## 🔥 The deeper picture

Don't think of OOP as just:

> "Classes and objects."

Think:

```text
                 OBJECT
                   │
          ┌────────┴────────┐
          │                 │
        STATE            BEHAVIOR
     properties           methods
          │                 │
          └────────┬────────┘
                   │
             ENCAPSULATION
                   │
        ┌──────────┴──────────┐
        │                     │
   INHERITANCE          COMPOSITION
        │                     │
        ↓                     ↓
   POLYMORPHISM          HAS-A relationship
        │
        ↓
    ABSTRACTION
```

## ⭐ What to Master First

If you're preparing for interviews or real TypeScript development, prioritize:

#### Must know

1. Class
2. Object
3. Constructor
4. `this`
5. Properties & methods
6. Access modifiers
7. Encapsulation
8. `extends`
9. `super`
10. Method overriding
11. Polymorphism
12. Abstract classes
13. Interfaces
14. `implements`
15. Composition

#### Then go deeper

16. `static`
17. Getters/setters
18. `readonly`
19. Method/property visibility
20. Inheritance vs composition
21. Interface vs abstract class
22. TypeScript's structural typing
23. Prototype chain
24. `instanceof`
25. SOLID principles
