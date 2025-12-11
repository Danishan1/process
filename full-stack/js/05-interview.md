
# ✅ **MASTER JAVASCRIPT INTERVIEW REVISION**

We will cover:

1. **Functions Deep Dive (HOF, callbacks, arrow, this)**
2. **Closures & Scope (TRICKIEST INTERVIEW TOPIC)**
3. **Promises & Async-Await**
4. **Event Loop & Microtask Queue**
5. **Prototype, Classes & OOP**
6. **ES6+ Important Features**
7. **DOM & Browser JS (Event Bubbling, Delegation)**
8. **Final Rapid-Fire Interview Revision (100 Qs)**

---

# ⭐ We begin now:

# ---------------------------------------------------------

# ✅ **1. FUNCTIONS DEEP DIVE**

# ---------------------------------------------------------

## **1.1 Function Declaration vs Expression**

```js
function a() {}      // hoisted fully
const b = function() {}; // NOT hoisted
```

---

## **1.2 Arrow Functions**

```js
const add = (a, b) => a + b;
```

### Differences from normal functions:

* No `this`
* No `arguments`
* Cannot be used as constructor
* No `prototype`

---

## **1.3 Higher-Order Functions (HOF)**

Functions that take OR return functions.

```js
function multiply(x) {
  return function(y) {
    return x * y;
  };
}
const double = multiply(2);
console.log(double(5)); // 10
```

---

## **1.4 Callback Function**

```js
function process(value, callback) {
  callback(value);
}

process("JS", console.log);
```

---

## **1.5 `this` in Functions**

### Normal function: `this` depends on **how** it's called.

### Arrow function: `this` is from **outer scope**.

```js
const obj = {
  value: 10,
  normal() { return this.value; },
  arrow: () => this.value
};

obj.normal(); // 10
obj.arrow();  // undefined
```

---

## **1.6 arguments object (only in normal functions)**

```js
function test() {
  console.log(arguments);
}
test(1,2,3);
```

Arrow functions don't have `arguments`.

---

# ---------------------------------------------------------

# ✅ **2. CLOSURES & SCOPE DEEP DIVE**

# ---------------------------------------------------------

## **2.1 Closure Definition**

A closure is when a function remembers variables from its outer scope.

```js
function outer() {
  let count = 0;
  return function inner() {
    count++;
    console.log(count);
  };
}
const c = outer();
c(); // 1
c(); // 2
```

---

## **2.2 Closure LOOP Problem**

```js
for(var i = 1; i <= 3; i++) {
  setTimeout(() => console.log(i), 1000);
}
```

Output:

```
4
4
4
```

Solution using `let`:

```
1
2
3
```

---

## **2.3 Closure with setTimeout Fix**

```js
for(var i=1;i<=3;i++){
  ((x)=>setTimeout(()=>console.log(x),1000))(i);
}
```

---

## **2.4 Lexical Scope**

JS looks for variables in parent scope.

---

## **2.5 Real Closure Interview Example**

```js
function createCounter() {
  let count = 0;
  return {
    inc: () => ++count,
    dec: () => --count
  };
}
```

---

# ---------------------------------------------------------

# ✅ **3. PROMISES & ASYNC-AWAIT**

# ---------------------------------------------------------

## **3.1 Basic Promise**

```js
new Promise(res => res("done")).then(console.log);
```

---

## **3.2 Promise Chaining**

```js
Promise.resolve(2)
  .then(x => x*2)
  .then(x => x+1)
  .then(console.log); // 5
```

---

## **3.3 Promise.all**

Runs all, fails if any fail.

---

## **3.4 Promise.race**

Returns first settled promise.

---

## **3.5 async-await**

```js
async function test() {
  const data = await Promise.resolve("hi");
  console.log(data);
}
test();
```

---

## **3.6 Interview Trick**

```js
async function a() { return "A"; }
console.log(a());
```

Output:

```
Promise { "A" }
```

---

# ---------------------------------------------------------

# ✅ **4. EVENT LOOP & MICROTASK QUEUE**

# ---------------------------------------------------------

## **Priority**

1. **Synchronous**
2. **Microtasks** → promises
3. **Macrotasks** → setTimeout, DOM events

---

## **Example**

```js
console.log("A");

setTimeout(() => console.log("B"), 0);

Promise.resolve().then(() => console.log("C"));

console.log("D");
```

Output:

```
A
D
C
B
```

---

## **Classic Interview Question**

```js
setTimeout(()=>console.log(1),0);
Promise.resolve().then(()=>console.log(2));
console.log(3);
```

Output:

```
3
2
1
```

---

# ---------------------------------------------------------

# ✅ **5. PROTOTYPE, CLASSES & OOP**

# ---------------------------------------------------------

## **Everything in JS uses prototype**

```js
function Person(name) {
  this.name = name;
}

Person.prototype.say = function() {
  console.log("Hi " + this.name);
};

const p = new Person("John");
p.say();
```

---

## **Prototype Chain**

```
p --> Person.prototype --> Object.prototype --> null
```

---

## **Classes (syntactic sugar)**

```js
class Animal {
  constructor(n) { this.name = n; }
  speak() { console.log(this.name + " speaks"); }
}
```

---

## **Inheritance**

```js
class Dog extends Animal {
  bark() { console.log("Woof"); }
}
```

---

# ---------------------------------------------------------

# ✅ **6. ES6+ IMPORTANT FEATURES**

# ---------------------------------------------------------

* let / const
* arrow functions
* rest & spread
* destructuring
* template literals
* promises
* classes
* default parameters
* modules
* optional chaining
* nullish coalescing
* generators

---

# ---------------------------------------------------------

# ✅ **7. DOM & BROWSER JS**

# ---------------------------------------------------------

## **Selecting Elements**

```js
document.querySelector();
document.getElementById();
```

---

## **Event Bubbling**

Events propagate:

child → parent → document

Stop it:

```js
event.stopPropagation();
```

---

## **Event Delegation**

Attach one listener to parent:

```js
document.querySelector("#list")
  .addEventListener("click", e => {
    if (e.target.tagName === "LI") console.log("Clicked");
});
```

---

## **DOM Manipulation**

```js
const div = document.createElement("div");
div.textContent = "Hi";
document.body.appendChild(div);
```

---

# ---------------------------------------------------------

# ✅ **8. FINAL INTERVIEW RAPID-FIRE REVISION (TOP 25)**

# ---------------------------------------------------------

Here are the **TOP must-know Qs**:

1. What is closure?
2. What is event loop?
3. What is hoisting?
4. var vs let vs const
5. Arrow vs normal function
6. Prototypal inheritance
7. Promise vs async/await
8. Event bubbling
9. Debounce vs throttle
10. Spread vs rest
11. Deep vs shallow copy
12. `this` keyword in JS
13. typeof results
14. Optional chaining
15. Nullish coalescing
16. Map vs Object
17. Set vs Array
18. What are microtasks?
19. What is call, apply, bind?
20. Pure function
21. Higher-order functions
22. Block scope vs function scope
23. Strict mode
24. Temporal dead zone
25. Arrow function limitations
