
# 🧠 Express.js Revision Map

```text
                    EXPRESS
                       │
          ┌────────────┼────────────┐
          ↓            ↓            ↓
        App          Router      Middleware
          │            │            │
          ↓            ↓            ↓
       Routes       Params       Auth
          │            │         Validation
          ↓            ↓         Logging
      Handlers      Query        CORS
          │         Body         etc.
          ↓
      Controller
          ↓
       Service
          ↓
     Repository
          ↓
       Database
```

## 🔥 Must-Know Express Concepts

Prioritize these:

```text
1. Express vs Node.js
2. Request / Response
3. Routing
4. HTTP methods
5. req.params
6. req.query
7. req.body
8. req.headers
9. Middleware
10. next()
11. Middleware ordering
12. Router
13. Error-handling middleware
14. Async error handling
15. HTTP status codes
16. REST APIs
17. Authentication
18. Authorization
19. CORS
20. Cookies
21. Validation
22. Streams / request-response flow
23. Controller-Service architecture
24. Security / rate limiting
```

### 🔥 The core mental model

```text
                 CLIENT
                    │
                    ↓
              HTTP REQUEST
                    │
                    ↓
                EXPRESS
                    │
             ┌──────┴──────┐
             ↓             ↓
        Middleware       Router
             │             │
       ┌─────┼─────┐       ↓
       ↓     ↓     ↓    Route
      Auth  Valid. Log     │
                           ↓
                       Controller
                           │
                           ↓
                        Service
                           │
                           ↓
                       Database
                           │
                           ↓
                       Response
                           │
                           ↓
                         CLIENT
```

If you understand this pipeline deeply, most Express concepts stop feeling like isolated APIs and start looking like **different pieces of one request-processing system**.


# 🚀 Express.js — Complete Revision

The right mental model is:

> **Express is a lightweight web framework built on top of Node.js's HTTP capabilities.**

Think:

```text
Client
  ↓
HTTP Request
  ↓
Node.js
  ↓
Express
  ↓
Middleware
  ↓
Route
  ↓
Controller / Handler
  ↓
Response
```

## 1. What is Express?

Without Express, you can create a server with Node:

```ts
import http from "node:http";

http.createServer((req, res) => {
  res.end("Hello");
});
```

Express simplifies routing, middleware, request handling, error handling, and application structure.

```ts
import express from "express";

const app = express();

app.get("/", (req, res) => {
  res.send("Hello");
});

app.listen(3000);
```

So:

```text
Node.js
→ runtime

Express
→ web framework running on Node.js
```

## 2. Express Application

The core object:

```ts
const app = express();
```

`app` represents your Express application.

You use it to configure:

```text
routes
middleware
error handlers
settings
server behavior
```

## 3. HTTP Methods

Express maps HTTP methods to route handlers:

```ts
app.get("/", handler);

app.post("/users", handler);

app.put("/users/:id", handler);

app.patch("/users/:id", handler);

app.delete("/users/:id", handler);
```

Common meanings:

| Method   | Typical purpose  |
| -------- | ---------------- |
| `GET`    | Retrieve         |
| `POST`   | Create / submit  |
| `PUT`    | Replace          |
| `PATCH`  | Partially update |
| `DELETE` | Delete           |

## 4. Routing

A route consists conceptually of:

```text
HTTP method
+
path
+
handler
```

Example:

```ts
app.get("/users", (req, res) => {
  res.json([]);
});
```

Means:

```text
GET /users
     ↓
handler
```

## 5. Route Parameters

```ts
app.get("/users/:id", (req, res) => {
  console.log(req.params.id);
});
```

Request:

```text
GET /users/123
```

Then:

```ts
req.params.id
```

is:

```text
"123"
```

Important:

> Route parameters arrive as strings unless you explicitly convert/validate them.

## 6. Multiple Route Parameters

```ts
app.get("/users/:userId/posts/:postId", (req, res) => {
  console.log(req.params.userId);
  console.log(req.params.postId);
});
```

Request:

```text
/users/10/posts/50
```

