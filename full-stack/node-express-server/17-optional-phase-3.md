Good, now we finish the authentication system with something every real production app requires:

> Email verification + OTP system

This is what separates “login systems” from **real user platforms**.

# 1. What problem we are solving

Right now your system allows:

- anyone to register with any email ❌
- fake accounts
- bots
- unverified users logging in immediately

We fix this with:

> “User must verify ownership of email before access”

# 2. What we are building

We will add:

## Step 1: Email verification (signup confirmation link)

## Step 2: OTP-based verification (optional upgrade layer)

We’ll implement email verification first (core industry standard), then OTP concept.

# 3. Database changes (MySQL)

We add verification fields:

```sql id="email-db"
ALTER TABLE users
ADD is_verified BOOLEAN DEFAULT FALSE,
ADD verification_token VARCHAR(255),
ADD otp_code VARCHAR(10),
ADD otp_expires DATETIME;
```

# 4. Email verification flow (REAL WORLD)

```text id="email-flow"
REGISTER
  ↓
Generate verification token
  ↓
Store in DB
  ↓
Send email link
  ↓
User clicks link
  ↓
Verify account
  ↓
Enable login
```

# 5. Install email tool (simple version)

We simulate email sending (for learning). In real apps you use:

- SMTP
- SendGrid
- AWS SES

For now we keep it simple.

# 6. Token generator service

## services/verificationService.js

```js id="verify1"
import crypto from "crypto";

/*
GENERATE VERIFICATION TOKEN
*/
export const generateVerificationToken = () => {
  return crypto.randomBytes(32).toString("hex");
};
```

# 7. Update user model

## models/userModel.js

```js id="verify-model"
import db from "../config/db.js";

/*
SAVE VERIFICATION TOKEN
*/
export const saveVerificationToken = (email, token) => {
  const sql = `
    UPDATE users 
    SET verification_token = ? 
    WHERE email = ?
  `;

  db.query(sql, [token, email]);
};

/*
VERIFY USER
*/
export const verifyUser = (token, callback) => {
  const sql = `
    UPDATE users 
    SET is_verified = true, verification_token = NULL 
    WHERE verification_token = ?
  `;

  db.query(sql, [token], callback);
};
```

# 8. Update REGISTER flow

## controllers/authController.js (REGISTER UPDATED)

```js id="register-email"
import { createUser, saveVerificationToken } from "../models/userModel.js";

import { generateVerificationToken } from "../services/verificationService.js";

import { hashPassword, validatePassword } from "../services/passwordService.js";

export const register = async (req, res) => {
  const { name, email, password } = req.body;

  const error = validatePassword(password);
  if (error) return res.status(400).json({ error });

  const hashed = await hashPassword(password);

  const token = generateVerificationToken();

  createUser(name, email, hashed, (err) => {
    if (err) return res.status(500).json(err);

    saveVerificationToken(email, token);

    // SIMULATED EMAIL (replace with real email service in production)
    console.log(
      `Verify your email: http://localhost:3000/auth/verify/${token}`,
    );

    res.json({
      message: "User created. Please verify email.",
    });
  });
};
```

# 9. Email verification endpoint

## controllers/authController.js (NEW)

```js id="verify-controller"
import { verifyUser } from "../models/userModel.js";

export const verifyEmail = (req, res) => {
  const { token } = req.params;

  verifyUser(token, (err, result) => {
    if (err) return res.status(500).json(err);

    if (result.affectedRows === 0) {
      return res.status(400).json({ error: "Invalid token" });
    }

    res.json({ message: "Email verified successfully" });
  });
};
```

# 10. BLOCK LOGIN FOR UNVERIFIED USERS

## UPDATE LOGIN LOGIC

Add this check:

```js id="verify-login"
if (!user.is_verified) {
  return res.status(403).json({
    error: "Please verify your email before login",
  });
}
```

# 11. Routes update

## routes/authRoutes.js

```js id="verify-routes"
import express from "express";
import {
  register,
  login,
  refreshToken,
  verifyEmail,
} from "../controllers/authController.js";
import { loginLimiter } from "../middleware/rateLimiter.js";

const router = express.Router();

router.post("/register", register);
router.post("/login", loginLimiter, login);
router.post("/refresh", refreshToken);

// EMAIL VERIFICATION ROUTE
router.get("/verify/:token", verifyEmail);

export default router;
```

# 12. OTP SYSTEM (ADVANCED LAYER)

Now we extend security further with OTP.

## OTP flow

```text id="otp-flow"
LOGIN
  ↓
Generate OTP (6-digit)
  ↓
Store in DB with expiry
  ↓
Send OTP (email/sms)
  ↓
User enters OTP
  ↓
Verify → login success
```

## OTP generator

```js id="otp-gen"
export const generateOTP = () => {
  return Math.floor(100000 + Math.random() * 900000).toString();
};
```

## Store OTP in DB

Already added:

- otp_code
- otp_expires

## OTP logic (simplified)

```js id="otp-store"
const otp = generateOTP();
const expires = new Date(Date.now() + 5 * 60 * 1000); // 5 min
```

# 13. Why email verification + OTP matters

## Email verification:

- ensures real users
- prevents fake accounts
- improves trust

## OTP:

- adds second authentication factor
- protects even if password is stolen

# 14. Full security system now

You now have:

## Identity layer

- strong password rules
- email verification

## Session layer

- JWT access token
- refresh token

## Protection layer

- rate limiting
- account lock

## Access control

- role-based authorization

## Verification layer

- email verification
- OTP (conceptually added)

# 15. Real-world mapping

This is now equivalent to:

- Google login system
- Banking authentication system
- SaaS platforms (Stripe, AWS, etc.)

# 16. What you’ve achieved (important)

You didn’t just “learn authentication”.

You built:

> A layered security architecture used in production backend systems
