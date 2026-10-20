Yes — for **revision**, it should be much more compact: syntax + parameters + one-line purpose, without explanations.

 JavaScript CP / DSA — Quick Revision Sheet

# JavaScript CP / DSA — Quick Revision

 ## 1\. Input / Output

```
const fs = require("fs");

const input = fs.readFileSync(0, "utf8").trim().split(/\s+/);
let idx = 0;

const next = () => input[idx++];
const nextInt = () => Number(input[idx++]);
```

 | Syntax | Purpose |
| --- | --- |
| `fs.readFileSync(0, "utf8")` | Read stdin |
| `.trim()` | Remove outer whitespace |
| `.split(/\s+/)` | Split by whitespace |
| `Number(x)` | String → Number |
| `parseInt(x, 10)` | String → Integer |
| `parseFloat(x)` | String → Float |
| `console.log(x)` | Output |
| `arr.join("\n")` | Multiple-line output |

---

 ## 2\. Variables / Numbers

```
let x = 10;
let big = 12345678901234567890n;
let s = "abc";
let ok = true;
```

```
Infinity
-Infinity
Number.MAX_SAFE_INTEGER
Number.MIN_SAFE_INTEGER
```

 > `Number` safe integer: `±(2^53 - 1)`\
>  Use `BigInt` for larger exact integers.

---

 ## 3\. Arrays

 ### Creation

```
[]
new Array(n)
new Array(n).fill(0)

Array.from({length: n}, () => Array(m).fill(0))
```

 ### Methods

 | Method | Parameters | Purpose | Complexity |
| --- | --- | --- | --- |
| `push(x)` | items | Add end | O(1)\* |
| `pop()` | — | Remove end | O(1) |
| `unshift(x)` | items | Add start | O(n) |
| `shift()` | — | Remove start | O(n) |
| `slice(l,r)` | start, end | Copy `[l,r)` | O(k) |
| `splice(i,d,...x)` | index, deleteCount, items | Modify | O(n) |
| `includes(x)` | value, fromIndex | Exists? | O(n) |
| `indexOf(x)` | value, fromIndex | Find index | O(n) |
| `reverse()` | — | Reverse | O(n) |
| `sort(fn)` | comparator | Sort | O(n log n) |

 `*` amortized.

 ### Common

```
arr.length
arr[i]
arr[arr.length - 1]

[...arr]             // copy
[...a, ...b]         // merge
[...new Set(arr)]    // unique
[a, b] = [b, a]     // swap
```

---

 ## 4\. Array Processing

 | Method | Parameters | Purpose |
| --- | --- | --- |
| `map(fn)` | `(value,index,array)` | Transform |
| `filter(fn)` | `(value,index,array)` | Keep matching |
| `reduce(fn,init)` | `(acc,value,index,array)` | Single result |
| `find(fn)` | callback | First value |
| `findIndex(fn)` | callback | First index |

```
arr.map(x => x * 2);

arr.filter(x => x > 5);

arr.reduce((sum, x) => sum + x, 0);

arr.find(x => x > 5);

arr.findIndex(x => x > 5);
```

---

 ## 5\. Sorting

```
arr.sort((a, b) => a - b); // ascending
arr.sort((a, b) => b - a); // descending
arr.reverse();
```

 ### Custom

```
arr.sort((a, b) => a.x - b.x);
```

 Multiple conditions:

```
arr.sort((a, b) =>
    a.x !== b.x ? a.x - b.x : a.y - b.y
);
```

 > `arr.sort()` → lexical/string sort.\
>  Always use comparator for numbers.

---

 ## 6\. Strings

 | Syntax | Purpose |
| --- | --- |
| `s.length` | Length |
| `s[i]` | Character |
| `s.charAt(i)` | Character |
| `s.split("")` | String → chars |
| `s.split(" ")` | String → words |
| `arr.join("")` | Array → string |
| `s.slice(l,r)` | `[l,r)` |
| `s.substring(l,r)` | `[l,r)` |
| `s.includes(x)` | Contains? |
| `s.indexOf(x)` | Position |
| `s.toUpperCase()` | Uppercase |
| `s.toLowerCase()` | Lowercase |

 ### Reverse

```
s.split("").reverse().join("");
```

 ### Character ↔ Code

```
s.charCodeAt(i);
String.fromCharCode(x);
```

---

 ## 7\. Map

```
const mp = new Map();
```

 | Method | Parameters | Purpose |
| --- | --- | --- |
| `set(k,v)` | key, value | Insert/update |
| `get(k)` | key | Get value |
| `has(k)` | key | Check key |
| `delete(k)` | key | Remove |
| `clear()` | — | Remove all |
| `size` | — | Count |

 ### Frequency

