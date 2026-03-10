# 1️⃣ React Setup & Hello World

**Practical Questions**

1. Create a new React project using **Create React App**.
2. Display **"Hello React"** on the screen.
3. Display your **name, course, and city** using JSX.
4. Create a component called `Welcome` and render it in `App.js`.
5. Display multiple elements using a **React Fragment**.

# 2️⃣ Components

**Practice Tasks**

1. Create **three components**:
   - Header
   - Content
   - Footer

2. Import and display them in `App.js`.

3. Create a component `Student` that displays:
   - name
   - age
   - course

4. Create a component that returns **multiple JSX elements**.

# 3️⃣ Functional Components

**Practice Questions**

1. Create a functional component called `Greeting`.
2. Display **Good Morning, User**.
3. Create multiple functional components and render them inside `App`.
4. Build a `Profile` component showing:
   - name
   - profession
   - profile picture.

# 4️⃣ Class Components

**Practice Questions**

1. Create a **class component** called `Welcome`.
2. Display a message inside the `render()` method.
3. Create a class component `User` that shows:
   - name
   - email

4. Convert a **functional component into a class component**.

# 5️⃣ JSX

**Practice Tasks**

1. Display variables inside JSX.
2. Perform **math calculations in JSX**.
3. Display an **image using JSX**.
4. Create a JSX layout containing:
   - heading
   - paragraph
   - button

5. Show the **current date and time** using JSX.

# 6️⃣ Props

**Practice Questions**

1. Pass a **name prop** to a `Greeting` component.
2. Pass multiple props:
   - name
   - age
   - city

3. Create a `Product` component that receives:
   - productName
   - price

4. Display **different data using props**.
5. Pass props from **parent → child component**.

Example task:

```jsx
<Product name="Laptop" price="50000" />
```

# 7️⃣ State

**Practice Questions**

1. Create a component with **state storing a name**.
2. Display the state value in JSX.
3. Create a **counter using state**.
4. Display a **welcome message that changes when state updates**.
5. Store **multiple values in state**.

Example:

```
count = 0
```

# 8️⃣ setState()

**Practice Exercises**

1. Create a **counter app** with:
   - Increment button
   - Decrement button

2. Change text on button click.

Example task:

```
Before Click → "Hello"
After Click → "Welcome"
```

3. Create a button to **increase likes**.

# 9️⃣ Destructuring Props & State

**Practice Questions**

1. Use **destructuring to display props**.
2. Destructure values inside a functional component.
3. Destructure state variables in a class component.
4. Display user details using destructuring.

Example task:

```
props = {name:"Rahul", age:20}
```

# 🔟 Event Handling

**Practice Tasks**

1. Create a button that shows an **alert when clicked**.
2. Log **"Button clicked"** in the console.
3. Change text when a button is clicked.
4. Display an alert showing **user name on click**.
5. Handle **multiple button events**.

# 1️⃣1️⃣ Binding Event Handlers

**Practice Questions**

1. Create a class component with a **click handler**.
2. Bind the event handler using:
   - constructor binding

3. Try **arrow function binding**.
4. Compare both methods.

# 1️⃣2️⃣ Methods as Props

**Practice Tasks**

1. Create a **Parent component**.
2. Pass a function to a **Child component**.
3. Trigger parent function from child.

Example scenario:

```
Parent passes function → Child button click → Parent alert
```

# 1️⃣3️⃣ Conditional Rendering

**Practice Questions**

1. Show **Login / Logout message** based on condition.
2. Show content only when user is logged in.
3. Use **ternary operator** for conditional UI.
4. Display **Admin Panel** only for admin users.
5. Show a message:

```
Cart is empty
```

if cart items = 0.

# 1️⃣4️⃣ List Rendering

**Practice Tasks**

1. Render a **list of names** using `map()`.
2. Display a list of **students**.
3. Render **products dynamically**.

Example array:

```
["Laptop","Mobile","Tablet"]
```

# 1️⃣5️⃣ Lists & Keys

**Practice Questions**

1. Render a list of items with **unique keys**.
2. Display student records using **id as key**.
3. Create a **dynamic product list**.

Example:

```
{id:1, name:"Laptop"}
```

# 1️⃣6️⃣ Index as Key (Anti-Pattern)

**Practice Tasks**

1. Render a list using **index as key**.
2. Replace index with **unique id**.
3. Explain the difference in behavior.

# 1️⃣7️⃣ Styling & CSS

**Practice Questions**

1. Style a component using **external CSS file**.
2. Apply **inline styles**.
3. Create a styled **button component**.
4. Change style dynamically.

Example:

```
Button color changes when clicked
```

5. Create a **simple card component** with CSS styling.

# ⭐ Mini Projects (Using These 20 Topics)

Try these **small projects**:

1️⃣ Counter App
2️⃣ Student List Display
3️⃣ Simple Login / Logout UI
4️⃣ Product List Component
5️⃣ Profile Card Component
6️⃣ Like Button System
7️⃣ Dynamic Greeting App
8️⃣ Simple Shopping Cart UI
