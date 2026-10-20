# Redux Toolkit Summary

| Topic                 | Purpose                   | Key Point                            | Example / Mental Model                             |
| --------------------- | ------------------------- | ------------------------------------ | -------------------------------------------------- |
| **Redux Toolkit**     | Simplifies Redux          | Less boilerplate                     | `configureStore`, `createSlice`, RTK Query         |
| `configureStore`      | Creates store             | Reducers + middleware + DevTools     | `configureStore({ reducer })`                      |
| Reducer keys          | Define state shape        | Key → `state.key`                    | `reducer: { users: usersReducer }` → `state.users` |
| `createSlice`         | Manage state              | Generates reducer + actions + types  | `createSlice({ name, initialState, reducers })`    |
| `name`                | Namespace actions         | Creates action type prefix           | `name:"auth"` + `login` → `"auth/login"`           |
| `initialState`        | Starting state            | Defines slice state                  | `{ user:null, loading:false }`                     |
| `reducers`            | Define state changes      | Also generates action creators/types | `login(state, action)`                             |
| `action`              | Describes event           | `type` + optional `payload`          | `{ type:"auth/login", payload:user }`              |
| `action.type`         | Says what happened        | Usually `slice/action`               | `"auth/login"`                                     |
| `action.payload`      | Carries data              | Main action data                     | `payload: user`                                    |
| **Immer**             | Enables immutable updates | Mutation-like syntax is safe         | `state.value++` → immutable next state             |
| `prepare`             | Build/normalize payload   | Customize generated action creator   | Add ID before dispatch                             |
| `extraReducers`       | Handle external actions   | Common with thunks                   | `builder.addCase(...)`                             |
| `builder.addCase`     | Handle specific action    | Most common `extraReducers` method   | `addCase(fetch.pending, ...)`                      |
| `createAsyncThunk`    | Async workflows           | Generates 3 lifecycle actions        | `pending → fulfilled/rejected`                     |
| Thunk `pending`       | Request started           | Set loading state                    | `status = "loading"`                               |
| Thunk `fulfilled`     | Request succeeded         | Store result                         | `items = action.payload`                           |
| Thunk `rejected`      | Request failed            | Store error                          | `error = action.error`                             |
| `createSelector`      | Derived state             | Memoizes result                      | Calculate cart total                               |
| Selector              | Read/derive state         | Keeps state access reusable          | `selectCartItems(state)`                           |
| `createEntityAdapter` | Normalize collections     | `{ ids, entities }`                  | Fast entity lookup + CRUD                          |
| **RTK Query**         | Server-state management   | Fetch + cache + invalidate           | `useGetUsersQuery()`                               |
| Client state          | App/UI-owned state        | Usually `createSlice`                | Theme, modal, cart UI                              |
| Server state          | API-owned data            | Usually RTK Query                    | Users, products, orders                            |
| Serializable state    | Debuggable Redux data     | Prefer plain data                    | Strings, numbers, arrays, objects                  |
| Middleware            | Intercept/process actions | `configureStore` adds defaults       | Async logic, checks                                |
| Redux DevTools        | Debug Redux               | Inspect actions/state                | `action → state change`                            |

| **API**               | **Remember It As**                         |
| --------------------- | ------------------------------------------ |
| `configureStore`      | **Create the store**                       |
| `createSlice`         | **Create state logic**                     |
| `createAction`        | **Create an action**                       |
| `createReducer`       | **Create a reducer**                       |
| `createAsyncThunk`    | **Handle async workflow**                  |
| `extraReducers`       | **React to external actions**              |
| `createSelector`      | **Memoized derived state**                 |
| `createEntityAdapter` | **Normalized entity data**                 |
| **RTK Query**         | **API/server state**                       |
| **Immer**             | **Immutable updates with mutation syntax** |

| **Core Flow**      | **Meaning**                         |
| ------------------ | ----------------------------------- |
| `dispatch(action)` | Component sends an event            |
| ↓ middleware       | Action can be processed/intercepted |
| ↓ reducer          | State transition happens            |
| ↓ Immer            | Immutable next state produced       |
| ↓ store            | New state stored                    |
| ↓ selector         | Relevant data selected              |
| ↓ React            | Component re-renders                |

