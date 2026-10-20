Yes. Let's build **one realistic Redux Toolkit use case** that forces us to use almost every important concept together.

We'll build a **Shopping Cart + Products** feature. This gives us:

* `configureStore`
* `createSlice`
* `createAsyncThunk`
* `extraReducers`
* `createSelector`
* `useSelector`
* `useDispatch`
* actions
* reducers
* payloads
* Immer
* middleware
* loading/error states
* derived state
* React integration
* Redux DevTools
* and finally we'll see where **RTK Query** would replace our manual async logic.

---

# The application

Imagine an e-commerce page:

```text
┌───────────────────────────────────────────────┐
│ Shop                         🛒 Cart: 3        │
├───────────────────────────────────────────────┤
│                                               │
│ Products                                      │
│                                               │
│ ┌────────────┐  ┌────────────┐               │
│ │ iPhone     │  │ Laptop     │               │
│ │ $999       │  │ $1200      │               │
│ │ [Add]      │  │ [Add]      │               │
│ └────────────┘  └────────────┘               │
│                                               │
├───────────────────────────────────────────────┤
│ Cart                                          │
│ iPhone × 2                         $1998       │
│ Laptop × 1                        $1200        │
│                                               │
│ Total: $3198                                  │
└───────────────────────────────────────────────┘
```

We need two pieces of state:

```text
products
cart
```

---

# 1. Project structure

Let's start with:

```text
src/
│
├── app/
│   └── store.js
│
├── features/
│   ├── products/
│   │   └── productsSlice.js
│   │
│   └── cart/
│       └── cartSlice.js
│
├── components/
│   ├── ProductList.jsx
│   ├── ProductCard.jsx
│   └── Cart.jsx
│
├── App.jsx
└── main.jsx
```

This is a feature-oriented structure.

---

# 2. Install Redux Toolkit

```bash
npm install @reduxjs/toolkit react-redux
```

Now let's build it.

---

# 3. Products slice

First, our products need to come from an API.

We'll deliberately use `createAsyncThunk` here so that we can understand the full Redux async lifecycle.

### `productsSlice.js`

```js
import {
  createSlice,
  createAsyncThunk,
} from "@reduxjs/toolkit";
```

Now create our async thunk:

```js
export const fetchProducts = createAsyncThunk(
  "products/fetchProducts",

  async () => {
    const response = await fetch(
      "https://fakestoreapi.com/products"
    );

    if (!response.ok) {
      throw new Error("Failed to fetch products");
    }

    return response.json();
  }
);
```

Pause here.

This:

```js
createAsyncThunk(
  "products/fetchProducts",
  async () => {}
);
```

creates three actions automatically:

```text
products/fetchProducts/pending
products/fetchProducts/fulfilled
products/fetchProducts/rejected
```

So we're getting:

```text
             fetchProducts()
                    │
                    ↓
              ┌──────────┐
              │ pending  │
              └────┬─────┘
                   │
              API request
                   │
            ┌──────┴──────┐
            ↓             ↓
        fulfilled      rejected
```

Now our initial state:

```js
const initialState = {
  items: [],
  status: "idle",
  error: null,
};
```

We need:

```text
items
status
error
```

because async operations have state.

---

# 4. Create the slice

```js
const productsSlice = createSlice({
  name: "products",

  initialState,

  reducers: {},

  extraReducers: (builder) => {
    builder
      .addCase(fetchProducts.pending, (state) => {
        state.status = "loading";
        state.error = null;
      })

      .addCase(fetchProducts.fulfilled, (state, action) => {
        state.status = "succeeded";
        state.items = action.payload;
      })

      .addCase(fetchProducts.rejected, (state, action) => {
        state.status = "failed";
        state.error = action.error.message;
      });
  },
});
```

Export the reducer:

```js
export default productsSlice.reducer;
```

Our complete file:

```js
import {
  createSlice,
  createAsyncThunk,
} from "@reduxjs/toolkit";

export const fetchProducts = createAsyncThunk(
  "products/fetchProducts",

  async () => {
    const response = await fetch(
      "https://fakestoreapi.com/products"
    );

    if (!response.ok) {
      throw new Error("Failed to fetch products");
    }

    return response.json();
  }
);

const initialState = {
  items: [],
  status: "idle",
  error: null,
};

const productsSlice = createSlice({
  name: "products",

  initialState,

  reducers: {},

  extraReducers: (builder) => {
    builder
      .addCase(fetchProducts.pending, (state) => {
        state.status = "loading";
        state.error = null;
      })

      .addCase(fetchProducts.fulfilled, (state, action) => {
        state.status = "succeeded";
        state.items = action.payload;
      })

      .addCase(fetchProducts.rejected, (state, action) => {
        state.status = "failed";
        state.error = action.error.message;
      });
  },
});

export default productsSlice.reducer;
```

