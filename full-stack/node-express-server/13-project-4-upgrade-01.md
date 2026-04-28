Good—now we complete the missing piece of real-world auth systems: **Authorization (roles & permissions)**.

Authentication = “Who are you?”
Authorization = “What are you allowed to do?”

We already built authentication (JWT login). Now we add role-based control.

# 1. What we are adding (Project upgrade)

We will extend your system so that:

## Roles:

- user → normal user
- admin → full control

## Permissions example:

| Action           | user | admin |
| ---------------- | ---- | ----- |
| Login            | Yes  | Yes   |
| Create account   | Yes  | Yes   |
| View own data    | Yes  | Yes   |
| Delete users     | No   | Yes   |
| Manage all posts | No   | Yes   |

# 2. Database update (MySQL)

We already had:

```sql id="role-db-1"
role VARCHAR(20) DEFAULT 'user'
```

So now every user has a role:

- user (default)
- admin (manually assigned)

# 3. Core idea of authorization layer

We will add:

## Middleware 1: Authentication

- verifies JWT
- gives req.user

## Middleware 2: Authorization

- checks role
- allows/blocks access

# 4. Middleware: Role-based access control

## middleware/authMiddleware.js (updated concept)

We already had auth middleware. Now we extend it.

## 4.1 Authentication middleware (unchanged core)

```js id="auth1"
import { verifyToken } from "../services/tokenService.js";

export const authMiddleware = (req, res, next) => {
  const token = req.headers.authorization;

  if (!token) {
    return res.status(401).json({ error: "No token provided" });
  }

  try {
    const decoded = verifyToken(token);
    req.user = decoded; // contains id + role
    next();
  } catch (err) {
    return res.status(401).json({ error: "Invalid token" });
  }
};
```

## 4.2 Authorization middleware (NEW)

```js id="auth2"
export const roleMiddleware = (allowedRoles) => {
  return (req, res, next) => {
    const userRole = req.user.role;

    if (!allowedRoles.includes(userRole)) {
      return res.status(403).json({
        error: "Access denied: insufficient permissions",
      });
    }

    next();
  };
};
```

# 5. How this works (important mental model)

### Step 1: Authentication

User logs in → gets token

### Step 2: Request with token

Client sends:

```http id="hdr1"
Authorization: token_here
```

### Step 3: authMiddleware

- verifies token
- attaches user to request

```js id="req1"
req.user = { id: 1, role: "admin" };
```

### Step 4: roleMiddleware

- checks role
- allows or blocks

# 6. Example usage in routes

We now protect routes.

## Example: Admin-only route

### routes/userRoutes.js

```js id="route1"
import express from "express";
import {
  authMiddleware,
  roleMiddleware,
} from "../middleware/authMiddleware.js";

const router = express.Router();

/*
DELETE USER (ADMIN ONLY)
*/
router.delete("/:id", authMiddleware, roleMiddleware(["admin"]), (req, res) => {
  res.json({ message: "User deleted by admin" });
});

export default router;
```

## Example: User + Admin access

```js id="route2"
router.get(
  "/profile",
  authMiddleware,
  roleMiddleware(["user", "admin"]),
  (req, res) => {
    res.json({
      message: "Profile data",
      user: req.user,
    });
  },
);
```

# 7. Real-world security flow

Now every request follows:

```id="flow-authz"
Client
  ↓
JWT Token
  ↓
authMiddleware (authentication)
  ↓
req.user created
  ↓
roleMiddleware (authorization)
  ↓
Controller logic
  ↓
Database
```

# 8. Why this is powerful (real-world thinking)

Without authorization:

- anyone logged in can do anything ❌

With authorization:

- controlled access ✅
- safer system
- enterprise-level design

# 9. Security layers recap (full system now)

You now built ALL core layers:

## Layer 1: Authentication

- email + password login

## Layer 2: Password rules

- strong password enforcement

## Layer 3: Hashing

- bcrypt secure storage

## Layer 4: Token system

- JWT authentication

## Layer 5: Authorization (NEW)

- role-based access control

# 10. What this means in real apps

This is exactly how systems like:

- Google
- Amazon
- banking apps
- SaaS platforms

control access.

# 11. Important mental model

Always remember:

> Authentication = identity
> Authorization = permission

# 12. Next upgrades (if you want to go further)

Now your system is already strong. Next real-world layers are:

1. Refresh tokens (long sessions)
2. Token expiration + re-login flow
3. Rate limiting (brute-force protection)
4. Account lock after failed attempts
5. Two-factor authentication (OTP)
6. Email verification system