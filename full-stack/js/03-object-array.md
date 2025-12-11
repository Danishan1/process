# ✅ **1. OBJECTS — BASIC to MEDIUM**

## **1.1 Creating an Object**

```js
const person = {
  name: "John",
  age: 25
};
```

---

## **1.2 Accessing Properties**

```js
console.log(person.name);   // dot notation
console.log(person["age"]); // bracket notation
```

Bracket notation is required when:

* Key has spaces
* Key starts with number
* Key is dynamic

```js
const key = "email";
person[key] = "john@mail.com";
```

---

## **1.3 Adding / Updating Properties**

```js
person.city = "Delhi";
person.age = 30;
```

---

## **1.4 Deleting Properties**

```js
delete person.age;
```

---

## **1.5 Looping Through Object**

### `for...in`

```js
for (let key in person) {
  console.log(key, person[key]);
}
```

---

## **1.6 Object.keys / values / entries**

```js
Object.keys(person);   // ["name", "city"]
Object.values(person); // ["John", "Delhi"]
Object.entries(person); // [["name", "John"], ["city", "Delhi"]]
```

---

## **1.7 Copying Objects**

### ❌ WRONG (shallow copy reference)

```js
const p2 = person; // same memory
```

### ✅ Correct shallow copy

```js
const p2 = { ...person };
```

### Deep copy (interview):

```js
const deepCopy = JSON.parse(JSON.stringify(person));
```

---

## **1.8 Object Destructuring**

```js
const user = { name: "Danish", age: 22 };

const { name, age } = user;

console.log(name, age);
```

Rename:

```js
const { name: username } = user;
```

---

## **1.9 Nested Object Destructuring**

```js
const obj = {
  user: {
    profile: {
      email: "a@b.com"
    }
  }
};

const { user: { profile: { email } } } = obj;
```

---

## **1.10 Object Methods**

```js
const car = {
  brand: "BMW",
  start() {
    console.log("Starting...");
  }
};

car.start();
```

---

## **1.11 "this" in Objects**

```js
const p = {
  name: "JS",
  show() {
    console.log(this.name);
  }
};

p.show(); // JS
```

---

# ✅ **2. ARRAYS — BASIC to MEDIUM**

## **2.1 Creating Arrays**

```js
const nums = [1, 2, 3];
```

---

## **2.2 Accessing Items**

```js
nums[0]; // 1
nums[2]; // 3
```

---

## **2.3 Adding / Removing Items**

### Add end — `push`

```js
nums.push(4);
```

### Remove end — `pop`

```js
nums.pop();
```

### Add at start — `unshift`

```js
nums.unshift(0);
```

### Remove from start — `shift`

```js
nums.shift();
```

---

## **2.4 Looping Arrays**

### For

```js
for (let i = 0; i < nums.length; i++) {
  console.log(nums[i]);
}
```

### for...of (BEST)

```js
for (let n of nums) {
  console.log(n);
}
```

---

## **2.5 Array Destructuring**

```js
const arr = [10, 20, 30];
const [a, b] = arr;

console.log(a, b); // 10 20
```

Skip values:

```js
const [x, , y] = [1, 2, 3];
```

---

# 🔥 **2.6 Important Array Methods (INTERVIEW USE)**

## **map() — transform**

```js
[1, 2, 3].map(x => x * 2); // [2, 4, 6]
```

---

## **filter() — keep only what matches**

```js
[1, 2, 3, 4].filter(x => x % 2 === 0); // [2,4]
```

---

## **reduce() — accumulate**

```js
[1, 2, 3].reduce((sum, n) => sum + n, 0); // 6
```

---

## **find() — find first matching**

```js
[10, 20, 30].find(n => n > 15); // 20
```

---

## **findIndex()**

```js
[10, 20, 30].findIndex(n => n === 20); // 1
```

---

## **some() — at least one true?**

```js
[1,2,3].some(n => n > 2); // true
```

---

## **every() — all true?**

```js
[1,2,3].every(n => n > 0); // true
```

---

## **includes()**

```js
[1, 2, 3].includes(2); // true
```

---

# 🔥 **2.7 Spread & Rest with Arrays**

### Spread

```js
const a1 = [1,2];
const a2 = [...a1, 3,4]; // [1,2,3,4]
```

### Rest

```js
function sum(...nums) {
  return nums.reduce((a,b) => a+b);
}

sum(1,2,3); // 6
```

---

# 🔥 **2.8 Array of Objects — Interview Common**

```js
const users = [
  { id: 1, name: "A" },
  { id: 2, name: "B" }
];

const names = users.map(u => u.name); // ["A", "B"]
```

Filter:

```js
users.filter(u => u.id !== 1);
```

Find:

```js
users.find(u => u.id === 2);
```

---

# 🔥 **2.9 Sorting Arrays**

### Numbers

```js
[10, 2, 5].sort((a, b) => a - b); // [2,5,10]
```

### Strings

```js
["banana", "apple"].sort();
```