---

# 5. What just happened?

This one file already demonstrates several RTK concepts.

```text
createAsyncThunk
       │
       ├── pending
       ├── fulfilled
       └── rejected
               │
               ↓
        extraReducers
               │
               ↓
         products state
```

And notice:

```js
state.status = "loading";
```

looks like mutation.

That's Immer.

RTK is effectively producing an immutable next state for us.

---

# 6. Cart slice

Now we create something more interesting.

Our cart needs:

```js
{
  items: []
}
```

Each item might look like:

```js
{
  id: 1,
  title: "iPhone",
  price: 999,
  quantity: 2
}
```

Create:

```js
import {
  createSlice,
  createSelector,
} from "@reduxjs/toolkit";
```

Initial state:

```js
const initialState = {
  items: [],
};
```

Now our slice:

```js
const cartSlice = createSlice({
  name: "cart",

  initialState,

  reducers: {
    addToCart(state, action) {
      const product = action.payload;

      const existingItem = state.items.find(
        item => item.id === product.id
      );

      if (existingItem) {
        existingItem.quantity += 1;
      } else {
        state.items.push({
          ...product,
          quantity: 1,
        });
      }
    },

    removeFromCart(state, action) {
      const productId = action.payload;

      state.items = state.items.filter(
        item => item.id !== productId
      );
    },

    increaseQuantity(state, action) {
      const item = state.items.find(
        item => item.id === action.payload
      );

      if (item) {
        item.quantity += 1;
      }
    },

    decreaseQuantity(state, action) {
      const item = state.items.find(
        item => item.id === action.payload
      );

      if (!item) return;

      if (item.quantity === 1) {
        state.items = state.items.filter(
          item => item.id !== action.payload
        );
      } else {
        item.quantity -= 1;
      }
    },

    clearCart(state) {
      state.items = [];
    },
  },
});
```

Export actions:

```js
export const {
  addToCart,
  removeFromCart,
  increaseQuantity,
  decreaseQuantity,
  clearCart,
} = cartSlice.actions;
```

Export reducer:

```js
export default cartSlice.reducer;
```

---

# 7. Let's understand one action deeply

Suppose the user clicks:

```text
[Add to cart]
```

The component does:

```js
dispatch(addToCart(product));
```

The action creator creates:

```js
{
  type: "cart/addToCart",

  payload: {
    id: 1,
    title: "iPhone",
    price: 999
  }
}
```

Then Redux sends it through the reducer.

Our reducer receives:

```js
addToCart(state, action)
```

where:

```text
state
  ↓
{
  items: []
}

action
  ↓
{
  type: "cart/addToCart",
  payload: {...}
}
```

Then:

```js
state.items.push(...)
```

happens.

Immer turns that into an immutable state update.

Result:

```js
{
  items: [
    {
      id: 1,
      title: "iPhone",
      price: 999,
      quantity: 1
    }
  ]
}
```

That's the complete Redux cycle.

---

# 8. `configureStore`

Now combine our slices.

### `store.js`

```js
import { configureStore } from "@reduxjs/toolkit";

import productsReducer from "../features/products/productsSlice";
import cartReducer from "../features/cart/cartSlice";

export const store = configureStore({
  reducer: {
    products: productsReducer,
    cart: cartReducer,
  },
});
```

This gives us the global state:

```js
{
  products: {
    items: [],
    status: "idle",
    error: null
  },

  cart: {
    items: []
  }
}
```

Notice how this:

```js
reducer: {
  products: productsReducer,
  cart: cartReducer,
}
```

defines the state shape.

---

# 9. Connect Redux to React

### `main.jsx`

```jsx
import React from "react";
import ReactDOM from "react-dom/client";

import { Provider } from "react-redux";

import { store } from "./app/store";
import App from "./App";

ReactDOM.createRoot(
  document.getElementById("root")
).render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>
);
```

The important part:

```jsx
<Provider store={store}>
  <App />
</Provider>
```

Now every component below `Provider` can access Redux.

---

# 10. Fetch products

In `App.jsx`:

```jsx
import { useEffect } from "react";
import { useDispatch } from "react-redux";

import { fetchProducts } from "./features/products/productsSlice";

import ProductList from "./components/ProductList";
import Cart from "./components/Cart";

function App() {
  const dispatch = useDispatch();

  useEffect(() => {
    dispatch(fetchProducts());
  }, [dispatch]);

  return (
    <>
      <ProductList />
      <Cart />
    </>
  );
}

export default App;
```

Here's an important distinction.

We don't do:

```js
fetchProducts();
```

We do:

```js
dispatch(fetchProducts());
```

because `fetchProducts()` is an async thunk action creator.

---

# 11. ProductList

Now let's read Redux state.

```jsx
import { useSelector } from "react-redux";

import ProductCard from "./ProductCard";

function ProductList() {
  const products = useSelector(
    state => state.products.items
  );

  const status = useSelector(
    state => state.products.status
  );

  const error = useSelector(
    state => state.products.error
  );

  if (status === "loading") {
    return <p>Loading...</p>;
  }

  if (status === "failed") {
    return <p>{error}</p>;
  }

  return (
    <div>
      {products.map(product => (
        <ProductCard
          key={product.id}
          product={product}
        />
      ))}
    </div>
  );
}

export default ProductList;
```

Now we're using:

```text
useSelector
   ↓
Redux store
   ↓
products
```

---

# 12. ProductCard

```jsx
import { useDispatch } from "react-redux";

import { addToCart } from "../features/cart/cartSlice";

function ProductCard({ product }) {
  const dispatch = useDispatch();

  return (
    <div>
      <h3>{product.title}</h3>

      <p>${product.price}</p>

      <button
        onClick={() => dispatch(addToCart(product))}
      >
        Add to cart
      </button>
    </div>
  );
}

export default ProductCard;
```

Now we have:

```text
ProductCard
    │
    │ click
    ↓
dispatch(addToCart(product))
    │
    ↓
cart/addToCart
    │
    ↓
cart reducer
    │
    ↓
Redux state changes
```

---

# 13. Now let's do selectors properly

Currently we're doing:

```js
state => state.cart.items
```

But let's create selectors.

Inside `cartSlice.js`:

```js
export const selectCartItems =
  state => state.cart.items;
```

Then:

```jsx
const items = useSelector(selectCartItems);
```

Much cleaner.

But we can go deeper.

---

# 14. Derived state: cart total

We don't want to store:

```js
{
  items: [...],
  total: 3198
}
```

if `total` can be calculated from `items`.

Why?

Because then we have two sources of truth.

Instead:

```text
items
  ↓
selector
  ↓
total
```

Use `createSelector`:

```js
export const selectCartTotal = createSelector(
  [selectCartItems],

  items =>
    items.reduce(
      (total, item) =>
        total + item.price * item.quantity,
      0
    )
);
```

Now:

```js
const total = useSelector(selectCartTotal);
```

We don't store `total`.

We **derive** it.

---

# 15. Cart item count

Same idea:

```js
export const selectCartItemCount = createSelector(
  [selectCartItems],

  items =>
    items.reduce(
      (count, item) => count + item.quantity,
      0
    )
);
```

Now our selectors are:

```js
export const selectCartItems =
  state => state.cart.items;

export const selectCartTotal = createSelector(
  [selectCartItems],
  items =>
    items.reduce(
      (total, item) =>
        total + item.price * item.quantity,
      0
    )
);

export const selectCartItemCount = createSelector(
  [selectCartItems],
  items =>
    items.reduce(
      (count, item) => count + item.quantity,
      0
    )
);
```

This is a very good Redux pattern:

> **Store the minimum necessary state. Derive the rest.**

---

# 16. Cart component

```jsx
import { useSelector, useDispatch } from "react-redux";

import {
  selectCartItems,
  selectCartTotal,
} from "../features/cart/cartSlice";

import {
  increaseQuantity,
  decreaseQuantity,
  removeFromCart,
} from "../features/cart/cartSlice";

function Cart() {
  const dispatch = useDispatch();

  const items = useSelector(selectCartItems);
  const total = useSelector(selectCartTotal);

  return (
    <div>
      <h2>Cart</h2>

      {items.map(item => (
        <div key={item.id}>
          <h3>{item.title}</h3>

          <p>
            ${item.price} × {item.quantity}
          </p>

          <button
            onClick={() =>
              dispatch(decreaseQuantity(item.id))
            }
          >
            -
          </button>

          <button
            onClick={() =>
              dispatch(increaseQuantity(item.id))
            }
          >
            +
          </button>

          <button
            onClick={() =>
              dispatch(removeFromCart(item.id))
            }
          >
            Remove
          </button>
        </div>
      ))}

      <h2>Total: ${total.toFixed(2)}</h2>
    </div>
  );
}

export default Cart;
```

