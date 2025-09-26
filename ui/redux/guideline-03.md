
# **Redux Toolkit Implementation Guide: Modular, Encapsulated Architecture**

---

## **1. Overview**

This document provides a **step-by-step guide** for implementing Redux Toolkit in React applications, considering:

* Separation of UI and business logic
* Encapsulation for multi-team collaboration (UI & Logic teams)
* Modular, extendable slices
* Avoiding hard-coded keys and strings
* Async API handling via `createAsyncThunk`
* Clear public interface using **custom hooks**
* Named exports for all public-facing items

**Architecture flow:**

```
UI Component → Custom Hook → Slice/Thunks → Service → Selector → State → UI
```

* **UI Team:** Only interacts with **hooks**.
* **Logic/State Team:** Handles slices, thunks, services, and selectors.

---

## **2. Core Principles**

| Principle                  | Implementation Notes                                                                                    |
| -------------------------- | ------------------------------------------------------------------------------------------------------- |
| **Separation of Concerns** | UI renders only based on props/state from hooks; logic lives in slices/thunks/services.                 |
| **Encapsulation**          | Expose only safe interfaces to UI via hooks; internal slice state, thunks, and services remain private. |
| **No Hardcoding**          | Use constants for keys, action types, and API endpoints.                                                |
| **Modularity**             | Feature-based folder structure; each feature contains slice, thunks, selectors, services, hooks.        |
| **Async Handling**         | Use `createAsyncThunk`; handle lifecycle (`pending`, `fulfilled`, `rejected`) in `extraReducers`.       |
| **Extendable & Reusable**  | Adding new features should not require modifying existing code; use reusable hooks for UI.              |

---

## **3. Recommended Folder Structure**

```
src/
  app/
    store.js              # Redux store configuration
    rootReducer.js        # Combine slices (optional)
  constants/
    keys.js               # Slice/action keys
    api.js                # API endpoints
  features/
    todos/
      todosSlice.js       # Slice definition (internal)
      todosThunks.js      # Async thunks (internal)
      todosSelectors.js   # Selectors (internal)
      TodosService.js     # API/business logic (internal)
      useTodos.js         # Encapsulated hook (public interface)
      index.js            # Barrel export exposing ONLY hook(s)
  components/
    todos/
      TodosList.js        # UI rendering only
    common/
      Spinner.js
      Error.js
  hooks/                  # optional shared hooks
  services/
    apiClient.js          # generic API client (fetch/axios)
  pages/
    TodosPage.js
```

---

## **4. Constants (No Hardcoding)**

**constants/keys.js**

```js
export const SLICE_KEYS = { TODOS: 'todos', AUTH: 'auth' };
export const THUNK_KEYS = { FETCH_TODOS: 'fetchTodos', ADD_TODO: 'addTodo' };
```

**constants/api.js**

```js
export const API_ENDPOINTS = { TODOS: '/api/todos', USERS: '/api/users' };
```

* Use constants in **thunks, slices, and services** instead of hardcoding.

---

## **5. Service Layer (Business Logic / API)**

**features/todos/TodosService.js**

```js
import apiClient from '../../services/apiClient';
import { API_ENDPOINTS } from '../../constants/api';

const TodosService = {
  getTodos: () => apiClient.get(API_ENDPOINTS.TODOS),
  addTodo: (todo) => apiClient.post(API_ENDPOINTS.TODOS, todo),
  removeTodo: (id) => apiClient.delete(`${API_ENDPOINTS.TODOS}/${id}`)
};

export default TodosService; // internal, not exposed to UI
```

* Services contain all API calls and business logic.
* UI team **never imports this**.

---

## **6. Async Thunks**

**features/todos/todosThunks.js**

```js
import { createAsyncThunk } from '@reduxjs/toolkit';
import TodosService from './TodosService';
import { SLICE_KEYS, THUNK_KEYS } from '../../constants/keys';

export const fetchTodos = createAsyncThunk(
  `${SLICE_KEYS.TODOS}/${THUNK_KEYS.FETCH_TODOS}`,
  async () => TodosService.getTodos()
);
```

* Use `createAsyncThunk` for async actions.
* Lifecycle actions (`pending`, `fulfilled`, `rejected`) handled in slice.

---

## **7. Slice**

**features/todos/todosSlice.js**

