# Redux Summary

| Concept                   | What it means                                  | Main purpose                                      | Typical API / Example                             | Key idea                                        |
| ------------------------- | ---------------------------------------------- | ------------------------------------------------- | ------------------------------------------------- | ----------------------------------------------- |
| **State**                 | Current application data                       | Represents what is true right now                 | `{ count: 0 }`                                    | State should be predictable                     |
| **Action**                | An event describing what happened              | Communicates a state-changing event               | `{ type: "cart/itemAdded", payload: product }`    | **What happened?**                              |
| **Reducer**               | Function that calculates next state            | Defines how state changes                         | `(state, action) => newState`                     | **How should state change?**                    |
| **Store**                 | Central Redux state container                  | Holds state and manages updates                   | `configureStore()`                                | Single source of truth                          |
| **Dispatch**              | Sends an action to Redux                       | Starts a state transition                         | `dispatch(addToCart(product))`                    | Components request changes through dispatch     |
| **Subscription**          | Listening for store updates                    | Allows UI to react to state changes               | Handled by React-Redux                            | State change → UI can update                    |
| **Redux data flow**       | One-way state update cycle                     | Makes state changes predictable                   | `UI → Action → Reducer → Store → UI`              | Data flows in one direction                     |
| **Pure reducer**          | Reducer without side effects                   | Makes state transitions predictable               | `reducer(state, action)`                          | Same state + action → same result               |
| **Immutability**          | Don't directly mutate existing state           | Enables reliable change detection                 | `{ ...state, value: 10 }`                         | New state/reference represents changes          |
| **Redux Toolkit (RTK)**   | Official modern Redux toolset                  | Simplifies Redux development                      | `@reduxjs/toolkit`                                | Modern way to write Redux                       |
| **`configureStore`**      | Creates/configures Redux store                 | Sets up reducers, middleware, DevTools, checks    | `configureStore({ reducer })`                     | Recommended store setup                         |
| **`createSlice`**         | Defines a state section and its reducers       | Combines state + reducers + actions               | `createSlice({ name, initialState, reducers })`   | Core RTK API                                    |
| **Slice**                 | Logical section of Redux state                 | Organizes application state by feature            | `authSlice`, `cartSlice`                          | One feature/state domain                        |
| **`initialState`**        | Starting state for a slice                     | Defines initial data                              | `{ items: [], status: "idle" }`                   | State shape starts here                         |
| **Case reducer**          | Reducer function inside a slice                | Defines a specific state transition               | `increment(state) { ... }`                        | Defines how an event changes state              |
| **Action creator**        | Function that creates an action                | Makes dispatching actions convenient              | `increment()`                                     | Generated automatically by `createSlice`        |
| **Action type**           | String identifying an action                   | Identifies the event                              | `"counter/increment"`                             | Usually `<sliceName>/<actionName>`              |
| **Payload**               | Data carried by an action                      | Supplies information to reducer                   | `action.payload`                                  | "What data came with the event?"                |
| **Immer**                 | Immutable-update library used by RTK           | Lets reducers use mutation-like syntax safely     | `state.value += 1`                                | Looks mutable, produces immutable state         |
| **`extraReducers`**       | Allows a slice to respond to external actions  | Handles actions not defined in its own `reducers` | `builder.addCase(...)`                            | One action can affect multiple slices           |
| **`builder.addCase`**     | Registers a reducer for a specific action      | Handles external/lifecycle actions                | `.addCase(action, reducer)`                       | Common with async thunks                        |
| **`createAsyncThunk`**    | Creates an async workflow                      | Handles async operations and lifecycle actions    | `createAsyncThunk("users/fetch", async () => {})` | Generates `pending`, `fulfilled`, `rejected`    |
| **Pending**               | Async operation started                        | Represents loading state                          | `fetchUsers.pending`                              | `status = "loading"`                            |
| **Fulfilled**             | Async operation succeeded                      | Stores successful result                          | `fetchUsers.fulfilled`                            | `action.payload` contains result                |
| **Rejected**              | Async operation failed                         | Stores error state                                | `fetchUsers.rejected`                             | Handle `action.error`                           |
| **Middleware**            | Logic between `dispatch()` and reducer         | Enables async logic, logging, checks, etc.        | Thunk middleware                                  | `dispatch → middleware → reducer`               |
| **`useDispatch`**         | React-Redux hook for dispatching               | Lets components send actions                      | `dispatch(addToCart(product))`                    | UI → event                                      |
| **`useSelector`**         | React-Redux hook for reading state             | Lets components subscribe to selected state       | `useSelector(state => state.cart)`                | Store → UI                                      |
| **Selector**              | Function that reads/derives state              | Encapsulates state access                         | `state => state.cart.items`                       | Keeps components independent of state structure |
| **Derived state**         | Data calculated from existing state            | Avoids duplicate sources of truth                 | `items → total`                                   | Store minimum necessary state                   |
| **`createSelector`**      | Creates memoized selectors                     | Efficiently derives data                          | `createSelector([selectItems], ...)`              | Recomputes when inputs change                   |
| **Memoization**           | Reuses previous calculation result             | Improves performance                              | `createSelector()`                                | Same inputs → cached result                     |
| **Normalization**         | Store related entities in lookup-friendly form | Efficient updates/lookups                         | `{ ids: [], entities: {} }`                       | Avoid duplicated nested data                    |
| **`createEntityAdapter`** | RTK utility for normalized entities            | CRUD + selectors for entity collections           | `createEntityAdapter()`                           | Great for large entity collections              |
| **RTK Query**             | RTK's server-state/data-fetching solution      | Fetching, caching, invalidation, refetching       | `createApi()`                                     | Designed for **server state**                   |
| **Client state**          | State belonging to application/UI behavior     | Controls local application behavior               | Cart, modal, theme, selected item                 | Often suited to `createSlice`                   |
| **Server state**          | Data originating from an API/backend           | Represents remote data + cache                    | Products, users, orders                           | Often suited to RTK Query                       |
| **Provider**              | Connects Redux store to React                  | Makes store available to components               | `<Provider store={store}>`                        | React-Redux integration                         |
| **DevTools**              | Redux debugging interface                      | Inspect actions/state transitions                 | Configured through `configureStore`               | Great for understanding state changes           |
| **Serializable state**    | State/actions made of serializable data        | Enables predictable debugging/devtools            | Objects, arrays, strings, numbers                 | Avoid functions/classes/complex instances       |
| **Feature architecture**  | Organizing Redux by business feature           | Keeps large apps maintainable                     | `features/cart`, `features/auth`                  | Organize around features, not API types         |

