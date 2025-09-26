

# **Redux Toolkit Implementation Guide for Developers**

## **1. Overview**

This guide explains how to implement **state management** using **Redux Toolkit (RTK)** in a React application while maintaining:

* Separation of **UI rendering** and **state/business logic**
* Modular, extendable slices
* Avoidance of hard-coded keys
* Low coupling between components and state management

---

## **2. Core Principles**

| Principle                  | Implementation Notes                                                                                                                                                |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Separation of Concerns** | UI components should only render based on props and selectors. All state management, conditions, and logic should reside in **slices, thunks, services, or hooks**. |
| **No Hardcoded Keys**      | Define all keys, action types, API endpoints, constants in separate **constants/config files**. Never hardcode strings directly in slices, thunks, or components.   |
| **Modularity**             | Each feature should have its own folder containing slice, selectors, thunks, and services.                                                                          |
| **Extendable**             | New functionality should be added as new actions/thunks or slices without altering existing ones.                                                                   |
| **Reusability**            | Create reusable hooks for common operations (e.g., `useTodos`, `useAuth`).                                                                                          |
| **Async Handling**         | Use `createAsyncThunk` for API calls and handle states (`pending`, `fulfilled`, `rejected`) in `extraReducers`.                                                     |
| **Best Practices**         | Use RTK’s `createEntityAdapter` for collections, `selectors` for reading state, and `services` for API calls.                                                       |

---

## **3. Folder Structure**

A recommended **feature-first modular structure**:

```
src/
  app/
    store.js             # Configure and export Redux store
    rootReducer.js       # Combine all slices (optional)
  constants/
    keys.js              # Store all slice/action keys
    api.js               # Store API endpoints
  features/
    todos/
      todosSlice.js
      todosSelectors.js
      todosThunks.js
      TodosService.js
      index.js           # barrel export
    auth/
      authSlice.js
      authSelectors.js
      authThunks.js
      AuthService.js
      index.js
  components/
    common/               # shared UI components
    layout/               # layout components
    todos/                # UI only for Todos
    auth/                 # UI only for Auth
  hooks/
    useTodos.js
    useAuth.js
  services/
    apiClient.js          # generic HTTP client (fetch/axios)
  pages/
    LoginPage.js
    TodosPage.js
```

---

## **4. No Hardcoded Keys**

All **constants and keys** should live in dedicated files:

**constants/keys.js**

```js
export const SLICE_KEYS = {
  TODOS: 'todos',
  AUTH: 'auth'
};

export const THUNK_KEYS = {
  FETCH_TODOS: 'fetchTodos',
  ADD_TODO: 'addTodo',
  FETCH_USER: 'fetchUser'
};
```

**constants/api.js**

```js
export const API_ENDPOINTS = {
  TODOS: '/api/todos',
  USERS: '/api/users'
};
```

Usage in slices or thunks:

```js
import { SLICE_KEYS, THUNK_KEYS } from '../../constants/keys';

export const fetchTodos = createAsyncThunk(
  `${SLICE_KEYS.TODOS}/${THUNK_KEYS.FETCH_TODOS}`,
  async () => { /* api call */ }
);
```

✅ This prevents typos and ensures consistency across the app.

---

## **5. Slice Design (Separation of Logic)**

Slices should **only handle state and reducers**. **No UI code**.

**features/todos/todosSlice.js**

```js
import { createSlice, createAsyncThunk, createEntityAdapter } from '@reduxjs/toolkit';
import TodosService from './TodosService';
import { THUNK_KEYS, SLICE_KEYS } from '../../constants/keys';

export const fetchTodos = createAsyncThunk(
  `${SLICE_KEYS.TODOS}/${THUNK_KEYS.FETCH_TODOS}`,
  async () => TodosService.getTodos()
);

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

export const { addTodo, removeTodo } = todosSlice.actions;
export default todosSlice.reducer;
```

---

