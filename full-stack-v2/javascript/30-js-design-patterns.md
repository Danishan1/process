# 28. Design Patterns & JavaScript Architecture

This is where your JavaScript revision shifts from:

> **"Do I know how JavaScript works?"**

to:

> **"Can I design a maintainable backend system using JavaScript?"**

For an SDE-II, design patterns are **not about memorizing class diagrams**.

The real skill is recognizing recurring problems:

```text
Problem
   ↓
What changes frequently?
   ↓
What should remain stable?
   ↓
Where should responsibility live?
   ↓
How do components communicate?
   ↓
What should be replaceable/testable?
```

A useful architecture model:

```text
                    Application
                        │
        ┌───────────────┼────────────────┐
        │               │                │
    Business          Data           Infrastructure
     Logic            Access           Services
        │               │                │
        ▼               ▼                ▼
   Services        Repositories      Redis/Kafka/API
        │               │
        └───────────────┘
                │
             Database
```

---

# 1. Module Pattern

The Module pattern is about:

> **Encapsulating implementation details and exposing only what consumers need.**

Modern JavaScript modules already give you this naturally.

```js
// userService.js

const users = new Map();

function validateUser(user) {
    // internal implementation
}

function createUser(user) {
    validateUser(user);
    users.set(user.id, user);
}

export {
    createUser
};
```

Consumers see:

```js
import { createUser } from "./userService.js";
```

They don't need to know:

```text
validateUser()
users
internal state
```

This gives:

```text
Public API
    │
    ▼
Module
 ┌───────────────┐
 │ private logic │
 │ private data  │
 │ public API    │
 └───────────────┘
```

### SDE-II insight

Don't confuse:

```text
module
```

with:

```text
file
```

A module is an **encapsulation boundary**, not merely a `.js` file.

---

# 2. Factory Pattern

Factory means:

> **Centralize object creation instead of making consumers know exactly how an object is constructed.**

Suppose you have different notification types:

```js
class EmailNotification {
    send(message) {
        console.log("Email:", message);
    }
}

class SMSNotification {
    send(message) {
        console.log("SMS:", message);
    }
}
```

Without a factory:

```js
const notification = new EmailNotification();
```

With a factory:

```js
function createNotification(type) {
    if (type === "email") {
        return new EmailNotification();
    }

    if (type === "sms") {
        return new SMSNotification();
    }

    throw new Error("Unsupported notification type");
}
```

Usage:

```js
const notification = createNotification("email");

notification.send("Hello");
```

Now the caller knows:

```text
"I need a notification"
```

rather than:

```text
"I need to know which concrete class to instantiate."
```

---

# 3. Factory in Backend Systems

This becomes useful when implementations vary.

For example:

```text
PaymentService
   │
   ├── StripePayment
   ├── RazorpayPayment
   └── PayPalPayment
```

Factory:

```js
function createPaymentProvider(provider) {
    switch (provider) {
        case "stripe":
            return new StripePayment();

        case "razorpay":
            return new RazorpayPayment();

        case "paypal":
            return new PaypalPayment();

        default:
            throw new Error("Unsupported provider");
    }
}
```

The rest of the application doesn't need to know construction details.

---

# 4. Singleton

Singleton means:

> **Ensure one shared instance of something within a particular scope/lifecycle.**

Simple example:

```js
class Config {
    constructor() {
        this.environment = process.env.NODE_ENV;
    }
}

const config = new Config();

export default config;
```

Every module importing it receives the same module-level instance.

But here's an important Node.js distinction:

> You often don't need to manually implement a Singleton class.

Node's module caching already gives modules shared module state within a process.

For example:

```js
// config.js

const config = {
    port: process.env.PORT
};

export default config;
```

Multiple imports generally reference the same module instance within that process.

---

# 5. Singleton Is Often Overused

Developers sometimes make everything:

```text
Singleton
```

Examples:

