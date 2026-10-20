# 25. Testing & Code Quality

For SDE-II, testing isn't just:

> "How do I write a Jest test?"

The real question is:

> **How do I build confidence that a change is correct, doesn't break existing behavior, and can be diagnosed when something fails?**

A useful mental model:

```text
                    Code Change
                        │
             ┌──────────┴──────────┐
             │                     │
          Testing              Code Quality
             │                     │
     ┌───────┼────────┐       ┌────┼─────┐
     │       │        │       │    │     │
    Unit Integration E2E   Lint Format Static
     │       │        │       Analysis
     └───────┼────────┘
             │
       Confidence
             │
        Production
             │
        ┌────┴────┐
        │         │
     Debugging  Diagnosis
```

---

# 1. Unit Testing

A **unit test** tests a small piece of behavior in isolation.

Usually:

```text
function
class
service
utility
```

Example:

```js
function calculateTotal(price, tax) {
    return price + tax;
}
```

Test:

```js
test("calculates total", () => {
    expect(calculateTotal(100, 18)).toBe(118);
});
```

The basic structure is:

```text
Arrange
   ↓
Act
   ↓
Assert
```

```js
test("calculates total", () => {
    // Arrange
    const price = 100;
    const tax = 18;

    // Act
    const result = calculateTotal(price, tax);

    // Assert
    expect(result).toBe(118);
});
```

### What makes a good unit?

A good unit test is generally:

* fast
* deterministic
* isolated
* focused on one behavior
* easy to understand

---

# 2. Integration Testing

Integration testing asks:

> **Do multiple components work correctly together?**

For a backend:

```text
API
 ↓
Service
 ↓
Repository
 ↓
Database
```

A unit test might test:

```text
Service alone
```

An integration test might test:

```text
Service + PostgreSQL
```

or:

```text
API + middleware + database
```

Example:

```text
POST /users
      ↓
Express
      ↓
validation
      ↓
UserService
      ↓
PostgreSQL
```

The test verifies the complete interaction.

### Key distinction

```text
Unit
→ Is this component correct?

Integration
→ Do these components work correctly together?
```

---

# 3. End-to-End Testing

Even though E2E isn't explicitly in your list, you should understand where it fits.

E2E tests the system from the user's perspective.

For example:

```text
Browser
   ↓
Frontend
   ↓
API
   ↓
Auth
   ↓
Database
```

Example:

```text
User logs in
    ↓
JWT/session created
    ↓
Dashboard loads
    ↓
User creates task
    ↓
Task appears
```

So the testing spectrum is:

```text
          More isolated
                │
                ▼
             Unit
                │
          Integration
                │
               E2E
                │
                ▼
          More realistic
```

But as you move toward E2E:

* tests become slower
* setup becomes more expensive
* failures become harder to diagnose

Therefore, you don't want everything to be E2E.

---

# 4. Test Doubles

A **test double** is a replacement for a real dependency during testing.

Suppose:

```js
async function createOrder(order, paymentService) {
    await paymentService.charge(order.amount);
    // ...
}
```

Instead of calling the real payment provider:

```text
Your test
    ↓
Fake payment service
    ↓
returns controlled result
```

"Test double" is the umbrella term.

Common types:

```text
Test Doubles
├── Dummy
├── Stub
├── Mock
├── Spy
└── Fake
```

The terminology can vary somewhat between testing frameworks and teams, but the conceptual differences matter.

---

# 5. Stub

A **stub provides predetermined behavior**.

Suppose:

```js
async function getUser(id, repository) {
    return repository.findById(id);
}
```

We can stub:

```js
const repository = {
    findById: async () => ({
        id: 1,
        name: "Danish"
    })
};
```

Now the test doesn't need a real database.

```js
const user = await getUser(1, repository);

expect(user.name).toBe("Danish");
```

The key idea:

> **Stub = control what the dependency returns.**

---

# 6. Mock

A mock is generally used when we care about **how a dependency was interacted with**.

For example:

```text
Was paymentService.charge()
called?

How many times?

With what arguments?
```

Conceptually:

```js
expect(paymentService.charge)
    .toHaveBeenCalledWith(500);
```

So:

```text
Stub
→ "What should the dependency return?"

Mock
→ "Was the dependency used correctly?"
```

In real-world frameworks, "mock" is sometimes used more broadly to include stubbing and spying, so focus on the underlying behavior rather than terminology wars.

---

# 7. Spy

A spy wraps or tracks a function so you can inspect its calls.

For example:

```text
logger.info()
     ↓
spy
     ↓
"Was it called?"
"With what?"
```

Useful when you want to observe behavior without completely replacing it.

---

# 8. Fake