```
const freq = new Map();

for (const x of arr) {
    freq.set(x, (freq.get(x) || 0) + 1);
}
```

---

 ## 8\. Set

```
const set = new Set();
```

 | Method | Purpose |
| --- | --- |
| `add(x)` | Insert |
| `has(x)` | Check |
| `delete(x)` | Remove |
| `clear()` | Remove all |
| `size` | Count |

```
const unique = [...new Set(arr)];
```

---

 ## 9\. Object Hashing

```
const freq = {};

freq[x] = (freq[x] || 0) + 1;
```

 | Syntax | Purpose |
| --- | --- |
| `Object.keys(obj)` | Keys |
| `Object.values(obj)` | Values |
| `Object.entries(obj)` | `[key,value]` |

---

 ## 10\. Loops

```
for (let i = 0; i < n; i++) {}

for (const x of arr) {}

while (condition) {}

do {} while (condition);
```

 | Loop | Use |
| --- | --- |
| `for` | Index |
| `for...of` | Values |
| `while` | Two pointers / window |

---

 ## 11\. Functions

```
function solve(a, b) {
    return a + b;
}

const add = (a, b) => a + b;
```

```
function fn(param1, param2) {}
```

 Parameters = input values.

---

 ## 12\. Math

 | Function | Parameters | Purpose |
| --- | --- | --- |
| `Math.max(a,b,...)` | values | Maximum |
| `Math.min(a,b,...)` | values | Minimum |
| `Math.abs(x)` | x | Absolute |
| `Math.sqrt(x)` | x | Square root |
| `Math.pow(a,b)` | base, exponent | Power |
| `Math.floor(x)` | x | ↓ |
| `Math.ceil(x)` | x | ↑ |
| `Math.round(x)` | x | Nearest |

```
Math.max(...arr);
Math.min(...arr);

2 ** 3;

Math.floor(a / b); // integer division
a % b;              // remainder
```

---

 ## 13\. Bit Manipulation

 | Operation | Syntax |
| --- | --- |
| AND | `a & b` |
| OR | `a \| b` |
| XOR | `a ^ b` |
| Left shift | `a << b` |
| Right shift | `a >> b` |
| Unsigned right | `a >>> b` |
| NOT | `~a` |

```
(x & 1) === 0              // even
(x & 1) === 1              // odd
x & (x - 1)                // remove lowest set bit
x > 0 && (x & (x - 1))===0 // power of 2
```

 > Bitwise operations use 32-bit integers. Use `BigInt` for larger values.

---

 ## 14\. Stack

```
const stack = [];

stack.push(x);
stack.pop();
stack[stack.length - 1]; // top
```

 **Use:** DFS, brackets, monotonic stack.

---

 ## 15\. Queue

```
const queue = [];
let front = 0;

queue.push(x);

while (front < queue.length) {
    const x = queue[front++];
}
```

 > Avoid `shift()` for large queues.

 **Use:** BFS.

---

 ## 16\. Heap

 Zero-based indexing:

```
parent = Math.floor((i - 1) / 2);
left   = 2 * i + 1;
right  = 2 * i + 2;
```

 | Operation | Complexity |
| --- | --- |
| Peek | O(1) |
| Insert | O(log n) |
| Remove min/max | O(log n) |

 **Use:** Priority Queue, Dijkstra, K-th element.

---

 ## 17\. Graph

 ### Adjacency List

```
const graph = Array.from({length: n}, () => []);

graph[u].push(v);
```

 ### Undirected

```
graph[u].push(v);
graph[v].push(u);
```

 ### Weighted

```
graph[u].push([v, weight]);
```

---

 ## 18\. Binary Search

```
let lo = 0;
let hi = n - 1;

while (lo <= hi) {
    const mid = Math.floor((lo + hi) / 2);

    if (arr[mid] === target) {
        // found
    } else if (arr[mid] < target) {
        lo = mid + 1;
    } else {
        hi = mid - 1;
    }
}
```

 | Variable | Meaning |
| --- | --- |
| `lo` | Left |
| `hi` | Right |
| `mid` | Middle |
| `target` | Search value |

 **Time:** O(log n)

---

 ## 19\. Two Pointers

```
let left = 0;
let right = n - 1;

while (left < right) {
    if (condition) {
        left++;
    } else {
        right--;
    }
}
```

 **Time:** O(n)

---

 ## 20\. Sliding Window

```
let left = 0;

for (let right = 0; right < n; right++) {

    // add arr[right]

    while (/* invalid */) {
        // remove arr[left]
        left++;
    }

    // window = [left, right]
}
```

 **Time:** Usually O(n)

---

 ## 21\. Prefix Sum

