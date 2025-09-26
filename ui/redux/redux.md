# Learning Redux by implementing it in React (step-by-step + definitions)

## Project setup (quick)

```bash
npx create-react-app redux-demo
cd redux-demo
npm install @reduxjs/toolkit react-redux
```

* `@reduxjs/toolkit` (RTK) is the modern, recommended Redux helper library that reduces boilerplate.
* `react-redux` provides the React bindings.


## File tree (what we’ll create)

```
src/
  index.js
  store.js
  features/
    counterSlice.js
  App.js
  components/
    Counter.js
```


## Step 0 — Core concepts (short definitions)

* **State** — the data describing your app right now (plain JS objects).
* **Store** — the single object that holds the entire application state.
* **Action** — a plain object describing *what happened* (`{type, payload}`), immutable description of event.
* **Reducer** — a *pure function* `(state, action) => newState` that returns updated state; never mutates original.
* **Dispatch** — the method used to send an action into the store.
* **Selector** — a function that extracts a piece of state (helps decouple UI from state shape).
* **Middleware** — code that runs between dispatching an action and reducers (used for logging, async flows, etc.).
* **Thunk (async action)** — a function action (via middleware) that can dispatch multiple actions asynchronously.
* **Provider** — React component that makes the store available to the component tree.


## Step 1 — Create the store (Single Source of Truth)

**Definition:** The store holds all your app state and exposes `getState()`, `dispatch()`, and `subscribe()`. With RTK you create it with `configureStore`.

`src/store.js`

```js
import { configureStore } from '@reduxjs/toolkit';
import counterReducer from './features/counterSlice';

const store = configureStore({
  reducer: {
    counter: counterReducer, // slice reducer registered under "counter"
  },
  // configureStore adds useful middleware (including redux-thunk) and devtools by default
});

export default store;
```

**Why:** Centralized state makes app behavior predictable and easier to debug.


## Step 2 — Define a slice: reducer + actions (Reducers & Actions)

**Definition:** A **reducer** is a pure function that returns the next state. **Actions** are descriptions of changes. RTK `createSlice` bundles them: it creates action creators and reducer cases.

`src/features/counterSlice.js`

```js
import { createSlice, createAsyncThunk } from '@reduxjs/toolkit';

// Example async thunk (demonstrates async action)
export const incrementAsync = createAsyncThunk(
  'counter/incrementAsync',
  async (amount, thunkAPI) => {
    // simulate async work, e.g., API call
    await new Promise(resolve => setTimeout(resolve, 500));
    return amount;
  }
);

const counterSlice = createSlice({
  name: 'counter',
  initialState: { value: 0, status: 'idle' },
  reducers: {
    increment: (state) => { state.value += 1; },
    decrement: (state) => { state.value -= 1; },
    reset: (state) => { state.value = 0; },
    incrementByAmount: (state, action) => { state.value += action.payload; },
  },
  extraReducers: (builder) => {
    builder
      .addCase(incrementAsync.pending, (state) => { state.status = 'loading'; })
      .addCase(incrementAsync.fulfilled, (state, action) => {
        state.status = 'idle';
        state.value += action.payload;
      })
      .addCase(incrementAsync.rejected, (state) => { state.status = 'failed'; });
  }
});

export const { increment, decrement, reset, incrementByAmount } = counterSlice.actions;
export default counterSlice.reducer;
```

**Key notes:**

* RTK uses Immer under the hood, so you can write "mutating" code (e.g., `state.value += 1`) while actually producing immutable updates.
* `createAsyncThunk` is the recommended way to handle async logic that dispatches actions before/after an async call.


## Step 3 — Provide the store to React (Provider)

**Definition:** `Provider` from `react-redux` makes the Redux store available to nested components via context.

`src/index.js`

```js
import React from 'react';
import ReactDOM from 'react-dom/client';
import { Provider } from 'react-redux';
import store from './store';
import App from './App';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <Provider store={store}>
    <App />
  </Provider>
);
```

**Why:** Without Provider, React components cannot access the store via hooks or `connect`.


## Step 4 — Read state & dispatch actions in components (Selectors & Dispatch)

**Definition (Selectors):** A selector is a function that reads a portion of the store state, e.g. `const selectCount = state => state.counter.value`.

**Definition (useSelector / useDispatch):**

* `useSelector` reads state from the store and subscribes your component to updates of that chosen slice.
* `useDispatch` returns the `dispatch` function you call to send actions.

`src/components/Counter.js`