A fake is a working but simplified implementation.

Example:

Production:

```text
PostgreSQL
```

Test:

```text
In-memory repository
```

For example:

```js
class FakeUserRepository {
    users = [];

    async save(user) {
        this.users.push(user);
    }

    async findById(id) {
        return this.users.find(user => user.id === id);
    }
}
```

This behaves somewhat like the real repository but is much simpler.

---

# 9. Assertions

An assertion checks whether actual behavior matches expected behavior.

Examples:

```js
expect(result).toBe(10);
```

```js
expect(user).toEqual({
    id: 1,
    name: "Danish"
});
```

```js
expect(users).toHaveLength(3);
```

```js
expect(fn).toThrow();
```

Conceptually:

```text
Actual result
      │
      ▼
   Assertion
      │
 ┌────┴────┐
 │         │
Pass      Fail
```

### Important

A test without meaningful assertions may execute successfully while proving almost nothing.

---

# 10. Async Testing

This is extremely important for Node.js.

Suppose:

```js
async function getUser() {
    return await repository.findUser();
}
```

Your test must wait for the Promise.

```js
test("gets user", async () => {
    const user = await getUser();

    expect(user.id).toBe(1);
});
```

Or return the Promise:

```js
test("gets user", () => {
    return getUser().then(user => {
        expect(user.id).toBe(1);
    });
});
```

---

# 11. Testing Rejections

Don't accidentally write:

```js
test("rejects invalid user", async () => {
    getUser();
});
```

The test may finish before the Promise rejection is properly observed.

Instead:

```js
test("rejects invalid user", async () => {
    await expect(getUser()).rejects.toThrow();
});
```

Or:

```js
await expect(
    createUser(null)
).rejects.toThrow("Invalid user");
```

The general rule:

> **Your test must wait for the async operation you're testing.**

---

# 12. Async Testing and the Event Loop

This connects directly to the earlier async/event-loop revision.

When you write:

```js
await service.process();
```

the test isn't blocking the entire Node process.

The async function pauses until the Promise settles.

The test framework waits for the resulting Promise.

Therefore:

```text
Test
 ↓
async function
 ↓
Promise
 ↓
I/O
 ↓
Promise settles
 ↓
microtask continuation
 ↓
test assertion
```

Understanding this prevents many flaky async tests.

---

# 13. Test Isolation

Each test should ideally be independent.

Bad:

```text
Test A
creates user

Test B
expects user from Test A
```

Now:

```text
Test B
```

depends on:

```text
Test A
```

That's dangerous.

If A fails:

```text
A fails
 ↓
B fails
 ↓
C fails
```

You don't know which failure is the real problem.

Good:

```text
Test A → own setup
Test B → own setup
Test C → own setup
```

---

# 14. Database Test Isolation

This becomes especially important for backend systems.

Suppose:

```text
Test 1 → INSERT user
Test 2 → SELECT users
```

If Test 2 sees Test 1's data, your tests are coupled.

Possible strategies:

### Transaction rollback

```text
BEGIN
 ↓
test
 ↓
ROLLBACK
```

### Separate test database

```text
Production DB
     X

Test DB
     ✓
```

### Reset database state

```text
beforeEach
   ↓
clear/reset
   ↓
run test
```

### Containers

Run a temporary database instance for tests.

The correct choice depends on the test suite and infrastructure.

---

# 15. Mocking Database Calls

Suppose:

```js
async function getUser(id) {
    return db.query(
        "SELECT * FROM users WHERE id = ?",
        [id]
    );
}
```

A unit test might mock:

```text
db.query()
    ↓
controlled response
```

This makes the test:

* fast
* deterministic
* independent of DB availability

But there is a tradeoff.

If you mock everything:

```text
Application
    ↓
mock
    ↓
mock
    ↓
mock
    ↓
mock
```

you may end up testing your mocks instead of your actual system.

That's why you need integration tests too.

---

# 16. Unit vs Integration — Important Tradeoff

Imagine a payment service:

```text
OrderService
     ↓
PaymentService
     ↓
Stripe API
```

Unit test:

```text
OrderService
     ↓
mock PaymentService
```

Fast and isolated.

Integration test:

```text
OrderService
     ↓
Payment integration
```

Tests the actual interaction.

E2E:

```text
HTTP request
     ↓
entire application
     ↓
payment flow
```

Tests the real system behavior.

A healthy test strategy doesn't choose only one.

---

# 17. Code Coverage

Coverage tells you which parts of the code were executed by tests.

Common metrics:

```text
Line coverage
Branch coverage
Function coverage
Statement coverage
```

Example:

```js
if (user.isAdmin) {
    allowAccess();
} else {
    denyAccess();
}
```