### The most important relationships

| If you understand...              | You understand...                           |
| --------------------------------- | ------------------------------------------- |
| **State + Action + Reducer**      | Core Redux                                  |
| **Dispatch → Reducer → Store**    | Redux state transitions                     |
| **`createSlice`**                 | Modern Redux reducer/action creation        |
| **Immer**                         | Why RTK reducers can appear to mutate state |
| **`extraReducers`**               | How slices respond to external actions      |
| **`createAsyncThunk`**            | Traditional RTK async workflows             |
| **Selectors + `createSelector`**  | Reading and deriving state properly         |
| **`configureStore`**              | How the Redux application is assembled      |
| **`useSelector` + `useDispatch`** | How React communicates with Redux           |
| **RTK Query**                     | Modern server-state management              |
| **Client state vs server state**  | When to use slices vs RTK Query             |

### The entire mental model in one table

| Layer                | Question it answers                                        | Redux/RTK piece                          |
| -------------------- | ---------------------------------------------------------- | ---------------------------------------- |
| **UI**               | What does the user see/do?                                 | React                                    |
| **Event**            | What happened?                                             | Action                                   |
| **Dispatch**         | How does the event enter Redux?                            | `dispatch()`                             |
| **Middleware**       | Does anything need to happen between dispatch and reducer? | Thunk, middleware                        |
| **Transition**       | How should state change?                                   | Reducer                                  |
| **Immutable update** | How do we safely create the next state?                    | Immer                                    |
| **Storage**          | Where does application state live?                         | Store                                    |
| **Read**             | What state does this component need?                       | Selector                                 |
| **Derive**           | Can we calculate data instead of storing it?               | `createSelector`                         |
| **Async**            | How do we handle an async workflow?                        | `createAsyncThunk`                       |
| **Server data**      | How do we fetch/cache/invalidate remote data?              | RTK Query                                |
| **React connection** | How does React access Redux?                               | `Provider`, `useSelector`, `useDispatch` |

### The one diagram to remember

```text
                    USER
                     │
                     ↓
                React UI
                     │
                     │ dispatch(action)
                     ↓
                  ACTION
                     │
                     ↓
                MIDDLEWARE
                     │
                     ↓
                  REDUCER
                     │
                 Immer
                     │
                     ↓
               NEW STATE
                     │
                     ↓
                  STORE
                     │
                     ↓
                SELECTOR
                     │
                     ↓
                React UI
                     │
                     └──────────────→ user interaction
```