```text
Database
Redis
Logger
Config
Cache
Service
Manager
```

The problem is hidden global state.

Singletons can make:

```text
testing
dependency replacement
parallel execution
reasoning
```

harder.

Instead of:

```js
Database.getInstance().query(...);
```

dependency injection can often be cleaner.

---

# 6. Observer Pattern

Observer means:

> **An object observes another object's state/events and gets notified when something changes.**

Conceptually:

```text
             Subject
                │
        ┌───────┼────────┐
        ▼       ▼        ▼
     Observer Observer Observer
```

Example:

```js
class EventEmitter {
    constructor() {
        this.listeners = [];
    }

    subscribe(listener) {
        this.listeners.push(listener);
    }

    emit(data) {
        for (const listener of this.listeners) {
            listener(data);
        }
    }
}
```

Usage:

```js
const emitter = new EventEmitter();

emitter.subscribe(data => {
    console.log("Listener 1", data);
});

emitter.subscribe(data => {
    console.log("Listener 2", data);
});

emitter.emit({ id: 1 });
```

This is essentially the conceptual foundation behind many event systems.

---

# 7. Observer vs EventEmitter

Node's `EventEmitter` is a practical implementation of event/listener behavior.

For example:

```js
emitter.on("userCreated", handler);
```

and:

```js
emitter.emit("userCreated", user);
```

The publisher doesn't need to know exactly what every listener does.

This creates loose coupling.

---

# 8. Pub/Sub

Pub/Sub takes the idea further.

Instead of:

```text
Publisher
   ↓
specific observers
```

you introduce a broker/channel:

```text
Publisher
    │
    ▼
   Topic
    │
 ┌──┴────┐
 ▼       ▼
Sub A   Sub B
```

For example:

```text
Order Service
      │
      ▼
order.created
      │
 ┌────┼─────────┐
 ▼    ▼         ▼
Email Billing Analytics
```

The order service doesn't directly call all three systems.

---

# 9. Observer vs Pub/Sub

Very useful distinction:

### Observer

Usually more direct:

```text
Subject
   ↓
Observers
```

Often within the same process.

### Pub/Sub

Usually mediated:

```text
Publisher
   ↓
Broker / Topic
   ↓
Subscribers
```

Can work across processes/services.

Examples:

```text
Observer:
Node EventEmitter

Pub/Sub:
Redis Pub/Sub
Kafka topics
message brokers
```

Kafka is more than just pub/sub, but the conceptual comparison is useful.

---

# 10. Strategy Pattern

Strategy means:

> **Define multiple interchangeable algorithms/behaviors behind a common interface.**

Suppose pricing changes based on customer type.

Without strategy:

```js
function calculatePrice(customerType, price) {
    if (customerType === "regular") {
        // ...
    }

    if (customerType === "premium") {
        // ...
    }

    if (customerType === "enterprise") {
        // ...
    }
}
```

This grows badly.

Instead:

```js
class RegularPricing {
    calculate(price) {
        return price;
    }
}

class PremiumPricing {
    calculate(price) {
        return price * 0.9;
    }
}

class EnterprisePricing {
    calculate(price) {
        return price * 0.8;
    }
}
```

Then:

```js
function calculateFinalPrice(strategy, price) {
    return strategy.calculate(price);
}
```

Usage:

```js
const strategy = new PremiumPricing();

calculateFinalPrice(strategy, 1000);
```

---

# 11. Strategy in Real Backend Systems

Very useful for:

```text
payment providers
authentication strategies
pricing algorithms
shipping calculations
notification providers
serialization formats
retry policies
storage providers
```

For example:

```text
PaymentService
      │
      ├── StripeStrategy
      ├── RazorpayStrategy
      └── PayPalStrategy
```

The business logic can remain stable while the strategy changes.

---

# 12. Adapter Pattern

Adapter means:

> **Make one interface look like another interface.**

Suppose your application expects:

