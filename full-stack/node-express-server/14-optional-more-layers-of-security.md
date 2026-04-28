# 1. Refresh Tokens (long sessions)

## Problem it solves

JWT tokens expire quickly (for safety). That means:

- user gets logged out often
- bad UX if used alone

## Solution

We use **two tokens**:

- Access token (short-lived, e.g. 15 min)
- Refresh token (long-lived, e.g. 7–30 days)

## Flow

```id="rt1"
Login
 → Access Token (short)
 → Refresh Token (long)
```

When access token expires:

```id="rt2"
Client → sends refresh token → gets new access token
```

## Why it matters

- Better UX
- Still secure (short-lived access tokens)

# 2. Token Expiration + Re-login flow

## Problem it solves

Without expiry:

- stolen token = permanent access ❌

## Solution

JWT includes:

- expiry time (`exp`)

Example:

- 15 minutes for access token

## Flow

```id="exp1"
Token expired → API rejects → user must re-login or refresh
```

## Why it matters

- limits damage if token is stolen

# 3. Rate Limiting (brute force protection)

## Problem it solves

Attackers try:

- thousands of passwords per second

## Solution

Limit requests:

Example:

- 100 requests per 15 minutes per IP

## Flow

```id="rl1"
Too many requests → block temporarily
```

## Why it matters

- stops brute-force login attacks

# 4. Account Lock after failed attempts

## Problem it solves

Even with rate limits:

- attackers try password guessing slowly

## Solution

Track failed logins:

Example:

- 5 failed attempts → lock account for 10 minutes

## Database idea

```sql id="lock1"
failed_attempts INT DEFAULT 0
lock_until DATETIME
```

## Flow

```id="lockflow"
Login fail → increment counter
5 failures → lock account
```

## Why it matters

- protects individual users (not just IP-based protection)

# 5. Two-Factor Authentication (OTP)

## Problem it solves

Even if password is stolen:

- attacker can still log in ❌

## Solution

Second verification step:

- email OTP
- SMS OTP
- authenticator app

## Flow

```id="2fa1"
Login success →
  send OTP →
  verify OTP →
  issue token
```

## Why it matters

- adds second security layer
- used in banking, Google, GitHub

# 6. Email Verification System

## Problem it solves

Fake accounts:

- bots
- spam users
- fake emails

## Solution

User must verify email before activation:

## Flow

```id="email1"
Register →
  send verification link →
  user clicks link →
  account activated
```

## Database idea

```sql id="email2"
is_verified BOOLEAN DEFAULT false
verification_token VARCHAR(255)
```

## Why it matters

- ensures real users
- reduces spam

# 7. How all layers work together (REAL SYSTEM)

Here is the full architecture:

```id="fullsec"
REGISTER
 → password rules
 → hash password
 → email verification

LOGIN
 → check email/password
 → check lock status
 → check 2FA (optional)
 → issue access + refresh token

REQUEST
 → rate limiting check
 → access token verify
 → role check
 → execute request

TOKEN EXPIRE
 → refresh token used
 → new access token issued
```

# 8. Security layers map (what each protects)

| Layer              | Protects against     |
| ------------------ | -------------------- |
| Password rules     | weak passwords       |
| Hashing            | database leaks       |
| Rate limiting      | brute force attacks  |
| Account lock       | repeated guessing    |
| Token expiry       | stolen tokens        |
| Refresh tokens     | UX + secure sessions |
| 2FA                | stolen passwords     |
| Email verification | fake users           |

# 9. Important truth (real-world mindset)

No single security feature is enough.

> Security = multiple weak layers combined into strong defense

# 10. What you should do next (recommended order)

We should NOT implement all at once.

Best learning path:

### Phase 1 (next step)

Refresh tokens + token expiry

### Phase 2

Rate limiting + account lock

### Phase 3

Email verification system

### Phase 4

Two-factor authentication (OTP)
