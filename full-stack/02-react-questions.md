# React Interview Questions and Answers

## 1. What is React?

React is a JavaScript library for building user interfaces. It follows a component-based architecture and uses a virtual DOM for efficient UI updates.

---

## 2. What is the Virtual DOM?

The Virtual DOM is a lightweight JavaScript representation of the actual DOM.
React compares the virtual DOM with the real DOM and updates only the parts that changed.
This process is called **reconciliation**, and it improves performance.

---

## 3. What is JSX?

JSX stands for JavaScript XML.
It allows developers to write HTML-like syntax inside JavaScript.
Example:

```jsx
const element = <h1>Hello</h1>;
```

---

## 4. What are components in React?

Components are reusable building blocks of a UI.

Two types of components:

* Functional Components
* Class Components (older, less used now)

Functional components are preferred due to simplicity and hooks support.

---

## 5. What are props?

Props (properties) are used to pass data from parent to child components.
They are read-only.

---

## 6. What is state?

State represents data that can change over time.
It is managed inside a component.

Example:

```jsx
const [count, setCount] = useState(0);
```

---

## 7. What is the difference between state and props?

| Props                         | State                      |
| ----------------------------- | -------------------------- |
| Passed from parent            | Managed inside component   |
| Immutable                     | Mutable                    |
| Used to configure a component | Used to store dynamic data |

---

## 8. What is useState?

`useState` is a hook that allows functional components to use state.

Example:

```jsx
const [name, setName] = useState("John");
```

---

## 9. What is useEffect?

`useEffect` is used to handle side effects such as:

* API calls
* Subscriptions
* Timers
* Updating document title

Example:

```jsx
useEffect(() => {
  console.log("Component mounted");
}, []);
```

---

## 10. What is the dependency array in useEffect?

The dependency array controls when the effect should run.

Examples:

* `[]` runs once
* `[value]` runs when value changes

---

## 11. What is lifting state up?

When multiple components need access to the same data, the state is moved to their closest common parent.
This avoids duplicate state logic.

---

## 12. What is conditional rendering in React?

Rendering components based on conditions.

Example:

```jsx
{loggedIn ? <Dashboard /> : <Login />}
```

---

## 13. What are keys in React lists?

Keys help React identify list items when updating the UI efficiently.

Example:

```jsx
{items.map(item => <li key={item.id}>{item.name}</li>)}
```

---

## 14. What is the Context API?

Context API allows sharing global data without passing props through multiple levels (prop drilling).
Steps:

1. Create context
2. Provide context
3. Consume context

---

## 15. What is prop drilling?

Prop drilling means passing data through multiple nested components even if only one child needs it.
Context API or state management libraries solve this.

---

## 16. What is React Router?

React Router is a library for client-side routing in single-page applications.

Example:

```jsx
<Route path="/home" element={<Home />} />
```

---

## 17. What is a single-page application (SPA)?

SPA loads a single HTML page and updates content dynamically without reloading the full page.
React is commonly used to build SPAs.

---

## 18. What is reconciliation?

Reconciliation is React’s algorithm that compares the new virtual DOM with the old one and updates only changed parts.

---

## 19. What is the difference between controlled and uncontrolled components?

### Controlled Component:

* Form data is managed by React state.

### Uncontrolled Component:

* Form data is handled by the DOM using refs.

---

## 20. What is useRef?

useRef stores mutable values that do not cause re-renders and is used for:

* Accessing DOM elements
* Storing previous values
* Managing timers

Example:

```jsx
const inputRef = useRef(null);
```

---

## 21. What is useMemo?

useMemo memoizes expensive calculations so they are not recalculated on every render.

---

## 22. What is useCallback?

useCallback returns a memoized function to avoid unnecessary re-renders in child components.

---

## 23. What is memo in React?

`React.memo` is used to memoize functional components to prevent unnecessary re-renders.

---

## 24. What is the difference between React and React Native?

* React is used for web applications.
* React Native is used to build mobile applications.

---

## 25. What is lazy loading in React?

Lazy loading allows loading components only when needed, improving performance.

Example:

```jsx
const About = React.lazy(() => import('./About'));
```