```js
paymentProvider.charge(amount);
```

But Stripe provides:

```js
stripe.paymentIntents.create(...);
```

Create an adapter:

```js
class StripeAdapter {
    constructor(stripe) {
        this.stripe = stripe;
    }

    async charge(amount) {
        return this.stripe.paymentIntents.create({
            amount
        });
    }
}
```

Now your business logic only knows:

```js
await paymentProvider.charge(1000);
```

---

# 13. Adapter's Value

Without adapter:

```text
Business Logic
    │
    ├── Stripe API
    ├── Razorpay API
    ├── PayPal API
    └── ...
```

This creates coupling.

With adapters:

```text
                 Business Logic
                       │
                 Payment Interface
                       │
          ┌────────────┼────────────┐
          ▼            ▼            ▼
       Adapter      Adapter      Adapter
          │            │            │
       Stripe       Razorpay      PayPal
```

This is extremely useful in production systems because external APIs change.

---

# 14. Decorator Pattern

Decorator means:

> **Add behavior to an existing object/function without modifying its core implementation.**

Example:

```js
function withLogging(fn) {
    return async function (...args) {
        console.log("Calling function");

        const result = await fn(...args);

        console.log("Function completed");

        return result;
    };
}
```

Usage:

```js
const createUserWithLogging = withLogging(createUser);
```

Now:

```text
createUser
    +
logging
    ↓
createUserWithLogging
```

---

# 15. Decorator in Backend Development

This pattern is everywhere conceptually.

For example:

```text
Controller
    ↓
Authentication
    ↓
Authorization
    ↓
Rate limiting
    ↓
Logging
    ↓
Controller
```

Or:

```text
Database operation
    ↓
Retry
    ↓
Metrics
    ↓
Caching
```

Middleware often behaves similarly to decorators/wrappers.

---

# 16. Proxy

You already covered Proxy in Point 23.

Recall:

> **Proxy intercepts operations on an object.**

Example:

```js
const user = {
    name: "Danish"
};

const proxy = new Proxy(user, {
    get(target, property) {
        console.log("Reading:", property);

        return Reflect.get(target, property);
    }
});
```

Then:

```js
proxy.name;
```

can intercept the operation.

---

# 17. Decorator vs Proxy

They can look similar but conceptually differ.

### Decorator

```text
object/function
      ↓
wrap
      ↓
enhanced object/function
```

Adds behavior around something.

### Proxy

```text
code
 ↓
proxy
 ↓
intercepts operations
 ↓
target
```

Controls fundamental operations such as:

```text
get
set
has
delete
apply
construct
```

---

# 18. Dependency Injection

This is one of the most important architectural concepts.

Dependency Injection means:

> **A component receives the dependencies it needs instead of constructing them itself.**

Bad coupling:

```js
class UserService {
    constructor() {
        this.db = new PostgresDatabase();
    }
}
```

`UserService` now knows:

```text
which DB
how DB is created
```

Instead:

```js
class UserService {
    constructor(db) {
        this.db = db;
    }
}
```

Then:

```js
const db = new PostgresDatabase();

const userService = new UserService(db);
```

---

# 19. Why Dependency Injection Matters

Testing becomes much easier.

Production:

```js
const service = new UserService(realDatabase);
```

Test:

```js
const service = new UserService(fakeDatabase);
```

You don't need to modify `UserService`.

This gives:

```text
UserService
     │
     │ depends on
     ▼
 Database interface
     ▲
     │
 ┌───┴─────────────┐
 │                 │
Real DB          Fake DB
```

---

# 20. Composition

Composition means:

> **Build complex behavior by combining smaller components.**

Instead of creating a huge inheritance hierarchy:

```text
Animal
 ├── Mammal
 │    ├── Dog
 │    └── Cat
 └── Bird
```

you can compose capabilities:

```text
Dog
 ├── canMove
 ├── canEat
 └── canSleep
```