If your tests only test:

```text
isAdmin = true
```

you might have high line coverage but incomplete branch coverage.

You haven't tested:

```text
isAdmin = false
```

### Important SDE-II point

> **100% coverage does not mean 100% correctness.**

You can execute every line and still have incorrect assertions.

Coverage is a signal, not proof of correctness.

---

# 18. Linting

Linting detects suspicious or undesirable code patterns.

Example:

```js
const x = 10;

console.log(x);
```

A linter may detect:

```text
unused variable
```

Other examples:

* unreachable code
* accidental globals
* inconsistent patterns
* problematic async usage
* questionable equality
* import issues

Common ecosystem tool:

```text
ESLint
```

Linting is primarily **static analysis of source code**.

---

# 19. Formatting

Formatting makes code structurally consistent.

Example:

```js
const user={name:"Danish",age:25};
```

becomes:

```js
const user = {
    name: "Danish",
    age: 25
};
```

A common formatter is:

```text
Prettier
```

The distinction:

```text
Formatter
→ How should code look?

Linter
→ Is this code pattern acceptable/problematic?
```

They overlap somewhat, but they solve different problems.

---

# 20. Static Analysis

Static analysis examines code **without executing it**.

Examples:

```text
TypeScript
ESLint
SonarQube
security scanners
```

It can detect:

```text
type errors
unused variables
unreachable code
bad patterns
potential security issues
complexity
dependency issues
```

Think:

```text
Source code
     ↓
Static analysis
     ↓
Potential problems
```

versus:

```text
Source code
     ↓
Execute
     ↓
Runtime behavior
```

---

# 21. TypeScript as Static Analysis

TypeScript provides a major layer of static checking.

Example:

```ts
function add(a: number, b: number) {
    return a + b;
}

add("10", 20);
```

TypeScript can identify the problem before runtime.

This is one reason TypeScript is valuable in larger codebases.

But:

> TypeScript types do not automatically validate external runtime data.

For example:

```ts
interface User {
    id: number;
}
```

doesn't magically validate:

```js
const response = await fetch(...);
```

The external data can still be malformed at runtime.

For APIs, runtime validation may be needed.

---

# 22. Debugging

Testing answers:

> "Does this behavior work?"

Debugging answers:

> **"Why doesn't it work?"**

For Node.js, common tools include:

```text
console.log()
debugger
Node inspector
IDE debugger
stack traces
logs
metrics
traces
```

But SDE-II debugging should be systematic rather than:

```text
console.log("here")
console.log("here2")
console.log("here3")
```

---

# 23. Stack Trace

Suppose:

```js
function a() {
    b();
}

function b() {
    c();
}

function c() {
    throw new Error("Something failed");
}
```

The stack trace tells you the chain:

```text
c()
 ↓
b()
 ↓
a()
 ↓
caller
```

Conceptually:

```text
Error
 ↓
current function
 ↓
caller
 ↓
caller
 ↓
request handler
```

A stack trace is one of your first debugging tools.

---

# 24. Error Diagnosis

Suppose production reports:

```text
API /orders is slow
```

Don't immediately assume:

```text
"Database is slow."
```

Break the problem down.

```text
Request
  │
  ├── middleware
  ├── authentication
  ├── business logic
  ├── Redis
  ├── database
  ├── external API
  └── response serialization
```

Measure each stage.

For example:

```text
Total: 1200 ms

Auth:       20 ms
Redis:      10 ms
Business:   30 ms
Postgres:  900 ms  ← suspicious
External:  200 ms
Response:   40 ms
```

Now you have evidence.

---

# 25. Error Diagnosis Framework

When something fails:

### Step 1 — Reproduce

Can you reproduce the issue?

```text
local
staging
production
```

### Step 2 — Identify scope

Is it:

```text
one user?
one endpoint?
all endpoints?
one server?
all servers?
```

### Step 3 — Check recent changes

```text
deployment
configuration
dependency update
database migration
infrastructure change
```

### Step 4 — Inspect logs

Look for:

```text
error
stack trace
request ID
timestamp
user/context
```

### Step 5 — Measure dependencies

```text
DB
Redis
Kafka
external APIs
filesystem
network
```

### Step 6 — Form a hypothesis

Example:

> "The latency started after deployment and DB query time increased, so the likely issue is the new query."

### Step 7 — Verify

Don't stop at correlation.

Measure it.

---

# 26. Testing + Logging + Observability

Testing and debugging shouldn't exist independently.

Production system:

```text
                    Application
                        │
          ┌─────────────┼─────────────┐
          │             │             │
        Tests          Logs        Metrics
          │             │             │
          │          Traces          │
          │             │             │
          └─────────────┼─────────────┘
                        │
                     Diagnosis
```

