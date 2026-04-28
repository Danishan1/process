# 0. Install dependencies

```bash id="auth-init"
npm init -y
npm install express mysql2 dotenv bcrypt jsonwebtoken
```

Enable ES6:

```json id="esm-auth"
{ "type": "module" }
```

# 1. Database setup (MySQL)

```sql id="auth-sql-1"
CREATE DATABASE auth_system;

USE auth_system;

CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100),
  email VARCHAR(150) UNIQUE,
  password VARCHAR(255),
  role VARCHAR(20) DEFAULT 'user',
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

# 2. Folder structure (final real-world version)

```id="auth-structure"
auth-system/
│
├── server.js
├── app.js
│
├── config/
│   └── db.js
│
├── controllers/
│   └── authController.js
│
├── routes/
│   └── authRoutes.js
│
├── middleware/
│   └── authMiddleware.js
│
├── services/
│   ├── passwordService.js
│   └── tokenService.js
│
└── models/
    └── userModel.js
```

# 3. Database connection

## config/db.js

```js id="db-auth"
import mysql from "mysql2";

const db = mysql.createConnection({
  host: "localhost",
  user: "root",
  password: "your_password",
  database: "auth_system",
});

db.connect((err) => {
  if (err) {
    console.error("DB connection failed", err);
    return;
  }
  console.log("Connected to MySQL");
});

export default db;
```

# 4. Password security service (IMPORTANT)

## services/passwordService.js

This implements:

- password hashing
- password validation rules (Layer 2 + Layer 3)

```js id="pwd-service"
import bcrypt from "bcrypt";

/*
LAYER 2 + 3: PASSWORD VALIDATION
*/
export const validatePassword = (password) => {
  const hasUpper = /[A-Z]/.test(password);
  const hasLower = /[a-z]/.test(password);
  const hasNumber = /[0-9]/.test(password);
  const hasSymbol = /[!@#$%^&*]/.test(password);

  const weakPatterns = ["123456", "111111", "777777", "password", "qwerty"];

  const isWeakPattern = weakPatterns.some((p) => password.includes(p));

  if (!hasUpper || !hasLower || !hasNumber || !hasSymbol) {
    return "Password must include upper, lower, number, symbol";
  }

  if (isWeakPattern) {
    return "Password is too common or predictable";
  }

  return null;
};

/*
LAYER 5: HASHING PASSWORD
*/
export const hashPassword = async (password) => {
  const salt = await bcrypt.genSalt(10);
  return await bcrypt.hash(password, salt);
};

/*
COMPARE PASSWORD
*/
export const comparePassword = async (password, hash) => {
  return await bcrypt.compare(password, hash);
};
```

# 5. Token service (JWT auth)

## services/tokenService.js

```js id="token-service"
import jwt from "jsonwebtoken";

const SECRET = "mysecretkey";

/*
GENERATE TOKEN
*/
export const generateToken = (user) => {
  return jwt.sign({ id: user.id, role: user.role }, SECRET, {
    expiresIn: "1h",
  });
};

/*
VERIFY TOKEN
*/
export const verifyToken = (token) => {
  return jwt.verify(token, SECRET);
};
```

# 6. User model (DB layer)

## models/userModel.js

```js id="user-model-auth"
import db from "../config/db.js";

export const createUser = (name, email, password, callback) => {
  const sql = "INSERT INTO users (name, email, password) VALUES (?, ?, ?)";
  db.query(sql, [name, email, password], callback);
};

export const findUserByEmail = (email, callback) => {
  const sql = "SELECT * FROM users WHERE email = ?";
  db.query(sql, [email], callback);
};
```

# 7. Auth controller (CORE LOGIC)

## controllers/authController.js

```js id="auth-controller"
import {
  validatePassword,
  hashPassword,
  comparePassword,
} from "../services/passwordService.js";

import { generateToken } from "../services/tokenService.js";

import { createUser, findUserByEmail } from "../models/userModel.js";

/*
REGISTER
*/
export const register = async (req, res) => {
  const { name, email, password } = req.body;

  // LAYER 2 + 3 validation
  const error = validatePassword(password);
  if (error) {
    return res.status(400).json({ error });
  }

  // hash password (LAYER 5)
  const hashedPassword = await hashPassword(password);

  createUser(name, email, hashedPassword, (err, result) => {
    if (err) return res.status(500).json(err);

    res.status(201).json({
      message: "User created successfully",
    });
  });
};

/*
LOGIN
*/
export const login = (req, res) => {
  const { email, password } = req.body;

  findUserByEmail(email, async (err, results) => {
    if (err) return res.status(500).json(err);

    if (results.length === 0) {
      return res.status(404).json({ error: "User not found" });
    }

    const user = results[0];

    const isMatch = await comparePassword(password, user.password);

    if (!isMatch) {
      return res.status(401).json({ error: "Invalid credentials" });
    }

    // generate token (AUTHENTICATION)
    const token = generateToken(user);

    res.json({
      message: "Login successful",
      token,
    });
  });
};
```

# 8. Routes

## routes/authRoutes.js

```js id="auth-routes"
import express from "express";
import { register, login } from "../controllers/authController.js";

const router = express.Router();

router.post("/register", register);
router.post("/login", login);

export default router;
```

# 9. Middleware (authorization layer)

## middleware/authMiddleware.js

```js id="auth-middleware"
import { verifyToken } from "../services/tokenService.js";

export const authMiddleware = (req, res, next) => {
  const token = req.headers.authorization;

  if (!token) {
    return res.status(401).json({ error: "No token provided" });
  }

  try {
    const decoded = verifyToken(token);
    req.user = decoded;
    next();
  } catch (err) {
    return res.status(401).json({ error: "Invalid token" });
  }
};
```

# 10. App setup

## app.js

```js id="app-auth"
import express from "express";
import authRoutes from "./routes/authRoutes.js";

const app = express();

app.use(express.json());

app.use("/auth", authRoutes);

export default app;
```

# 11. Server

## server.js

```js id="server-auth"
import app from "./app.js";

app.listen(3000, () => {
  console.log("Server running on http://localhost:3000");
});
```

# 12. How security layers are now implemented

## Layer 1: Basic auth

- email + password login

## Layer 2 + 3: Password rules

- uppercase, lowercase, number, symbol
- blocks weak patterns

## Layer 5: Hashing

- bcrypt stores safe passwords

## Layer 6: Authentication

- JWT token issued on login

## Layer 7: Authorization

- middleware checks token

# 13. Request flow (very important)

### Register

```
Client → Controller → Validation → Hash → DB
```

### Login

```
Client → DB lookup → Password compare → JWT token → Response
```

### Protected route

```
Client → Middleware → Token verify → Controller → Response
```

# 14. What you just built

You now have:

- Real authentication system
- Password security rules
- Hashing system
- JWT authentication
- Middleware-based authorization
- MySQL user system

This is **production-level backend design foundation**.
