# 1️⃣ React Introduction

**Q1. What is React?**
React is a **JavaScript library for building user interfaces**, mainly for **single-page applications (SPA)**. It allows developers to create **reusable UI components**.

**Q2. Who developed React?**
React was developed by **Facebook (now Meta Platforms)**.

**Q3. What are the main features of React?**

- Component-based architecture
- Virtual DOM
- Reusable components
- Unidirectional data flow
- High performance
- JSX syntax

**Q4. What is a Single Page Application (SPA)?**
A web application that **loads a single HTML page** and dynamically updates content without reloading the entire page.

**Q5. Why is React popular?**

- Fast rendering using Virtual DOM
- Reusable components
- Large community
- Easy integration with other libraries

# 2️⃣ Hello World & Setup

**Q6. How do you create a React app?**

Using **Create React App**:

```bash
npx create-react-app my-app
cd my-app
npm start
```

**Q7. What command runs the React development server?**

```bash
npm start
```

**Q8. What is Node.js used for in React projects?**
Node.js provides the runtime environment for installing packages and running development tools.

# 3️⃣ React Folder Structure

**Q9. What are important folders in a React project?**

Common folders:

- **node_modules** → installed dependencies
- **public** → static files
- **src** → main React code
- **package.json** → project configuration

**Q10. What is the entry point of a React app?**

Usually:

```
src/index.js
```

# 4️⃣ Components

**Q11. What are components in React?**
Components are **independent, reusable pieces of UI**.

Example:

```jsx
function Welcome() {
  return <h1>Hello</h1>;
}
```

**Q12. Why are components important?**

- Code reuse
- Easy maintenance
- Better organization

**Q13. What are the types of components?**

Two types:

1. Functional components
2. Class components

# 5️⃣ Functional Components

**Q14. What is a Functional Component?**
A component written as a **JavaScript function**.

Example:

```jsx
function Hello() {
  return <h1>Hello React</h1>;
}
```

**Q15. Advantages of Functional Components**

- Simpler
- Less code
- Easier to read
- Supports Hooks

# 6️⃣ Class Components

**Q16. What is a Class Component?**

A component defined using ES6 class syntax.

```jsx
class Hello extends React.Component {
  render() {
    return <h1>Hello</h1>;
  }
}
```

**Q17. Difference between Functional and Class Components**

| Functional       | Class                  |
| ---------------- | ---------------------- |
| Simple functions | ES6 classes            |
| Use Hooks        | Uses lifecycle methods |
| Less boilerplate | More code              |

# 7️⃣ Hooks

**Q18. What are Hooks in React?**
Hooks allow functional components to **use state and lifecycle features**.

Example:

```jsx
useState();
useEffect();
```

**Q19. Why were Hooks introduced?**

- To avoid class components
- Better code reuse
- Simpler logic

# 8️⃣ JSX

**Q20. What is JSX?**
JSX stands for **JavaScript XML**.

It allows writing **HTML-like syntax inside JavaScript**.

Example:

```jsx
const element = <h1>Hello</h1>;
```

**Q21. Is JSX required in React?**
No, but it makes writing UI easier.

**Q22. Why can JSX return only one parent element?**
Because React components must return **a single root element**.

# 9️⃣ Props

**Q23. What are Props?**
Props (properties) are **inputs passed to components**.

Example:

```jsx
<Greet name="Rahul" />
```

**Q24. Are props mutable?**
No. Props are **read-only (immutable)**.

# 🔟 State

**Q25. What is State in React?**
State is an **object that stores component data** and can change over time.

**Q26. Difference between Props and State**

| Props              | State                    |
| ------------------ | ------------------------ |
| Passed from parent | Managed inside component |
| Immutable          | Mutable                  |
| External data      | Internal data            |

# 1️⃣1️⃣ setState

**Q27. What is `setState()`?**
It updates the component state and triggers re-render.

Example:

```jsx
this.setState({ count: 1 });
```

**Q28. Why should we not modify state directly?**

Incorrect:

```js
this.state.count = 1;
```

Correct:

```js
this.setState({ count: 1 });
```

# 1️⃣2️⃣ Destructuring Props & State

**Q29. What is destructuring?**

Extracting values from objects.

Example:

```jsx
const { name, age } = props;
```

Benefits:

- Cleaner code
- Easier access

# 1️⃣3️⃣ Event Handling

**Q30. How are events handled in React?**

Using camelCase syntax.

Example:

```jsx
<button onClick={handleClick}>
```

**Q31. Difference between HTML and React events**

| HTML           | React            |
| -------------- | ---------------- |
| onclick        | onClick          |
| string handler | function handler |

# 1️⃣4️⃣ Binding Event Handlers

**Q32. Why bind event handlers in class components?**

Because `this` needs to reference the component instance.

Example:

```js
this.handleClick = this.handleClick.bind(this);
```

# 1️⃣5️⃣ Methods as Props

**Q33. Can a component pass a function as props?**

Yes.

Example:

```jsx
<Child clickHandler={this.handleClick} />
```

Used for **parent-child communication**.

# 1️⃣6️⃣ Conditional Rendering

**Q34. What is Conditional Rendering?**

Displaying UI based on conditions.

Methods:

- if/else
- ternary operator
- logical &&

Example:

```jsx
{
  isLoggedIn ? <Dashboard /> : <Login />;
}
```

# 1️⃣7️⃣ List Rendering

**Q35. What is List Rendering?**

Rendering multiple items using `map()`.

Example:

```jsx
names.map((name) => <h1>{name}</h1>);
```

# 1️⃣8️⃣ Keys

**Q36. What are Keys in React?**

Keys help React **identify list elements uniquely**.

Example:

```jsx
<li key={id}>{name}</li>
```

**Q37. Why are keys important?**

- Improve performance
- Help React track changes

# 1️⃣9️⃣ Index as Key Anti-Pattern

**Q38. Why should we avoid index as key?**

Using index can cause **incorrect UI updates** when items change order.

Better:

```jsx
key={user.id}
```

# 2️⃣0️⃣ Styling & CSS

**Q39. How can you style React components?**

Methods:

1. CSS Stylesheet
2. Inline Styling
3. CSS Modules
4. Styled Components

Example:

```jsx
const style = {color:"red"}
<h1 style={style}>Hello</h1>
```

# ⭐ Very Important Interview Questions (From These 20 Lectures)

1. What is React?
2. What is JSX?
3. Difference between props and state?
4. Functional vs class components?
5. What are hooks?
6. What is Virtual DOM?
7. What are keys in React?
8. What is conditional rendering?
9. How does event handling work in React?
10. What is list rendering?
