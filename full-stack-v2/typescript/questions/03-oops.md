# 🧠 OOP — 3-Level Question Bank

## 🟢 Level 1 — Beginner

**Goal: Understand the basic building blocks.**

### Classes & Objects

1. What is a class?
2. What is an object?
3. What's the difference between a class and an object?
4. What does `new` do?
5. What is a constructor?
6. When is a constructor executed?
7. What does `this` refer to inside a class method?
8. What is a property?
9. What is a method?
10. Can one class create multiple objects?

### Access & Encapsulation

11. What does `public` mean?
12. What does `private` mean?
13. What does `protected` mean?
14. What is encapsulation?
15. Why would you make a property `private`?
16. What is the difference between a public and private property?

### Inheritance

17. What does `extends` do?

```ts id="v8f0d7"
class Dog extends Animal {}
```

18. What does `super()` do?
19. Why does a child constructor usually need `super()` before using `this`?
20. What is method overriding?
21. What does a child class inherit from its parent?

### Polymorphism & Abstraction

22. What is polymorphism?
23. What is abstraction?
24. What is an abstract class?
25. Can you create an object directly from an abstract class?
26. What is an interface?
27. What does `implements` mean?
28. What's the difference between `extends` and `implements`?

### Composition

29. What does "has-a relationship" mean?
30. What does "is-a relationship" mean?
31. Is a `Car` an `Engine`, or does a `Car` have an `Engine`?
32. What is composition?

# 🟡 Level 2 — Intermediate

**Goal: Predict behavior and explain why.**

## Classes & `this`

### 33. What is the output?

```ts
class User {
  constructor(public name: string) {}

  greet() {
    console.log(this.name);
  }
}

const user = new User("John");

user.greet();
```

Explain what `this` refers to.

### 34. What happens here?

```ts
class Counter {
  count = 0;

  increment() {
    this.count++;
  }
}

const a = new Counter();
const b = new Counter();

a.increment();

console.log(a.count);
console.log(b.count);
```

Why don't `a` and `b` share the same `count`?

### 35. What happens if the method is extracted?

```ts
const greet = user.greet;

greet();
```

Why can `this` behave differently?

## Constructors

### 36. Predict the output.

```ts
class Person {
  constructor(public name: string) {
    console.log("Constructor:", name);
  }
}

const a = new Person("John");
const b = new Person("Alice");
```

How many times does the constructor execute?

### 37. What happens if a subclass constructor doesn't call `super()`?

```ts
class Animal {
  constructor(public name: string) {}
}

class Dog extends Animal {
  constructor(name: string) {
    // no super()
  }
}
```

Why?

# Encapsulation

### 38. Why is this better?

```ts
class BankAccount {
  private balance = 0;

  deposit(amount: number) {
    if (amount > 0) {
      this.balance += amount;
    }
  }
}
```

than:

```ts
class BankAccount {
  public balance = 0;
}
```

### 39. Create a `BankAccount` class that:

* keeps `balance` private
* allows deposit
* allows withdrawal
* prevents withdrawing more than the balance
* provides a way to read the balance

# Inheritance

### 40. What is the output?

```ts
class Animal {
  speak() {
    console.log("Animal");
  }
}

class Dog extends Animal {
  speak() {
    console.log("Dog");
  }
}

const dog = new Dog();

dog.speak();
```

Which method executes?

Why?

### 41. What happens here?

```ts
class Animal {
  speak() {
    console.log("Animal");
  }
}

class Dog extends Animal {
  speak() {
    super.speak();
    console.log("Dog");
  }
}

new Dog().speak();
```

Predict the output.

### 42. What is the difference between:

```ts
extends
```

and:

```ts
implements
```

Give an example of each.

# Interfaces

### 43. What's wrong with this?

```ts
interface User {
  name: string;
  age: number;
}

class Admin implements User {
  name = "John";
}
```

Why does TypeScript complain?

### 44. Can a class implement multiple interfaces?

Try:

```ts
interface Flyable {
  fly(): void;
}

interface Swimmable {
  swim(): void;
}
```

Create a class that implements both.

# Polymorphism

### 45. What happens?

```ts
class Animal {
  speak() {
    console.log("Animal");
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

function makeSound(animal: Animal) {
  animal.speak();
}

makeSound(new Dog());
makeSound(new Cat());
```

Why can the same function produce different behavior?

### 46. Why is this useful?

```ts
function processPayment(payment: PaymentMethod) {
  payment.pay(100);
}
```

instead of:

```ts
function processPayment(payment: CreditCard) {
  payment.pay(100);
}
```

# Composition

### 47. Design:

```text
Car
 └── Engine
```

Create:

```ts
class Engine {}
class Car {}
```

using composition.

### 48. Why is this generally better?

```ts
class Car {
  constructor(private engine: Engine) {}
}
```

than:

```ts
class Car {
  private engine = new Engine();
}
```

Think about **dependency injection**.

# 🔴 Level 3 — Hard

**Goal: Combine concepts and understand JavaScript's underlying behavior.**

## Prototype + Inheritance

### 49. What is the approximate prototype chain?

```ts
class Animal {
  eat() {}
}

class Dog extends Animal {
  bark() {}
}

const dog = new Dog();
```

Explain the path JavaScript follows when executing:

```ts
dog.eat();
```

### 50. Where does `bark()` live?

```ts
class Dog {
  bark() {
    console.log("Woof");
  }
}

const dog1 = new Dog();
const dog2 = new Dog();
```

Is a separate `bark` function stored inside `dog1` and `dog2`?

Explain.

