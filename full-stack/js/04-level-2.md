# ✅ **1. Destructuring Deep Dive (Objects + Arrays + Nested + Functions)**

---

## **1.1 Basic Object Destructuring**

```js
const user = { name: "John", age: 25 };
const { name, age } = user;
```

---

## **1.2 Renaming Keys**

```js
const { name: username } = user;
console.log(username); // "John"
```

---

## **1.3 Default Values**

```js
const { city = "Unknown" } = user;
console.log(city); // "Unknown"
```

---

## **1.4 Nested Destructuring**

```js
const obj = {
  user: {
    profile: {
      email: "a@mail.com"
    }
  }
};

const { user: { profile: { email } } } = obj;

console.log(email);
```

---

## **1.5 Array Destructuring**

```js
const arr = [10, 20, 30];
const [a, b] = arr; // 10, 20
```

Skipping:

```js
const [x, , y] = [1, 2, 3];
```

---

## **1.6 Swap Values**

```js
let x = 1, y = 2;
[x, y] = [y, x];

console.log(x, y); // 2, 1
```

---

## **1.7 Rest with Destructuring**

```js
const [first, ...others] = [10, 20, 30, 40];
```

---

## **1.8 Function Parameter Destructuring**

```js
function print({ name, age }) {
  console.log(name, age);
}

print({ name: "Danish", age: 22 });
```

---

# ✅ **2. Spread vs Rest — Interview Differences**

---

## **2.1 Spread = EXPAND**

```js
const arr = [1,2];
console.log(...arr); // 1 2
```

Used in:

* Copy arrays
* Merge arrays
* Expand into function

---

## **2.2 Rest = COLLECT**

```js
function sum(...nums) {
  return nums.reduce((a,b) => a+b);
}
```

Used in:

* Function parameters
* Destructuring

---

## ❗ Real Interview Example

```js
const arr = [1,2,3,4];
const [a, ...b] = arr;

console.log(a, b);
```

Output:

```
1 [2,3,4]
```

---

# ✅ **3. Output-Based Tricky Questions (Objects + Arrays)**

---

## **Q1.**

```js
console.log(typeof []);
console.log(typeof {});
console.log(Array.isArray([]));
```

Output:

```
object
object
true
```

---

## **Q2.**

```js
const obj = { a: 1 };
const copy = obj;

copy.a = 5;

console.log(obj.a);
```

Output:

```
5
```

(because objects are reference types)

---

## **Q3. Nested Copy**

```js
const obj = { x: { y: 1 } };
const c = { ...obj };

c.x.y = 10;

console.log(obj.x.y);
```

Output:

```
10
```

(Spread does **shallow** copy only)

---

## **Q4.**

```js
const arr = [1, 2, 3];
arr[10] = 5;

console.log(arr.length);
```

Output:

```
11
```

---

# ✅ **4. Advanced Array Manipulations**

---

## **4.1 Flatten Array**

```js
const flat = [1, [2, [3, 4]]].flat(2);
console.log(flat);
```

Output:

```
[1,2,3,4]
```

---

## **4.2 Remove duplicates**

```js
const unique = [...new Set([1,2,2,3])];
```

---

## **4.3 Grouping by property**

```js
const people = [
  { name: "A", city: "Delhi" },
  { name: "B", city: "Delhi" },
  { name: "C", city: "Mumbai" }
];

const grouped = people.reduce((acc, p) => {
  acc[p.city] = acc[p.city] || [];
  acc[p.city].push(p);
  return acc;
}, {});
```

Output:

```
{
  Delhi: [{...},{...}],
  Mumbai: [{...}]
}
```

---

## **4.4 Sorting array of objects**

```js
users.sort((a, b) => a.age - b.age);
```

---

## **4.5 Chunk array**

```js
function chunk(arr, size) {
  const res = [];
  for (let i = 0; i < arr.length; i += size) {
    res.push(arr.slice(i, i + size));
  }
  return res;
}

console.log(chunk([1,2,3,4,5], 2));
```

Output:

```
[[1,2],[3,4],[5]]
```

---

# ✅ **5. Objects vs Arrays — Practical Differences**

| Feature  | Object                  | Array                  |
| -------- | ----------------------- | ---------------------- |
| Type     | Key-value pairs         | Ordered list           |
| Index    | String keys             | Number indexes         |
| Length   | No `length`             | Has `length`           |
| Loop     | `for...in`, Object.keys | `for`, `map`, `filter` |
| Use Case | Structured data         | Collections / lists    |

---

## **Check type**

```js
typeof {}  // object
typeof []  // object
Array.isArray([]) // true
```

---

# ✅ **6. Map / Set / WeakMap / WeakSet — Interview Deep Dive**

---

# **6.1 Map**

✔ Key/value pairs
✔ Keys can be **objects, functions, arrays**

```js
const map = new Map();
map.set("a", 1);
map.set({ x:10 }, "obj");

console.log(map.size);
```

---

# **6.2 Set**

✔ Stores unique values

```js
const set = new Set([1,2,2,3]);
console.log([...set]); // [1,2,3]
```

---

# **6.3 WeakMap**

✔ Keys MUST be objects
✔ Garbage-collectable
✔ Not iterable

Used in:

* private data storage in classes
* memory-safe caches

```js
let obj = {};
const wm = new WeakMap();

wm.set(obj, "data");

obj = null; // GC cleans
```

---

# **6.4 WeakSet**

✔ Only objects
✔ No duplicates
✔ Not iterable

```js
const ws = new WeakSet();
ws.add({});
```