| **Most Important Distinction** | **Use**               |
| ------------------------------ | --------------------- |
| Application/client state       | `createSlice`         |
| Custom async business logic    | `createAsyncThunk`    |
| API/server state               | **RTK Query**         |
| Read/derive state              | Selectors             |
| Large normalized collections   | `createEntityAdapter` |

| **5-Second Mental Model** |                              |
| ------------------------- | ---------------------------- |
| **Store**                 | Holds state                  |
| **Slice**                 | Manages a piece of state     |
| **Action**                | Says what happened           |
| **Reducer**               | Changes state                |
| **Immer**                 | Makes immutable updates easy |
| **Thunk**                 | Handles async workflows      |
| **Selector**              | Reads/derives state          |
| **RTK Query**             | Manages API data             |

# Deep Down : Redux Toolkit

## 1. First: what exactly is Redux Toolkit?

Think of RTK as a toolbox around Redux:

```text
                    Redux Toolkit
                         │
       ┌─────────────────┼──────────────────┐
       ↓                 ↓                  ↓
 configureStore      createSlice       createAsyncThunk
       │                 │                  │
       ↓                 ↓                  ↓
    Store            Reducers +         Async lifecycle
                     Actions             actions
                         │
                         ↓
                      Immer
                         │
                         ↓
                 Immutable updates
                         
       ┌─────────────────────────────────────┐
       │             RTK Query               │
       │                                     │
       │ fetching + caching + invalidation   │
       └─────────────────────────────────────┘
```

The most important RTK APIs to master are:

1. `configureStore`
2. `createSlice`
3. `createAction`
4. `createReducer`
5. `createAsyncThunk`
6. `createSelector`
7. `createEntityAdapter`
8. **RTK Query**

We should understand the first two extremely deeply before moving on.

## 2. `configureStore()` — the foundation

Let's start here.

```js
import { configureStore } from "@reduxjs/toolkit";

const store = configureStore({
  reducer: {
    counter: counterReducer,
    users: usersReducer,
  },
});
```

At first glance, this looks simple.

But there's a lot happening.

The object:

```js
{
  counter: counterReducer,
  users: usersReducer
}
```

defines the shape of your Redux state.

So your state becomes:

```js
{
  counter: {
    value: 0
  },

  users: {
    items: [],
    status: "idle"
  }
}
```

Notice:

```js
state.counter
state.users
```

come directly from the keys:

```js
reducer: {
  counter: ...
  users: ...
}
```

This is the **root reducer structure**.

## 3. Why `configureStore()` instead of `createStore()`?

Old Redux:

```js
const store = createStore(rootReducer);
```

Modern RTK:

```js
const store = configureStore({
  reducer: rootReducer
});
```

But `configureStore` isn't merely a renamed `createStore`.

It configures useful defaults for you.

Conceptually:

```text
configureStore()
      │
      ├── combine reducers
      ├── middleware
      ├── DevTools
      ├── development checks
      └── store creation
```

Among the default middleware are checks related to:

* immutability
* serializability

These are extremely useful while learning Redux because they catch mistakes.

## 4. Why serializable state matters

Redux strongly prefers state and actions to contain serializable values.

Good:

```js
{
  id: 1,
  name: "Alice",
  createdAt: "2026-08-09"
}
```

Potentially problematic:

```js
{
  user: new User(),
  socket: someWebSocket,
  callback: () => {}
}
```

Why?

Because Redux DevTools and Redux's debugging model work extremely well when state can be represented as data.

You want to be able to conceptually say:

```text
State before
      ↓
Action
      ↓
State after
```

and inspect that history.

## 5. Now the star: `createSlice()`

Suppose we're building authentication.

```js
const initialState = {
  user: null,
  isAuthenticated: false,
};
```

We could manually write actions and reducers.

RTK lets us say:

```js
const authSlice = createSlice({
  name: "auth",

  initialState,

  reducers: {
    login(state, action) {
      state.user = action.payload;
      state.isAuthenticated = true;
    },

    logout(state) {
      state.user = null;
      state.isAuthenticated = false;
    },
  },
});
```

Then:

```js
export const { login, logout } = authSlice.actions;

export default authSlice.reducer;
```

This one `createSlice()` call produces several things.

Conceptually:

```text
                 createSlice()
                      │
          ┌───────────┼───────────┐
          ↓           ↓           ↓
      reducer     action       action
                  creators      types
```