And for modern RTK architecture:

```text
                 Redux Toolkit
                       │
          ┌────────────┴────────────┐
          ↓                         ↓
    Client/App State           Server State
          │                         │
     createSlice                RTK Query
          │                         │
          ↓                         ↓
       Reducers              Fetch + Cache
       Actions               Invalidation
       Selectors             Refetching
```

**Core takeaway:** Redux is the architecture; **Redux Toolkit is the modern toolkit for implementing that architecture efficiently**. `createSlice` is the heart of client-state management, while **RTK Query is the major tool for server-state management**.


# Deep Down

## Our roadmap

```text
                 REDUX
                   │
        ┌──────────┴──────────┐
        ↓                     ↓
   Core concepts          React integration
        │                     │
        ├─ State              ├─ Provider
        ├─ Actions            ├─ useSelector
        ├─ Reducers           └─ useDispatch
        ├─ Store
        ├─ Dispatch
        └─ Subscriptions
                   │
                   ↓
             Redux Toolkit
                   │
        ┌──────────┼──────────┐
        ↓          ↓          ↓
   createSlice  Store setup  Immer
        │
        ↓
     Middleware
        │
        ↓
   Async Redux
        │
        ├─ Thunks
        ├─ createAsyncThunk
        └─ RTK Query
        │
        ↓
      Advanced
        ├─ Selectors
        ├─ Memoization
        ├─ Normalization
        ├─ Entity adapters
        ├─ Performance
        └─ Architecture
```

But before writing much code, there's one question we need to nail.

# 1. Why does Redux exist?

Suppose we have this React application:

```jsx
function App() {
  return (
    <>
      <Navbar />
      <Products />
      <Cart />
    </>
  );
}
```

Now imagine:

* `Products` knows about products.
* `Cart` needs to know which products were added.
* `Navbar` needs to display the cart count.
* `Checkout` needs the cart.
* `Profile` needs the current user.
* `Products` needs to know whether the user is authenticated.

You can pass data with props:

```text
App
├── Navbar ← cart
├── Products ← user
└── Cart ← products
```

But as the application grows:

```text
App
 ↓
Layout
 ↓
Page
 ↓
Section
 ↓
Component
 ↓
Child
```

you can end up passing state through components that **don't actually care about that state**.

That's commonly called **prop drilling**.

Redux attacks a different problem:

> How do we manage shared application state in a predictable way?

---

# 2. Redux is not primarily about avoiding prop drilling

This distinction is important.

People often say:

> "Redux is for global state."

That's incomplete.

You could make almost everything global and create a terrible Redux application.

Redux is really about:

> **Centralized, predictable state transitions.**

Consider a shopping cart.

We have:

```js
{
  items: [],
  total: 0
}
```

Several things can change it:

```text
Add product
Remove product
Change quantity
Clear cart
Apply discount
Restore cart from storage
Checkout
```

We want a predictable system for those transitions.

Instead of components randomly changing shared data:

```text
Component A ──┐
Component B ──┼──> shared state 😵
Component C ──┘
```

Redux establishes:

```text
Component
    │
    │ "something happened"
    ↓
  Action
    │
    ↓
 Reducer
    │
    ↓
 New State
```

That constraint is actually Redux's superpower.

---

# 3. The Redux rule

The fundamental rule is:

> **State changes through actions.**

Imagine:

```js
state = {
  count: 0
}
```

A component shouldn't conceptually say:

```js
state.count++;
```

Instead:

```js
dispatch({
  type: "counter/increment"
});
```

The action says:

> "An increment happened."

Then the reducer decides:

> "Given that event, the state should become this."

```js
function reducer(state, action) {
  if (action.type === "counter/increment") {
    return {
      ...state,
      count: state.count + 1
    };
  }

  return state;
}
```

This gives us a very important separation:

```text
WHO caused it?
      ↓
  Component

WHAT happened?
      ↓
   Action

HOW does state change?
      ↓
   Reducer
```

That separation becomes incredibly valuable in large applications.

---

# 4. Actions are events

This is one of the deepest ideas in Redux.

Suppose a user adds a laptop to their cart.

An action might be:

```js
{
  type: "cart/itemAdded",
  payload: {
    id: 42,
    name: "Laptop",
    price: 1200
  }
}
```

The action isn't:

```js
"change cart state"
```

It's describing an event:

```text
itemAdded
```

That's why Redux actions are often best thought of as **events**.

For example:

