
# 🧠 Summary: React Big Picture

```text
                         REACT
                           │
        ┌──────────────────┼──────────────────┐
        ↓                  ↓                  ↓
   Components            State              Props
        │                  │                  │
        ↓                  ↓                  ↓
       JSX             useState           Parent → Child
        │                  │
        └──────────┬───────┘
                   ↓
                Render
                   ↓
             Reconciliation
                   ↓
                  DOM
                   │
                   ↓
               Browser UI
```

Then around that:

```text
Components
   │
   ├── Events
   ├── State
   ├── Effects
   ├── Refs
   ├── Context
   ├── Custom Hooks
   └── Memoization
```

## 🔥 The React Concepts You MUST Know

If we're building toward **strong React + TypeScript + full-stack development**, prioritize these:

| Priority | Concept                      |
| -------- | ---------------------------- |
| ⭐⭐⭐⭐⭐    | Components                   |
| ⭐⭐⭐⭐⭐    | JSX                          |
| ⭐⭐⭐⭐⭐    | Props                        |
| ⭐⭐⭐⭐⭐    | State                        |
| ⭐⭐⭐⭐⭐    | Rendering / re-rendering     |
| ⭐⭐⭐⭐⭐    | Event handling               |
| ⭐⭐⭐⭐⭐    | `useState`                   |
| ⭐⭐⭐⭐⭐    | `useEffect`                  |
| ⭐⭐⭐⭐⭐    | Component lifecycle thinking |
| ⭐⭐⭐⭐⭐    | Forms                        |
| ⭐⭐⭐⭐⭐    | State immutability           |
| ⭐⭐⭐⭐⭐    | Lists + keys                 |
| ⭐⭐⭐⭐     | `useRef`                     |
| ⭐⭐⭐⭐     | Context                      |
| ⭐⭐⭐⭐     | Custom hooks                 |
| ⭐⭐⭐⭐     | `useMemo`                    |
| ⭐⭐⭐⭐     | `useCallback`                |
| ⭐⭐⭐⭐     | `React.memo`                 |
| ⭐⭐⭐⭐     | State lifting                |
| ⭐⭐⭐⭐     | Controlled vs uncontrolled   |
| ⭐⭐⭐      | Suspense                     |
| ⭐⭐⭐      | Lazy loading                 |
| ⭐⭐⭐      | SSR / CSR                    |
| ⭐⭐⭐      | Hydration                    |
| ⭐⭐⭐      | Server Components            |
| ⭐⭐⭐      | Performance optimization     |

## 🔥 The Most Important React Flow

Memorize this:

```text
                 USER
                  │
                  ↓
               EVENT
                  │
                  ↓
            EVENT HANDLER
                  │
                  ↓
             SET STATE
                  │
                  ↓
            RE-RENDER
                  │
                  ↓
          NEW UI DESCRIPTION
                  │
                  ↓
          RECONCILIATION
                  │
                  ↓
              DOM UPDATE
                  │
                  ↓
              SCREEN
```

And the component data flow:

```text
             Parent
             /     \
          props     state
           ↓         ↓
        Child      Parent
           │
           ↓
      event callback
           │
           ↓
        Parent
           │
           ↓
      state update
           │
           ↓
        re-render
```

That's the **core React mental model**.

# ⚛️ React.js — Complete Revision

The core mental model:

> **React is a JavaScript library for building user interfaces using components and state.**

Think:

```text
User interaction
      ↓
     Event
      ↓
   State change
      ↓
   Re-render
      ↓
 React creates new UI description
      ↓
 Reconciliation
      ↓
 DOM updated
```

## 1. What is React?

React is primarily a **UI library**.

You build interfaces from components:

```tsx
function Welcome() {
  return <h1>Hello World</h1>;
}
```

Then compose them:

```tsx
function App() {
  return (
    <>
      <Navbar />
      <Welcome />
      <Footer />
    </>
  );
}
```

Mental model:

```text
App
 ├── Navbar
 ├── Welcome
 └── Footer
```

## 2. Component ⭐⭐⭐⭐⭐

A component is a reusable piece of UI.

```tsx
function Button() {
  return <button>Click</button>;
}
```

Components can contain:

```text
UI
logic
state
events
other components
```

A React application is essentially a **tree of components**.

```text
App
│
├── Header
│   ├── Logo
│   └── Navigation
│
├── Main
│   ├── ProductList
│   │   ├── Product
│   │   └── Product
│   └── Sidebar
│
└── Footer
```

## 3. JSX

JSX lets you write UI-like syntax inside JavaScript/TypeScript.

```tsx
const element = <h1>Hello</h1>;
```

It looks like HTML but isn't HTML.

Conceptually:

```tsx
<h1>Hello</h1>
```

is transformed into React element creation.

JSX allows JavaScript expressions:

```tsx
const name = "John";

return <h1>Hello {name}</h1>;
```

Use `{}` for expressions:

```tsx
<p>{user.name}</p>
```

## 4. JSX vs HTML

They're similar but not identical.

```tsx
className
```

instead of:

```html
class
```

And:

```tsx
onClick
```

instead of:

```html
onclick
```

Example:

```tsx
<button
  className="btn"
  onClick={handleClick}
>
  Click
</button>
```

## 5. Props ⭐⭐⭐⭐⭐

Props are data passed **from parent to child**.

```tsx
function User({ name }: { name: string }) {
  return <h2>{name}</h2>;
}
```

Parent:

```tsx
<User name="John" />
```

Think:

```text
Parent
   │
   │ props
   ↓
Child
```

Props are conceptually **inputs to a component**.

## 6. Props Are Read-Only

Don't mutate props:

```tsx
function User(props) {
  props.name = "Alice"; // ❌
}
```

Instead:

```text
Props
 ↓
read
 ↓
render UI
```

If the value needs to change, that usually belongs in **state**.

## 7. State ⭐⭐⭐⭐⭐

State represents data that can change and affect rendering.

```tsx
const [count, setCount] = useState(0);
```

Here:

```text
count
→ current state

setCount
→ state updater
```

When state changes:

```text
setCount(...)
      ↓
React schedules update
      ↓
component renders again
```

## 8. State Is Not Just a Variable

This:

```tsx
let count = 0;

count++;
```

doesn't tell React:

> "Please update the UI."

But:

```tsx
const [count, setCount] = useState(0);

setCount(count + 1);
```

does.

So:

```text
normal variable
→ JavaScript value

state
→ value React tracks for rendering
```

## 9. `useState`

Basic:

```tsx
const [name, setName] = useState("John");
```

Update:

```tsx
setName("Alice");
```

For previous state:

```tsx
setCount(prev => prev + 1);
```

Prefer the functional form when the new value depends on the previous value.

## 10. Why Functional Updates Matter

Suppose:

```tsx
setCount(count + 1);
setCount(count + 1);
```

Both updates may calculate from the same captured `count`.

Instead:

```tsx
setCount(prev => prev + 1);
setCount(prev => prev + 1);
```

Each updater receives the latest pending state.

Result:

```text
count + 2
```

## 11. Rendering

React components return a description of what the UI should look like.

```tsx
function App() {
  return <h1>Hello</h1>;
}
```

When relevant data changes:

```text
state/props/context change
        ↓
component renders
        ↓
React compares result
        ↓
DOM updates where necessary
```

Important:

> **A re-render does not mean React destroys and recreates the entire DOM.**

## 12. Re-render ⭐⭐⭐⭐⭐

A component can re-render because of things such as:

```text
state update
parent re-render
context update
external store update
```

A re-render means:

> React calls the component again to produce the next UI description.

It does **not** necessarily mean every DOM node gets recreated.

## 13. Virtual DOM

React maintains an internal representation of the UI.

Historically this is commonly described as the **Virtual DOM**.

Conceptually:

```text
Previous UI
    ↓
New UI
    ↓
Compare
    ↓
Determine changes
    ↓
Update real DOM
```

This process is associated with **reconciliation**.

## 14. Reconciliation

Suppose:

```tsx
<h1>Hello</h1>
```

becomes:

```tsx
<h1>Hello John</h1>
```

React determines what needs to change.

The goal isn't:

```text
delete everything
create everything
```

It's closer to:

```text
keep what can be reused
update what changed
```

## 15. Events

React uses event handlers:

```tsx
function Button() {
  function handleClick() {
    console.log("Clicked");
  }

  return (
    <button onClick={handleClick}>
      Click
    </button>
  );
}
```

Notice:

```tsx
onClick={handleClick}
```

not:

```tsx
onClick={handleClick()}
```

The second calls the function immediately during rendering.

## 16. Conditional Rendering

React uses normal JavaScript conditions.

#### `if`

```tsx
if (isLoading) {
  return <Spinner />;
}
```

#### Ternary

```tsx
return isLoggedIn
  ? <Dashboard />
  : <Login />;
```

#### `&&`

```tsx
{isAdmin && <AdminPanel />}
```

Think:

> React doesn't have a special `if` syntax. You use JavaScript to decide what JSX to return.

## 17. Rendering Lists

```tsx
const users = ["John", "Alice", "Bob"];

return (
  <ul>
    {users.map(user => (
      <li key={user}>
        {user}
      </li>
    ))}
  </ul>
);
```

Important:

```tsx
key
```

## 18. Keys ⭐⭐⭐⭐⭐

Keys help React identify list items across renders.

```tsx
users.map(user => (
  <User key={user.id} user={user} />
))
```

Good:

```text
stable + unique among siblings
```

Avoid using array index as key when list items can be reordered, inserted, or removed.

Bad pattern:

```tsx
key={index}
```

when the list is dynamic.

## 19. Component Identity and Keys

Keys aren't only about removing console warnings.

React uses keys as part of deciding whether a component instance represents the **same identity** across renders.

Changing a key can cause React to treat it as a different component identity, which can reset its state.

## 20. `useEffect` ⭐⭐⭐⭐⭐

`useEffect` is used to synchronize a component with something **outside React's rendering calculation**.

Examples:

```text
network request
subscription
timer
browser API
external system
```

Example:

```tsx
useEffect(() => {
  document.title = `Count: ${count}`;
}, [count]);
```

Think:

```text
Render
 ↓
DOM/UI update
 ↓
Effect runs
```

## 21. Dependency Array

```tsx
useEffect(() => {
  // effect
}, []);
```

Empty array:

```text
run after initial mount
```

With dependencies:

```tsx
useEffect(() => {
  // effect
}, [userId]);
```

Runs when `userId` changes.

Without dependency array:

```tsx
useEffect(() => {
  // effect
});
```

runs after every render.

Important:

> Effects are not a generic place to put ordinary component logic.

## 22. Cleanup

Some effects need cleanup:

```tsx
useEffect(() => {
  const id = setInterval(() => {
    console.log("tick");
  }, 1000);

  return () => {
    clearInterval(id);
  };
}, []);
```

Conceptually:

```text
setup
  ↓
effect active
  ↓
cleanup
```

Cleanup can happen when dependencies change before the effect is re-run, and when the component is removed from the UI.

## 23. `useRef`

`useRef` gives you a persistent mutable object:

```tsx
const inputRef = useRef<HTMLInputElement>(null);
```

Access DOM:

```tsx
inputRef.current?.focus();
```

```tsx
<input ref={inputRef} />
```

Another use:

```tsx
const renderCount = useRef(0);
```

Unlike state:

```text
ref.current change
→ doesn't itself trigger re-render
```

## 24. `useMemo`

`useMemo` caches a calculated value:

```tsx
const result = useMemo(() => {
  return expensiveCalculation(data);
}, [data]);
```

Think:

```text
dependencies unchanged
        ↓
reuse cached value
```

Use it when memoization actually helps.

Don't treat it as something every calculation needs.

## 25. `useCallback`

`useCallback` memoizes a function reference:

```tsx
const handleClick = useCallback(() => {
  doSomething(id);
}, [id]);
```

Think:

```text
useMemo
→ memoize a value

useCallback
→ memoize a function reference
```

It's mainly useful when function identity matters, such as with memoized children or dependency arrays.

## 26. `React.memo`

`memo` can skip re-rendering a component when its props haven't changed according to its comparison.

```tsx
const User = memo(function User({ name }) {
  return <div>{name}</div>;
});
```

Mental model:

```text
Parent renders
      ↓
Child props same?
   ↙       ↘
 yes       no
 ↓          ↓
skip      render
```

