
### The mental model

Imagine your React app has many components:

```text
App
├── Navbar
│   └── CartButton
├── ProductList
│   └── ProductCard
└── CartPage
    └── CartItem
```

Some data needs to be shared across distant components:

```text
user
cart
products
theme
notifications
```

Without a state-management solution, you can end up passing props through many layers:

```text
App
 ↓
Layout
 ↓
Page
 ↓
Component
 ↓
Child
```

Redux gives us a **central store**:

```text
                 ┌───────────────┐
                 │  Redux Store  │
                 │               │
                 │ user          │
                 │ cart          │
                 │ products      │
                 │ notifications │
                 └───────┬───────┘
                         │
             ┌───────────┼───────────┐
             ↓           ↓           ↓
          Navbar      Product      Cart
```

Components don't directly mutate that state.

Instead:

```text
Component
    │
    │ dispatch(action)
    ↓
  Reducer
    │
    │ calculates new state
    ↓
 Redux Store
    │
    │ updated state
    ↓
Components re-render
```

That's the core idea.

# 1. First understand Redux itself

Redux revolves around a few concepts:

### Store

The central place containing application state.

```js
{
  counter: {
    value: 10
  },
  user: {
    name: "John"
  }
}
```

### Action

An object describing **what happened**.

```js
{
  type: "counter/increment"
}
```

or:

```js
{
  type: "cart/addItem",
  payload: {
    id: 123,
    name: "Laptop"
  }
}
```

Notice something important:

> An action describes an event. It doesn't directly change the state.

### Reducer

A function that determines what the new state should be after an action.

Conceptually:

```js
(state, action) => newState
```

For example:

```js
function counterReducer(state, action) {
  if (action.type === "counter/increment") {
    return {
      ...state,
      value: state.value + 1
    };
  }

  return state;
}
```

### Dispatch

`dispatch()` sends an action into Redux.

```js
dispatch({
  type: "counter/increment"
});
```

Redux then essentially does:

```text
dispatch(action)
       ↓
   reducer(state, action)
       ↓
    new state
       ↓
    store updates
       ↓
 React components update
```

### Selector

A selector reads something from the store.

For example:

```js
const count = useSelector(
  state => state.counter.value
);
```

So we have:

```text
                Redux
                  │
       ┌──────────┼──────────┐
       ↓          ↓          ↓
     Store      Actions    Reducers
       │          │          │
       │       dispatch       │
       │          │           │
       └──────────┼───────────┘
                  ↓
              new state
                  │
                  ↓
              Selectors
                  │
                  ↓
               React
```

If you understand this diagram, you understand the fundamental Redux architecture.

# 2. Then comes Redux Toolkit

Here's where things get interesting.

**Redux Toolkit (RTK)** is the modern recommended way to write Redux.

Old Redux code could be quite verbose.

You might have had:

```js
const INCREMENT = "counter/increment";

function increment() {
  return {
    type: INCREMENT
  };
}

function counterReducer(state = { value: 0 }, action) {
  switch (action.type) {
    case INCREMENT:
      return {
        ...state,
        value: state.value + 1
      };

    default:
      return state;
  }
}
```

RTK dramatically simplifies this.

```js
import { createSlice } from "@reduxjs/toolkit";

const counterSlice = createSlice({
  name: "counter",

  initialState: {
    value: 0
  },

  reducers: {
    increment(state) {
      state.value += 1;
    }
  }
});
```

This looks like we're mutating state:

```js
state.value += 1;
```

But we're not actually violating Redux's immutability rules.

RTK uses **Immer** underneath to translate this into an immutable update.

That's one of the important things to understand later.

# 3. A slice

You'll hear the word **slice** constantly when working with RTK.

A slice is basically:

> One logical section of your Redux state + the logic that modifies it.

For example:

```text
Redux Store
│
├── counter
│
├── user
│
├── cart
│
└── products
```

You might have:

```text
counterSlice
userSlice
cartSlice
productsSlice
```

A slice typically contains:

```js
const cartSlice = createSlice({
  name: "cart",

  initialState: {
    items: []
  },

  reducers: {
    addItem(state, action) {
      // ...
    },

    removeItem(state, action) {
      // ...
    }
  }
});
```

So instead of thinking:

> "Redux is one giant file."

Think:

> "Redux state is divided into logical slices."

# 4. `configureStore`

RTK also simplifies creating the Redux store.

```js
import { configureStore } from "@reduxjs/toolkit";

const store = configureStore({
  reducer: {
    counter: counterReducer,
    cart: cartReducer
  }
});
```

Now your state looks conceptually like:

```js
{
  counter: {
    value: 0
  },

  cart: {
    items: []
  }
}
```

# 5. Connecting Redux to React

We normally wrap the React application with `<Provider>`:

```jsx
import { Provider } from "react-redux";

<Provider store={store}>
  <App />
</Provider>
```

Now components can access Redux.

### Reading state

```jsx
import { useSelector } from "react-redux";

function Counter() {
  const count = useSelector(
    state => state.counter.value
  );

  return <div>{count}</div>;
}
```

### Changing state

```jsx
import { useDispatch } from "react-redux";

function Counter() {
  const dispatch = useDispatch();

  return (
    <button
      onClick={() => dispatch(increment())}
    >
      Increment
    </button>
  );
}
```

And the complete flow becomes:

```text
          User clicks button
                  │
                  ↓
          dispatch(increment())
                  │
                  ↓
              Action
                  │
                  ↓
             Reducer
                  │
                  ↓
            Redux Store
                  │
                  ↓
          State changes
                  │
                  ↓
        useSelector detects it
                  │
                  ↓
           React re-renders
```

# 6. The most important distinction

When learning Redux, keep these three things separate in your head:

### State

**What is currently true?**

```js
{
  isLoggedIn: true,
  username: "Alice"
}
```

### Action

**What happened?**

```js
{
  type: "auth/loggedIn"
}
```

or:

```js
{
  type: "cart/itemAdded",
  payload: product
}
```

### Reducer

**Given what happened, how should state change?**

```js
loggedIn(state, action) {
  state.isLoggedIn = true;
}
```

This gives us:

```text
STATE
  ↑
  │
REDUCER
  ↑
  │
ACTION
  ↑
  │
USER / APPLICATION EVENT
```

# 7. Where I suggest we go next

Rather than memorizing APIs, I'd learn Redux in this order:

```text
1. Why Redux exists
        ↓
2. Redux data flow
        ↓
3. Store
        ↓
4. Actions
        ↓
5. Reducers
        ↓
6. Dispatch
        ↓
7. Selectors
        ↓
8. Redux Toolkit
        ↓
9. createSlice
        ↓
10. configureStore
        ↓
11. React + Redux
        ↓
12. Async logic
        ↓
13. createAsyncThunk
        ↓
14. RTK Query
        ↓
15. Real application architecture
```

And I'd strongly recommend that we **first build a tiny counter in plain Redux conceptually**, then build the same thing with **Redux Toolkit**.

That makes the reason RTK exists immediately obvious.

If you're ready, we can start with **"What problem does Redux actually solve?"** and build the mental model from a real React example.
