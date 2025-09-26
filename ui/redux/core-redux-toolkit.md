## Core Redux Toolkit Functions

### 1. `configureStore`

* **What:** Creates the Redux store with good defaults.
* **Why:** Less boilerplate than `createStore`. Auto-adds DevTools + middleware (like `redux-thunk`).
* **Usage:**

  ```js
  const store = configureStore({
    reducer: { counter: counterReducer },
    middleware: (getDefaultMiddleware) =>
      getDefaultMiddleware().concat(customMiddleware),
    devTools: process.env.NODE_ENV !== 'production'
  });
  ```


### 2. `createSlice`

* **What:** Generates reducer logic + action creators from a single config object.
* **Why:** No need to write action type strings or switch cases.
* **Usage:**

  ```js
  const counterSlice = createSlice({
    name: 'counter',
    initialState: { value: 0 },
    reducers: {
      increment: (state) => { state.value += 1 },
      decrement: (state) => { state.value -= 1 }
    }
  });

  export const { increment, decrement } = counterSlice.actions;
  export default counterSlice.reducer;
  ```


### 3. `createAsyncThunk`

* **What:** Simplifies async logic (like API calls).
* **Why:** Auto-creates pending, fulfilled, rejected actions.
* **Usage:**

  ```js
  export const fetchTodos = createAsyncThunk(
    'todos/fetchTodos',
    async () => {
      const res = await fetch('/api/todos');
      return res.json();
    }
  );
  ```


### 4. `createEntityAdapter`

* **What:** Normalizes and manages collections of items (like todos, posts, users).
* **Why:** Gives selectors + CRUD helpers out of the box.
* **Usage:**

  ```js
  const todosAdapter = createEntityAdapter();
  const initialState = todosAdapter.getInitialState();

  const todosSlice = createSlice({
    name: 'todos',
    initialState,
    reducers: {
      addTodo: todosAdapter.addOne,
      removeTodo: todosAdapter.removeOne,
      setAllTodos: todosAdapter.setAll
    }
  });
  ```



## React-Redux Hooks

### 1. `useSelector`

* **What:** Reads a slice of the store state.
* **Usage:**

  ```js
  const count = useSelector((state) => state.counter.value);
  ```
* **Tip:** Use selectors from slices or entity adapters to keep components decoupled.


### 2. `useDispatch`

* **What:** Returns the `dispatch` function to send actions.
* **Usage:**

  ```js
  const dispatch = useDispatch();
  dispatch(increment());
  dispatch(fetchTodos()); // works for async thunks too
  ```



### 3. `useStore` (less common)

* **What:** Gives direct access to the store object.
* **When:** Rare, but useful for debugging or advanced custom logic.
* **Usage:**

  ```js
  const store = useStore();
  console.log(store.getState());
  ```


## Useful Props/Options in RTK Functions

### In `createSlice`

* `name`: unique slice name (prefix for action types).
* `initialState`: initial state of slice.
* `reducers`: object of case reducers (pure functions).
* `extraReducers`: handles actions from other slices or thunks.


### In `createAsyncThunk`

* `typePrefix`: string prefix for action types.
* `payloadCreator`: async function that returns data.
* `condition`: optional function to cancel execution before running.
* **Generated action types:**

  * `${typePrefix}/pending`
  * `${typePrefix}/fulfilled`
  * `${typePrefix}/rejected`


### In `createEntityAdapter`

* `selectId`: how to identify items (default: `entity.id`).
* `sortComparer`: optional function for sorting.
* **Helpers:** `addOne`, `addMany`, `removeOne`, `updateOne`, `setAll`, etc.
* **Selectors:** `getSelectors((state) => state.todos)`.


# Middleware & Store Enhancers

* `redux-thunk` → built in by default (for async).
* `logger` → custom logging middleware (handy in dev).
* **Usage inside store:**

  ```js
  const store = configureStore({
    reducer,
    middleware: (gDM) => gDM().concat(logger),
  });
  ```

# Most Frequently Used (Day-to-Day)

* **RTK functions:**

  * `configureStore`
  * `createSlice`
  * `createAsyncThunk`
  * `createEntityAdapter` (if managing lists)

* **Hooks:**

  * `useSelector`
  * `useDispatch`

* **Props/options:**

  * `reducers` (in slices)
  * `extraReducers` (for thunks/other slices)
  * `initialState`
  * Async thunk states (`pending`, `fulfilled`, `rejected`)
