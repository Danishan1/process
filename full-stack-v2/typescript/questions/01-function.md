Exactly. The goal should be **not just remembering definitions**, but being forced to predict behavior, explain *why*, and connect multiple concepts.

Let's build this as a progressive question bank.

# 🧠 Functions — Question Bank

## 🟢 Level 1 — Beginner

**Goal:** Understand and identify individual concepts.

1. What is a function, and why do we use functions?
2. What is the difference between a **parameter** and an **argument**?
3. What does `return` do?
4. What happens when a function doesn't have a `return` statement?
5. What's the difference between a **function declaration** and a **function expression**?
6. What is an arrow function?
7. Convert this into an arrow function:

   ```ts
   function add(a: number, b: number) {
     return a + b;
   }
   ```
8. What is a callback function?
9. Is every callback asynchronous?
10. Is a callback the same thing as a Promise?
11. What is a higher-order function?
12. What does "functions are first-class citizens" mean?
13. What is scope?
14. What is the difference between global and local scope?
15. What is a closure?
16. What is `this` used for?
17. What is a Promise?
18. What does `async` do?
19. What does `await` do?
20. What is recursion?

# 🟡 Level 2 — Intermediate

**Goal:** Predict behavior and connect concepts.

### Callbacks & Higher-Order Functions

21. What will this print?

```ts
function process(callback: () => void) {
  console.log("A");
  callback();
  console.log("C");
}

process(() => console.log("B"));
```

22. Why is `map()` considered a higher-order function?

23. In this code, identify the **higher-order function** and the **callback**:

```ts
const numbers = [1, 2, 3];

numbers.map(n => n * 2);
```

24. Write your own `myMap()` function that accepts an array and a callback.

25. Can a function be both a callback and a higher-order function?

### Scope & Closures

26. What will this print?

```ts
let x = 10;

function test() {
  let x = 20;
  console.log(x);
}

test();
console.log(x);
```

27. What will this print?

```ts
function outer() {
  let count = 0;

  return function inner() {
    count++;
    console.log(count);
  };
}

const fn = outer();

fn();
fn();
fn();
```

28. Why doesn't `count` disappear after `outer()` finishes?

29. Create a `createCounter()` function using closure.

30. Create a function that remembers a user's name using closure.

### `this`

31. What will this print?

```ts
const user = {
  name: "John",
  greet() {
    console.log(this.name);
  }
};

user.greet();
```

32. What happens to `this` when the method is extracted?

```ts
const greet = user.greet;
greet();
```

33. Why does an arrow function behave differently with `this`?

34. When would you choose a normal function over an arrow function?

### Async / Promise

35. What does an `async` function return?

36. What is the difference between:

```ts
return value;
```

and

```ts
return Promise.resolve(value);
```

inside an `async` function?

37. What will execute first?

```ts
console.log("A");

setTimeout(() => {
  console.log("B");
}, 0);

console.log("C");
```

38. Why doesn't `setTimeout(..., 0)` execute immediately?

39. Convert this Promise code into `async/await`:

```ts
getUser()
  .then(user => console.log(user))
  .catch(error => console.log(error));
```

40. What is the difference between a callback and `async/await`?

# 🔴 Level 3 — Hard

**Goal:** Combine concepts and reason about JavaScript's behavior.

### Closures + Loops

41. What will this print, and **why**?

```ts
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 0);
}
```

42. What changes if `var` becomes `let`?

```ts
for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 0);
}
```

43. Can you reproduce the `let` behavior using `var` and a closure?

### Closures + Functions

44. What does this print?

```ts
function createCounter() {
  let count = 0;

  return {
    increment: () => ++count,
    decrement: () => --count,
    getCount: () => count
  };
}

const counter = createCounter();

console.log(counter.increment());
console.log(counter.increment());
console.log(counter.decrement());
console.log(counter.getCount());
```

45. Why can't we directly access `count`?

46. What JavaScript concept is being used to create this private state?

### Higher-Order Functions

47. Implement your own:

```ts
myFilter(array, callback)
```

without using `.filter()`.

48. Implement your own:

```ts
myReduce(array, callback, initialValue)
```

49. Can you implement `map()` using `reduce()`?

50. Write a function:

```ts
compose(f, g)
```

such that:

```ts
compose(f, g)(x)
```

means:

```ts
f(g(x))
```

### `this` + Arrow Functions

51. What will this output?

```ts
const user = {
  name: "John",

  regular: function () {
    console.log(this.name);
  },

  arrow: () => {
    console.log(this.name);
  }
};

user.regular();
user.arrow();
```

52. Why are arrow functions commonly used inside callbacks?

53. What problem does an arrow function solve in code like this?

```ts
class Counter {
  count = 0;

  start() {
    setTimeout(() => {
      this.count++;
    }, 1000);
  }
}
```

### Async + Event Loop

54. What is the exact output order?

```ts
console.log("1");

Promise.resolve().then(() => {
  console.log("2");
});

setTimeout(() => {
  console.log("3");
}, 0);

console.log("4");
```

55. Why does the Promise callback execute before the `setTimeout` callback?

56. What is the difference between **synchronous code**, **microtasks**, and **macrotasks**?

57. Predict the output:

```ts
async function test() {
  console.log("A");

  await Promise.resolve();

  console.log("B");
}

console.log("C");

test();

console.log("D");
```

### Deep Conceptual Questions

58. If JavaScript is single-threaded, how can it handle asynchronous operations?

59. Why does `async/await` not actually make JavaScript synchronous?

60. Explain the relationship between:

```text
Function
↓
First-class function
↓
Callback
↓
Higher-order function
↓
Closure
↓
Async callback
↓
Promise
↓
async/await
↓
Event loop
```

# 🏆 Bonus: "Think Like an Interviewer"

These are less about writing code and more about **explaining your mental model**:

61. Why are closures useful in real applications?

62. Why are callbacks necessary if we have Promises?

63. Why are Promises considered better than deeply nested callbacks?

64. Is `async/await` replacing Promises, or is it built on top of them?

65. Can you have a closure without an arrow function?

66. Can you have a callback without asynchronous code?

67. Can a function be both a callback and a closure?

68. Can a higher-order function return a Promise?

69. Can a callback itself return a Promise?

70. Explain **closure + callback + Promise + async/await** using one real-world example.
