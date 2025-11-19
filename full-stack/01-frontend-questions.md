Below are **theoretical interview questions and answers for the *Frontend Development* section** written in clean, professional Markdown format.
No emojis are used.

---

# Frontend Development: Interview Questions and Answers

This section covers theoretical questions from:

1. HTML
2. CSS
3. JavaScript
4. React

You can ask for more depth or scenario-based questions anytime.

---

# 1. HTML Interview Questions and Answers

### 1. What is HTML?

HTML (HyperText Markup Language) is the standard markup language used to structure content on web pages. It defines the layout using elements such as headings, paragraphs, links, lists, images, tables, and forms.

### 2. What are semantic HTML elements?

Semantic elements clearly describe their meaning to both browser and developer.
Examples:
`<header>`, `<footer>`, `<nav>`, `<article>`, `<section>`, `<aside>`.
They improve accessibility, SEO, and readability.

### 3. What is the difference between block-level and inline elements?

* Block-level elements take the full width available and start on a new line. Example: `<div>`, `<p>`, `<h1>`.
* Inline elements only take the necessary width and do not start on a new line. Example: `<span>`, `<a>`, `<strong>`.

### 4. What is the purpose of the DOCTYPE?

The DOCTYPE tells the browser which version of HTML to use.
`<!DOCTYPE html>` is used for HTML5 and ensures standards mode.

### 5. What is the difference between `<div>` and `<span>`?

`<div>` is a block-level container for larger layout sections.
`<span>` is an inline container used for styling small parts of text.

### 6. What are meta tags?

Meta tags provide metadata such as author, description, viewport settings, and SEO information. They do not render any visible content.

### 7. What is the purpose of the `alt` attribute in images?

It provides alternative text for screen readers and displays when the image fails to load. It improves accessibility and SEO.

### 8. What is the difference between HTML and XHTML?

* HTML is flexible with syntax.
* XHTML is stricter and requires well-formed XML rules like closed tags and lowercase names.

---

# 2. CSS Interview Questions and Answers

### 1. What is CSS?

CSS (Cascading Style Sheets) is used to style and layout web pages. It controls colors, typography, spacing, responsiveness, animations, and the visual appearance of HTML elements.

### 2. What is the box model?

The box model describes how elements are rendered.
It includes:
Content → Padding → Border → Margin.

### 3. What is the difference between `display: inline`, `block`, and `inline-block`?

* Inline: No line break, cannot set height/width.
* Block: Starts new line, can set height/width.
* Inline-block: Does not start new line but supports height/width.

### 4. What is specificity?

Specificity determines which CSS rule is applied when multiple rules affect the same element.
Hierarchy: Inline styles > IDs > Classes > Tags > Universal selector.

### 5. What are Flexbox and Grid?

* Flexbox: One-dimensional layout (row or column).
* Grid: Two-dimensional layout system (rows and columns).
  Both are core tools for responsive design.

### 6. What are media queries?

Media queries apply CSS rules based on device size or characteristics.
Example: modifying layout for mobile screens.

### 7. What is the difference between relative, absolute, and fixed positioning?

* Relative: Positioned relative to its normal position.
* Absolute: Positioned relative to the nearest positioned ancestor.
* Fixed: Positioned relative to the viewport.

### 8. What is the difference between `visibility: hidden` and `display: none`?

* `display: none` removes the element from the layout flow.
* `visibility: hidden` keeps the element in the flow but hides it visually.

### 9. What are pseudo-classes and pseudo-elements?

* Pseudo-classes: Select elements based on state (e.g., `:hover`, `:focus`).
* Pseudo-elements: Select part of an element (`::before`, `::after`).

---

# 3. JavaScript Interview Questions and Answers

### 1. What is JavaScript?

JavaScript is a high-level, event-driven, interpreted programming language used to add interactivity and dynamic behavior to websites.

### 2. What are the data types in JavaScript?

Primitive: string, number, boolean, null, undefined, bigint, symbol
Non-primitive: object, array, function

### 3. What is the difference between `var`, `let`, and `const`?

* `var` is function-scoped and allows redeclaration.
* `let` is block-scoped and does not allow redeclaration.
* `const` is block-scoped and cannot be reassigned.

### 4. What is a closure?

A closure is a function that remembers its outer scope even after the outer function has returned. It enables data privacy and function factories.

### 5. What is the event loop?

The event loop manages asynchronous operations. It continuously checks the call stack and message queue to handle callbacks, promises, and async tasks.

### 6. Explain synchronous vs asynchronous code.

* Synchronous: Code executes line-by-line, blocking the thread.
* Asynchronous: Tasks run in background; execution continues without waiting.

### 7. What are Promises?

Promises represent the eventual completion or failure of an asynchronous operation. States: pending, fulfilled, rejected.

### 8. What is the difference between `==` and `===`?

* `==` compares values with type coercion.
* `===` checks values without type conversion.

### 9. What is hoisting?

Hoisting moves variable and function declarations to the top of their scope before execution.
`var` declarations are hoisted with undefined; `let` and `const` are hoisted but not initialized.

### 10. What is the DOM?

The DOM (Document Object Model) represents HTML elements as a tree structure, allowing JavaScript to access and manipulate them.

### 11. What is event bubbling and capturing?

* Capturing: Events go from root to target.
* Bubbling: Events go from target to root.
  JavaScript uses bubbling by default.

---

# 4. React Interview Questions and Answers

### 1. What is React?

React is a component-based JavaScript library for building user interfaces. It uses a virtual DOM and declarative programming style.

### 2. What is the Virtual DOM?

It is a lightweight copy of the real DOM. React updates only the changed parts using diffing, improving performance.

### 3. What are components?

Components are reusable UI elements.
Types:

* Functional components
* Class components (older method)

### 4. What is the difference between props and state?

* Props: Passed from parent to child; read-only.
* State: Maintained inside a component; mutable.

### 5. What are React hooks?

Hooks allow functional components to use state and lifecycle features.
Important hooks: `useState`, `useEffect`, `useRef`, `useMemo`, `useCallback`.

### 6. What is useEffect?

It handles side effects such as API calls, timers, and subscriptions. It runs after a render.

### 7. What is lifting state up?

Moving shared state to a common parent component so children can access consistent data.

### 8. What is React Router?

A library used to handle routing.
It provides components like `BrowserRouter`, `Route`, `Link`, and `useNavigate`.

### 9. Why is key used in lists?

Keys help React identify which items have been added, removed, or updated, improving rendering efficiency.

### 10. What is controlled vs uncontrolled components?

* Controlled: Form values controlled by React state.
* Uncontrolled: Form values handled by the DOM using refs.

### 11. What is Context API?

A React feature for global state management. It avoids prop drilling.

### 12. What is reconciliation?

React’s process of comparing the virtual DOM with previous versions to update the minimal required parts.