## **6. Service Layer (Business Logic / API)**

All API calls or business logic should be in services, not slices or UI components.

**features/todos/TodosService.js**

```js
import apiClient from '../../services/apiClient';
import { API_ENDPOINTS } from '../../constants/api';

const TodosService = {
  getTodos: () => apiClient.get(API_ENDPOINTS.TODOS),
  addTodo: (todo) => apiClient.post(API_ENDPOINTS.TODOS, todo),
  removeTodo: (id) => apiClient.delete(`${API_ENDPOINTS.TODOS}/${id}`)
};

export default TodosService;
```

---

## **7. Selectors (State Interface)**

Selectors expose state in a **decoupled way**.

**features/todos/todosSelectors.js**

```js
import { todosAdapter } from './todosSlice';

export const { selectAll: selectAllTodos, selectById: selectTodoById } =
  todosAdapter.getSelectors((state) => state.todos);

export const selectTodosStatus = (state) => state.todos.status;
export const selectTodosError = (state) => state.todos.error;
```

* Components **never access `state.todos` directly**.
* Use selectors to maintain low coupling.

---

## **8. Custom Hooks (Optional, DRY)**

Encapsulate logic for UI components:

**hooks/useTodos.js**

```js
import { useDispatch, useSelector } from 'react-redux';
import { fetchTodos, addTodo, removeTodo } from '../features/todos/todosSlice';
import { selectAllTodos, selectTodosStatus } from '../features/todos/todosSelectors';

export default function useTodos() {
  const dispatch = useDispatch();
  const todos = useSelector(selectAllTodos);
  const status = useSelector(selectTodosStatus);

  return {
    todos,
    status,
    fetchTodos: () => dispatch(fetchTodos()),
    addTodo: (todo) => dispatch(addTodo(todo)),
    removeTodo: (id) => dispatch(removeTodo(id))
  };
}
```

✅ Now, UI components **don’t need to know about thunks, actions, or store** — only the hook.

---

## **9. UI Components (Rendering Only)**

**components/todos/TodosList.js**

```js
import React, { useEffect } from 'react';
import useTodos from '../../hooks/useTodos';

export default function TodosList() {
  const { todos, status, fetchTodos } = useTodos();

  useEffect(() => {
    fetchTodos();
  }, []);

  if (status === 'loading') return <div>Loading...</div>;
  if (status === 'failed') return <div>Error loading todos</div>;

  return (
    <ul>
      {todos.map(todo => <li key={todo.id}>{todo.title}</li>)}
    </ul>
  );
}
```

* Notice: **No state management or API logic here**.
* Component only **renders based on props/state** from the hook.

---

## **10. Key Implementation Guidelines**

1. **Always use constants for keys, action types, and API endpoints.**
2. **Slices = state + reducers only**.
3. **ExtraReducers = handle async thunks or external actions.**
4. **Services = API / business logic only**.
5. **Selectors = interface for reading state**.
6. **Hooks = encapsulate dispatch & selectors for UI consumption**.
7. **Components = purely render based on props/hooks**.
8. **Separate statuses for each async thunk** to avoid overlapping loading states.
9. **Use `createEntityAdapter`** for collections for normalized state and reusable selectors.
10. **Keep slices feature-focused**; don’t create a “mega-slice”.

---

## **11. Example Flow Summary**

```
Component → calls Hook → dispatch async thunk → Slice extraReducers updates state → Selector returns updated state → Component re-renders
```

* **Component:** Rendering only
* **Hook:** Encapsulates selectors + dispatch actions
* **Slice:** State & reducers
* **Service:** API / business logic
* **Selectors:** Interface for state

---

## **12. Benefits**

* Low coupling between **UI and state**
* Easy to extend features (add new thunk or slice)
* Easy to maintain (separate constants, services, hooks)
* Consistent approach across multiple developers
* Makes testing easier (slice/unit tests for state, hook tests for UI interactions)

---