gives:

```ts
req.params
// {
//   userId: "10",
//   postId: "50"
// }
```

## 7. Query Parameters

Request:

```text
GET /users?page=2&limit=10
```

Access:

```ts
req.query.page;
req.query.limit;
```

So:

```text
/users/:id
        ↑
     params

/users?page=2
        ↑
      query
```

#### Key distinction

| Type            | Example              | Access        |
| --------------- | -------------------- | ------------- |
| Route parameter | `/users/123`         | `req.params`  |
| Query parameter | `/users?page=2`      | `req.query`   |
| Request body    | JSON payload         | `req.body`    |
| Header          | `Authorization: ...` | `req.headers` |

## 8. Request Body

Suppose client sends:

```json
{
  "name": "John",
  "age": 25
}
```

You need body-parsing middleware:

```ts
app.use(express.json());
```

Then:

```ts
app.post("/users", (req, res) => {
  console.log(req.body);
});
```

Without the appropriate parser, `req.body` may not contain the parsed JSON object you expect.

## 9. Middleware ⭐⭐⭐⭐⭐

This is probably the **most important Express concept**.

Middleware is a function that participates in processing a request.

Basic shape:

```ts
(req, res, next) => {
  // do something

  next();
}
```

Think:

```text
Request
   ↓
Middleware 1
   ↓
Middleware 2
   ↓
Middleware 3
   ↓
Route Handler
   ↓
Response
```

## 10. Why `next()` Matters

Consider:

```ts
app.use((req, res, next) => {
  console.log("Middleware");
  next();
});
```

`next()` says, roughly:

> Continue processing the middleware/route chain.

Without `next()` and without sending a response:

```ts
app.use((req, res, next) => {
  console.log("Hello");
});
```

the request may remain hanging.

## 11. Middleware Can End the Request

Middleware doesn't always need to call `next()`.

```ts
app.use((req, res) => {
  res.status(401).json({
    message: "Unauthorized"
  });
});
```

It can terminate the request-response cycle.

So middleware can either:

```text
continue
   ↓
next()
```

or:

```text
send response
   ↓
stop
```

## 12. Application-Level Middleware

```ts
app.use((req, res, next) => {
  console.log(req.method);
  next();
});
```

Runs broadly for requests that match its mounting path.

You can also scope it:

```ts
app.use("/api", middleware);
```

Now it applies to routes under `/api`.

## 13. Router-Level Middleware

Express provides:

```ts
const router = express.Router();
```

Then:

```ts
router.use(authMiddleware);

router.get("/profile", getProfile);
router.get("/settings", getSettings);

app.use("/api", router);
```

Conceptually:

```text
/api
 ↓
router
 ↓
auth middleware
 ↓
route
```

This helps organize larger applications.

## 14. Router

Instead of:

```ts
app.get("/users", ...);
app.post("/users", ...);
app.get("/users/:id", ...);
```

you can organize:

```ts
const router = express.Router();

router.get("/", getUsers);
router.post("/", createUser);
router.get("/:id", getUser);

app.use("/users", router);
```

Now:

```text
GET  /users
POST /users
GET  /users/:id
```

## 15. Middleware Ordering ⭐⭐⭐⭐⭐

This is extremely important.

Express processes middleware in order.

```ts
app.use(auth);
app.use(logger);

app.get("/users", handler);
```

roughly:

```text
Request
 ↓
auth
 ↓
logger
 ↓
handler
```

If you reverse them:

```ts
app.use(logger);
app.use(auth);
```

the execution order changes.

Therefore:

> **Middleware order matters.**

## 16. Authentication Middleware

Typical pattern:

```ts
function authenticate(req, res, next) {
  const token = req.headers.authorization;

  if (!token) {
    return res.status(401).json({
      message: "Unauthorized"
    });
  }

  next();
}
```

Then:

```ts
app.get("/profile", authenticate, (req, res) => {
  res.json({
    message: "Private profile"
  });
});
```

Pipeline:

```text
Request
 ↓
authenticate
 ↓
valid?
 ├── no  → 401
 └── yes → next()
             ↓
          handler
```

