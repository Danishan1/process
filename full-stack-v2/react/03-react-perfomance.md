
# 🧠 The Performance Mental Model

Remember this:

```text
                     USER
                       ↓
                    EVENT
                       ↓
                 STATE UPDATE
                       ↓
                REACT SCHEDULES
                       ↓
                    RENDER
                       ↓
              RECONCILIATION
                       ↓
                    COMMIT
                       ↓
                      DOM
                       ↓
             BROWSER RENDERING
                       ↓
                    SCREEN
```

At every stage ask:

```text
             WHERE IS THE COST?
                    │
      ┌─────────────┼─────────────┐
      ↓             ↓             ↓
    React         Network       Browser
      │             │             │
   renders       requests       layout
   calculations  payloads       paint
   reconciliation caching       composite
```

## 🔥 Performance Revision Table

| Problem                               | First thing to investigate  | Possible solution                   |
| ------------------------------------- | --------------------------- | ----------------------------------- |
| Component renders too often           | Why is it rendering?        | State placement, props, context     |
| Expensive calculation                 | Repeated calculation?       | `useMemo`                           |
| Child re-renders due to function prop | Function identity           | `useCallback` + `memo` if justified |
| Context causes broad updates          | Provider/consumer structure | Split context / restructure state   |
| Huge list                             | Too many DOM nodes          | Virtualization                      |
| Search fires many requests            | Input frequency             | Debounce                            |
| Scroll handler fires constantly       | Event frequency             | Throttle                            |
| Initial JS too large                  | Bundle                      | Code splitting/lazy loading         |
| API feels slow                        | Request waterfall           | Parallel fetching/caching           |
| Duplicate API requests                | Missing cache               | Server-state caching                |
| UI freezes                            | CPU-heavy JS                | Web Worker / move work              |
| Images slow page                      | Asset size                  | Compression/responsive/lazy loading |
| Browser layout slow                   | DOM/layout work             | Reduce layout changes               |
| Unknown bottleneck                    | Don't guess                 | Profile first                       |

## 🎯 The 10 Rules I'd Memorize

```text
1. Re-render ≠ DOM update.

2. First ask "Why did it render?"

3. Keep state as local as possible.

4. Don't duplicate derived state.

5. Don't use effects for ordinary calculations.

6. memo/useMemo/useCallback are optimization tools,
   not default requirements.

7. Referential equality matters.

8. Large lists need virtualization.

9. Network + bundle size can matter more than
   component optimization.

10. Measure first, optimize second.
```

And the deepest principle:

> **The best React optimization is usually not making React do the same work faster; it's making React, JavaScript, the browser, or the network do less work in the first place.**

# ⚡ React Performance & Optimization

### 1. First Principle: What Does “Performance” Mean?

A React app can be slow because of different things:

```text
                PERFORMANCE
                    │
      ┌─────────────┼─────────────┐
      ↓             ↓             ↓
   Rendering      Network       Browser
      │             │             │
  too much UI    large API     layout/paint
  expensive JS   large data    DOM work
```

So don't automatically blame React.

## 2. The React Rendering Pipeline ⭐⭐⭐⭐⭐

Understand this first:

```text
State / Props / Context change
            ↓
      React schedules work
            ↓
       Component renders
            ↓
     Reconciliation
            ↓
       Commit phase
            ↓
        DOM changes
            ↓
       Browser paints
```

Performance problems can happen at multiple points.

#### Example

```tsx
function ProductList({ products }) {
  const filtered = products.filter(expensiveFilter);

  return <List products={filtered} />;
}
```

Potential problem:

```text
Every render
    ↓
expensiveFilter()
    ↓
large calculation
```

Optimization might be:

```tsx
const filtered = useMemo(
  () => products.filter(expensiveFilter),
  [products]
);
```

But only if that calculation is actually expensive enough to matter.

## 3. Re-render ≠ DOM Update ⭐⭐⭐⭐⭐

This is one of the most important concepts.

Suppose:

```tsx
function App() {
  const [count, setCount] = useState(0);

  return <h1>{count}</h1>;
}
```

Calling:

```tsx
setCount(1);
```

causes a render.

But React doesn't blindly replace the entire DOM.

```text
render
 ↓
compare
 ↓
only necessary DOM changes
```

So:

> **A re-render is not automatically a performance problem.**

## 4. The Real Question: Why Did It Render?

