
# **Step-by-Step Guide: Creating a New Component with Redux Toolkit**

---

## **Step 0: Understand the Problem**

**Problem:**
You need to create a new component (e.g., `TodoItem`) that:

* Displays data from the backend
* May trigger updates or async actions
* Needs to be reusable and isolated
* Must respect separation of concerns (UI vs logic)

**Solution:**
Follow the architecture:

```
Component (UI) → Hook → Slice → Service → Selector → Component
```

Each layer has a clear responsibility, with **constants** defined externally and **no hardcoded keys**.

---

## **Step 1: Define Constants and Keys**

**Problem:** Hardcoding API endpoints, slice names, or action keys leads to typos and inconsistencies.

**Solution:** Create/extend constants files.

**Implementation Step:**

```js
// constants/keys.js
export const SLICE_KEYS = { TODOS: 'todos' };
export const THUNK_KEYS = { FETCH_TODOS: 'fetchTodos', ADD_TODO: 'addTodo' };

// constants/api.js
export const API_ENDPOINTS = { TODOS: '/api/todos' };
```

✅ Now all slices, thunks, services, and components use these constants.

---

## **Step 2: Create Service for Business/Async Logic**

**Problem:** Business logic or API calls embedded in components make code hard to maintain.

**Solution:** Create a `Service` module for the feature.

**Implementation Step:**

```js
// features/todos/TodosService.js
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

## **Step 3: Create Async Thunks (if needed)**

**Problem:** Async API calls without proper Redux lifecycle handling can break state consistency.

**Solution:** Use `createAsyncThunk` for each API call.

**Implementation Step:**

```js
// features/todos/todosThunks.js
import { createAsyncThunk } from '@reduxjs/toolkit';
import TodosService from './TodosService';
import { SLICE_KEYS, THUNK_KEYS } from '../../constants/keys';

export const fetchTodos = createAsyncThunk(
  `${SLICE_KEYS.TODOS}/${THUNK_KEYS.FETCH_TODOS}`,
  async () => TodosService.getTodos()
);
```

---

## **Step 4: Create Slice**

**Problem:** Centralized state is needed for multiple components, but hardcoding actions or mixing async/sync logic reduces maintainability.

**Solution:** Use `createSlice` for synchronous reducers, `extraReducers` for async thunks.

**Implementation Step:**

```js
// features/todos/todosSlice.js
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

export const { addTodo, removeTodo } = todosSlice.actions;
export default todosSlice.reducer;
```

---

## **Step 5: Create Selectors**

**Problem:** Direct access to store inside components leads to tight coupling.

**Solution:** Use selectors as the interface to slice state.

**Implementation Step:**

```js
// features/todos/todosSelectors.js
import { todosAdapter } from './todosSlice';

export const { selectAll: selectAllTodos, selectById: selectTodoById } =
  todosAdapter.getSelectors((state) => state.todos);

export const selectTodosStatus = (state) => state.todos.status;
export const selectTodosError = (state) => state.todos.error;
```

---

## **Step 6: Create Custom Hook (Optional)**

**Problem:** Components repeatedly dispatch actions and select state, creating duplicate code.

**Solution:** Encapsulate Redux logic in a hook.

**Implementation Step:**

```js
// hooks/useTodos.js
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

---

## **Step 7: Create UI Component (Rendering Only)**

**Problem:** Mixing API logic or state management in UI components violates separation of concerns.

**Solution:** Component only renders state from hook and triggers callbacks from hook.

**Implementation Step:**

```js
// components/todos/TodosList.js
import React, { useEffect } from 'react';
import useTodos from '../../hooks/useTodos';

export default function TodosList() {
  const { todos, status, fetchTodos } = useTodos();

  useEffect(() => { fetchTodos(); }, []);

  if (status === 'loading') return <div>Loading...</div>;
  if (status === 'failed') return <div>Error loading todos</div>;

  return (
    <ul>
      {todos.map(todo => <li key={todo.id}>{todo.title}</li>)}
    </ul>
  );
}
```

---

## **Step 8: Testing Considerations**

1. **Slice unit tests** → test reducers and async thunks separately.
2. **Hook tests** → test custom hooks return correct data and dispatch actions.
3. **Component tests** → ensure rendering works based on hook output.

---

## **Step 9: Summary / Checklist**

When creating a new component:

| Step | Task                                              | Responsibility |
| ---- | ------------------------------------------------- | -------------- |
| 0    | Understand problem & requirements                 | Developer      |
| 1    | Create constants / API keys                       | Developer      |
| 2    | Create service layer (API calls / business logic) | Developer      |
| 3    | Create async thunks if needed                     | Developer      |
| 4    | Create slice (reducers + extraReducers)           | Developer      |
| 5    | Create selectors for state                        | Developer      |
| 6    | Create custom hook for component                  | Developer      |
| 7    | Create UI component (render only)                 | Developer      |
| 8    | Write tests                                       | Developer / QA |
| 9    | Review adherence to separation & modularity       | Team           |

✅ This ensures **low coupling, modularity, and maintainability**, and makes onboarding new developers straightforward.