## 17. Response Object

Express gives you:

```ts
res
```

Common methods:

```ts
res.send()
res.json()
res.status()
res.end()
res.redirect()
res.sendFile()
```

Example:

```ts
res.status(201).json({
  id: 123,
  name: "John"
});
```

## 18. `res.status()` Does Not Send the Response

This:

```ts
res.status(404);
```

sets the status.

You typically then send something:

```ts
res.status(404).json({
  message: "Not found"
});
```

Think:

```text
status()
   ↓
configure response
   ↓
json()/send()/end()
   ↓
send response
```

## 19. `res.json()`

```ts
res.json({
  name: "John",
  age: 25
});
```

Express serializes the object as JSON and sends an appropriate response.

Very common for APIs.

## 20. Status Codes

Know these:

| Status | Meaning               |
| -----: | --------------------- |
|  `200` | OK                    |
|  `201` | Created               |
|  `204` | No Content            |
|  `400` | Bad Request           |
|  `401` | Unauthenticated       |
|  `403` | Forbidden             |
|  `404` | Not Found             |
|  `409` | Conflict              |
|  `422` | Unprocessable Content |
|  `500` | Internal Server Error |

Important distinction:

```text
401
→ authentication missing/invalid

403
→ authenticated but not allowed
```

## 21. Error Handling ⭐⭐⭐⭐⭐

Express has special error-handling middleware:

```ts
app.use((err, req, res, next) => {
  console.error(err);

  res.status(500).json({
    message: "Internal server error"
  });
});
```

Notice the **four parameters**:

```ts
(err, req, res, next)
```

That signature tells Express:

> This is an error-handling middleware.

## 22. Passing Errors

Middleware can pass an error:

```ts
next(new Error("Something went wrong"));
```

Then Express moves toward error handling.

```text
Middleware
   ↓
next(error)
   ↓
Error middleware
   ↓
Response
```

## 23. Async Error Handling

Suppose:

```ts
app.get("/users", async (req, res) => {
  const users = await getUsers();

  res.json(users);
});
```

If `getUsers()` rejects, how that rejection reaches Express's error handler depends on the Express version and how the handler is structured.

For modern Express versions, rejected promises from route handlers are propagated to error handling.

For older Express setups, developers commonly used wrappers such as:

```ts
const asyncHandler =
  (fn) => (req, res, next) => {
    Promise.resolve(fn(req, res, next))
      .catch(next);
  };
```

The important conceptual point:

> **Async errors must eventually reach Express's error pipeline.**

## 24. 404 Handling

A 404 is not necessarily an application exception.

A common final middleware is:

```ts
app.use((req, res) => {
  res.status(404).json({
    message: "Route not found"
  });
});
```

Put it **after your routes**.

Why?

Because Express processes things in order.

```text
Routes
  ↓
No route matched
  ↓
404 middleware
```

## 25. 404 vs 500

These are conceptually different:

```text
404
→ requested resource/route wasn't found

500
→ server encountered an unexpected problem
```

Don't turn every missing route into a 500.

## 26. Express Request Lifecycle ⭐⭐⭐⭐⭐

A typical API:

```text
Client
  ↓
HTTP Request
  ↓
Express
  ↓
Global Middleware
  ↓
Authentication
  ↓
Validation
  ↓
Router
  ↓
Controller
  ↓
Service
  ↓
Database
  ↓
Service
  ↓
Controller
  ↓
Response
  ↓
Client
```

This is the architecture you should keep in your head.

## 27. Controller vs Service

A clean backend often separates responsibilities.

#### Controller

Deals with HTTP:

```ts
async function getUser(req, res, next) {
  try {
    const user = await userService.getUser(req.params.id);

    res.json(user);
  } catch (error) {
    next(error);
  }
}
```

#### Service

Deals with business logic:

```ts
async function getUser(id: string) {
  return userRepository.findById(id);
}
```

Conceptually:

```text
HTTP concern
    ↓
Controller
    ↓
Business concern
    ↓
Service
    ↓
Data concern
    ↓
Repository / DB
```

