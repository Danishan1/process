## 1. `reducers` in `createSlice`

### **Definition**

* The `reducers` field defines **case reducers** for **synchronous, local actions** that belong to this slice.
* RTK automatically **creates action creators** and **action types** for each reducer.

### **When to use**

* Use when you want to change state directly from *inside the slice*, usually for synchronous updates.

### **Example**

```js
const counterSlice = createSlice({
  name: 'counter',
  initialState: { value: 0 },
  reducers: {
    increment: (state) => { state.value += 1 },
    decrement: (state) => { state.value -= 1 },
    reset: (state) => { state.value = 0 },
  }
});

export const { increment, decrement, reset } = counterSlice.actions;
```
 Here, `increment`, `decrement`, and `reset` are **auto-generated action creators**.


## 2. `extraReducers` in `createSlice`

### **Definition**

* The `extraReducers` field handles **actions that were not defined inside this slice’s `reducers`**.
* Commonly used for:

  * Handling `createAsyncThunk` actions (`pending`, `fulfilled`, `rejected`).
  * Responding to actions from **other slices**.

### **When to use**

* Use when your slice needs to respond to **external actions** (e.g., async API calls or other slice’s actions).

### **Example with `createAsyncThunk`**

```js
import { createSlice, createAsyncThunk } from '@reduxjs/toolkit';

// async thunk
export const fetchUser = createAsyncThunk(
  'user/fetchUser',
  async (id) => {
    const res = await fetch(`/api/user/${id}`);
    return res.json();
  }
);

const userSlice = createSlice({
  name: 'user',
  initialState: { data: null, status: 'idle' },
  reducers: {
    clearUser: (state) => { state.data = null; }
  },
  extraReducers: (builder) => {
    builder
      .addCase(fetchUser.pending, (state) => { state.status = 'loading'; })
      .addCase(fetchUser.fulfilled, (state, action) => {
        state.status = 'succeeded';
        state.data = action.payload;
      })
      .addCase(fetchUser.rejected, (state) => { state.status = 'failed'; });
  }
});

export const { clearUser } = userSlice.actions;
```

Here:

* `clearUser` is a normal action from `reducers`.
* `fetchUser` is an **async thunk**, and we handle its lifecycle in `extraReducers`.


## 3. **Side-by-Side Comparison**

| Feature              | `reducers`                        | `extraReducers`                                           |
| -------------------- | --------------------------------- | --------------------------------------------------------- |
| Who defines actions? | This slice defines them.          | Other slices or async thunks define them.                 |
| Auto action creators | Yes, generated automatically.   | No, you must import/use existing actions.               |
| Use case             | Synchronous, local state updates. | Handling async actions or responding to external actions. |
| Example              | `increment`, `reset`.             | `fetchUser.pending`, `fetchUser.fulfilled`.               |


## 4. Rule of Thumb

* **Use `reducers`** → when the action **belongs to this slice** and is **synchronous**.
* **Use `extraReducers`** → when the action is **async** (`createAsyncThunk`) or **belongs to another slice**.



### 1. What is `fetchUser`?

When you create an async thunk:

```js
import { createAsyncThunk } from '@reduxjs/toolkit';

export const fetchUser = createAsyncThunk(
  'user/fetchUser', // action type prefix
  async (userId) => {
    const response = await fetch(`/api/users/${userId}`);
    return response.json();
  }
);
```

* `fetchUser` is **an async thunk action creator**.
* You call it in your component like:

```js
dispatch(fetchUser(123));
```


### 2. Lifecycle Actions of `createAsyncThunk`

Every async thunk automatically generates **three action types**:

| Phase       | Action type                  | Meaning                                                       |
| ----------- | ---------------------------- | ------------------------------------------------------------- |
| `pending`   | `'user/fetchUser/pending'`   | The async call has started. Good for showing a loading state. |
| `fulfilled` | `'user/fetchUser/fulfilled'` | The async call succeeded. Payload contains the result.        |
| `rejected`  | `'user/fetchUser/rejected'`  | The async call failed. Payload contains the error.            |


### 3. How `fetchUser.pending` is used

Inside a slice’s `extraReducers`, you handle these phases:

```js
const userSlice = createSlice({
  name: 'user',
  initialState: { data: null, status: 'idle' },
  reducers: {},
  extraReducers: (builder) => {
    builder
      .addCase(fetchUser.pending, (state) => {
        state.status = 'loading';  // show spinner
      })
      .addCase(fetchUser.fulfilled, (state, action) => {
        state.status = 'succeeded';
        state.data = action.payload; // store fetched user
      })
      .addCase(fetchUser.rejected, (state) => {
        state.status = 'failed';
      });
  }
});
```

* `fetchUser.pending` is **an action type**, automatically created by Redux Toolkit.
* RTK generates it under the hood: `'user/fetchUser/pending'`.
* You don’t manually write `'user/fetchUser/pending'` — RTK gives you `.pending`, `.fulfilled`, `.rejected` helpers.


