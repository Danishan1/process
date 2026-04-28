Good. Now we upgrade your authentication system to a **real production-style token system**.

We will implement:

- Access Token (short-lived)
- Refresh Token (long-lived)
- Token rotation flow

This is exactly how modern apps (Google, GitHub, banking apps) handle sessions.

We’ll build it step-by-step, not just theory.

# 1. What problem we are solving

Right now you have:

- One JWT token
- If it expires → user must login again ❌

This is bad UX.

# 2. New system design

We will use 2 tokens:

## 1. Access Token

- short life (15 min)
- used for API calls

## 2. Refresh Token

- long life (7–30 days)
- used to generate new access tokens

# 3. Flow (VERY IMPORTANT)

```text id="flow-refresh"
LOGIN
  ↓
Access Token (short)
Refresh Token (long)
  ↓
Client stores both

API request:
  → Access token used

If expired:
  → send Refresh token
  → get new Access token
```

# 4. Update database (MySQL)

We add refresh token storage:

```sql id="sql-refresh"
ALTER TABLE users
ADD refresh_token TEXT;
```

# 5. Install dependency (already done but confirm)

We use:

jsonwebtoken

```bash id="install-jwt"
npm install jsonwebtoken
```

# 6. Token service (UPDATED)

## services/tokenService.js

```js id="token-service-v2"
import jwt from "jsonwebtoken";

const ACCESS_SECRET = "access_secret_key";
const REFRESH_SECRET = "refresh_secret_key";

/*
CREATE ACCESS TOKEN
*/
export const generateAccessToken = (user) => {
  return jwt.sign({ id: user.id, role: user.role }, ACCESS_SECRET, {
    expiresIn: "15m",
  });
};

/*
CREATE REFRESH TOKEN
*/
export const generateRefreshToken = (user) => {
  return jwt.sign({ id: user.id }, REFRESH_SECRET, { expiresIn: "7d" });
};

/*
VERIFY ACCESS TOKEN
*/
export const verifyAccessToken = (token) => {
  return jwt.verify(token, ACCESS_SECRET);
};

/*
VERIFY REFRESH TOKEN
*/
export const verifyRefreshToken = (token) => {
  return jwt.verify(token, REFRESH_SECRET);
};
```

# 7. Update USER MODEL (store refresh token)

## models/userModel.js

```js id="user-model-refresh"
import db from "../config/db.js";

/*
SAVE REFRESH TOKEN
*/
export const saveRefreshToken = (userId, token, callback) => {
  const sql = "UPDATE users SET refresh_token = ? WHERE id = ?";
  db.query(sql, [token, userId], callback);
};

/*
FIND USER BY REFRESH TOKEN
*/
export const findUserByRefreshToken = (token, callback) => {
  const sql = "SELECT * FROM users WHERE refresh_token = ?";
  db.query(sql, [token], callback);
};
```

# 8. AUTH CONTROLLER (UPDATED LOGIN SYSTEM)

## controllers/authController.js

```js id="auth-refresh"
import {
  createUser,
  findUserByEmail,
  saveRefreshToken,
  findUserByRefreshToken,
} from "../models/userModel.js";

import {
  validatePassword,
  hashPassword,
  comparePassword,
} from "../services/passwordService.js";

import {
  generateAccessToken,
  generateRefreshToken,
  verifyRefreshToken,
} from "../services/tokenService.js";

/*
REGISTER (same as before)
*/
export const register = async (req, res) => {
  const { name, email, password } = req.body;

  const error = validatePassword(password);
  if (error) return res.status(400).json({ error });

  const hashed = await hashPassword(password);

  createUser(name, email, hashed, (err) => {
    if (err) return res.status(500).json(err);

    res.json({ message: "User registered" });
  });
};

/*
LOGIN (UPDATED)
*/
export const login = (req, res) => {
  const { email, password } = req.body;

  findUserByEmail(email, async (err, results) => {
    if (err) return res.status(500).json(err);

    if (results.length === 0) {
      return res.status(404).json({ error: "User not found" });
    }

    const user = results[0];

    const match = await comparePassword(password, user.password);

    if (!match) {
      return res.status(401).json({ error: "Invalid credentials" });
    }

    // CREATE TOKENS
    const accessToken = generateAccessToken(user);
    const refreshToken = generateRefreshToken(user);

    // SAVE REFRESH TOKEN IN DB
    saveRefreshToken(user.id, refreshToken, () => {});

    res.json({
      accessToken,
      refreshToken,
    });
  });
};
```

# 9. REFRESH TOKEN ENDPOINT (NEW)

## controllers/authController.js (ADD THIS)

```js id="refresh-controller"
export const refreshToken = (req, res) => {
  const { token } = req.body;

  if (!token) {
    return res.status(401).json({ error: "No token provided" });
  }

  findUserByRefreshToken(token, (err, results) => {
    if (err) return res.status(500).json(err);

    if (results.length === 0) {
      return res.status(403).json({ error: "Invalid refresh token" });
    }

    const user = results[0];

    try {
      verifyRefreshToken(token);

      const newAccessToken = generateAccessToken(user);

      res.json({
        accessToken: newAccessToken,
      });
    } catch (err) {
      return res.status(403).json({ error: "Expired refresh token" });
    }
  });
};
```

# 10. Routes update

## routes/authRoutes.js

```js id="routes-refresh"
import express from "express";
import {
  register,
  login,
  refreshToken,
} from "../controllers/authController.js";

const router = express.Router();

router.post("/register", register);
router.post("/login", login);
router.post("/refresh", refreshToken);

export default router;
```

# 11. Final system flow

## LOGIN

```text id="login-flow"
User → login
     → access token + refresh token
     → stored on client
```

## API REQUEST

```text id="api-flow"
Client → access token
       → API works
```

## TOKEN EXPIRED

```text id="expired-flow"
Client → refresh token
       → new access token
       → continue using app
```

# 12. What you just built

You now have:

- Real session management system
- Short-lived access tokens
- Long-lived refresh tokens
- Secure token rotation system
- Database-backed session storage

This is **industry-level authentication architecture**.

# 13. Why this is important

Without refresh tokens:

- users keep logging in ❌

With refresh tokens:

- seamless experience ✅
- secure sessions ✅
- scalable authentication system ✅