## 28. Validation

Never blindly trust:

```ts
req.body
req.params
req.query
```

Example:

```ts
const age = Number(req.body.age);

if (!Number.isInteger(age) || age < 0) {
  return res.status(400).json({
    message: "Invalid age"
  });
}
```

In real applications, schema validation libraries are commonly used.

The principle is:

> **External input is untrusted input.**

## 29. Authentication vs Authorization

Very important.

#### Authentication

> Who are you?

```text
Login
 ↓
identity
```

#### Authorization

> What are you allowed to do?

```text
User
 ↓
permissions
 ↓
can/cannot perform action
```

Example:

```text
Authentication
→ valid JWT

Authorization
→ is this user an admin?
```

## 30. CORS

CORS = **Cross-Origin Resource Sharing**.

Suppose:

```text
Frontend:
http://localhost:3000

API:
http://localhost:5000
```

Different origins.

The browser applies cross-origin restrictions.

Express applications commonly configure CORS through middleware.

Conceptually:

```text
Browser
  ↓
cross-origin request
  ↓
CORS policy
  ↓
allowed?
```

Important:

> CORS is primarily a **browser security mechanism**. It isn't an authentication system.

## 31. Cookies

Express can work with cookies.

A cookie might contain:

```text
session identifier
preferences
other small pieces of state
```

Important security attributes include:

```text
HttpOnly
Secure
SameSite
```

For example:

```text
HttpOnly
→ JavaScript cannot directly read the cookie

Secure
→ send over HTTPS

SameSite
→ controls cross-site cookie behavior
```

## 32. JWT

Express itself does not require JWT.

JWT is one possible authentication mechanism.

Typical flow:

```text
Login
 ↓
Server verifies credentials
 ↓
Token generated
 ↓
Client sends token
 ↓
Authentication middleware verifies token
 ↓
req.user
 ↓
route handler
```

Don't confuse:

```text
Express
→ framework

JWT
→ token format/mechanism
```

## 33. Security Middleware

A production Express application commonly needs protections around:

```text
input validation
headers
CORS
authentication
authorization
rate limiting
cookies
CSRF where applicable
request size limits
logging
```

Express gives you the framework; you compose the security controls around it.

## 34. Request Body Size

You should avoid blindly accepting enormous request bodies.

For example:

```ts
app.use(express.json({
  limit: "1mb"
}));
```

This helps reduce certain denial-of-service/resource-exhaustion risks.

## 35. REST API Structure

A typical Express REST API:

```text
GET    /users
GET    /users/:id
POST   /users
PATCH  /users/:id
DELETE /users/:id
```

Think resource-oriented:

```text
/users
```

represents the collection.

```text
/users/123
```

represents one user.

## 36. Middleware Composition

Express becomes powerful because you can compose middleware:

```ts
app.post(
  "/users",
  authenticate,
  validateCreateUser,
  createUser
);
```

Pipeline:

```text
Request
   ↓
authenticate
   ↓
validate
   ↓
createUser
   ↓
Response
```

This is essentially **function composition around HTTP requests**.

And this connects directly to the function concepts we revised earlier.

## 37. Express + Everything We've Learned

Remember our previous topics?

#### Functions

Middleware is a function:

```ts
(req, res, next) => {}
```

#### Callbacks

Route handlers are functions passed to Express:

```ts
app.get("/", handler);
```

#### Promises

Handlers can be asynchronous:

```ts
app.get("/", async (req, res) => {});
```

#### `this`

Usually less central in modern Express code because handlers are commonly standalone functions rather than methods relying on dynamic `this`.

#### TypeScript

You can type:

```ts
(req: Request, res: Response, next: NextFunction)
```

#### Event loop

Express ultimately runs on Node's asynchronous runtime.

So:

```text
Functions
 ↓
Callbacks
 ↓
Promises
 ↓
async/await
 ↓
Node Event Loop
 ↓
Express Middleware
 ↓
HTTP API
```

That's the bigger picture.
