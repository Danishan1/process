# ✅ **1. OOP Pillars in JavaScript**

OOP in JS is based on 4 major concepts:

1. **Encapsulation** – grouping data + methods
2. **Abstraction** – hiding details
3. **Inheritance** – one object inherits properties from another
4. **Polymorphism** – same method, different behavior

But **JavaScript is NOT class-based**, it is **prototype-based**.
Classes in JS are just *syntactic sugar*.

---

# ✅ **2. Objects in JS**

Basic object:

```js
const person = {
  name: "John",
  speak() {
    console.log("Hello");
  }
};

console.log(person.name);
person.speak();
```

---

# ✔ **3. Constructor Functions (Old-school OOP)**

Before ES6, JS used *constructor functions*:

```js
function Person(name, age) {
  this.name = name;
  this.age = age;
}

const p1 = new Person("Alice", 25);
console.log(p1.name);
```

---

# ✔ **4. Prototype (THE REAL OOP in JS)**

Every function in JS has a `.prototype`.

```js
Person.prototype.sayHello = function () {
  console.log("Hi, I am " + this.name);
};

p1.sayHello();
```

### Meaning:

`p1` → inherits from → `Person.prototype`

---

# 🔥 **Prototype Chain**

```js
console.log(p1.__proto__ === Person.prototype); // true
```

Prototype chain:

```
p1 → Person.prototype → Object.prototype → null
```

---

# ✔ **5. ES6 Classes (Syntactic Sugar)**

```js
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  speak() {
    console.log("Hello, my name is " + this.name);
  }
}

const p = new Person("Danishan", 26);
p.speak();
```

---

# ✔ **6. Inheritance (class extends)**

```js
class Animal {
  eat() {
    console.log("eating");
  }
}

class Dog extends Animal {
  bark() {
    console.log("woof");
  }
}

const d = new Dog();
d.eat();
d.bark();
```

---

# ✔ **7. super()**

Used to call parent constructor.

```js
class Animal {
  constructor(name) {
    this.name = name;
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name);
    this.breed = breed;
  }
}
```

---

# ✔ **8. Polymorphism**

Same method → different behavior.

```js
class Animal {
  speak() {
    console.log("Animal sound");
  }
}

class Dog extends Animal {
  speak() {
    console.log("Bark");
  }
}

new Dog().speak(); // Bark
```

---

# ✔ **9. Encapsulation (private fields)**

Modern JS supports **true private fields** using `#`.

```js
class BankAccount {
  #balance = 0; // private

  deposit(amount) {
    this.#balance += amount;
  }

  getBalance() {
    return this.#balance;
  }
}

const acc = new BankAccount();
acc.deposit(100);
console.log(acc.getBalance()); // 100
console.log(acc.#balance); // Error
```

---

# ✔ **10. Static Methods**

Methods that belong to the class, not to the object.

```js
class MathUtils {
  static add(a, b) {
    return a + b;
  }
}

console.log(MathUtils.add(5, 10));
```

---

# 🔥 **11. Factory Functions**

An alternative to constructor functions:

```js
function createUser(name, age) {
  return {
    name,
    age,
    sayHi() {
      console.log("Hi " + name);
    }
  };
}

const u = createUser("Sam", 30);
u.sayHi();
```

---

# 🔥 **12. THIS in OOP**

### Inside class:

```js
class Person {
  name = "John";
  print = () => console.log(this.name);
}
```

Arrow function binds `this` lexically.

### Constructor function:

```js
function User() {
  this.name = "X";
}
```

---

# 🔥 **13. Interview Questions (Advanced)**

### **Q1: Difference between class and prototype?**

* `class` is syntactic sugar
* Under the hood JS still uses **prototypes**

### Q2: Why use `super()`?

To call parent constructors.

### Q3: What is prototype chain?

Linking objects through their internal `[[Prototype]]`.

### Q4: Can you polyfill a class?

No, but you can recreate it via prototype functions.

### Q5: Why does JS prefer composition over inheritance?

Because JS is flexible and avoids deep inheritance chains.

---

# 🔥 **14. TRICKY: What is `__proto__` vs `prototype`?**

✔ `prototype` → available on **functions**
✔ `__proto__` → available on **objects**

```
function User() {}
const u = new User();

u.__proto__ === User.prototype  // true
```

---

# 🔥 **15. Real Example: OOP Design**

### Car → ElectricCar

```js
class Car {
  constructor(model, brand) {
    this.model = model;
    this.brand = brand;
  }

  start() {
    console.log(`${this.brand} ${this.model} starting`);
  }
}

class ElectricCar extends Car {
  charge() {
    console.log("Charging...");
  }
}

const tesla = new ElectricCar("Model 3", "Tesla");
tesla.start();
tesla.charge();
```
