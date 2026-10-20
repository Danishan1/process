## JavaScript CP Quick Revision Table

| Category | Function / Method | Usage           | Example                 |
| -------- | ----------------- | --------------- | ----------------------- |
| **Math** | `Math.max()`      | Find maximum    | `Math.max(...arr)`      |
|          | `Math.min()`      | Find minimum    | `Math.min(...arr)`      |
|          | `Math.abs()`      | Absolute value  | `Math.abs(-5)` → `5`    |
|          | `Math.sqrt()`     | Square root     | `Math.sqrt(25)` → `5`   |
|          | `Math.pow()`      | Power           | `Math.pow(2,3)` → `8`   |
|          | `Math.floor()`    | Round down      | `Math.floor(4.9)` → `4` |
|          | `Math.ceil()`     | Round up        | `Math.ceil(4.1)` → `5`  |
|          | `Math.round()`    | Nearest integer | `Math.round(4.5)` → `5` |

---

## Array Methods

| Method       | Purpose               | Example                |
| ------------ | --------------------- | ---------------------- |
| `push()`     | Add at end            | `arr.push(5)`          |
| `pop()`      | Remove last           | `arr.pop()`            |
| `shift()`    | Remove first          | `arr.shift()`          |
| `unshift()`  | Add at start          | `arr.unshift(1)`       |
| `sort()`     | Sort array            | `arr.sort((a,b)=>a-b)` |
| `reverse()`  | Reverse array         | `arr.reverse()`        |
| `slice()`    | Copy part (no change) | `arr.slice(1,3)`       |
| `splice()`   | Modify/remove/insert  | `arr.splice(2,1)`      |
| `includes()` | Check existence       | `arr.includes(5)`      |
| `indexOf()`  | Find index            | `arr.indexOf(5)`       |

---

## Array Processing

| Method        | Purpose                 | Example                    |
| ------------- | ----------------------- | -------------------------- |
| `map()`       | Transform each item     | `arr.map(x=>x*2)`          |
| `filter()`    | Keep matching items     | `arr.filter(x=>x>5)`       |
| `reduce()`    | Convert to single value | `arr.reduce((a,b)=>a+b,0)` |
| `find()`      | First matching value    | `arr.find(x=>x>5)`         |
| `findIndex()` | Index of match          | `arr.findIndex(x=>x>5)`    |

---

## String Methods

| Method          | Purpose            | Example                  |
| --------------- | ------------------ | ------------------------ |
| `split()`       | String → Array     | `"abc".split("")`        |
| `join()`        | Array → String     | `["a","b"].join("")`     |
| `toUpperCase()` | Uppercase          | `"abc".toUpperCase()`    |
| `toLowerCase()` | Lowercase          | `"ABC".toLowerCase()`    |
| `charAt()`      | Character at index | `"abc".charAt(1)`        |
| `substring()`   | Extract part       | `"hello".substring(1,4)` |

---

## Hashing / Frequency

| Tool           | Purpose           | Example           |
| -------------- | ----------------- | ----------------- |
| `Set`          | Unique values     | `new Set(arr)`    |
| `set.add()`    | Add value         | `s.add(5)`        |
| `set.has()`    | Check value       | `s.has(5)`        |
| `set.delete()` | Remove value      | `s.delete(5)`     |
| `set.size`     | Count values      | `s.size`          |
| `Map`          | Key-value storage | `new Map()`       |
| `map.set()`    | Insert            | `mp.set(key,val)` |
| `map.get()`    | Retrieve          | `mp.get(key)`     |
| `map.has()`    | Check key         | `mp.has(key)`     |

---

## Object as HashMap

| Function           | Purpose         | Example               |
| ------------------ | --------------- | --------------------- |
| `Object.keys()`    | Get keys        | `Object.keys(obj)`    |
| `Object.values()`  | Get values      | `Object.values(obj)`  |
| `Object.entries()` | Key-value pairs | `Object.entries(obj)` |

Frequency pattern:

```javascript
freq[x] = (freq[x] || 0) + 1;
```

---

## Conversion

| Function       | Purpose            | Example              |
| -------------- | ------------------ | -------------------- |
| `Number()`     | String → Number    | `Number("123")`      |
| `parseInt()`   | Integer conversion | `parseInt("12")`     |
| `parseFloat()` | Decimal conversion | `parseFloat("3.14")` |
| `String()`     | Convert to string  | `String(123)`        |

---

## Common CP Tricks

| Task              | Code                               |
| ----------------- | ---------------------------------- |
| Copy array        | `[...arr]`                         |
| Merge arrays      | `[...a,...b]`                      |
| Remove duplicates | `[...new Set(arr)]`                |
| Array length      | `arr.length`                       |
| Last element      | `arr[arr.length-1]`                |
| Swap values       | `[a,b]=[b,a]`                      |
| Reverse string    | `str.split("").reverse().join("")` |
| Loop values       | `for(const x of arr)`              |
| Loop indexes      | `for(const i in arr)`              |

---

## Most Used in LeetCode (Top 20)

| Rank | Function           |
| ---- | ------------------ |
| ⭐ 1  | `sort((a,b)=>a-b)` |
| ⭐ 2  | `push()`           |
| ⭐ 3  | `pop()`            |
| ⭐ 4  | `slice()`          |
| ⭐ 5  | `splice()`         |
| ⭐ 6  | `reverse()`        |
| ⭐ 7  | `Map`              |
| ⭐ 8  | `Set`              |
| ⭐ 9  | `Object.entries()` |
| ⭐ 10 | `Math.max()`       |
| ⭐ 11 | `Math.min()`       |
| ⭐ 12 | `includes()`       |
| ⭐ 13 | `indexOf()`        |
| ⭐ 14 | `split()`          |
| ⭐ 15 | `join()`           |
| ⭐ 16 | `reduce()`         |
| ⭐ 17 | `filter()`         |
| ⭐ 18 | `map()`            |
| ⭐ 19 | `shift()`          |
| ⭐ 20 | `unshift()`        |

This table covers most JavaScript tools needed for **arrays, strings, hashing, two pointers, sliding window, sorting, and greedy problems**.
