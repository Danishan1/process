For **DSA / Competitive Programming in JavaScript (Node.js)**, the standard way to take console input is using `fs.readFileSync(0, 'utf8')`.

 ### 1\. Read all input

```
const fs = require('fs');

const input = fs.readFileSync(0, 'utf8').trim();

console.log(input);
```

 If the input is:

```
5
10 20 30 40 50
```

 then `input` contains the entire input as a string.

 ### 2\. Convert input into an array of tokens

 This is the most common CP pattern:

```
const fs = require('fs');

const input = fs.readFileSync(0, 'utf8').trim().split(/\s+/);

let idx = 0;

const n = Number(input[idx++]);

const arr = [];

for (let i = 0; i < n; i++) {
    arr.push(Number(input[idx++]));
}

console.log(arr);
```

 For:

```
5
10 20 30 40 50
```

 Output:

```
[10, 20, 30, 40, 50]
```

 ### 3\. Shorter approach for numbers

 If **all input is numbers**, you can do:

```
const fs = require('fs');

const input = fs.readFileSync(0, 'utf8')
    .trim()
    .split(/\s+/)
    .map(Number);

let idx = 0;

const n = input[idx++];
const arr = input.slice(idx, idx + n);

console.log(arr);
```

 ### 4\. Example DSA problem

 Input:

```
5
1 2 3 4 5
```

 Find the sum:

```
const fs = require('fs');

const input = fs.readFileSync(0, 'utf8').trim().split(/\s+/).map(Number);

let idx = 0;

const n = input[idx++];

let sum = 0;

for (let i = 0; i < n; i++) {
    sum += input[idx++];
}

console.log(sum);
```

 Output:

```
15
```

 ### CP template to remember

```
const fs = require('fs');

const input = fs.readFileSync(0, 'utf8').trim().split(/\s+/);
let idx = 0;

// Read integer
const n = Number(input[idx++]);

// Read array
const arr = [];
for (let i = 0; i < n; i++) {
    arr.push(Number(input[idx++]));
}

// Your solution here
```

 **Key idea:** `input[idx++]` reads the next token, and `Number(...)` converts it from a string to a number. This pattern works very well for Codeforces, LeetCode-style contests, CSES, HackerRank, etc.