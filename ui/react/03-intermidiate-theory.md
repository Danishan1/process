Here is a **complete list of theoretical questions** based on **Lectures 21–40** of **React**.
These questions are useful for **interviews, viva, exams, and concept revision**.

# 1️⃣ Form Handling (Lecture 21)

**Q1. What is form handling in React?**
Form handling in React means **managing form input values using component state** and handling user input using event handlers.

**Q2. What are controlled components?**
Controlled components are form elements whose values are **controlled by React state**.

Example:

```jsx
<input value={this.state.name} onChange={this.handleChange} />
```

**Q3. What are uncontrolled components?**
Uncontrolled components store form data in the **DOM itself instead of React state**, usually accessed using **refs**.

**Q4. What is the purpose of `onChange` in forms?**
`onChange` updates the component state whenever the user changes input values.

**Q5. Why do we use `preventDefault()` in form submission?**
It prevents the **default page reload behavior** of HTML forms.

# 2️⃣ Component Lifecycle Methods (Lecture 22)

**Q6. What are lifecycle methods in React?**
Lifecycle methods are special methods that execute during different phases of a component’s life.

**Q7. What are the three main lifecycle phases?**

1. Mounting
2. Updating
3. Unmounting

**Q8. Why are lifecycle methods important?**

- Managing side effects
- Fetching data
- Optimizing rendering

# 3️⃣ Mounting Lifecycle Methods (Lecture 23)

**Q9. What is the mounting phase?**
Mounting occurs when a component is **created and inserted into the DOM**.

**Q10. Name the mounting lifecycle methods.**

- `constructor()`
- `static getDerivedStateFromProps()`
- `render()`
- `componentDidMount()`

**Q11. What is `componentDidMount()` used for?**

Common uses:

- API calls
- DOM manipulation
- Setting timers

# 4️⃣ Updating Lifecycle Methods (Lecture 24)

**Q12. What is the updating phase?**
Updating occurs when **state or props change**, causing the component to re-render.

**Q13. Name updating lifecycle methods.**

- `static getDerivedStateFromProps()`
- `shouldComponentUpdate()`
- `render()`
- `getSnapshotBeforeUpdate()`
- `componentDidUpdate()`

**Q14. What does `shouldComponentUpdate()` do?**
It allows developers to **control whether a component should re-render**.

**Q15. What is `componentDidUpdate()` used for?**

- Performing side effects after updates
- Network requests after state changes

# 5️⃣ Fragments (Lecture 25)

**Q16. What are fragments in React?**
Fragments allow grouping multiple elements **without adding extra DOM nodes**.

Example:

```jsx
<React.Fragment>
```

**Q17. Why are fragments used?**

- Avoid unnecessary div wrappers
- Cleaner DOM structure

**Q18. What is the shorthand syntax for fragments?**

```jsx
<>
```

# 6️⃣ Pure Components (Lecture 26)

**Q19. What is a Pure Component?**
A Pure Component is a special type of component that performs **shallow comparison of props and state** to prevent unnecessary re-renders.

Example:

```jsx
class MyComponent extends React.PureComponent
```

**Q20. What is shallow comparison?**
It checks whether **values of props/state have changed** instead of deeply comparing objects.

# 7️⃣ React Memo (Lecture 27)

**Q21. What is `React.memo`?**
**React.memo** is a higher-order component that **prevents unnecessary re-rendering of functional components**.

**Q22. When should we use React.memo?**

- When component renders frequently
- When props rarely change

# 8️⃣ Refs (Lecture 28)

**Q23. What are refs in React?**
Refs provide a way to **directly access DOM elements or React components**.

**Q24. How are refs created?**

```jsx
React.createRef();
```

**Q25. What are common use cases of refs?**

- Accessing DOM elements
- Managing focus
- Integrating third-party libraries

# 9️⃣ Refs with Class Components (Lecture 29)

**Q26. Can refs be used with class components?**
Yes, refs can reference **class component instances**.

**Q27. What can we access using refs in class components?**

- Component methods
- DOM nodes

# 🔟 Forwarding Refs (Lecture 30)

**Q28. What is ref forwarding?**
Ref forwarding allows passing a **ref from parent component to child DOM element**.

**Q29. Which API is used for ref forwarding?**

**React.forwardRef**

Example:

```jsx
React.forwardRef();
```

# 1️⃣1️⃣ Portals (Lecture 31)

**Q30. What are portals in React?**
**React Portals** allow rendering children **outside the parent DOM hierarchy**.

**Q31. Why are portals used?**

Common use cases:

- Modals
- Tooltips
- Popups

# 1️⃣2️⃣ Error Boundaries (Lecture 32)

**Q32. What are Error Boundaries?**
Error boundaries are components that **catch JavaScript errors in child components**.

**Q33. Which lifecycle methods are used in Error Boundaries?**

- `static getDerivedStateFromError()`
- `componentDidCatch()`

**Q34. What is the benefit of Error Boundaries?**

They prevent the **entire application from crashing**.

# 1️⃣3️⃣ Higher Order Components (Lectures 33–35)

**Q35. What is a Higher Order Component (HOC)?**
A HOC is a function that **takes a component and returns a new enhanced component**.

**Q36. Why are HOCs used?**

- Code reuse
- Logic sharing between components

Example:

```jsx
withCounter(Component);
```

**Q37. Give examples of HOC usage.**

- Authentication logic
- Logging
- Data fetching

# 1️⃣4️⃣ Render Props (Lectures 36–37)

**Q38. What are render props?**
Render props is a technique for **sharing code between components using a function prop**.

**Q39. What is the main idea behind render props?**

Instead of passing JSX, you pass a **function that returns JSX**.

Example:

```jsx
<Component render={(data) => <UI />} />
```

# 1️⃣5️⃣ Context API (Lectures 38–40)

**Q40. What is the Context API?**
The **React Context API** allows data to be **shared globally between components without prop drilling**.

**Q41. What is prop drilling?**
Passing props through multiple intermediate components unnecessarily.

**Q42. What are the main parts of Context API?**

1. Context creation
2. Provider
3. Consumer

**Q43. How do you create a context?**

```jsx
React.createContext();
```

**Q44. What does the Provider do?**

It supplies data to all child components.

**Q45. What does the Consumer do?**

It reads data from the context.

# ⭐ Most Important Interview Questions (21–40)

Focus on these for interviews:

1. Controlled vs uncontrolled components
2. React lifecycle phases
3. `componentDidMount()` purpose
4. Pure Component vs normal component
5. What is `React.memo`?
6. What are refs in React?
7. What is ref forwarding?
8. What are React portals?
9. What are Error Boundaries?
10. What is a Higher Order Component?
11. Render Props concept
12. What is Context API?
13. What is prop drilling?