For a backend, structured logs are particularly useful.

Instead of:

```js
console.log("error");
```

prefer contextual information such as:

```text
requestId
userId
endpoint
operation
error
duration
```

while avoiding sensitive information.

---

# 27. Testing Pyramid

A useful mental model:

```text
                 /\
                /  \
               / E2E\
              /------\
             /        \
            /Integration\
           /------------\
          /              \
         /     Unit       \
        /__________________\
```

Generally:

```text
Many unit tests
Some integration tests
Fewer expensive E2E tests
```

Why?

Because unit tests are:

```text
fast + cheap + isolated
```

while E2E tests are:

```text
slow + expensive + fragile
```

But this isn't a rigid mathematical rule. Modern teams often use a broader "testing trophy" or other distributions depending on the application.

---

# 28. What Should You Mock?

A very important SDE-II judgment question.

Don't ask:

> "Should I mock everything?"

Ask:

> **"What behavior am I trying to verify?"**

For a unit test:

```text
Business logic
     ↓
mock external dependency
```

For an integration test:

```text
Business logic
     ↓
real repository
     ↓
real test DB
```

For an API integration test:

```text
HTTP
 ↓
Express
 ↓
Service
 ↓
DB
```

You might mock only an external payment provider.

The boundary you choose determines what confidence the test provides.

---

# 29. Common Testing Mistakes

### Mistake 1 — Testing implementation instead of behavior

Bad:

```text
"private function X was called exactly once"
```

when what really matters is:

```text
"order was successfully created"
```

Tests tightly coupled to implementation become painful during refactoring.

---

### Mistake 2 — Mocking everything

You can get:

```text
100% tests passing
```

while the actual database integration is broken.

---

### Mistake 3 — Shared state

```text
Test A changes global state
Test B depends on it
```

causes flaky tests.

---

### Mistake 4 — Async tests that don't await

```js
test("something", () => {
    asyncOperation();
});
```

Potentially finishes before the operation completes.

---

### Mistake 5 — Chasing coverage blindly

```text
"Need 100% coverage!"
```

can encourage meaningless tests.

Quality of assertions and coverage of important behavior matter more.

---

# 30. SDE-II Mental Model

For your level, I'd organize Point 25 like this:

```text
                  Testing & Quality
                         │
       ┌─────────────────┼─────────────────┐
       │                 │                 │
    Testing          Static Quality     Diagnosis
       │                 │                 │
 ┌─────┼─────┐      ┌────┼─────┐      ┌────┼─────┐
 │     │     │      │    │     │      │    │     │
Unit  Int.  E2E    Lint Format Types  Logs Debug Metrics
 │     │
 │     └── test doubles
 │          ├── stub
 │          ├── mock
 │          ├── spy
 │          └── fake
 │
 └── assertions
      │
      ├── async
      ├── isolation
      └── coverage
```

### The key distinctions to know cold

| Concept          | Core question                                   |
| ---------------- | ----------------------------------------------- |
| Unit test        | Is this component's behavior correct?           |
| Integration test | Do components work together?                    |
| E2E              | Does the complete user flow work?               |
| Stub             | What should the dependency return?              |
| Mock             | Was the dependency interacted with correctly?   |
| Spy              | What calls/interactions happened?               |
| Fake             | Can I use a simplified working implementation?  |
| Assertion        | What behavior must be true?                     |
| Coverage         | What code did tests execute?                    |
| Linting          | Is this code pattern problematic?               |
| Formatting       | Is the code consistently structured?            |
| Static analysis  | What problems can we detect without running it? |
| Debugging        | Why is the behavior wrong?                      |
| Diagnosis        | What is the actual root cause?                  |

---

# 31. What I Would Expect From an SDE-II

You don't need to memorize every Jest API.

You should be able to explain something like:

> "For business logic, I'd keep most tests at the unit level and mock external boundaries such as payment providers. For repository behavior, I'd add integration tests against an isolated test database. I'd make async tests explicitly await Promises and keep test state isolated. Coverage would help identify untested areas, but I wouldn't treat 100% coverage as proof of correctness. In CI, I'd run tests, linting, type checking and static analysis. For production failures, I'd use structured logs, request IDs, metrics and traces to narrow the failure before changing code."

**That is the SDE-II answer.**

The deeper connection with the previous chapters is also important:

```text
JavaScript
   ↓
Async behavior
   ↓
Event loop
   ↓
Node runtime
   ↓
Application code
   ↓
Testing
   ↓
Production diagnosis
```

So Point 25 isn't really a separate topic—it is where your understanding of JavaScript and Node becomes **engineering discipline**.
