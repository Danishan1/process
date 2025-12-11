
# ✅ **1. CONDITIONS**

Covers: `if`, `else if`, ternary, logical ops, switch, truthy/falsy.

---

## **1.1 Basic Condition**

```js
let x = 10;

if (x > 5) {
  console.log("greater");
} else {
  console.log("smaller");
}
```

---

## **1.2 Multiple Conditions**

```js
let age = 20;

if (age < 13) {
  console.log("Child");
} else if (age < 18) {
  console.log("Teen");
} else {
  console.log("Adult");
}
```

---

## **1.3 Logical Operators**

### `&&` (AND)

```js
if (age > 18 && age < 60) { ... }
```

### `||` (OR)

```js
if (age === 0 || age === null) { ... }
```

### `!` (NOT)

```js
if (!isLoggedIn) { ... }
```

---

## **1.4 Truthy / Falsy**

Falsy values:

```
false, 0, "", null, undefined, NaN
```

### Example:

```js
if (" ") console.log("Truthy"); // runs (non-empty string)
if ("") console.log("Falsy");   // doesn't run
```

---

## **1.5 Ternary Operator**

```js
let access = age > 18 ? "Allowed" : "Denied";
```

---

## **1.6 Switch Statement**

```js
let day = 2;

switch(day) {
  case 1: console.log("Mon"); break;
  case 2: console.log("Tue"); break;
  default: console.log("Unknown");
}
```

---

# ✅ **2. LOOPS**

Covers: `for`, `while`, `do-while`, `for…of`, `for…in`, array loops.

---

## **2.1 Basic For Loop**

```js
for (let i = 0; i < 5; i++) {
  console.log(i);
}
```

---

## **2.2 While Loop**

```js
let i = 0;
while (i < 3) {
  console.log(i);
  i++;
}
```

---

## **2.3 Do-While Loop** (runs at least once)

```js
let n = 5;
do {
  console.log("Runs once");
} while (n < 5);
```

---

## **2.4 for…of (BEST FOR arrays)**

```js
const arr = [10, 20, 30];

for (const num of arr) {
  console.log(num);
}
```

---

## **2.5 for…in (BEST FOR objects)**

```js
const obj = { a: 1, b: 2 };

for (const key in obj) {
  console.log(key, obj[key]);
}
```

---

## **2.6 Looping Over Arrays — Interview Style**

### `map` — transform array

```js
[1,2,3].map(x => x * 2);   // [2,4,6]
```

### `filter` — filter items

```js
[1,2,3,4].filter(x => x % 2 === 0);  // [2,4]
```

### `reduce` — accumulate

```js
[1,2,3].reduce((acc, x) => acc + x, 0); // 6
```

---

# ✅ **3. FUNCTIONS**

Covers: declaration, expression, arrow, parameters, return, scope.

---

## **3.1 Function Declaration**

```js
function add(a, b) {
  return a + b;
}
```

---

## **3.2 Function Expression**

```js
const add = function(a, b) {
  return a + b;
};
```

---

## **3.3 Arrow Functions**

```js
const add = (a, b) => a + b;
```

---

## **3.4 Default Parameters**

```js
function greet(name = "Guest") {
  console.log("Hello " + name);
}
```

---

## **3.5 Rest Parameters**

```js
function sum(...nums) {
  return nums.reduce((a, b) => a + b);
}

sum(1, 2, 3, 4); // 10
```

---

## **3.6 Return Basics**

```js
function say() {
  return "Hi";
  console.log("Nope"); // never runs
}
```

---

## **3.7 Callback Function**

```js
function processUser(name, callback) {
  callback(name);
}

processUser("John", (n) => console.log("Hello " + n));
```

---

## **3.8 Higher-Order Functions**

Functions that return functions or accept functions.

```js
function multiplier(x) {
  return function(y) {
    return x * y;
  };
}

const double = multiplier(2);
console.log(double(5)); // 10
```