```text
userLoggedIn
productAdded
productRemoved
checkoutStarted
paymentSucceeded
paymentFailed
```

This becomes especially useful when applications get complicated.

---

# 5. Reducers are state transition functions

A reducer is fundamentally a function:

```text
(previous state, event) → next state
```

Mathematically:

```text
R(S, A) → S'
```

Where:

* `S` = current state
* `A` = action
* `S'` = next state

Example:

```js
const state = {
  count: 10
};

const action = {
  type: "counter/increment"
};
```

Reducer:

```js
function reducer(state, action) {
  switch (action.type) {
    case "counter/increment":
      return {
        ...state,
        count: state.count + 1
      };

    default:
      return state;
  }
}
```

Result:

```js
{
  count: 11
}
```

This leads to an important Redux property:

### Same state + same action → same result

Ideally:

```text
R(S, A) = same result
```

That's why reducers should be **pure**.

They shouldn't:

```js
fetch(...)
Math.random()
Date.now()
localStorage.setItem(...)
```

inside the reducer.

Reducers should primarily calculate state.

---

# 6. Why immutability?

Suppose:

```js
const state = {
  user: {
    name: "Alice"
  }
};
```

You might mutate it:

```js
state.user.name = "Bob";
```

The object itself remains the same reference.

Redux/React ecosystems rely heavily on **reference identity** to efficiently determine what changed.

Instead, conceptually:

```js
const newState = {
  ...state,
  user: {
    ...state.user,
    name: "Bob"
  }
};
```

Now:

```text
old state ────────┐
                  │
new state ────────┴── different reference
```

This makes change detection much easier.

---

# 7. And this is where RTK becomes interesting

If you've used Redux Toolkit, you've probably seen this:

```js
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

Wait.

Didn't we just say:

> Don't mutate Redux state?

Yes.

So why is this allowed?

Because RTK uses **Immer**.

You write:

```js
state.value += 1;
```

Immer interprets that as:

```text
"I want the resulting state to have value + 1."
```

and produces the appropriate immutable state update.

Conceptually:

```text
Your reducer
     │
     ↓
 state.value += 1
     │
     ↓
    Immer
     │
     ↓
Immutable next state
```

So RTK isn't throwing away Redux's principles.

It's giving us a much nicer syntax for following them.

---

# 8. The Store

Now let's zoom out.

Redux has a **store**.

Think of it as the application's state container.

For example:

```js
{
  auth: {
    user: null,
    isAuthenticated: false
  },

  cart: {
    items: [],
    total: 0
  },

  products: {
    items: [],
    status: "idle"
  }
}
```

The store isn't simply a giant object.

It's a system responsible for things like:

```text
State
  +
Dispatching
  +
Reducer execution
  +
Subscriptions
```

Conceptually:

```text
             STORE
       ┌─────────────────┐
       │                 │
       │     State       │
       │                 │
       │    Reducers      │
       │                 │
       │    Dispatch      │
       │                 │
       │  Subscribers     │
       │                 │
       └─────────────────┘
```

---

# 9. What actually happens when you dispatch?

This is worth understanding at a low level.

Suppose:

```js
dispatch({
  type: "counter/increment"
});
```

Redux roughly goes through:

```text
dispatch(action)
       │
       ↓
send action to reducer
       │
       ↓
reducer(currentState, action)
       │
       ↓
nextState
       │
       ↓
store updates
       │
       ↓
subscribers notified
       │
       ↓
React-Redux checks subscriptions
       │
       ↓
components needing changed data re-render
```

This is the **Redux data flow**.

And notice something:

### The flow is one-directional.

```text
Action
  ↓
Reducer
  ↓
State
  ↓
UI
  ↓
User interaction
  ↓
Action
  ↓
...
```

That's why Redux applications can be easier to reason about.

---

# 10. Redux + React

Redux itself doesn't actually require React.

Redux is a state-management library.

React-Redux is the library that connects Redux with React.

That's an important distinction:

```text
Redux
  │
  │ state management
  ↓
Store
```

while:

```text
React-Redux
  │
  │ integration
  ↓
React components
```

React-Redux gives us hooks such as:

```js
useSelector()
useDispatch()
```

---

# 11. `useSelector` is more important than it looks

Consider:

```js
const count = useSelector(
  state => state.counter.value
);
```

You're effectively saying:

> "This component depends on `state.counter.value`."

React-Redux can use that information to determine whether the selected value changed.

So selectors aren't merely:

```js
"How do I get data?"
```

They're also part of the architecture for:

```text
state → derived data → component
```

Later we'll go deep into:

* selector composition
* derived state
* memoized selectors
* Reselect
* performance

---

# 12. What does `useDispatch` actually do?

You write:

```js
const dispatch = useDispatch();