```js
import { createSlice, createEntityAdapter } from '@reduxjs/toolkit';
import { fetchTodos } from './todosThunks';
import { SLICE_KEYS } from '../../constants/keys';

const todosAdapter = createEntityAdapter();

const todosSlice = createSlice({
  name: SLICE_KEYS.TODOS,
  initialState: todosAdapter.getInitialState({ status: 'idle', error: null }),
  reducers: {
    addTodo: todosAdapter.addOne,
    removeTodo: todosAdapter.removeOne
  },
  extraReducers: (builder) => {
    builder
      .addCase(fetchTodos.pending, (state) => { state.status = 'loading'; })
      .addCase(fetchTodos.fulfilled, (state, action) => {
        state.status = 'succeeded';
        todosAdapter.setAll(state, action.payload);
      })
      .addCase(fetchTodos.rejected, (state, action) => {
        state.status = 'failed';
        state.error = action.error.message;
      });
  }
});

export default todosSlice.reducer; // Internal only, no direct action exposure
```

* **Only slice reducer is exported**, internal actions are private.
* UI team **cannot directly dispatch internal actions**.

---

## **8. Selectors**

**features/todos/todosSelectors.js**

```js
import { todosAdapter } from './todosSlice';

export const { selectAll: selectAllTodos, selectById: selectTodoById } =
  todosAdapter.getSelectors((state) => state.todos);

export const selectTodosStatus = (state) => state.todos.status;
export const selectTodosError = (state) => state.todos.error;
```

* Selectors are **internal helpers**.
* UI team **access state only via hooks**.

---

## **9. Encapsulated Hook (Public Interface for UI Team)**

**features/todos/useTodos.js**

```js
import { useDispatch, useSelector } from 'react-redux';
import { fetchTodos, addTodo, removeTodo } from './todosSlice';
import { selectAllTodos, selectTodosStatus, selectTodosError } from './todosSelectors';

export function useTodos() {
  const dispatch = useDispatch();
  const todos = useSelector(selectAllTodos);
  const status = useSelector(selectTodosStatus);
  const error = useSelector(selectTodosError);

  return {
    todos,
    status,
    error,
    fetchTodos: () => dispatch(fetchTodos()),
    addTodo: (todo) => dispatch(addTodo(todo)),
    removeTodo: (id) => dispatch(removeTodo(id))
  };
}
```

* **Only named export (`useTodos`)** is exposed.
* UI team **cannot access slice, thunks, or services**.

---

## **10. Barrel Export for Feature**

**features/todos/index.js**

```js
export { useTodos } from './useTodos';
```

* UI team imports from:

```js
import { useTodos } from 'features/todos';
```

* Encapsulation ensures **low coupling**.

---

## **11. UI Component (Rendering Only)**

**components/todos/TodosList.js**

```js
import React, { useEffect } from 'react';
import { useTodos } from '../../features/todos';

export function TodosList() {
  const { todos, status, error, fetchTodos } = useTodos();

  useEffect(() => { fetchTodos(); }, []);

  if (status === 'loading') return <div>Loading...</div>;
  if (status === 'failed') return <div>{error || 'Error loading todos'}</div>;

  return (
    <ul>
      {todos.map(todo => <li key={todo.id}>{todo.title}</li>)}
    </ul>
  );
}
```

* Component **renders state only via hook**.
* UI team **never deals with slice, thunk, or service**.

---

## **12. Step-by-Step Checklist for Creating a New Component**

1. **Define Constants**

   * Slice keys, thunk keys, API endpoints in `constants/`.

2. **Create Service**

   * Encapsulate all API/business logic.
   * Never import this in UI components.

3. **Create Async Thunks**

   * For any API calls, use `createAsyncThunk`.

4. **Create Slice**

   * Define `reducers` for sync actions, `extraReducers` for async thunks.
   * Only export slice reducer.

5. **Create Selectors**

   * Internal selectors to extract state; not exposed to UI.

6. **Create Encapsulated Hook**

   * Named export only.
   * Hook handles dispatch, selects state, exposes safe actions.

7. **Create Barrel Export**

   * Expose only hooks for the UI team.

8. **Create UI Component**

   * Import hook from barrel export.
   * Render UI based on state from hook.
   * No direct access to slice/thunks/services.

9. **Testing**

   * Slice: test reducers and thunks
   * Hook: test state mapping and dispatch
   * Component: test rendering

---

## **13. Benefits**

* **UI team independence** → only consumes hooks
* **Encapsulation** → internal state & logic hidden
* **Consistency** → same pattern for every feature
* **Low coupling & high cohesion** → easy to maintain
* **Safe refactoring** → internal logic can change without affecting UI

---

This document provides a **complete roadmap for implementing new features/components**, enforcing **encapsulation, modularity, separation of concerns, and named exports** for all public-facing hooks.