### 51. Predict:

```ts
class Animal {
  speak() {
    console.log("Animal");
  }
}

class Dog extends Animal {
  speak() {
    console.log("Dog");
  }
}

const dog = new Dog();

dog.speak();
```

Explain the method lookup process using the prototype chain.

# `this` + Method Extraction

### 52. What happens?

```ts
class Counter {
  count = 0;

  increment() {
    this.count++;
  }
}

const counter = new Counter();

const fn = counter.increment;

fn();
```

Why might this fail?

### 53. How could you fix it using an arrow function?

```ts
class Counter {
  count = 0;

  // ?
}
```

Explain why the fix works.

# Polymorphism + Interfaces

### 54. What concepts are being used here?

```ts
interface NotificationService {
  send(message: string): void;
}

class EmailService implements NotificationService {
  send(message: string) {
    console.log("Email:", message);
  }
}

class SmsService implements NotificationService {
  send(message: string) {
    console.log("SMS:", message);
  }
}

function notify(service: NotificationService) {
  service.send("Hello");
}
```

Identify:

* abstraction
* interface
* polymorphism
* composition/dependency injection

### 55. Why can this work?

```ts
notify(new EmailService());
notify(new SmsService());
```

even though the two objects are completely different classes?

# Inheritance + Polymorphism

### 56. Predict the output:

```ts
class Animal {
  speak() {
    console.log("Animal");
  }
}

class Dog extends Animal {
  speak() {
    console.log("Dog");
  }
}

class Cat extends Animal {
  speak() {
    console.log("Cat");
  }
}

const animals: Animal[] = [
  new Dog(),
  new Cat(),
  new Animal()
];

for (const animal of animals) {
  animal.speak();
}
```

More importantly:

> Why doesn't TypeScript need to know which concrete class each element is?

# Encapsulation + Invariants

### 57. Design a `BankAccount` where:

```text
balance can never become negative
balance cannot be directly changed
withdrawal cannot exceed balance
deposit cannot be negative
```

Explain why your design guarantees those rules.

### 58. Why is this bad encapsulation?

```ts
class User {
  public age = 20;
}
```

If the application requires:

```text
age >= 0
```

How could you enforce that invariant?

# Composition + Dependency Injection

### 59. Consider:

```ts
class MySQLDatabase {
  connect() {}
}

class UserService {
  private db = new MySQLDatabase();
}
```

What is the problem with this design?

How would you redesign it using an interface?

### 60. Design:

```ts
interface Database {
  save(data: string): void;
}
```

Then create:

```text
MySQLDatabase
MongoDatabase
UserService
```

so that `UserService` can work with either database.

# 🏆 Deepest Questions

### 61. Explain this statement:

> "TypeScript's types disappear at runtime."

What does that mean for:

```ts
interface User {
  name: string;
}
```

Can JavaScript check whether an object "implements" `User` at runtime?

### 62. What's the difference between these?

```ts
class Dog extends Animal {}
```

and:

```ts
class Dog implements Animal {}
```

Can you even use `implements` if `Animal` is a class? What does TypeScript check?

### 63. Explain the difference between:

```text
Encapsulation
Abstraction
```

using a `BankAccount` example.

### 64. Explain the difference between:

```text
Inheritance
Composition
```

using a `Car` example.

### 65. Why can composition make a system easier to test?

Think about:

```ts
class UserService {
  constructor(private database: Database) {}
}
```

What happens if you inject a fake database during testing?

### 66. Explain this entire flow:

```text
class
 ↓
new
 ↓
object
 ↓
prototype
 ↓
prototype chain
 ↓
method lookup
 ↓
this
 ↓
inheritance
 ↓
overriding
 ↓
polymorphism
```

Don't just define each word—explain **how they are connected**.

# 🔥 Final OOP Challenges

These are the ones I'd actually use to test whether you've mastered OOP.

### 67. Payment System

Design:

```text
PaymentMethod
 ├── CreditCard
 ├── PayPal
 └── UPI

Checkout
```

Requirements:

* Use an interface.
* Use polymorphism.
* Use composition.
* Inject the payment method.
* `Checkout` shouldn't know the concrete payment type.

### 68. Notification System

Design:

```text
NotificationService
 ├── Email
 ├── SMS
 └── Push
```

Then:

```ts
notify(service, "Hello");
```

should work with all three.

### 69. Library System

Design:

```text
Library
 ├── Books
 ├── Members
 └── Borrow/Return
```

Requirements:

* encapsulate book availability
* prevent borrowing unavailable books
* use classes
* use composition
* maintain valid state

### 70. Explain the Design

Given:

```ts
class OrderService {
  constructor(
    private payment: PaymentMethod,
    private database: Database,
    private notifier: NotificationService
  ) {}

  placeOrder() {
    // ...
  }
}
```

Explain **why this is a strong OOP design**.

Identify:

```text
Abstraction
Encapsulation
Polymorphism
Composition
Dependency Injection
Loose Coupling
```

## 🎯 How to use these

Don't immediately write code for every question.

For **Level 1**, answer from memory.

For **Level 2**:

> **Predict → run → compare → explain.**

For **Level 3**:

> **Draw the objects/prototype relationships → predict → explain the runtime behavior.**

And the most important deep-OOP chain to master is:

**Object → `new` → `this` → Prototype → Prototype Chain → Inheritance → Overriding → Polymorphism → Abstraction → Interface → Composition → Dependency Injection.**

If you can explain that chain in your own words, you're no longer just memorizing OOP—you understand how JavaScript/TypeScript OOP actually fits together.