But `memo` is an optimization, not a correctness mechanism.

## 27. Context ⭐⭐⭐⭐⭐

Context allows data to be made available to descendants without manually passing it through every intermediate component.

Typical examples:

```text
theme
current user
locale
configuration
```

Conceptually:

```text
Provider
   ↓
   ↓
   ↓
Consumer
```

Example:

```tsx
const ThemeContext = createContext("light");

function App() {
  return (
    <ThemeContext.Provider value="dark">
      <Dashboard />
    </ThemeContext.Provider>
  );
}
```

A descendant can consume it with the appropriate context API.

## 28. Prop Drilling

Suppose:

```text
App
 ↓
A
 ↓
B
 ↓
C
 ↓
D
```

But only `D` needs the data.

You might pass:

```text
App → A → B → C → D
```

even though A/B/C don't use it.

That's **prop drilling**.

Context can sometimes solve this.

But don't automatically replace every prop with context.

## 29. State Lifting

Suppose two sibling components need the same data.

Instead of:

```text
Component A → own state
Component B → own state
```

move the state to their closest appropriate common parent:

```text
        Parent
       /      \
      A        B
```

Parent owns state and passes required data/actions down.

This is:

> **Lifting state up.**

## 30. Controlled Components

A controlled input's value comes from React state:

```tsx
const [name, setName] = useState("");

<input
  value={name}
  onChange={e => setName(e.target.value)}
/>
```

Flow:

```text
Input
 ↓
onChange
 ↓
setState
 ↓
render
 ↓
value updated
```

React is the source of truth.

## 31. Uncontrolled Components

An uncontrolled input lets the DOM maintain its current value.

Often accessed through a ref:

```tsx
const inputRef = useRef<HTMLInputElement>(null);

<input ref={inputRef} />
```

Think:

```text
Controlled
→ React owns value

Uncontrolled
→ DOM owns value
```

## 32. Forms

Typical controlled form:

```tsx
function Form() {
  const [email, setEmail] = useState("");

  function handleSubmit(e: React.FormEvent) {
    e.preventDefault();

    console.log(email);
  }

  return (
    <form onSubmit={handleSubmit}>
      <input
        value={email}
        onChange={e => setEmail(e.target.value)}
      />

      <button type="submit">
        Submit
      </button>
    </form>
  );
}
```

Important:

```text
input
→ state
→ validation
→ submit
→ API
```

## 33. Custom Hooks ⭐⭐⭐⭐⭐

A custom hook is a reusable function that uses React hooks.

```tsx
function useUser(id: string) {
  const [user, setUser] = useState(null);

  // ...

  return user;
}
```

Then:

```tsx
const user = useUser(id);
```

Custom hooks let you reuse **logic**, not UI.

Important distinction:

```text
Component
→ reusable UI

Custom Hook
→ reusable stateful/React logic
```

## 34. Rules of Hooks

Hooks have important rules.

#### Only call hooks at the top level

Don't:

```tsx
if (condition) {
  useState(0); // ❌
}
```

Don't:

```tsx
function handleClick() {
  useEffect(...); // ❌
}
```

Instead:

```tsx
function Component() {
  const [state, setState] = useState(0);

  if (...) {
    // use state here
  }
}
```

#### Call hooks from:

```text
React components
custom hooks
```

## 35. Why Hooks Must Be Called Consistently

React needs to associate hook calls with their state across renders.

Conceptually:

```text
Render 1:
useState()
useEffect()
useRef()

Render 2:
useState()
useEffect()
useRef()
```

The order stays consistent.

If you conditionally call hooks:

```text
Render 1:
useState()
useEffect()

Render 2:
useState()
```

the association becomes ambiguous.

That's why the Rules of Hooks exist.

## 36. Data Fetching

A common pattern:

```tsx
useEffect(() => {
  async function loadUsers() {
    const response = await fetch("/api/users");
    const data = await response.json();

    setUsers(data);
  }

  loadUsers();
}, []);
```

But modern React applications often use dedicated data-fetching libraries/framework features rather than manually managing every request with `useEffect`.

The important conceptual separation is:

```text
UI state
≠
server/cache state
```

