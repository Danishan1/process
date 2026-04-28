Good. Now we add two of the most important real-world security layers:

- Rate limiting (stops brute-force attacks)
- Account lockout (protects individual users)

This is what makes login systems in real apps actually secure.

# 1. What problem we are solving

Even with JWT + refresh tokens, attackers can still try:

- thousands of password guesses
- repeated login attempts
- slow automated attacks

So we need **defense at the request level + user level**

# 2. Two new security layers

## Layer A: Rate Limiting (system-level protection)

Stops too many requests from one IP.

Example:

- 100 requests / 15 minutes
- then block temporarily

## Layer B: Account Lock (user-level protection)

If someone tries wrong password too many times:

- lock that account temporarily

# 3. Install rate limiter

We use:

express-rate-limit

```bash id="rate-install"
npm install express-rate-limit
```

# 4. Add rate limiting middleware

## middleware/rateLimiter.js

```js id="rate1"
import rateLimit from "express-rate-limit";

export const loginLimiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 10, // limit each IP to 10 login requests
  message: {
    error: "Too many login attempts. Try again later.",
  },
});
```

# 5. Apply rate limiting to login route

## routes/authRoutes.js

```js id="rate2"
import express from "express";
import {
  register,
  login,
  refreshToken,
} from "../controllers/authController.js";
import { loginLimiter } from "../middleware/rateLimiter.js";

const router = express.Router();

router.post("/register", register);

// APPLY RATE LIMIT ONLY ON LOGIN
router.post("/login", loginLimiter, login);

router.post("/refresh", refreshToken);

export default router;
```

# 6. Add account lock fields in database

## MySQL update

```sql id="lock-sql"
ALTER TABLE users
ADD failed_attempts INT DEFAULT 0,
ADD lock_until DATETIME NULL;
```

# 7. Update login logic (ACCOUNT LOCK SYSTEM)

## controllers/authController.js (UPDATED LOGIN)

We now track:

- failed attempts
- lock time

```js id="lock-login"
import db from "../config/db.js";

export const login = (req, res) => {
  const { email, password } = req.body;

  findUserByEmail(email, async (err, results) => {
    if (err) return res.status(500).json(err);

    if (results.length === 0) {
      return res.status(404).json({ error: "User not found" });
    }

    const user = results[0];

    // CHECK IF ACCOUNT IS LOCKED
    if (user.lock_until && new Date(user.lock_until) > new Date()) {
      return res.status(403).json({
        error: "Account is temporarily locked. Try later.",
      });
    }

    const match = await comparePassword(password, user.password);

    if (!match) {
      // INCREMENT FAILED ATTEMPTS
      let attempts = user.failed_attempts + 1;
      let lockTime = null;

      // LOCK ACCOUNT AFTER 5 FAILS
      if (attempts >= 5) {
        lockTime = new Date(Date.now() + 10 * 60 * 1000); // 10 min lock
        attempts = 0;
      }

      const sql = `
        UPDATE users 
        SET failed_attempts = ?, lock_until = ? 
        WHERE id = ?
      `;

      db.query(sql, [attempts, lockTime, user.id]);

      return res.status(401).json({
        error: "Invalid credentials",
      });
    }

    // RESET FAILED ATTEMPTS ON SUCCESS
    const resetSql = `
      UPDATE users 
      SET failed_attempts = 0, lock_until = NULL 
      WHERE id = ?
    `;

    db.query(resetSql, [user.id]);

    // GENERATE TOKENS
    const accessToken = generateAccessToken(user);
    const refreshToken = generateRefreshToken(user);

    saveRefreshToken(user.id, refreshToken, () => {});

    res.json({
      accessToken,
      refreshToken,
    });
  });
};
```

# 8. Final security flow

Now login system works like this:

```text id="final-flow"
LOGIN REQUEST
   ↓
Rate limiter check (IP level)
   ↓
Check if account is locked
   ↓
Verify password
   ↓
If wrong:
   → increase failed attempts
   → lock account if needed
   ↓
If correct:
   → reset attempts
   → generate tokens
```

# 9. What you just built

You now have:

## System-level protection

- Rate limiting (blocks brute-force traffic)

## User-level protection

- Account lock after failed attempts

## Combined with earlier layers:

- Password validation
- Hashing (bcrypt)
- JWT authentication
- Refresh tokens
- Role-based authorization

# 10. Real-world mapping

This is exactly how systems like:

- Google login
- banking apps
- SaaS platforms

protect accounts.

# 11. Security layers recap (full system now)

You have implemented:

### Identity security

- password rules
- hashing
- email validation (conceptually)

### Session security

- JWT access token
- refresh token system

### Attack protection

- rate limiting
- account lockout

### Access control

- roles (admin/user)
