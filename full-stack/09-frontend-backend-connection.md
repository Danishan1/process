# 📘 React & Express Communication – Student Notes

---

## 1️⃣ Big Picture: How Frontend & Backend Communicate

* **Frontend (React)** and **Backend (Node + Express)** are two separate applications.
* They communicate using **HTTP requests**.

### Basic Flow:

1. React sends a request
2. Express receives and processes it
3. Express sends back a JSON response
4. React updates the UI

👉 React → HTTP Request → Express → JSON Response → React UI Update

That’s the core idea.

---

## 2️⃣ Real-Life Analogy (Restaurant Model)

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

## 3️⃣ Technical Flow (Step-by-Step)

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

## 4️⃣ Simple Working Example

---

### 🔹 Backend (Node + Express)

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

### 🔹 Frontend (React)

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

## 5️⃣ Understanding REST API

REST uses HTTP methods:

| Method | Purpose       |
| ------ | ------------- |
| GET    | Retrieve data |
| POST   | Create data   |
| PUT    | Update data   |
| DELETE | Remove data   |

### Example: Create User

Backend:

```js
app.post("/api/users", (req, res) => {
  const user = req.body;
  res.json({ message: "User created", user });
});
```

Frontend:

```js
axios.post("/api/users", { name: "John" });
```

---

## 6️⃣ Clean Project Structure (Professional Setup)

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

## 7️⃣ Important Concepts to Remember

### 🔹 CORS

Allows frontend and backend (different ports) to communicate.

```js
app.use(cors());
```

---

### 🔹 JSON

Backend sends structured data in JSON format.

---

### 🔹 Async / Await

API calls are asynchronous.

```js
const data = await axios.get("/api/users");
```

---

### 🔹 HTTP Status Codes

```js
res.status(200).json(...)
res.status(404).json(...)
res.status(500).json(...)
```

* 200 → Success
* 404 → Not Found
* 500 → Server Error

---

## 8️⃣ Full Data Flow Diagram

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




# 🌍 Real-World Use Case: User Login System

## 🧠 Scenario

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

# 🏗️ Big Picture

### What happens when a user logs in?

1. User enters email & password in React
2. React sends POST request to Express
3. Express validates user
4. Express sends back success/error JSON
5. React updates UI

---

# 🔁 Real-World Flow

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

# 🔹 Backend (Express with ES6 Modules)

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

### 🔎 Important ES6 Concepts Used

* `import` instead of `require`
* Arrow functions
* Destructuring (`{ email, password }`)
* `find()` array method
* Template literals

---

# 🔹 Frontend (React Login Example)

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

# 🔍 What Happens When User Clicks Login?

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

# 🧱 Professional Folder Structure

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

# 🎯 Real-World Concepts This Example Teaches

* REST API (POST request)
* JSON communication
* HTTP status codes (200, 401)
* Form handling in React
* Async/await
* Error handling
* ES6 syntax
* CORS usage

---

# 🚀 How This Works in Production

In a real app:

* Passwords are hashed (bcrypt)
* Database is MongoDB / PostgreSQL
* JWT token is generated
* Token stored in localStorage or cookies
* Protected routes use middleware

---

# 🧠 Final Mental Model

Think of login like this:

```
React = Sends credentials
Express = Verifies
Database = Confirms identity
Express = Sends response
React = Updates UI
```