## 37. Client State vs Server State

#### Client/UI state

```text
modal open?
selected tab?
input value?
theme?
```

#### Server state

```text
users
products
orders
API results
```

Server state has additional concerns:

```text
loading
caching
refetching
stale data
deduplication
pagination
revalidation
```

This is why libraries such as TanStack Query are commonly used in React applications.

## 38. Routing

React itself doesn't define your application's URL routing system.

A common choice is **React Router**.

Conceptually:

```text
URL
 ↓
Router
 ↓
Route
 ↓
Component
```

For example:

```text
/users
/products
/products/123
/settings
```

Routing is separate from React's core component/state model.

## 39. React Server Components / SSR / CSR

These concepts are often confused.

#### CSR — Client-Side Rendering

```text
Browser
 ↓
JavaScript
 ↓
React
 ↓
UI
```

#### SSR — Server-Side Rendering

```text
Server
 ↓
HTML
 ↓
Browser
 ↓
React hydrates
```

#### Server Components

Some React ecosystems/frameworks allow components to execute on the server and send a representation to the client without shipping all their implementation to the browser.

This is an advanced topic and depends heavily on the framework/runtime you're using.

## 40. Hydration

With SSR:

```text
Server
 ↓
HTML
 ↓
Browser displays HTML
 ↓
React loads
 ↓
Hydration
```

Hydration means React attaches its client-side behavior to server-rendered markup.

Think:

```text
SSR
→ HTML exists

Hydration
→ React takes over that HTML
```

## 41. React Strict Mode

You may see:

```tsx
<StrictMode>
  <App />
</StrictMode>
```

Strict Mode helps identify potential problems during development.

One confusing behavior is that certain functions/effects may appear to execute more than once in development under Strict Mode.

Don't conclude:

> "React always runs my effect twice in production."

Development Strict Mode behavior is intentionally designed to expose unsafe assumptions.

## 42. State Immutability ⭐⭐⭐⭐⭐

Don't mutate state directly.

Bad:

```tsx
user.name = "Alice";
```

Better:

```tsx
setUser(prev => ({
  ...prev,
  name: "Alice"
}));
```

For arrays:

```tsx
setUsers(prev => [
  ...prev,
  newUser
]);
```

Instead of:

```tsx
users.push(newUser); // ❌
```

Think:

```text
old state
   ↓
create new value
   ↓
set state
```

## 43. Why Immutability Matters

React relies heavily on identity/reference comparisons.

For example:

```tsx
const newUser = {
  ...user,
  name: "Alice"
};
```

creates a new object reference.

This makes changes easier to detect and reason about.

## 44. React Performance Model

Don't immediately think:

```text
re-render = bad
```

Rendering is normal.

Instead ask:

```text
Is the render expensive?
Is unnecessary work happening?
Are expensive calculations repeated?
Are large subtrees re-rendering?
Is the UI actually slow?
```

Then consider:

```text
memo
useMemo
useCallback
component restructuring
state placement
list virtualization
code splitting
```

## 45. Code Splitting / Lazy Loading

React supports lazy-loading components:

```tsx
const Dashboard = lazy(() =>
  import("./Dashboard")
);
```

Usually combined with:

```tsx
<Suspense fallback={<Loading />}>
  <Dashboard />
</Suspense>
```

Conceptually:

```text
Initial bundle
      ↓
User needs Dashboard
      ↓
Load Dashboard chunk
      ↓
Render
```

## 46. Suspense

Suspense provides a mechanism for showing fallback UI while something React supports as suspending is not ready.

Basic shape:

```tsx
<Suspense fallback={<Spinner />}>
  <Component />
</Suspense>
```

Think:

```text
Not ready
   ↓
fallback

Ready
   ↓
actual UI
```

Its exact capabilities depend on the React feature/framework you're using.

## 47. React Architecture

A reasonably organized application might look like:

```text
src/
│
├── components/
│
├── pages/
│
├── hooks/
│
├── services/
│
├── context/
│
├── utils/
│
├── types/
│
└── App.tsx
```

The exact structure isn't mandatory.

The important thing is separating:

```text
UI
logic
data fetching
state
utilities
types
```

when the application becomes large.