When optimizing a component, ask:

```text
Why did this component render?
```

Common reasons:

```text
state changed
props changed
context changed
parent rendered
external store changed
```

This question is much more valuable than:

> "Where can I add `useMemo`?"

## 5. State Placement ⭐⭐⭐⭐⭐

One of the best optimizations is simply putting state in the right place.

Bad architecture:

```text
             App
              │
        huge global state
              │
      ┌───────┼───────┐
      ↓       ↓       ↓
    A       B       C
```

A small state change can cause a large part of the tree to participate in rendering.

Better:

```text
App
 ├── Header
 ├── ProductPage
 │     └── Search state
 └── Footer
```

#### Principle

> **Colocate state as close as possible to where it's needed.**

This is often more effective than memoization.

## 6. Example: Bad State Placement

```tsx
function App() {
  const [search, setSearch] = useState("");

  return (
    <>
      <Header />
      <Search value={search} onChange={setSearch} />
      <HugeDashboard />
      <Footer />
    </>
  );
}
```

Typing into search updates:

```text
search
 ↓
App renders
 ↓
Header
HugeDashboard
Footer
```

Even if React eventually skips some DOM changes, you've created more rendering work.

You might move search state closer to the search-related subtree.

## 7. Component Boundaries

Component boundaries can help isolate rendering work.

Instead of:

```tsx
function HugeComponent() {
  // 500 lines
}
```

split meaningful regions:

```text
Page
 ├── Header
 ├── Search
 ├── Filters
 ├── ProductList
 └── Summary
```

Now you can reason about:

```text
state ownership
rendering
memoization
data flow
```

much more easily.

## 8. `React.memo` ⭐⭐⭐⭐⭐

`memo` can prevent a component from rendering when its props are unchanged according to its comparison.

```tsx
const UserCard = memo(function UserCard({ user }) {
  return <div>{user.name}</div>;
});
```

Without memo:

```text
Parent renders
 ↓
UserCard renders
```

With memo:

```text
Parent renders
 ↓
Are props equivalent?
 ├── yes → skip component render
 └── no  → render
```

But there is a catch.

## 9. `memo` Doesn't Mean “Never Re-render”

This:

```tsx
const User = memo(...);
```

doesn't prevent rendering caused by the component's own:

```text
state
context
other relevant subscriptions
```

And if props are always new references:

```tsx
<User user={{ name: "John" }} />
```

memoization may not help.

## 10. Referential Equality ⭐⭐⭐⭐⭐

This is the foundation of many React performance issues.

Consider:

```tsx
const user = {
  name: "John"
};
```

On every render:

```text
Render 1 → object A
Render 2 → object B
Render 3 → object C
```

Even though the contents are identical:

```text
A !== B
B !== C
```

JavaScript compares objects by reference.

This matters for:

```text
memo
useMemo
useCallback
useEffect dependencies
context values
```

## 11. The Classic `memo` Problem

```tsx
const Child = memo(function Child({ user }) {
  return <div>{user.name}</div>;
});

function Parent() {
  const user = {
    name: "John"
  };

  return <Child user={user} />;
}
```

Every parent render:

```text
new user object
      ↓
new reference
      ↓
memo sees changed prop
      ↓
Child renders
```

You might use:

```tsx
const user = useMemo(
  () => ({ name: "John" }),
  []
);
```

But don't blindly do this.

First ask:

> Is the child actually expensive enough that this matters?

## 12. `useMemo` ⭐⭐⭐⭐⭐

`useMemo` caches a calculated value.

```tsx
const filteredProducts = useMemo(() => {
  return products.filter(product =>
    product.name.includes(search)
  );
}, [products, search]);
```

Think:

```text
dependencies changed?
     │
 ┌───┴───┐
no      yes
↓        ↓
reuse   calculate
```

## 13. When `useMemo` Helps

Good candidates:

```text
expensive calculation
large filtering/sorting
large derived data
stable object identity needed
```

Example:

```tsx
const sortedProducts = useMemo(
  () => expensiveSort(products),
  [products]
);
```

## 14. When `useMemo` Is Pointless

Don't do:

```tsx
const name = useMemo(() => user.name, [user]);
```

if:

```text
user.name
```

is trivial.

You've potentially added:

```text
memoization bookkeeping
dependency tracking
mental complexity
```