So:

```js
authSlice.reducer
```

is the reducer.

And:

```js
authSlice.actions.login
authSlice.actions.logout
```

are action creators.

## 6. Let's understand `name`

```js
createSlice({
  name: "auth",
  ...
});
```

The name contributes to generated action types.

For:

```js
login
```

RTK generates:

```js
"auth/login"
```

For:

```js
logout
```

it generates:

```js
"auth/logout"
```

So:

```js
dispatch(login(user));
```

creates something conceptually like:

```js
{
  type: "auth/login",
  payload: user
}
```

This naming convention is extremely useful when debugging.

You can look at Redux DevTools and see:

```text
auth/login
cart/itemAdded
products/fetchSuccess
```

rather than anonymous events.

## 7. `reducers` is doing two jobs

This is subtle.

When you write:

```js
reducers: {
  increment(state) {
    state.value += 1;
  }
}
```

RTK uses this to create:

#### A reducer case

```js
increment(state, action) {
   ...
}
```

#### An action creator

```js
increment()
```

#### An action type

```text
"counter/increment"
```

So one definition generates the whole little ecosystem.

That's why `createSlice()` is so powerful.

## 8. Payloads

Now:

```js
addTodo(state, action) {
  state.todos.push(action.payload);
}
```

Call:

```js
dispatch(addTodo({
  id: 1,
  title: "Learn Redux"
}));
```

The resulting action is conceptually:

```js
{
  type: "todos/addTodo",

  payload: {
    id: 1,
    title: "Learn Redux"
  }
}
```

The convention is:

```js
action.payload
```

for the primary data associated with an action.

You can think:

```text
type
 ↓
"What happened?"

payload
 ↓
"What data came with it?"
```

## 9. Immer — the magic underneath

This is probably the most important RTK implementation detail to understand.

You write:

```js
increment(state) {
  state.value += 1;
}
```

It appears to mutate state.

Traditional Redux says:

> Don't mutate state.

So what's happening?

RTK uses **Immer**.

Conceptually:

```text
                Your reducer
                     │
                     ↓
             "draft" state
                     │
              state.value++
                     │
                     ↓
                  Immer
                     │
                     ↓
           immutable next state
```

Imagine:

```js
const oldState = {
  user: {
    name: "Alice"
  },
  settings: {
    theme: "dark"
  }
};
```

You write:

```js
state.user.name = "Bob";
```

Immer produces a new immutable state while preserving references for parts that didn't change.

Conceptually:

```text
oldState
 ├── user ──→ old user
 └── settings ──→ old settings

newState
 ├── user ──→ NEW user
 └── settings ──→ SAME settings
```

That's an important performance property.

## 10. Why this matters

Suppose your state is:

```js
{
  users: [...],
  products: [...],
  cart: [...],
  settings: {...}
}
```

You update one cart item.

You don't want every nested object to be recreated unnecessarily.

Immutable updates generally preserve references for unchanged portions.

That's why React-Redux can efficiently determine what changed.

## 11. `prepare` — advanced `createSlice`

Here's something you'll eventually encounter:

```js
const todosSlice = createSlice({
  name: "todos",

  initialState: [],

  reducers: {
    todoAdded: {
      reducer(state, action) {
        state.push(action.payload);
      },

      prepare(text) {
        return {
          payload: {
            id: crypto.randomUUID(),
            text,
            completed: false
          }
        };
      }
    }
  }
});
```

Now:

```js
dispatch(todoAdded("Learn RTK"));
```

can generate:

```js
{
  type: "todos/todoAdded",
  payload: {
    id: "...",
    text: "Learn RTK",
    completed: false
  }
}
```

The `prepare` callback is useful when the action creator needs to construct or normalize its payload.

## 12. `extraReducers`

Now we reach one of the most important RTK concepts.

Suppose the `auth` slice needs to react to an action created elsewhere.

You can do:

```js
extraReducers: builder => {
  builder.addCase(someAction, (state, action) => {
    // update state
  });
}
```

Why?

Because not every action belongs exclusively to one slice.

Imagine:

```text
fetchUser.pending
fetchUser.fulfilled
fetchUser.rejected
```

The user slice might care.

The notification slice might also care.

The loading indicator might care.

So one action can affect multiple slices.

