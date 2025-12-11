# ✅ **1. Variables (`var`, `let`, `const`)**

### Key Points

* `var` → function-scoped, hoisted with default = undefined
* `let`, `const` → block-scoped, hoisted but NOT initialized (TDZ)

### Example

```js
console.log(a); // undefined
var a = 10;

console.log(b); // ReferenceError
let b = 20;
```

---

# ✅ **2. Data Types & Type Coercion**

JavaScript has:

* Primitive: string, number, boolean, null, undefined, symbol, bigint
* Non-primitive: objects, arrays, functions

### Trick

```js
console.log(typeof null); // "object"
console.log(typeof NaN);  // "number"
```

### Coercion example:

```js
console.log(1 + "1");  // "11"
console.log(1 - "1");  // 0
console.log("5" * 2);  // 10
```

---

# ✅ **3. Functions & Scopes**

### Function Scope vs Block Scope

```js
function test() {
  if (true) {
    var x = 10;
    let y = 20;
  }
  console.log(x); // 10
  console.log(y); // Error
}
```

---

# ✅ **4. Closures (Medium Level)**

A closure remembers its outer scope.

### Example

```js
function outer() {
  let count = 0;
  return function () {
    count++;
    console.log(count);
  };
}
const inc = outer();
inc(); // 1
inc(); // 2
```

---

# ✅ **5. `this` Keyword Basics**

### Normal function:

`this` depends on *how* the function is called.

### Arrow function:

`this` is taken from outer scope (lexical).

### Example

```js
const obj = {
  name: "JS",
  show() { console.log(this.name); }, 
  arrow: () => console.log(this.name)
};

obj.show();   // "JS"
obj.arrow();  // undefined or "window.name"
```

---

# ✅ **6. Arrays — map, filter, reduce**

### `map`

```js
[1,2,3].map(x => x*2); // [2,4,6]
```

### `filter`

```js
[1,2,3,4].filter(x => x % 2 === 0); // [2,4]
```

### `reduce`

```js
[1,2,3].reduce((sum, x) => sum + x, 0); // 6
```

---

# ✅ **7. Spread & Rest Operators**

### Spread

```js
const arr = [1,2,3];
const newArr = [...arr, 4]; // [1,2,3,4]
```

### Rest

```js
function sum(...nums) {
  return nums.reduce((a,b) => a+b);
}
sum(1, 2, 3); // 6
```

---

# ✅ **8. Promises (Medium Level)**

### Basic Promise

```js
const p = new Promise((resolve, reject) => {
  resolve("Done");
});

p.then(console.log); // "Done"
```

### Promise chaining

```js
Promise.resolve(2)
  .then(x => x * 2)
  .then(x => x + 1)
  .then(console.log); // 5
```

---

# ✅ **9. Async / Await**

```js
function delay() {
  return new Promise(resolve => setTimeout(resolve, 1000, "Hello"));
}

async function show() {
  const msg = await delay();
  console.log(msg);
}
show();
```

---

# ✅ **10. Event Loop Basics (Medium)**

Order of execution:

1. Synchronous code
2. Microtasks (Promise callbacks)
3. Macrotasks (setTimeout)

### Example

```js
console.log("A");

setTimeout(() => console.log("B"));

Promise.resolve().then(() => console.log("C"));

console.log("D");
```

### Output:

```
A
D
C
B
```

---

# ✅ **11. Hoisting**

### Functions are hoisted fully:

```js
say(); // works
function say() { console.log("Hi"); }
```

### Variables hoisted but not initialized:

```js
console.log(a); // undefined
var a = 5;

console.log(b); // error
let b = 10;
```

---

# ✅ **12. Prototypal Inheritance**

```js
const parent = { x: 1 };
const child = Object.create(parent);

console.log(child.x); // 1 (from prototype)

child.x = 5;
console.log(parent.x, child.x); // 1, 5
```

