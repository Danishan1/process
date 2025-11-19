# Backend Development – Node.js Interview Questions

**Includes: Core Node.js, Asynchronous Programming, APIs, Architecture, Databases, Security, DevOps basics, and Best Practices**

---

## 1. Core Node.js Fundamentals

### 1.1 What is Node.js and why is it used?

Node.js is a runtime environment built on the V8 JavaScript engine that allows JavaScript to run outside the browser. It is used for building scalable network applications due to its event-driven, non-blocking I/O model.

### 1.2 Explain the Event Loop.

Event Loop is the heart of Node.js that handles asynchronous operations. It continuously checks for pending callbacks, timers, and I/O events, executing tasks without blocking the main thread.

### 1.3 What is the Call Stack, Callback Queue, and Event Loop relationship?

* **Call Stack**: Executes synchronous code.
* **Callback Queue**: Stores callbacks waiting to be executed.
* **Event Loop**: Moves tasks from the queue to the stack when the stack is free.

### 1.4 What is Non-Blocking I/O?

Node.js uses asynchronous system calls, allowing other operations to proceed without pausing execution.

### 1.5 What are CommonJS and ES Modules?

* **CommonJS**: `require()` and `module.exports`
* **ESM**: `import` and `export`
  ESM supports static analysis and tree shaking.

---

## 2. Asynchronous Patterns

### 2.1 Callback Hell

Callback nested structures that become hard to read and maintain.

### 2.2 Promises

An object representing eventual completion or failure of an async task.

### 2.3 async/await

Syntactic sugar over Promises, making asynchronous code appear synchronous.

### 2.4 What is the difference between microtasks and macrotasks?

* **Microtasks**: Promise callbacks, queueMicrotask
* **Macrotasks**: setTimeout, setInterval, setImmediate, I/O callbacks
  Microtasks have higher priority.

---

## 3. Node.js Modules & Packages

### 3.1 What is npm?

Node package manager used to install, manage, and publish packages.

### 3.2 What is npx?

Runs Node binaries without installing them globally.

### 3.3 package.json vs package-lock.json

* **package.json**: Project metadata and dependency ranges.
* **package-lock.json**: Exact versions for reproducible builds.

---

## 4. Node.js Architecture

### 4.1 Explain the Single-Threaded Model.

The main JS thread is single but uses workers (libuv threadpool) for CPU-intensive tasks and I/O.

### 4.2 When Node.js is NOT good?

* CPU-heavy operations
* Machine learning
* Video encoding
* Big data processing

### 4.3 What is Cluster Module?

Allows Node.js to spawn multiple processes to utilize multi-core CPUs.

### 4.4 Worker Threads?

Used for CPU-intensive tasks; different from clustering.

---

## 5. API Development With Express / Fastify

### 5.1 What is Express.js?

Minimal web framework for building APIs and web servers.

### 5.2 What are Middlewares?

Functions that process requests sequentially: auth, logging, parsing, validation.

### 5.3 What is the difference between express.Router() and app.use()?

* **Router** for modular route grouping
* **app.use** for registering routes/middlewares globally

### 5.4 HTTP Methods and Status Codes

Explain GET, POST, PUT, PATCH, DELETE
Know 200, 201, 204, 400, 401, 403, 404, 409, 500.

### 5.5 Error Handling

Use centralized error middleware.

---

## 6. Database Integration

### 6.1 SQL vs NoSQL

* SQL: Structured, ACID
* NoSQL: Flexible schema, distributed

### 6.2 ORM/ODM:

* Sequelize
* Prisma
* TypeORM
* Mongoose

### 6.3 Database Connection Pooling

Reusing DB connections to improve performance.

### 6.4 Transactions

Used for atomic operations.

---

## 7. Authentication & Security

### 7.1 JWT (JSON Web Tokens)

Used for stateless authentication.

### 7.2 Bcrypt / Argon2

Hashing passwords.

### 7.3 OWASP Top 10 Issues

* Injection
* Broken auth
* Sensitive data exposure
* CSRF
* XSS

### 7.4 Helmet in Express

Sets secure HTTP headers.

### 7.5 Rate Limiting

Prevents brute force and abuse.

---

## 8. Performance & Scalability

### 8.1 Caching

* Redis
* In-memory caching
* CDN caching

### 8.2 Horizontal vs Vertical Scaling

### 8.3 Streaming in Node.js

Readable and writable streams for large files.

### 8.4 Load Balancing

Using Nginx, HAProxy, AWS ELB.

---

## 9. Testing & Quality

### 9.1 Jest / Mocha / Chai

Unit and integration testing.

### 9.2 Supertest

Testing HTTP endpoints.

### 9.3 Mocking / Stubbing

Simulating modules or DB calls.

---

## 10. DevOps & Deployment Basics

### 10.1 PM2

Process manager for Node apps.

### 10.2 Docker basics

Create containers for reproducible environments.

### 10.3 CI/CD

GitHub Actions, GitLab CI, Jenkins.

### 10.4 Environment Variables

Using dotenv.