```text
             fetchUser.fulfilled
                    │
          ┌─────────┼─────────┐
          ↓         ↓         ↓
       userSlice  authSlice  uiSlice
```

This is where `extraReducers` becomes powerful.

## 13. `builder`

You'll frequently see:

```js
extraReducers: builder => {
  builder
    .addCase(...)
    .addCase(...)
    .addMatcher(...)
    .addDefaultCase(...);
}
```

Think of `builder` as a declarative way to say:

> "When these actions happen, this slice should respond."

The most important one initially is:

```js
builder.addCase(action, reducer)
```

## 14. `createAsyncThunk`

Now we enter the async world.

Suppose we need:

```text
GET /api/users
```

A request isn't instantaneous.

There are at least three states:

```text
idle
  ↓
loading
  ↓
success

or

loading
  ↓
error
```

`createAsyncThunk()` models this beautifully.

```js
const fetchUsers = createAsyncThunk(
  "users/fetchUsers",
  async () => {
    const response = await fetch("/api/users");
    return response.json();
  }
);
```

One declaration generates three lifecycle actions:

```text
users/fetchUsers/pending
users/fetchUsers/fulfilled
users/fetchUsers/rejected
```

That's a major RTK concept.

## 15. Handling the lifecycle

Then:

```js
const usersSlice = createSlice({
  name: "users",

  initialState: {
    items: [],
    status: "idle",
    error: null
  },

  reducers: {},

  extraReducers: builder => {
    builder
      .addCase(fetchUsers.pending, state => {
        state.status = "loading";
      })

      .addCase(fetchUsers.fulfilled, (state, action) => {
        state.status = "succeeded";
        state.items = action.payload;
      })

      .addCase(fetchUsers.rejected, (state, action) => {
        state.status = "failed";
        state.error = action.error.message;
      });
  }
});
```

Now we have:

```text
             dispatch(fetchUsers())
                       │
                       ↓
                    pending
                       │
                       ↓
                   API request
                    /       \
                   /         \
             success        failure
                │               │
                ↓               ↓
           fulfilled         rejected
                │               │
                ↓               ↓
            state update    state update
```

This is one of the most useful patterns in traditional Redux applications.

## 16. But there's an important modern distinction

For **server state**, you shouldn't automatically reach for `createAsyncThunk`.

Why?

Because server state has problems beyond simply "make a request":

```text
fetching
caching
deduplication
refetching
stale data
polling
cache invalidation
request lifecycle
loading state
error state
```

That's where **RTK Query** comes in.

This is a huge topic and deserves its own deep dive.

## 17. Client state vs server state

This distinction will make your Redux architecture much better.

#### Client/application state

Things like:

```js
{
  theme: "dark",
  sidebarOpen: true,
  selectedProductId: 42,
  cartItems: [...]
}
```

Redux slices are excellent here.

#### Server state

Things like:

```text
users from API
products from API
orders from API
comments from API
```

RTK Query is often the better solution.

Think:

```text
Redux Toolkit
│
├── createSlice
│      ↓
│   application/client state
│
└── RTK Query
       ↓
    server/cache state
```

This distinction is one of the biggest things separating a beginner Redux application from a well-designed one.

## 18. Selectors

Now let's talk about reading state properly.

Instead of sprinkling:

```js
state.cart.items
```

everywhere, create selectors.

```js
const selectCartItems = state => state.cart.items;
```

Then:

```js
const items = useSelector(selectCartItems);
```

But selectors can do more.

Suppose:

```js
state.cart.items
```

contains:

```js
[
  { price: 100, quantity: 2 },
  { price: 50, quantity: 3 }
]
```

We can derive:

```js
const selectCartTotal = state =>
  state.cart.items.reduce(
    (total, item) =>
      total + item.price * item.quantity,
    0
  );
```

Now:

```text
Raw state
   ↓
Selector
   ↓
Derived data
   ↓
Component
```

This is a powerful pattern.

## 19. Memoized selectors

Suppose the calculation is expensive.

We can use `createSelector`.

```js
import { createSelector } from "@reduxjs/toolkit";

const selectCartItems = state => state.cart.items;

const selectCartTotal = createSelector(
  [selectCartItems],

  items =>
    items.reduce(
      (total, item) =>
        total + item.price * item.quantity,
      0
    )
);
```

Now the selector can memoize its result.

