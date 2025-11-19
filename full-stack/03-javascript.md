
# JavaScript Interview Questions and Answers

## 1. What is JavaScript?

JavaScript is a high-level, dynamic, interpreted language used to create interactive and dynamic behavior on web pages. It is the core language of the web alongside HTML and CSS.

---

## 2. What are the data types in JavaScript?

### Primitive Types:

* string
* number
* boolean
* null
* undefined
* bigint
* symbol

### Non-Primitive:

* object
* array
* function

---

## 3. What is the difference between `var`, `let`, and `const`?

| Feature       | var                                | let                       | const                     |
| ------------- | ---------------------------------- | ------------------------- | ------------------------- |
| Scope         | function scoped                    | block scoped              | block scoped              |
| Redeclaration | allowed                            | not allowed               | not allowed               |
| Reassignment  | allowed                            | allowed                   | not allowed               |
| Hoisting      | hoisted (initialized as undefined) | hoisted (not initialized) | hoisted (not initialized) |

---

## 4. What is hoisting?

Hoisting is JavaScript’s behavior of moving variable and function declarations to the top of the scope before execution.
`var` variables are hoisted with default value undefined, while `let` and `const` are hoisted but remain in “temporal dead zone.”

---

## 5. What are closures?

A closure is formed when an inner function retains access to variables from its outer function even after the outer function has returned.

Closures enable:

* Data privacy
* Function factories
* Implementing once-only functions

---

## 6. What is the event loop?

The event loop is responsible for handling asynchronous operations in JavaScript.
It monitors the call stack and the callback/microtask queues and executes code in a non-blocking manner.

---

## 7. What are synchronous vs asynchronous operations?

* **Synchronous:** Code is executed line-by-line; each line waits for the previous one to finish.
* **Asynchronous:** Code does not block execution; operations like API calls run in the background.

---

## 8. What is a Promise?

A Promise is an object representing the eventual result of an asynchronous operation.

States:

* pending
* fulfilled
* rejected

---

## 9. What is async/await?

`async` and `await` provide a cleaner way to work with Promises.
`await` pauses execution until a Promise resolves or rejects.

---

## 10. What is the DOM?

The DOM (Document Object Model) is a representation of HTML in tree form that JavaScript can manipulate to modify content, style, or structure.

---

## 11. What is event bubbling and capturing?

* **Capturing:** Event moves from window → parent → target.
* **Bubbling:** Event moves from target → parent → window. React and browsers mostly use bubbling by default.

---

## 12. What is the difference between `==` and `===`?

* `==` compares values with type coercion.
* `===` compares values without converting types.

---

## 13. What is a callback function?

A function passed as an argument to another function to be executed later.

---

## 14. What is the difference between `null` and `undefined`?

* `undefined` means a variable is declared but not assigned.
* `null` is an intentional empty value.

---

## 15. What is the spread operator?

The spread operator (`...`) expands arrays or objects into individual elements.

Example:

```js
const arr2 = [...arr1, 4, 5];
```

---

## 16. What is destructuring?

Destructuring allows extracting values from objects or arrays into variables.

Example:

```js
const { name, age } = person;
```

---

## 17. What are higher-order functions?

Functions that take other functions as arguments or return functions.

Examples: `map`, `filter`, `reduce`.

---

## 18. What is `this` in JavaScript?

`this` refers to the execution context.
Its value depends on how a function is called:

* In an object method: the object
* In global scope: window/global
* In classes: the instance

---

## 19. What is a pure function?

A function that:

* Returns the same output for the same input
* Has no side effects

---

## 20. What is debouncing and throttling?

### Debouncing:

Ensures a function runs only after a certain period of inactivity.

### Throttling:

Ensures a function runs at most once in a certain time interval.