In JavaScript, functions are particularly useful for composition.

```js
function withLogging(service) {
    // ...
    return service;
}

function withCaching(service) {
    // ...
    return service;
}

function withMetrics(service) {
    // ...
    return service;
}
```

Then:

```js
const service =
    withMetrics(
        withCaching(
            withLogging(
                baseService
            )
        )
    );
```

---

# 21. Composition vs Inheritance

This is a classic design discussion.

Inheritance:

```text
Dog extends Animal
```

creates a strong structural relationship.

Composition:

```text
Dog has behavior X
Dog has behavior Y
```

is generally more flexible.

A useful rule:

> **Prefer composition when behavior can be assembled independently.**

Inheritance is still useful when the relationship genuinely represents a stable abstraction.

---

# 22. Event-Driven Design

This is particularly relevant to your Node.js/backend work.

Instead of:

```text
Order Service
   │
   ├── Email Service
   ├── Billing Service
   └── Analytics Service
```

directly calling everything:

```text
Order created
      ↓
publish event
      ↓
order.created
      ↓
┌─────┼──────┐
▼     ▼      ▼
Email Billing Analytics
```

The producer knows:

```text
"order.created happened"
```

rather than:

```text
"EmailService.send()"
"BillingService.process()"
"AnalyticsService.record()"
```

---

# 23. Benefits of Event-Driven Design

### Loose coupling

Services don't need direct knowledge of every consumer.

### Scalability

Consumers can scale independently.

### Extensibility

Adding:

```text
Fraud Detection
```

doesn't necessarily require modifying Order Service.

### Asynchronous processing

Expensive work can happen outside the request path.

---

# 24. Event-Driven Tradeoffs

It's not automatically better.

You introduce:

```text
eventual consistency
debugging complexity
message ordering concerns
duplicate events
retry logic
dead-letter handling
observability challenges
```

For example:

```text
Order created
      ↓
Kafka
      ↓
Billing
      ↓
failure
```

Now you need to think about:

```text
retry?
duplicate?
idempotency?
ordering?
DLQ?
```

SDE-II design means understanding these tradeoffs.

---

# 25. Layered Architecture

A common backend structure:

```text
                 API Layer
                     │
                     ▼
              Business Layer
                     │
                     ▼
              Data Access Layer
                     │
                     ▼
                 Database
```

For example:

```text
controllers/
services/
repositories/
models/
```

### Controller

Deals with:

```text
HTTP
request
response
status codes
```

### Service

Deals with:

```text
business rules
```

### Repository

Deals with:

```text
database operations
```

---

# 26. Why Layers?

Imagine this:

```js
app.post("/users", async (req, res) => {

    // validation

    // business logic

    // SQL query

    // Redis

    // email

    // response
});
```

It may work.

But eventually the controller becomes:

```text
500 lines
```

and impossible to reason about.

Layering separates responsibilities:

```text
HTTP concerns
      ↓
business concerns
      ↓
data concerns
```

---

# 27. Separation of Concerns

This is broader than layered architecture.

It means:

> **Different responsibilities should be separated so changes in one concern don't unnecessarily affect others.**

For example:

```text
Authentication
Authorization
Business logic
Persistence
Caching
Logging
Notification
```

shouldn't become one giant function.

---

# 28. Example of Poor Separation

```js
async function createOrder(req, res) {

    // validate JWT

    // validate input

    // calculate price

    // query database

    // update inventory

    // publish Kafka event

    // send email

    // log metrics

    // send HTTP response
}
```

Technically possible.

Architecturally problematic.

---

# 29. Better Separation

```text
Controller
    ↓
OrderService
    ↓
 ┌─────────────┬─────────────┐
 ▼             ▼             ▼
Repository   Inventory     EventBus
```

The controller becomes primarily:

```text
HTTP → service → HTTP
```

The service handles business orchestration.

The repository handles persistence.

---

# 30. SOLID in JavaScript

