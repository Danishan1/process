Below is a **complete list of theoretical questions** for the **Hooks section (Lectures 44–76)** of **React**. These cover **core React Hooks concepts often asked in interviews and exams**.

# 1️⃣ Introduction to Hooks (Lecture 44)

### Q1. What are React Hooks?

Hooks are functions that allow **functional components to use state, lifecycle methods, and other React features without writing class components**.

### Q2. Why were Hooks introduced?

Hooks were introduced to:

- Use state in functional components
- Avoid complex class components
- Reuse logic between components
- Improve readability and maintainability

### Q3. When were Hooks introduced?

Hooks were introduced in **React 16.8**.

### Q4. What are the rules of Hooks?

1. Only call hooks **at the top level** of a component.
2. Only call hooks **inside React functions or custom hooks**.

# 2️⃣ useState Hook (Lectures 45–48)

### Q5. What is the `useState` Hook?

The **useState** hook allows functional components to **manage state**.

Example:

```jsx
const [count, setCount] = useState(0);
```

### Q6. What does `useState` return?

It returns an **array with two elements**:

1. Current state value
2. Function to update the state

### Q7. How do you update state using `useState`?

```jsx
setCount(count + 1);
```

### Q8. What is updating state based on previous state?

When new state depends on the previous value.

Example:

```jsx
setCount((prevCount) => prevCount + 1);
```

### Q9. Can `useState` store objects?

Yes.

Example:

```jsx
const [user, setUser] = useState({ name: "John", age: 25 });
```

### Q10. Can `useState` store arrays?

Yes.

Example:

```jsx
const [items, setItems] = useState([]);
```

# 3️⃣ useEffect Hook (Lectures 49–57)

### Q11. What is the `useEffect` Hook?

The **useEffect** is used to **perform side effects in functional components**.

Examples of side effects:

- API calls
- DOM manipulation
- timers
- subscriptions

### Q12. What is the syntax of `useEffect`?

```jsx
useEffect(() => {
  // side effect code
});
```

### Q13. When does `useEffect` run?

By default, it runs **after every render**.

### Q14. How can we run `useEffect` only once?

Using an **empty dependency array**.

```jsx
useEffect(() => {}, []);
```

### Q15. What is a dependency array?

It is a list of variables that determine **when the effect should run again**.

Example:

```jsx
useEffect(() => {}, [count]);
```

### Q16. What is cleanup in `useEffect`?

Cleanup removes **subscriptions, timers, or event listeners** to prevent memory leaks.

Example:

```jsx
return () => {
  // cleanup code
};
```

### Q17. Why are incorrect dependencies problematic?

Incorrect dependencies can cause:

- infinite loops
- unnecessary re-renders

# 4️⃣ Fetching Data with useEffect (Lectures 55–57)

### Q18. Why is `useEffect` used for API calls?

Because API calls should happen **after the component renders**.

### Q19. Which libraries are commonly used for API calls?

- **Axios**
- **Fetch API**

### Q20. What states are commonly used during data fetching?

1. Loading state
2. Data state
3. Error state

# 5️⃣ useContext Hook (Lectures 58–60)

### Q21. What is the `useContext` Hook?

The **useContext** allows functional components to **access values from the Context API**.

### Q22. What problem does `useContext` solve?

It solves **prop drilling**.

### Q23. What is prop drilling?

Passing data through multiple nested components unnecessarily.

# 6️⃣ useReducer Hook (Lectures 61–68)

### Q24. What is the `useReducer` Hook?

The **useReducer** is used for **complex state management**.

### Q25. What are the main elements of `useReducer`?

1. State
2. Action
3. Reducer function

### Q26. What is a reducer function?

A function that determines **how state changes based on actions**.

Example:

```jsx
function reducer(state, action) {
  switch (action.type) {
    case "increment":
      return { count: state.count + 1 };
  }
}
```

### Q27. When should we use `useReducer` instead of `useState`?

Use `useReducer` when:

- state logic is complex
- multiple state updates depend on each other

### Q28. Can multiple reducers be used in one component?

Yes.

This pattern is called **multiple reducers**.

### Q29. Why combine `useReducer` with Context?

To create **global state management**.

# 7️⃣ useCallback Hook (Lecture 69)

### Q30. What is the `useCallback` Hook?

The **useCallback** returns a **memoized version of a function**.

### Q31. Why is `useCallback` used?

To prevent **unnecessary re-creation of functions during re-renders**.

# 8️⃣ useMemo Hook (Lecture 70)

### Q32. What is the `useMemo` Hook?

The **useMemo** memoizes **expensive computations**.

### Q33. Why is `useMemo` useful?

It improves performance by **avoiding repeated calculations**.

# 9️⃣ useRef Hook (Lectures 71–72)

### Q34. What is the `useRef` Hook?

The **useRef** creates a **mutable reference that persists across renders**.

### Q35. What are common uses of `useRef`?

- Access DOM elements
- Store mutable values
- Manage focus

# 🔟 Custom Hooks (Lectures 73–76)

### Q36. What are custom hooks?

Custom hooks are **user-defined hooks that reuse logic between components**.

Example:

```jsx
function useCounter() {}
```

### Q37. Why are custom hooks useful?

- Code reuse
- Cleaner components
- Better separation of logic

### Q38. What naming rule must custom hooks follow?

Custom hooks must start with **"use"**.

Example:

```
useCounter
useInput
useDocumentTitle
```

# ⭐ Most Important Interview Questions (Hooks Section)

Focus especially on these:

1. What are React Hooks?
2. What is `useState`?
3. What is `useEffect`?
4. What is dependency array?
5. What is cleanup in `useEffect`?
6. What is `useContext`?
7. What is `useReducer`?
8. Difference between `useState` and `useReducer`
9. What is `useCallback`?
10. What is `useMemo`?
11. What is `useRef`?
12. What are custom hooks?