Conceptually:

```text
items reference unchanged?
        │
       YES
        ↓
return cached result

        │
       NO
        ↓
calculate again
```

This becomes very important when optimizing larger applications.

## 20. `createEntityAdapter`

Now we're getting into advanced RTK.

Suppose you have 10,000 users:

```js
[
  { id: 1, name: "Alice" },
  { id: 2, name: "Bob" },
  ...
]
```

You frequently need:

```text
find user by ID
update user
remove user
insert user
```

An array isn't always the ideal representation.

RTK's `createEntityAdapter()` can normalize the data into something like:

```js
{
  ids: [1, 2, 3],

  entities: {
    1: { id: 1, name: "Alice" },
    2: { id: 2, name: "Bob" },
    3: { id: 3, name: "Charlie" }
  }
}
```

Now:

```js
entities[2]
```

gives you Bob directly.

And the adapter gives you useful reducers/selectors for common CRUD operations.

## 21. RTK Query

Finally, the big one.

Instead of manually doing:

```js
const fetchUsers = createAsyncThunk(...);
```

and then manually maintaining:

```js
status
error
data
```

RTK Query can manage server data for you.

Conceptually:

```text
                  RTK Query
                     │
       ┌─────────────┼──────────────┐
       ↓             ↓              ↓
    fetching       caching       invalidation
       │             │              │
       └─────────────┼──────────────┘
                     ↓
                 React hooks
```

You define an API:

```js
const api = createApi({
  reducerPath: "api",

  baseQuery: fetchBaseQuery({
    baseUrl: "/api"
  }),

  endpoints: builder => ({
    getUsers: builder.query({
      query: () => "/users"
    })
  })
});
```

Then RTK Query generates hooks:

```js
const {
  data,
  error,
  isLoading
} = useGetUsersQuery();
```

And suddenly you don't need to manually manage a lot of:

```text
loading
success
error
cache
refetch
```

## 22. The full RTK mental model

Now put everything together:

```text
                       Redux Toolkit
                            │
       ┌────────────────────┼─────────────────────┐
       │                    │                     │
       ↓                    ↓                     ↓
configureStore         createSlice          createAsyncThunk
       │                    │                     │
       ↓                    ↓                     ↓
     Store             Client state        Async workflows
                            │                     │
                            │               extraReducers
                            │                     │
                            └──────────┬──────────┘
                                       ↓
                                    Reducers
                                       │
                                       ↓
                                    Store
                                       │
                              ┌────────┴────────┐
                              ↓                 ↓
                         Selectors          Components
                              │
                              ↓
                        Derived data


                     ┌─────────────────┐
                     │    RTK Query    │
                     │                 │
                     │ server state    │
                     │ caching         │
                     │ fetching        │
                     │ invalidation    │
                     └─────────────────┘
```

## 23. The architecture I want you to eventually be comfortable with

A realistic application might look like:

```text
src/
│
├── app/
│   └── store.js
│
├── features/
│   ├── auth/
│   │   ├── authSlice.js
│   │   ├── authSelectors.js
│   │   └── authApi.js
│   │
│   ├── cart/
│   │   ├── cartSlice.js
│   │   └── cartSelectors.js
│   │
│   └── products/
│       ├── productsSlice.js
│       └── productsApi.js
│
└── components/
```

And conceptually:

```text
              UI
               │
       ┌───────┴────────┐
       ↓                ↓
 dispatch           useSelector
       │                │
       ↓                ↑
    Actions         Selectors
       │                ↑
       ↓                │
    Reducers ───────→ Store
       │
       ↓
    State
```

For server data:

```text
UI
 │
 ↓
RTK Query hook
 │
 ↓
API
 │
 ↓
cache
 │
 ↓
UI
```

### The next deep dive

I recommend we **not jump to RTK Query yet**.

The next thing to master is `createSlice()` at implementation level:

```text
createSlice
    │
    ├── name
    ├── initialState
    ├── reducers
    │     ├── case reducers
    │     ├── action creators
    │     └── action types
    │
    ├── extraReducers
    ├── prepare
    └── Immer
```

Then we can actually **trace one action from `dispatch()` → middleware → reducer → Immer → store → selector → React render**, including what references change and why.

That is where Redux Toolkit starts becoming genuinely intuitive rather than just "some React library APIs."