to avoid a tiny calculation.

#### Rule:

> **Memoize expensive work, not every piece of work.**

## 15. `useCallback` ⭐⭐⭐⭐⭐

`useCallback` memoizes a function reference.

```tsx
const handleDelete = useCallback(
  (id: string) => {
    deleteUser(id);
  },
  []
);
```

Useful when function identity matters.

Most commonly:

```text
memoized child
+
function passed as prop
```

## 16. Why `useCallback` Can Matter

```tsx
const Child = memo(function Child({ onClick }) {
  // ...
});
```

Parent:

```tsx
function Parent() {
  const handleClick = () => {};

  return <Child onClick={handleClick} />;
}
```

Every render:

```text
handleClick A
handleClick B
handleClick C
```

Therefore:

```text
new function reference
 ↓
Child's props changed
 ↓
memo cannot skip
```

Using:

```tsx
const handleClick = useCallback(() => {}, []);
```

can preserve the reference.

## 17. But Don't Overuse `useCallback`

This is bad thinking:

```text
"Functions are recreated,
therefore every function needs useCallback."
```

No.

Function creation is usually cheap.

Use `useCallback` when:

```text
function identity matters
+
there is a meaningful optimization to gain
```

Otherwise you're adding complexity for little benefit.

## 18. `useMemo` + `useCallback` + `memo`

These three often work together:

```text
               Parent
                  │
        ┌─────────┴─────────┐
        ↓                   ↓
useMemo(value)      useCallback(function)
        │                   │
        └─────────┬─────────┘
                  ↓
              Child
             memo(...)
                  ↓
          skip unnecessary work
```

But all of this is based on:

> **referential equality.**

## 19. Context Performance ⭐⭐⭐⭐⭐

Context can create unexpected rendering.

Suppose:

```tsx
const AppContext = createContext(null);
```

Provider:

```tsx
<AppContext.Provider value={{ user, theme }}>
```

Every provider render can create a new object:

```text
new value object
      ↓
context value changes
      ↓
consumers update
```

You can sometimes stabilize the value:

```tsx
const value = useMemo(
  () => ({ user, theme }),
  [user, theme]
);
```

But again, the real solution may be better state architecture.

## 20. Context Splitting

Instead of:

```text
AppContext
 ├── user
 ├── theme
 ├── notifications
 ├── settings
 └── cart
```

you can split contexts:

```text
UserContext
ThemeContext
NotificationContext
CartContext
```

Now changes to one concern don't necessarily involve consumers of unrelated contexts.

This is a powerful architectural optimization.

## 21. Avoid Huge Context Providers

A giant provider:

```tsx
<AppProvider>
  <Everything />
</AppProvider>
```

can become difficult to reason about.

Prefer focused providers when appropriate:

```text
AuthProvider
ThemeProvider
CartProvider
```

This improves both architecture and rendering behavior.

## 22. Derived Data ⭐⭐⭐⭐⭐

Don't duplicate state unnecessarily.

Bad:

```tsx
const [products, setProducts] = useState([]);
const [filteredProducts, setFilteredProducts] = useState([]);
```

Now you must keep:

```text
products
filteredProducts
```

synchronized.

Often better:

```tsx
const filteredProducts = useMemo(
  () => products.filter(...),
  [products]
);
```

You have:

```text
one source of truth
        ↓
derived value
```

## 23. Avoid Unnecessary Effects

A very common performance problem:

```tsx
const [fullName, setFullName] = useState("");

useEffect(() => {
  setFullName(`${firstName} ${lastName}`);
}, [firstName, lastName]);
```

This can create:

```text
render
 ↓
effect
 ↓
setFullName
 ↓
another render
```

Instead:

```tsx
const fullName = `${firstName} ${lastName}`;
```

No extra state update.

#### Rule:

> **Don't use an effect to calculate something that can be calculated during rendering.**

## 24. Expensive Calculations

Suppose:

```tsx
const result = heavyCalculation(data);
```

If the component renders 100 times:

```text
heavyCalculation()
heavyCalculation()
heavyCalculation()
...
```

If `data` hasn't changed, memoization may help:

```tsx
const result = useMemo(
  () => heavyCalculation(data),
  [data]
);
```

But measure first if you're unsure.

## 25. List Rendering ⭐⭐⭐⭐⭐

This is one of the biggest real-world performance areas.

Imagine:

```text
100,000 products
```

Rendering all of them:

```text
100,000 components
100,000 DOM nodes
```

is expensive.

Instead:

```text
Virtualization
```

renders only visible rows.

## 26. Virtualization

Concept:

```text
100,000 items
       ↓
Only ~20 visible
       ↓
Render those
       ↓
User scrolls
       ↓
Replace visible window
```

This dramatically reduces DOM work for huge lists.

Common techniques/libraries use:

```text
windowing
virtualization
```

## 27. Pagination vs Infinite Scroll vs Virtualization

These solve different problems.

| Technique       | Main purpose                |
| --------------- | --------------------------- |
| Pagination      | Don't fetch everything      |
| Infinite scroll | Fetch more as user scrolls  |
| Virtualization  | Don't render everything     |
| Memoization     | Avoid repeated calculations |
| Code splitting  | Don't load all JS initially |

You can combine them.

For example:

```text
API
 ↓
pagination
 ↓
1000 loaded items
 ↓
virtualization
 ↓
20 rendered
```

## 28. Debouncing ⭐⭐⭐⭐⭐

Suppose search triggers API requests:

```text
J
Jo
Joh
John
```

Without debounce:

```text
4 requests
```

With debounce:

```text
user types
 ↓
wait
 ↓
user stops
 ↓
request
```

Conceptually:

```text
typing ────────┐
               ↓
             wait
               ↓
             search
```

Useful for:

```text
search
autocomplete
filter APIs
resize handlers
```

## 29. Throttling

Throttling is different.

It limits execution to at most a certain frequency.

Example:

```text
scroll events:
████████████████████████

throttled:
█    █    █    █    █
```

Useful for:

```text
scroll
mousemove
resize
continuous events
```

#### Remember

```text
Debounce
→ wait until activity stops

Throttle
→ run at controlled intervals
```

## 30. Network Optimization ⭐⭐⭐⭐⭐

Your UI can be perfectly optimized but still feel slow because of the network.

Look at:

```text
API latency
payload size
number of requests
duplicate requests
caching
compression
pagination
prefetching
```

Example:

Bad:

```text
Page
 ↓
50 API requests
```

Better:

```text
Page
 ↓
optimized data fetching
 ↓
fewer requests / parallel requests / cached results
```

## 31. Parallel vs Sequential Requests

Bad:

```tsx
const user = await getUser();
const posts = await getPosts();
const comments = await getComments();
```

If independent:

```tsx
const [user, posts, comments] =
  await Promise.all([
    getUser(),
    getPosts(),
    getComments()
  ]);
```

Conceptually:

```text
Sequential:
A → B → C

Parallel:
A ─┐
B ─┼→ result
C ─┘
```

This is a JavaScript/network optimization, not specifically a React optimization.

But it matters enormously for React applications.

## 32. Data Fetching and Caching

Instead of manually:

```text
fetch
 ↓
loading
 ↓
success
 ↓
error
 ↓
cache
 ↓
refetch
```

many applications use a dedicated server-state library.

The goal:

```text
request
 ↓
cache
 ↓
reuse
 ↓
background refetch
```

This prevents unnecessary network requests and improves perceived performance.

## 33. Code Splitting ⭐⭐⭐⭐⭐

Suppose your app contains:

```text
Dashboard
Admin
Editor
Analytics
Settings
```

Don't necessarily load everything immediately.

Instead:

```text
Initial bundle
      ↓
Dashboard

Admin chunk
Editor chunk
Analytics chunk
Settings chunk
```

Load them when needed.

## 34. Lazy Loading

React supports:

```tsx
const Admin = lazy(() => import("./Admin"));
```

Then:

```tsx
<Suspense fallback={<Loading />}>
  <Admin />
</Suspense>
```

Flow:

```text
User visits Admin
       ↓
Load Admin chunk
       ↓
Show fallback
       ↓
Chunk arrives
       ↓
Render Admin
```

## 35. Bundle Size ⭐⭐⭐⭐⭐

Your JavaScript bundle might contain:

```text
React
UI library
date library
charts
icons
utilities
your code
```

If the bundle becomes huge:

```text
Browser downloads
 ↓
parses
 ↓
compiles
 ↓
executes
```

That can delay interactivity.

Optimize using:

```text
tree shaking
code splitting
lazy loading
smaller dependencies
modern builds
avoiding unnecessary libraries
```