SOLID isn't a Java-specific concept.

It applies to JavaScript/TypeScript too.

---

## S — Single Responsibility Principle

> A component should have one reason to change.

Bad:

```text
UserService
 ├── user business logic
 ├── email sending
 ├── PDF generation
 ├── database connection
 └── logging
```

Better:

```text
UserService
EmailService
PdfService
UserRepository
Logger
```

Not:

> "One class can only have one function."

It's about **responsibilities and reasons for change**.

---

# 31. O — Open/Closed Principle

> Open for extension, closed for modification.

Suppose:

```js
function calculatePrice(type) {
    if (type === "regular") ...
    if (type === "premium") ...
    if (type === "enterprise") ...
}
```

Every new pricing model modifies the function.

Strategy can help:

```text
Pricing interface
      │
 ┌────┼───────┐
 ▼    ▼       ▼
Regular Premium Enterprise
```

New strategy:

```text
NewPricing
```

can be added without rewriting the core pricing workflow.

---

# 32. L — Liskov Substitution Principle

The idea:

> **If B is treated as a subtype of A, replacing A with B shouldn't break the expected behavior.**

Classic conceptual example:

```text
Storage
  │
  ├── S3Storage
  └── LocalStorage
```

If your application expects:

```js
storage.save(file);
```

both implementations should honor the contract.

If:

```js
S3Storage.save()
```

behaves fundamentally differently in a way consumers cannot handle, the abstraction is broken.

In TypeScript, interfaces can make these contracts clearer.

---

# 33. I — Interface Segregation Principle

> Don't force consumers to depend on methods they don't need.

Suppose:

```ts
interface UserSystem {
    createUser();
    deleteUser();
    sendEmail();
    generateReport();
    exportPDF();
}
```

A component needing only:

```text
createUser
```

shouldn't need to depend on everything else.

Prefer smaller contracts:

```ts
interface UserCreator {
    createUser();
}

interface UserDeleter {
    deleteUser();
}
```

JavaScript doesn't have TypeScript's compile-time interfaces, but the design principle still applies.

---

# 34. D — Dependency Inversion Principle

This is closely related to dependency injection.

> High-level business logic should depend on abstractions/contracts rather than concrete infrastructure.

Bad:

```text
OrderService
     ↓
PostgresDatabase
```

Better:

```text
OrderService
     ↓
OrderRepository contract
     ↑
 ┌───┴────────┐
Postgres     Mock
```

Now infrastructure can change without rewriting business logic.

---

# 35. Abstraction Boundaries

This is perhaps more important than memorizing SOLID.

An abstraction boundary answers:

> **What does this component know, and what should it not know?**

Example:

```text
OrderService
```

should know:

```text
saveOrder()
```

but perhaps shouldn't know:

```text
PostgreSQL connection pool
SQL syntax
database host
```

So:

```text
OrderService
      │
      ▼
OrderRepository
      │
      ▼
PostgreSQL
```

The repository is an abstraction boundary.

---

# 36. Coupling

Coupling means:

> **How strongly components depend on each other.**

High coupling:

```text
A ────────> B
│           │
│           ▼
└─────────> C
```

Changing B may require changes to A and C.

Low coupling:

```text
A
│
▼
Interface
▲
│
B
```

A doesn't care about B's implementation details.

---

# 37. Cohesion

Cohesion means:

> **How closely related the responsibilities inside a component are.**

High cohesion:

```text
UserRepository
 ├── findUser
 ├── createUser
 ├── updateUser
 └── deleteUser
```

These belong together.

Low cohesion:

```text
Utils
 ├── sendEmail
 ├── calculateTax
 ├── parseCSV
 ├── connectDatabase
 ├── resizeImage
 └── generateJWT
```

This becomes a "god utility."

---

# 38. Coupling vs Cohesion

Ideal architecture:

```text
High Cohesion
      +
Low Coupling
      ↓
Maintainable system
```

