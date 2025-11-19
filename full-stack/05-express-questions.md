
# Core Express.js Interview Questions and Answers

## 1. What is Express.js?

Express.js is a minimal and flexible Node.js web application framework that provides a robust set of features for building APIs and web applications. It simplifies server creation, routing, middleware handling, and HTTP utilities.

---

## 2. What is Middleware in Express.js?

Middleware functions are functions that sit in the request-response pipeline. They receive `req`, `res`, and a `next()` function.
They are used for:

* Logging
* Authentication
* Error handling
* Parsing bodies
* Modifying requests

Example:

```js
app.use((req, res, next) => {
  console.log("Request received");
  next();
});
```

---

## 3. What are the types of Middleware in Express?

1. Application-level middleware (`app.use`)
2. Router-level middleware (`router.use`)
3. Error-handling middleware (`(err, req, res, next) => {}`)
4. Built-in middleware (`express.json()`)
5. Third-party middleware (cors, morgan, helmet, etc.)

---

## 4. What is Routing in Express?

Routing defines how an application responds to client requests.

Example:

```js
app.get('/users', (req, res) => { res.send("Users list") });
```

Supports:

* HTTP methods (GET, POST, PUT, DELETE)
* Route parameters
* Query parameters
* Wildcard routes

---

## 5. What are Route Parameters?

Dynamic segments in a route.

Example:

```js
app.get('/user/:id', (req, res) => {
  console.log(req.params.id);
});
```

---

## 6. How do you handle errors in Express?

Use error-handling middleware.

```js
app.use((err, req, res, next) => {
  res.status(500).json({ message: err.message });
});
```

---

## 7. What is `next()` in Express?

`next()` passes control to the next middleware function.
`next(err)` triggers error-handling middleware.

---

## 8. How does Express handle JSON and URL-encoded data?

```js
app.use(express.json());
app.use(express.urlencoded({ extended: true }));
```

---

## 9. How to create modular routes using Express Router?

```js
const router = require("express").Router();

router.get("/products", (req, res) => {});
module.exports = router;

app.use("/api", router);
```

---

## 10. What is CORS and how to enable it in Express?

Cross-Origin Resource Sharing.
Enable using a package:

```js
const cors = require("cors");
app.use(cors());
```

---

## 11. What is Helmet in Express?

Express middleware for security headers.

```js
const helmet = require("helmet");
app.use(helmet());
```

---

## 12. How do you implement authentication in Express?

Common methods:

* JWT tokens
* Sessions and cookies
* OAuth

Example JWT:

```js
const jwt = require("jsonwebtoken");
const token = jwt.sign({ userId }, "secret", { expiresIn: "1h" });
```

---

## 13. What is the difference between `app.use()` and `app.get()`?

* `app.use()` registers middleware for all routes or subpaths.
* `app.get()` listens only to GET requests for a specific route.

---

## 14. How do you handle file uploads in Express?

Using `multer`:

```js
const upload = multer({ dest: "uploads/" });
app.post("/upload", upload.single("file"), (req, res) => {});
```

---

## 15. What is the Express.js request lifecycle?

Request → Route Matching → Middleware Stack → Controller → Response.

---

## 16. What are template engines in Express?

Used for server-side rendering:
Examples: EJS, Pug, Handlebars.

Setup:

```js
app.set("view engine", "ejs");
```

---

## 17. How do you secure an Express API?

Key methods:

* Rate limiting
* Validation (Joi, Zod, Yup)
* Sanitization
* HTTPS
* Helmet
* CORS configuration
* Proper error handling

---

## 18. What is the difference between `res.send`, `res.json`, and `res.end`?

* `res.send()` sends buffered data or string.
* `res.json()` sends JSON with correct header.
* `res.end()` ends the response without content.

---

## 19. How to handle async errors in Express?

Recommended:

```js
app.get("/data", async (req, res, next) => {
  try {
    const data = await fetchData();
    res.json(data);
  } catch (err) {
    next(err);
  }
});
```

Or use a wrapper:

```js
const asyncHandler = fn => (req, res, next) => {
  Promise.resolve(fn(req, res, next)).catch(next);
};
```

---

## 20. Explain the concept of environment variables in Express

Used for:

* Secrets
* Configuration
* DB URLs

Example:

```js
require("dotenv").config();
const port = process.env.PORT;
```
