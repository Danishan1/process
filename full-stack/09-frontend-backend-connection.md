
# React & Express Communication – Student Notes

---

## 1 Big Picture: How Frontend & Backend Communicate

* **Frontend (React)** and **Backend (Node + Express)** are two separate applications.
* They communicate using **HTTP requests**.

### Basic Flow:

1. React sends a request
2. Express receives and processes it
3. Express sends back a JSON response
4. React updates the UI

React → HTTP Request → Express → JSON Response → React UI Update

That’s the core idea.

---

## 2 Real-Life Analogy (Restaurant Model)

| Role     | Real World         | In Web App  |
| -------- | ------------------ | ----------- |
| Customer | Orders food        | React       |
| Waiter   | Carries order      | Axios       |
| Kitchen  | Prepares food      | Express API |
| Storage  | Stores ingredients | Database    |

### Flow:

1. React asks for data
2. Axios sends request
3. Express processes it
4. Database returns data
5. Express sends JSON
6. React updates UI

This makes the system easier to visualize.

---

## 3 Technical Flow (Step-by-Step)

### Step 1: User Action

User clicks a button in React.

### Step 2: Axios Sends Request

```js
axios.get("http://localhost:5000/api/users");
```

This creates an **HTTP GET request**.

---

### Step 3: Express Handles Request

```js
app.get("/api/users", (req, res) => {
  res.json({ name: "Danishan", role: "Developer" });
});
```

* Express listens on that route
* Sends JSON response

---

### Step 4: React Receives Response

```js
const response = await axios.get("/api/users");
console.log(response.data);
```

---

### Step 5: UI Updates

* React updates state
* Component re-renders
* Data appears on screen

---

## 4 Simple Working Example

---

### Backend (Node + Express)

```js
// server.js
const express = require("express");
const cors = require("cors");

const app = express();
app.use(cors());
app.use(express.json());

app.get("/api/message", (req, res) => {
  res.json({ message: "Hello from backend!" });
});

app.listen(5000, () => {
  console.log("Server running on port 5000");
});
```

### Important Concepts:

* `cors()` → Allows frontend to connect
* `express.json()` → Parses incoming JSON
* `res.json()` → Sends JSON response

---

### Frontend (React)

```js
import axios from "axios";
import { useEffect, useState } from "react";

function App() {
  const [message, setMessage] = useState("");

  useEffect(() => {
    axios.get("http://localhost:5000/api/message")
      .then(res => {
        setMessage(res.data.message);
      });
  }, []);

  return (
    <div>
      <h1>{message}</h1>
    </div>
  );
}

export default App;
```

### What Happens:

* Component loads
* API call runs
* Backend sends JSON
* State updates
* UI displays message

---

## 5 Understanding REST API

REST (Representational State Transfer) is a way for the frontend and backend to communicate using **HTTP methods**.

Each HTTP method has a specific purpose.

---

## HTTP Methods

| Method | Purpose             | Sends Data? | Returns Data? |
| ------ | ------------------- | ----------- | ------------- |
| GET    | Retrieve data       | Usually No  | Yes           |
| POST   | Create new data     | Yes (body)  | Yes           |
| PUT    | Replace entire data | Yes (body)  | Yes           |
| PATCH  | Update part of data | Yes (body)  | Yes           |
| DELETE | Remove data         | Sometimes   | Yes           |

---

# Understanding Arguments and Returns

When working with APIs, always think in two parts:

## 5.1 Arguments (What You Send to the Server)

Arguments are the **data you send in the request**.

They can be sent:

* In the URL (params)
* In the query string
* In the request body (for POST, PUT, PATCH)

Example of sending arguments in a POST request:

```js
axios.post("/api/users", {
  name: "John",
  email: "john@test.com"
});
```

Here:

* `name` and `email` are arguments
* They are sent inside the request body
* Express receives them using `req.body`

Backend example:

```js
app.post("/api/users", (req, res) => {
  const { name, email } = req.body;

  res.json({
    message: "User created",
    user: { name, email }
  });
});
```

`req.body` contains the arguments sent from React.

---

## 5.2 Returns (What the Server Sends Back)

The backend always sends a response.

Usually it sends:

* JSON data
* A status code

Example response:

```js
res.status(201).json({
  success: true,
  message: "User created",
  user: { name, email }
});
```

On the frontend:

```js
const response = await axios.post("/api/users", {
  name: "John",
  email: "john@test.com"
});

console.log(response.data);
```

`response.data` is what the server returned.

---

# Method-by-Method Explanation

---

## GET – Retrieve Data

Used to fetch data from the server.

Frontend:

```js
const response = await axios.get("/api/users");
console.log(response.data);
```

Backend:

```js
app.get("/api/users", (req, res) => {
  res.json([
    { id: 1, name: "John" },
    { id: 2, name: "Sarah" }
  ]);
});
```

Key idea:

* No body is sent
* Server returns data

---

## POST – Create New Data

Used to create something new.

Frontend:

```js
axios.post("/api/users", {
  name: "John"
});
```

Backend:

```js
app.post("/api/users", (req, res) => {
  const newUser = req.body;

  res.status(201).json({
    message: "User created",
    user: newUser
  });
});
```

Key idea:

* Sends full new object
* Server creates resource
* Returns created data

---

## PUT – Replace Entire Resource

PUT replaces the whole object.

Frontend:

```js
axios.put("/api/users/1", {
  name: "Updated Name",
  email: "new@email.com"
});
```

Backend:

```js
app.put("/api/users/:id", (req, res) => {
  const updatedUser = req.body;

  res.json({
    message: "User replaced",
    user: updatedUser
  });
});
```

Key idea:

* You send the complete updated object
* Old version is replaced

---

## PATCH – Update Part of a Resource

PATCH updates only specific fields.

This is very common in real applications.

Frontend:

```js
axios.patch("/api/users/1", {
  name: "Only Name Updated"
});
```

Backend:

```js
app.patch("/api/users/:id", (req, res) => {
  const updates = req.body;

  res.json({
    message: "User partially updated",
    updatedFields: updates
  });
});
```

Key idea:

* Only send fields you want to change
* Does not replace entire object
* More efficient than PUT for small changes

---

## DELETE – Remove Data

Frontend:

```js
axios.delete("/api/users/1");
```

Backend:

```js
app.delete("/api/users/:id", (req, res) => {
  res.json({
    message: "User deleted"
  });
});
```

Key idea:

* Removes resource
* Usually returns confirmation message

---

# HTTP Status Codes (Important for Beginners)

| Code | Meaning      |
| ---- | ------------ |
| 200  | Success      |
| 201  | Created      |
| 400  | Bad Request  |
| 401  | Unauthorized |
| 404  | Not Found    |
| 500  | Server Error |

Example:

```js
res.status(404).json({
  message: "User not found"
});
```

Frontend can catch errors:

```js
try {
  const res = await axios.get("/api/users/99");
} catch (error) {
  console.log(error.response.data.message);
}
```

---

# Simple Mental Model for New Developers

When working with REST APIs, always ask:

1. What am I sending? (Arguments)
2. What method am I using? (GET, POST, PUT, PATCH, DELETE)
3. What will the server return? (JSON + status code)
4. How will I use the returned data in React?


---

## 6 Clean Project Structure (Professional Setup)

### Backend Structure

```
backend/
  controllers/
  routes/
  models/
  server.js
```

---

### Frontend Structure

```
frontend/
  src/
    components/
    services/api.js
```

---

### Service Layer Example

```js
// services/api.js
import axios from "axios";

const API = axios.create({
  baseURL: "http://localhost:5000/api"
});

export default API;
```

Usage:

```js
API.get("/users");
```

This keeps API logic organized and reusable.

---

## 7 Important Concepts to Remember

### CORS

Allows frontend and backend (different ports) to communicate.

```js
app.use(cors());
```

---

### JSON

Backend sends structured data in JSON format.

---

### Async / Await

API calls are asynchronous.

```js
const data = await axios.get("/api/users");
```

---

### HTTP Status Codes

```js
res.status(200).json(...)
res.status(404).json(...)
res.status(500).json(...)
```

* 200 → Success
* 404 → Not Found
* 500 → Server Error

---

## 8 Full Data Flow Diagram