## 36. Tree Shaking

Suppose a library contains:

```text
100 functions
```

but you use:

```text
2 functions
```

A good build system may remove unused code.

Conceptually:

```text
Library
 ↓
unused code removed
 ↓
smaller bundle
```

This is **tree shaking**.

It works best with statically analyzable module systems such as ESM.

## 37. Images Can Be Bigger Than JavaScript

Don't focus only on React code.

A page can be slow because of:

```text
10 MB images
```

Optimization:

```text
responsive images
modern formats
compression
lazy loading
proper dimensions
CDN
```

Example:

```tsx
<img
  src={image}
  loading="lazy"
  alt="Product"
/>
```

For above-the-fold images, blindly lazy-loading everything may hurt perceived performance.

## 38. Browser Rendering Performance

After React commits DOM changes, the browser still has work:

```text
DOM
 ↓
Style calculation
 ↓
Layout
 ↓
Paint
 ↓
Composite
```

Expensive operations can cause:

```text
layout thrashing
large paints
slow animations
```

For smooth animations, prefer properties that can often be handled efficiently by compositing, such as:

```text
transform
opacity
```

rather than repeatedly changing layout-heavy properties.

## 39. Avoid Layout Thrashing

Bad pattern:

```text
read layout
write layout
read layout
write layout
...
```

Repeatedly forcing the browser to calculate layout can be expensive.

Conceptually:

```text
read → write → read → write
```

can cause repeated layout calculations.

Better:

```text
read all needed measurements
 ↓
perform writes
```

This is primarily a browser performance concept, but React developers need to understand it.

## 40. Web Workers

If you have heavy CPU computation:

```text
Huge calculation
 ↓
main browser thread
 ↓
UI freezes
```

A Web Worker can move the work away from the main UI thread.

```text
Main thread
   │
   └──── Worker
          ↓
      heavy computation
```

This is different from React's `useMemo`.

#### `useMemo`

```text
avoid repeating calculation
```

#### Web Worker

```text
move calculation away from main thread
```

## 41. `useMemo` Does NOT Make Code Asynchronous

This:

```tsx
useMemo(() => heavyCalculation(), []);
```

still runs JavaScript on the same thread.

It doesn't magically move computation elsewhere.

If the calculation is huge:

```text
useMemo
→ cache it

Worker
→ move it
```

These solve different problems.

## 42. Profiling ⭐⭐⭐⭐⭐

The most important optimization skill:

> **Measure before optimizing.**

Useful tools:

```text
React DevTools Profiler
Chrome DevTools Performance
Chrome Network panel
Lighthouse / performance audits
bundle analyzers
```

Don't guess.

## 43. React DevTools Profiler

The profiler can help answer:

```text
Which component rendered?
Why did it render?
How long did rendering take?
Which components were expensive?
```

This turns:

```text
"I think this component is slow"
```

into:

```text
"This component rendered 300 times
and spent 80ms rendering."
```

Much better.

## 44. Performance Budget

You can establish limits for:

```text
JS bundle size
image size
API latency
initial load
interaction latency
rendering time
```

Then treat performance as a measurable requirement rather than a last-minute optimization project.

## 45. `useMemo` vs `React.memo` vs `useCallback`

This is worth memorizing:

| Tool          | What it memoizes                   |
| ------------- | ---------------------------------- |
| `useMemo`     | Value                              |
| `useCallback` | Function reference                 |
| `React.memo`  | Component rendering based on props |

Example:

```tsx
const data = useMemo(() => calculate(), []);

const handleClick = useCallback(() => {
  // ...
}, []);

const Child = memo(function Child() {
  // ...
});
```

All three relate to **avoiding unnecessary work**, but in different places.

## 46. Optimization Hierarchy ⭐⭐⭐⭐⭐

When something is slow, think in this order:

```text
1. Is the architecture correct?
          ↓
2. Is state placed correctly?
          ↓
3. Am I doing unnecessary work?
          ↓
4. Can I reduce data/network work?
          ↓
5. Can I reduce rendering?
          ↓
6. Can I reduce expensive calculations?
          ↓
7. Can I reduce bundle size?
          ↓
8. Can I optimize browser work?
          ↓
9. Only then use micro-optimizations
```

This is much better than:

```text
useMemo
useCallback
useMemo
useCallback
...
```