```
const prefix = new Array(n + 1).fill(0);

for (let i = 0; i < n; i++) {
    prefix[i + 1] = prefix[i] + arr[i];
}
```

 Range sum `[l,r]`:

```
prefix[r + 1] - prefix[l];
```

 **Build:** O(n)\
 **Query:** O(1)

---

 ## 22\. Kadane

```
let current = arr[0];
let best = arr[0];

for (let i = 1; i < n; i++) {
    current = Math.max(arr[i], current + arr[i]);
    best = Math.max(best, current);
}
```

 **Maximum subarray:** O(n)

---

 ## 23\. BFS / DFS

 ### BFS

```
const queue = [start];
let front = 0;
const visited = new Set([start]);

while (front < queue.length) {
    const node = queue[front++];

    for (const next of graph[node]) {
        if (!visited.has(next)) {
            visited.add(next);
            queue.push(next);
        }
    }
}
```

 ### DFS

```
function dfs(node) {
    visited[node] = true;

    for (const next of graph[node]) {
        if (!visited[next]) {
            dfs(next);
        }
    }
}
```

 **Both:** O(V + E)

---

 ## 24\. Sieve

```
const prime = new Array(n + 1).fill(true);

prime[0] = prime[1] = false;

for (let i = 2; i * i <= n; i++) {
    if (prime[i]) {
        for (let j = i * i; j <= n; j += i) {
            prime[j] = false;
        }
    }
}
```

 **Time:** O(n log log n)

---

 ## 25\. DSU

 Core operations:

```
find(x)       → representative
union(a,b)    → merge sets
```

 **Use:** Connected components, Kruskal, connectivity.

 **Complexity:** Almost O(1) amortized with path compression + union by rank/size.

---

 # 26\. Algorithm Selection

 | Pattern | Think |
| --- | --- |
| Sorted array | Binary Search / Two Pointers |
| Pair/triplet | Two Pointers / Hashing |
| Contiguous subarray | Sliding Window / Prefix Sum |
| Range sum | Prefix Sum |
| Maximum subarray | Kadane |
| Frequency | Map |
| Unique values | Set |
| LIFO | Stack |
| FIFO | Queue |
| Shortest unweighted path | BFS |
| Deep traversal | DFS |
| Shortest weighted path | Dijkstra |
| Repeated min/max | Heap |
| Connectivity | DSU |
| Prime preprocessing | Sieve |

---

 # 27\. Complexity Quick Reference

 | Operation | Time |
| --- | --- |
| Array access | O(1) |
| `push()` | O(1)\* |
| `pop()` | O(1) |
| `shift()` | O(n) |
| `unshift()` | O(n) |
| Map/Set lookup | O(1)\* |
| Sort | O(n log n) |
| Binary Search | O(log n) |
| Two Pointers | O(n) |
| Sliding Window | O(n) |
| Prefix Sum | O(n) build |
| BFS / DFS | O(V + E) |
| Dijkstra + Heap | O((V+E) log V) |
| Sieve | O(n log log n) |

 `*` average/amortized.

---

 # 28\. CP One-Liners

```
Math.max(...arr)
Math.min(...arr)

arr.reduce((a, b) => a + b, 0)

arr.map(x => x * 2)

arr.filter(x => x % 2 === 0)

arr.find(x => x > 5)

arr.includes(x)
arr.indexOf(x)

[...new Set(arr)]

[...arr]

[...a, ...b]

[a, b] = [b, a]

arr[arr.length - 1]

s.split("").reverse().join("")
```

---

 # 29\. Contest Checklist

```
□ Input parsed correctly
□ Number conversion done
□ Numeric sort comparator used
□ Index bounds checked
□ Empty / single element cases
□ Duplicates
□ Negative values
□ Large numbers / BigInt
□ Time complexity
□ Space complexity
□ Output format
```

---

 # 30\. Final Template

```
const fs = require("fs");

const data = fs.readFileSync(0, "utf8").trim();

if (!data) process.exit(0);

const input = data.split(/\s+/);
let idx = 0;

const next = () => input[idx++];
const nextInt = () => Number(input[idx++]);

function solve() {
    const n = nextInt();

    const arr = [];

    for (let i = 0; i < n; i++) {
        arr.push(nextInt());
    }

    // solution
}

solve();
```

 ## ⭐ Must Remember

```
Input       → fs.readFileSync(0, "utf8")
Array       → push / pop / slice / splice / sort
String      → split / join / slice
Hashing     → Map / Set
Stack       → push / pop
Queue       → array + front pointer
Search      → Binary Search
Subarray    → Sliding Window / Prefix Sum / Kadane
Graph       → BFS / DFS / Dijkstra
Connectivity→ DSU
Primes      → Sieve
```