```
[ React UI ]
      ↓
   Axios
      ↓
HTTP Request
      ↓
[ Express API ]
      ↓
Database
      ↓
JSON Response
      ↓
React State Update
```

---

# Real-World Use Case: User Login System

## Scenario

You are building a **Task Management App**.

Users must:

* Register
* Login
* View their profile

We’ll focus on the **Login flow** using:

* React (Frontend)
* Express (Backend)
* ES6 Modules
* JSON
* Axios

---

# Big Picture

### What happens when a user logs in?

1. User enters email & password in React
2. React sends POST request to Express
3. Express validates user
4. Express sends back success/error JSON
5. React updates UI

---

# Real-World Flow

```
User → React Form → Axios POST
       ↓
     Express API
       ↓
   Check Database
       ↓
   Send JSON Response
       ↓
React Updates UI
```

---

# Backend (Express with ES6 Modules)

### Step 1: Install Dependencies

```bash
npm init -y
npm install express cors
```

---

### Step 2: server.js (ES6 Version)

```js
// server.js
import express from "express";
import cors from "cors";

const app = express();
const PORT = 5000;

app.use(cors());
app.use(express.json());

// Fake database
const users = [
  { email: "admin@test.com", password: "123456", name: "Admin User" }
];

// Login Route
app.post("/api/login", (req, res) => {
  const { email, password } = req.body;

  const user = users.find(
    u => u.email === email && u.password === password
  );

  if (user) {
    res.status(200).json({
      success: true,
      message: "Login successful",
      user: { name: user.name, email: user.email }
    });
  } else {
    res.status(401).json({
      success: false,
      message: "Invalid credentials"
    });
  }
});

app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

---

### Important ES6 Concepts Used

* `import` instead of `require`
* Arrow functions
* Destructuring (`{ email, password }`)
* `find()` array method
* Template literals

---

# Frontend (React Login Example)

Install Axios:

```bash
npm install axios
```

---

### Login Component

```js
import { useState } from "react";
import axios from "axios";

function Login() {
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");
  const [message, setMessage] = useState("");

  const handleLogin = async (e) => {
    e.preventDefault();

    try {
      const res = await axios.post(
        "http://localhost:5000/api/login",
        { email, password }
      );

      setMessage(res.data.message);
      console.log("User:", res.data.user);

    } catch (error) {
      setMessage(error.response.data.message);
    }
  };

  return (
    <div>
      <h2>Login</h2>

      <form onSubmit={handleLogin}>
        <input
          type="email"
          placeholder="Enter email"
          value={email}
          onChange={e => setEmail(e.target.value)}
        />

        <input
          type="password"
          placeholder="Enter password"
          value={password}
          onChange={e => setPassword(e.target.value)}
        />

        <button type="submit">Login</button>
      </form>

      <p>{message}</p>
    </div>
  );
}

export default Login;
```

---

# What Happens When User Clicks Login?

### Step 1

User types:

```
Email: admin@test.com
Password: 123456
```

### Step 2

Axios sends:

```json
POST /api/login
{
  "email": "admin@test.com",
  "password": "123456"
}
```

### Step 3

Express checks fake database.

### Step 4

If correct:

```json
{
  "success": true,
  "message": "Login successful",
  "user": {
    "name": "Admin User",
    "email": "admin@test.com"
  }
}
```

### Step 5

React updates message + logs user data.

---

# Professional Folder Structure

## Backend

```
backend/
  server.js
```

(For larger apps:)

```
backend/
  controllers/
  routes/
  models/
  server.js
```

---

## Frontend

```
frontend/
  src/
    components/
      Login.js
    App.js
```

---

# Real-World Concepts This Example Teaches

* REST API (POST request)
* JSON communication
* HTTP status codes (200, 401)
* Form handling in React
* Async/await
* Error handling
* ES6 syntax
* CORS usage

---

# How This Works in Production

In a real app:

* Passwords are hashed (bcrypt)
* Database is MongoDB / PostgreSQL
* JWT token is generated
* Token stored in localStorage or cookies
* Protected routes use middleware

---

# Final Mental Model

Think of login like this:

```
React = Sends credentials
Express = Verifies
Database = Confirms identity
Express = Sends response
React = Updates UI
```
