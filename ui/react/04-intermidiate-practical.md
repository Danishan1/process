# 1️⃣ Form Handling (Lecture 21)

**Practice Tasks**

1. Create a **simple login form** with:
   - email
   - password

2. Store form values in **component state**.
3. Display entered values when the user submits the form.
4. Create a **registration form** with:
   - name
   - email
   - password
   - gender

5. Create a **dropdown select form**.
6. Create a **controlled component form**.
7. Show a **form validation message** when input is empty.

Example practice:

```jsx
handleChange(e){
  this.setState({name:e.target.value})
}
```

# 2️⃣ Component Lifecycle Methods (Lecture 22)

**Practice Questions**

1. Create a class component that logs messages in lifecycle methods.
2. Display the order of lifecycle method execution.
3. Show lifecycle logs in the browser console.

Example task:

```
Constructor called
Component Mounted
Component Updated
```

# 3️⃣ Mounting Lifecycle Methods (Lecture 23)

Mounting methods include:

- `constructor()`
- `render()`
- `componentDidMount()`

**Practice Tasks**

1. Create a component that displays **“Component Mounted Successfully”**.
2. Fetch data after component mounts.
3. Show a **timer that starts after mounting**.

# 4️⃣ Updating Lifecycle Methods (Lecture 24)

Important methods:

- `shouldComponentUpdate()`
- `componentDidUpdate()`
- `getSnapshotBeforeUpdate()`

**Practice Tasks**

1. Create a counter and log **component updates**.
2. Prevent component update using `shouldComponentUpdate`.
3. Track previous state values.

# 5️⃣ Fragments (Lecture 25)

**Practice Questions**

1. Return **multiple elements without a div wrapper**.
2. Create a component using:

```jsx
<React.Fragment>
```

3. Use **short syntax fragment**:

```jsx
<>
```

4. Render multiple table columns using fragments.

# 6️⃣ Pure Components (Lecture 26)

**Practice Tasks**

1. Create a parent component with state.
2. Create a **PureComponent child**.
3. Observe rendering behavior when state doesn't change.
4. Compare normal component vs PureComponent.

# 7️⃣ React Memo (Lecture 27)

**Practice Tasks**

Use **React.memo**

1. Create a functional component.
2. Wrap it with `React.memo`.
3. Prevent unnecessary re-renders.
4. Compare behavior with and without memo.

Example:

```jsx
export default React.memo(MyComponent);
```

# 8️⃣ Refs (Lecture 28)

Refs allow **direct access to DOM elements**.

**Practice Questions**

1. Create an input field.
2. Focus the input when a button is clicked.
3. Access input value using refs.

Example:

```jsx
this.inputRef = React.createRef();
```

# 9️⃣ Refs with Class Components (Lecture 29)

**Practice Tasks**

1. Create a parent and child component.
2. Use refs to access child component methods.
3. Call a child function from the parent using refs.

# 🔟 Forwarding Refs (Lecture 30)

Use **React.forwardRef**.

**Practice Tasks**

1. Create a reusable input component.
2. Forward ref from parent to child input.
3. Focus the input from the parent component.

Example scenario:

```
Parent -> InputComponent -> HTML input
```

# 1️⃣1️⃣ Portals (Lecture 31)

Use **React Portals**.

**Practice Tasks**

1. Create a **modal popup component**.
2. Render modal outside root DOM element.
3. Create a **popup notification using portals**.

Example use case:

```
Modal dialogs
Tooltips
Overlays
```

# 1️⃣2️⃣ Error Boundaries (Lecture 32)

Error boundaries catch runtime errors.

**Practice Tasks**

1. Create an **ErrorBoundary component**.
2. Throw an error intentionally in a child component.
3. Display fallback UI when error occurs.

Example fallback UI:

```
Something went wrong
```

# 1️⃣3️⃣ Higher Order Components (Lectures 33–35)

A **Higher Order Component (HOC)** is a function that takes a component and returns a new component.

**Practice Tasks**

1. Create an HOC that adds **click counter functionality**.
2. Use HOC with two components:
   - Button
   - Hover component

3. Share logic between multiple components using HOC.

Example pattern:

```
withCounter(Component)
```

# 1️⃣4️⃣ Render Props (Lectures 36–37)

Render props share logic using a function prop.

**Practice Questions**

1. Create a component that tracks **mouse position**.
2. Pass a function as a prop.
3. Render UI dynamically using the render prop.

Example:

```jsx
<Mouse
  render={(x, y) => (
    <h1>
      {x},{y}
    </h1>
  )}
/>
```

# 1️⃣5️⃣ Context API (Lectures 38–40)

Use **React Context API**.

It allows **global data sharing without prop drilling**.

### Practice Tasks

1. Create a **Theme Context** (light/dark).
2. Pass theme data to deeply nested components.
3. Create a **User Context** storing username.
4. Consume context using:

```jsx
Context.Consumer;
```

5. Use context in functional components.

Example flow:

```
App → Provider → Child → GrandChild
```

# ⭐ Mini Projects Using Topics 21–40

Practice with these projects:

### 1️⃣ Login Form with Validation

Topics:

- Forms
- State
- Event handling

### 2️⃣ Modal Popup System

Topics:

- Portals
- Refs

### 3️⃣ Theme Switcher App

Topics:

- Context API

### 4️⃣ Click Counter with HOC

Topics:

- Higher Order Components
- Render Props

### 5️⃣ Smart Input Focus App

Topics:

- Refs
- Forwarding refs