### 4. Why is this useful?

* It **simplifies async state management**:

  * No need to manually define separate action types (`FETCH_USER_START`, `FETCH_USER_SUCCESS`, `FETCH_USER_ERROR`).
  * Automatically gives you three lifecycle actions.
  * Works perfectly with `extraReducers`.


### 5. Usage in Component

```js
const dispatch = useDispatch();

useEffect(() => {
  dispatch(fetchUser(123)); // triggers pending → fulfilled/rejected automatically
}, [dispatch]);
```

* When dispatched:

  1. `fetchUser.pending` → sets loading state.
  2. `fetchUser.fulfilled` → updates store with result.
  3. `fetchUser.rejected` → handles error.


In short:

```text
fetchUser.pending  => Action dispatched automatically when the async thunk starts
fetchUser.fulfilled => Action dispatched automatically when the async thunk succeeds
fetchUser.rejected  => Action dispatched automatically when the async thunk fails
```

### You can have **multiple async calls in a single slice**. In Redux Toolkit, each async call is typically defined as a separate **`createAsyncThunk`**, and you handle each one in `extraReducers`.

Let me explain step by step.


### 1. Multiple `createAsyncThunk` in one slice

Suppose you have a `userSlice` that needs to:

1. Fetch user details
2. Fetch user posts
3. Update user info

You can define multiple thunks:

```js
import { createSlice, createAsyncThunk } from '@reduxjs/toolkit';

// 1. Fetch user details
export const fetchUser = createAsyncThunk(
  'user/fetchUser',
  async (userId) => {
    const res = await fetch(`/api/users/${userId}`);
    return res.json();
  }
);

// 2. Fetch user posts
export const fetchUserPosts = createAsyncThunk(
  'user/fetchUserPosts',
  async (userId) => {
    const res = await fetch(`/api/users/${userId}/posts`);
    return res.json();
  }
);

// 3. Update user info
export const updateUser = createAsyncThunk(
  'user/updateUser',
  async ({ userId, data }) => {
    const res = await fetch(`/api/users/${userId}`, {
      method: 'PUT',
      body: JSON.stringify(data),
      headers: { 'Content-Type': 'application/json' }
    });
    return res.json();
  }
);
```


### 2. Handle all async thunks in one slice

You use **`extraReducers`** to respond to each thunk’s lifecycle actions:

```js
const userSlice = createSlice({
  name: 'user',
  initialState: {
    details: null,
    posts: [],
    status: 'idle',       // could also have separate statuses
    error: null,
  },
  reducers: {},
  extraReducers: (builder) => {
    // Fetch user
    builder
      .addCase(fetchUser.pending, (state) => { state.status = 'loading'; })
      .addCase(fetchUser.fulfilled, (state, action) => {
        state.status = 'succeeded';
        state.details = action.payload;
      })
      .addCase(fetchUser.rejected, (state, action) => {
        state.status = 'failed';
        state.error = action.error.message;
      });

    // Fetch user posts
    builder
      .addCase(fetchUserPosts.pending, (state) => { state.status = 'loading'; })
      .addCase(fetchUserPosts.fulfilled, (state, action) => {
        state.status = 'succeeded';
        state.posts = action.payload;
      })
      .addCase(fetchUserPosts.rejected, (state, action) => {
        state.status = 'failed';
        state.error = action.error.message;
      });

    // Update user
    builder
      .addCase(updateUser.pending, (state) => { state.status = 'loading'; })
      .addCase(updateUser.fulfilled, (state, action) => {
        state.status = 'succeeded';
        state.details = action.payload;
      })
      .addCase(updateUser.rejected, (state, action) => {
        state.status = 'failed';
        state.error = action.error.message;
      });
  }
});

export default userSlice.reducer;
```


### 3. Tips for managing multiple async calls

1. **Separate status per async call**
   Instead of one `status` for all calls, maintain separate ones:

```js
initialState: {
  details: null,
  posts: [],
  fetchStatus: 'idle',
  postsStatus: 'idle',
  updateStatus: 'idle',
  error: null,
}
```

This prevents one API call from overwriting another’s loading state.

2. **Keep slice focused**
   Only put related async calls in one slice. For example, `userSlice` handles user-related API calls, `todosSlice` handles todos, etc.

3. **Use service layer**
   Keep API calls in a separate `UserService.js` to avoid repeating fetch logic.

4. **Use selectors**
   Create selectors per state piece:

```js
export const selectUserDetails = (state) => state.user.details;
export const selectUserPosts = (state) => state.user.posts;
export const selectUserStatus = (state) => state.user.fetchStatus;
```


 **Summary**

* Yes, a single slice can have **multiple async thunks**.
* Handle them all in `extraReducers`.
* Optionally maintain **separate statuses** for each async call.
* Keep slices modular and focused on a single domain.