```js
import React, { useState } from 'react';
import { useSelector, useDispatch } from 'react-redux';
import {
  increment,
  decrement,
  reset,
  incrementByAmount,
  incrementAsync
} from '../features/counterSlice';

// Selector - extraction function
const selectCount = (state) => state.counter.value;
const selectStatus = (state) => state.counter.status;

export default function Counter() {
  const count = useSelector(selectCount);
  const status = useSelector(selectStatus);
  const dispatch = useDispatch();
  const [amount, setAmount] = useState(5);

  return (
    <div style={{ textAlign: 'center', marginTop: 40 }}>
      <h1>Counter</h1>
      <h2>{count}</h2>
      <div>
        <button onClick={() => dispatch(decrement())}>-</button>
        <button onClick={() => dispatch(increment())}>+</button>
        <button onClick={() => dispatch(reset())}>Reset</button>
      </div>

      <hr style={{ width: '300px', margin: '20px auto' }} />

      <div>
        <input
          type="number"
          value={amount}
          onChange={(e) => setAmount(Number(e.target.value))}
          style={{ width: 80 }}
        />
        <button onClick={() => dispatch(incrementByAmount(amount))}>
          Add amount
        </button>
        <button
          onClick={() => dispatch(incrementAsync(amount))}
          disabled={status === 'loading'}
        >
          Add Async
        </button>
      </div>

      <p>Status: {status}</p>
    </div>
  );
}
```

`src/App.js`

```js
import React from 'react';
import Counter from './components/Counter';

function App() {
  return <Counter />;
}

export default App;
```

**Why:** This shows the unidirectional flow:

1. User clicks → component `dispatch`es an action.
2. Action flows through middleware (if any), reaches reducers.
3. Reducer returns new state.
4. Components subscribed via `useSelector` re-render with new state.


## Step 5 — Middleware & async flows (definition and example)

**Definition:** Middleware sits between dispatching and reducers. Useful for logging, crash reporting, performing async tasks (thunk), etc.

RTK's `configureStore` includes `redux-thunk` by default. You can add a custom logger:

Example custom logger middleware (optional — show where to add it)

```js
// src/loggerMiddleware.js
const logger = (storeAPI) => (next) => (action) => {
  console.log('will dispatch', action);
  const result = next(action);
  console.log('state after dispatch', storeAPI.getState());
  return result;
};

export default logger;
```

Then to add it (if you want custom middleware):

```js
// in store.js
import logger from './loggerMiddleware';

const store = configureStore({
  reducer: { counter: counterReducer },
  middleware: (getDefaultMiddleware) => getDefaultMiddleware().concat(logger),
});
```

**Why:** Middleware is powerful for handling side effects and cross-cutting concerns.


## Step 6 — DevTools & debugging

ConfigureStore enables Redux DevTools by default in development. Open browser Redux DevTools to time-travel, inspect actions & state. This is a major plus of using Redux.


## Step 7 — Selectors & performance

* Use simple selectors (functions that read state). For expensive derived calculations, use memoized selectors (reselect) to avoid recalculating on every render.

Example (simple selector already shown):

```js
const selectCount = state => state.counter.value;
```


## Step 8 — Testing a reducer (unit test)

**Definition:** Reducer tests verify that for a given state+action the reducer returns the correct next state.

Example (Jest):

```js
// src/features/counterSlice.test.js
import counterReducer, { increment, decrement, reset } from './counterSlice';

test('increment reducer', () => {
  const initialState = { value: 0 };
  const nextState = counterReducer(initialState, increment());
  expect(nextState.value).toBe(1);
});

test('decrement reducer', () => {
  const initialState = { value: 2 };
  const nextState = counterReducer(initialState, decrement());
  expect(nextState.value).toBe(1);
});
```

Run tests with `npm test`.


## Why use Redux Toolkit (RTK) vs vanilla Redux

* RTK reduces boilerplate (no manual action type constants, action creators, or switch statements).
* Built-in Immer for safe immutable updates with simpler syntax.
* Includes `configureStore` which wires up sensible defaults (devtools, thunk).
* Official recommended approach.


## Common pitfalls & best practices

* **Do not mutate state** outside reducers; with RTK reducers are written as mutating but are safe.
* Keep reducer logic pure (no side effects).
* Use selectors to encapsulate state shape so components are decoupled.
* Keep local UI state in component state (use Redux for shared/important/global state).
* Break state into logical slices.


## Quick recap (minimal)

1. Install RTK + react-redux.
2. Create slice (`createSlice`) → actions + reducer.
3. configureStore → register reducer(s).
4. Wrap app with `<Provider store={store}>`.
5. Read state with `useSelector`, send actions with `useDispatch`.
6. Use `createAsyncThunk` for async side-effects.
7. Debug with Redux DevTools.