Think:

```text
Inside component
→ things should belong together

Between components
→ dependencies should be minimal and explicit
```

---

# 39. Putting Everything Together

Imagine you're designing a payment system.

You could have:

```text
                    PaymentController
                           │
                           ▼
                    PaymentService
                           │
             ┌─────────────┼─────────────┐
             ▼             ▼             ▼
        PaymentStrategy  Repository    EventBus
             │
       ┌─────┼─────┐
       ▼     ▼     ▼
    Stripe Razorpay PayPal
```

Now patterns naturally emerge:

```text
Module
→ encapsulate each component

Factory
→ create payment provider

Strategy
→ interchangeable payment behavior

Adapter
→ normalize provider APIs

Dependency Injection
→ provide provider/repository/event bus

Repository
→ persistence abstraction

Pub/Sub/Event-driven
→ payment events

Decorator
→ logging/metrics/retry

Layering
→ controller/service/repository

SOLID
→ guide boundaries

Composition
→ combine behavior
```

Notice something important:

> **We didn't start by asking "Which design pattern should I use?"**

We started with:

> **What problem are we solving?**

The pattern emerged from the problem.

---

# 40. Pattern Selection Cheat Sheet

| Problem                               | Useful pattern         |
| ------------------------------------- | ---------------------- |
| Hide implementation behind API        | Module                 |
| Centralize object creation            | Factory                |
| One shared instance                   | Singleton              |
| Notify multiple listeners             | Observer               |
| Communicate through events/topics     | Pub/Sub                |
| Swap algorithms/behaviors             | Strategy               |
| Normalize incompatible APIs           | Adapter                |
| Add behavior without changing core    | Decorator              |
| Intercept object operations           | Proxy                  |
| Supply dependencies externally        | Dependency Injection   |
| Build behavior from smaller parts     | Composition            |
| Decouple services through events      | Event-driven design    |
| Separate application responsibilities | Layered architecture   |
| Prevent responsibility mixing         | Separation of concerns |
| Guide maintainable dependencies       | SOLID                  |
| Define what a component knows         | Abstraction boundary   |
| Reduce dependency strength            | Low coupling           |
| Keep responsibilities focused         | High cohesion          |

---

# 41. The Most Important SDE-II Distinction

Don't say:

> "We use Factory because Factory is a design pattern."

Say:

> "Object creation varies based on provider, so I want to isolate that decision from business logic. A factory gives us a single creation boundary."

Don't say:

> "We use Strategy."

Say:

> "These algorithms change independently, so I'll make them interchangeable behind a common contract."

Don't say:

> "We use Adapter."

Say:

> "The external providers expose different APIs, so I'll normalize them behind an internal interface."

That demonstrates **engineering reasoning**, rather than pattern memorization.

---

# 42. Your Architecture Mental Model

For SDE-II, I'd keep this model in your head:

```text
                    REQUIREMENT
                         │
                         ▼
                 Identify responsibilities
                         │
                         ▼
                 Define boundaries
                         │
              ┌──────────┴──────────┐
              ▼                     ▼
          High cohesion        Low coupling
              │                     │
              └──────────┬──────────┘
                         ▼
                 Choose abstractions
                         │
                         ▼
               Inject dependencies
                         │
                         ▼
             Compose small components
                         │
                         ▼
              Add events where useful
                         │
                         ▼
                 Handle tradeoffs
```

And the ultimate rule:

> **Patterns are tools, not architecture.**

A good SDE-II doesn't try to fit every problem into Factory + Singleton + Observer + Strategy.

A good SDE-II asks:

```text
What changes?
What should remain stable?
Who owns this responsibility?
What does this component need to know?
What can I isolate?
What needs to be replaceable?
Where should dependencies point?
Should this communication be synchronous or event-driven?
What complexity am I introducing?
```

If you can answer those questions, the design pattern usually becomes obvious.