Now we've used almost everything we've discussed.

---

# 17. Let's trace the entire application

Suppose the user opens the application.

### Step 1

`App` runs:

```js
dispatch(fetchProducts());
```

### Step 2

Thunk dispatches:

```text
products/fetchProducts/pending
```

### Step 3

`extraReducers` handles it:

```js
state.status = "loading";
```

### Step 4

API request happens.

### Step 5

Suppose API succeeds.

Thunk dispatches:

```text
products/fetchProducts/fulfilled
```

with:

```js
payload: [...]
```

### Step 6

Reducer executes:

```js
state.items = action.payload;
state.status = "succeeded";
```

### Step 7

`ProductList` is subscribed to:

```js
state.products.items
```

Its selected data changed.

React updates the component.

---

# 18. Now user clicks Add

User clicks:

```text
Add to cart
```

Component:

```js
dispatch(addToCart(product));
```

Action:

```js
{
  type: "cart/addToCart",
  payload: product
}
```

Reducer:

```js
addToCart(state, action) {
  const product = action.payload;

  // ...
}
```

State changes:

```text
cart.items
```

Then selectors recalculate:

```text
cart.items
   │
   ├────→ selectCartItemCount
   │
   └────→ selectCartTotal
```

Components using those selectors update.

---

# 19. The whole architecture

Here's the complete flow:

```text
                         React UI
                            │
             ┌──────────────┴──────────────┐
             │                             │
         useSelector                  useDispatch
             │                             │
             ↓                             ↓
         Selectors                     Actions
             │                             │
             │                             ↓
             │                         dispatch()
             │                             │
             │                             ↓
             │                       Middleware
             │                             │
             │                    ┌────────┴────────┐
             │                    │                 │
             │                normal action       thunk
             │                    │                 │
             │                    ↓                 ↓
             │                 Reducer           API call
             │                    │                 │
             │                    │          pending/fulfilled/
             │                    │             rejected
             │                    │                 │
             │                    └────────┬────────┘
             │                             ↓
             │                           Store
             │                             │
             └─────────────────────────────┘
```

This is Redux Toolkit in action.

---

# 20. What concepts have we actually used?

| Concept           | Where we used it              |
| ----------------- | ----------------------------- |
| Store             | `configureStore()`            |
| Slice             | `createSlice()`               |
| State             | `initialState`                |
| Action            | `addToCart()`                 |
| Payload           | `action.payload`              |
| Reducer           | `addToCart`, `removeFromCart` |
| Immer             | `state.items.push()`          |
| Dispatch          | `dispatch(addToCart())`       |
| Selector          | `selectCartItems`             |
| Memoized selector | `createSelector()`            |
| Async thunk       | `createAsyncThunk()`          |
| Pending           | loading state                 |
| Fulfilled         | successful API                |
| Rejected          | error state                   |
| `extraReducers`   | thunk lifecycle               |
| Provider          | React integration             |
| `useSelector`     | reading state                 |
| `useDispatch`     | changing state                |
| Derived state     | cart total/count              |
| Middleware        | thunk + RTK defaults          |
| DevTools          | `configureStore()` defaults   |

That's a **proper Redux Toolkit example**, rather than an isolated counter.

---

# 21. One important improvement

There's something deliberately "old-school" about our example:

```js
createAsyncThunk(...)
```

for fetching products.

It works.

But in a modern RTK application, **server data is often better handled with RTK Query**.

Our current architecture says:

```text
API
 ↓
createAsyncThunk
 ↓
Redux state
 ↓
manual loading/error
```

RTK Query can instead give us:

```text
API
 ↓
RTK Query
 ↓
cache
 ↓
React hook
```

and handle things like:

```text
caching
deduplication
loading
errors
refetching
cache invalidation
request lifecycle
```

So the next level of this exact project would be:

```text
                    Redux Toolkit
                         │
              ┌──────────┴──────────┐
              ↓                     ↓
        Client State            Server State
              │                     │
         createSlice             RTK Query
              │                     │
              ↓                     ↓
            Cart                 Products
```

**That distinction—`createSlice` for client/application state vs RTK Query for server/cache state—is one of the most important architectural ideas in modern Redux Toolkit.**