dispatch(increment());
```

`useDispatch()` gives you access to the store's dispatch function.

So:

```js
dispatch(action)
```

is basically telling Redux:

> "Process this event."

The component doesn't need to know:

```text
how the state is stored
how the reducer works
how other components use the state
```

It just dispatches the event.

---

# 13. Why Redux Toolkit?

Now we can understand RTK properly.

Redux itself gives us the architecture.

RTK gives us the **recommended tools for implementing that architecture**.

RTK provides things like:

```text
configureStore
createSlice
createAsyncThunk
createEntityAdapter
createSelector
RTK Query
```

And sensible defaults for things like:

```text
middleware
DevTools
immutability checks
serializability checks
```

So don't think:

```text
Redux vs Redux Toolkit
```

Think:

```text
Redux
  ↓
architecture / principles

Redux Toolkit
  ↓
modern way of implementing Redux
```

---

# 14. The next level: `createSlice`

Let's build something real.

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
    },

    decrement(state) {
      state.value -= 1;
    },

    incrementByAmount(state, action) {
      state.value += action.payload;
    }
  }
});
```

Something extremely important happens here.

`createSlice()` generates action creators for us.

So:

```js
increment()
```

produces something conceptually like:

```js
{
  type: "counter/increment"
}
```

And:

```js
incrementByAmount(5)
```

produces:

```js
{
  type: "counter/incrementByAmount",
  payload: 5
}
```

This is one of the biggest improvements over old Redux.

Instead of manually writing:

```js
const INCREMENT = "counter/increment";

function increment() {
  return {
    type: INCREMENT
  };
}
```

RTK generates it.

---

# 15. One subtle distinction: reducer vs action creator

This confuses almost everyone initially.

Inside:

```js
reducers: {
  increment(state) {
    state.value += 1;
  }
}
```

`increment` is a **case reducer**.

After `createSlice`, you get:

```js
counterSlice.actions.increment
```

which is an **action creator**.

So:

```text
case reducer
     │
     │ createSlice generates
     ↓
action creator
     │
     │ called
     ↓
action object
```

For example:

```js
increment()
```

returns:

```js
{
  type: "counter/increment"
}
```

Then:

```js
dispatch(increment());
```

sends that action to Redux.

This distinction becomes very important when we get into more advanced Redux.

---

# 16. And `extraReducers`

You'll eventually encounter:

```js
extraReducers: builder => {
  // ...
}
```

Don't worry about it yet.

But remember the distinction:

### `reducers`

Usually define actions/events belonging to that slice:

```js
reducers: {
  itemAdded() {},
  itemRemoved() {}
}
```

### `extraReducers`

Allows a slice to respond to actions **defined somewhere else**.

This becomes particularly important with:

```js
createAsyncThunk
```

and we'll get there.

---

# 17. The deeper mental model

If you remember only one diagram from our exploration, remember this:

```text
                 ┌──────────────┐
                 │     UI       │
                 └──────┬───────┘
                        │
                  user interaction
                        │
                        ↓
                 ┌──────────────┐
                 │    ACTION    │
                 │              │
                 │ "itemAdded"  │
                 └──────┬───────┘
                        │
                    dispatch
                        │
                        ↓
                 ┌──────────────┐
                 │   REDUCER    │
                 │              │
                 │ state +      │
                 │ action       │
                 └──────┬───────┘
                        │
                  next state
                        │
                        ↓
                 ┌──────────────┐
                 │    STORE     │
                 └──────┬───────┘
                        │
                     select
                        │
                        ↓
                 ┌──────────────┐
                 │      UI      │
                 └──────────────┘
```

That loop is Redux.

**RTK makes that loop easier and safer to implement.**

---

## Where we'll go from here

I'd like to go **one layer deeper than tutorials usually do**.

Next, we can build a tiny Redux implementation **ourselves**, without Redux or RTK:

```js
createStore()
dispatch()
subscribe()
getState()
```

Then we'll implement reducers manually.

Then we'll see exactly why Redux's architecture looks the way it does.

After that:

```text
DIY Redux
   ↓
real Redux
   ↓
Redux Toolkit
   ↓
createSlice
   ↓
React-Redux
   ↓
async middleware
   ↓
createAsyncThunk
   ↓
RTK Query
```

That route will make Redux feel much less like a collection of APIs and much more like a system you actually understand.
