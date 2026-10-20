# Advanced Node.js Interview Questions

## I/O-Heavy Systems — Senior / Staff Level

# 1. Event Loop & I/O

### Q1. 10,000 Concurrent HTTP Requests

**Question**

Explain exactly what happens in Node.js when **10,000 concurrent HTTP requests** are waiting on database I/O.

**What the interviewer is testing:**

* Event loop fundamentals
* Non-blocking I/O
* Promise/callback scheduling
* Connection pooling
* Resource limits

### Q2. Blocking I/O

**Question**

Why doesn't blocking I/O necessarily block the Node.js event loop?

**Follow-up:**

What is the difference between:

* Blocking the **event loop**
* Waiting for an **I/O operation**
* Consuming a **limited resource**, such as DB connections?

### Q3. CPU Work After I/O

**Question**

What happens if the callback or Promise continuation after an I/O operation performs **500 ms of CPU-heavy work**?

**Follow-up:**

If 100 requests complete their I/O around the same time, what happens to event-loop latency?

### Key Concept

> **Async I/O doesn't make CPU-bound JavaScript asynchronous.**

# 2. High-Concurrency API

You have a Node.js API receiving:

```text
50,000 requests/sec
```

Each request performs:

```text
2 database queries
+
1 external API call
```

### Question

How would you design the system to:

* Avoid exhausting DB connections?
* Control concurrency?
* Handle slow downstream services?
* Prevent cascading failures?
* Maintain acceptable p99 latency?

### Areas to Discuss

* Connection pooling
* Bounded concurrency
* Timeouts/deadlines
* Circuit breakers
* Bulkheads
* Caching
* Queues
* Backpressure
* Load shedding
* Retry policies
* p95/p99 monitoring

### Senior-Level Principle

> **The system must control downstream concurrency instead of allowing request volume to directly translate into unlimited downstream work.**

# 3. Connection Pooling

Your PostgreSQL pool has **100 connections**, and your Node.js application has **20 instances**.

### Question

What happens if traffic suddenly increases to **100,000 concurrent requests**?

Think through:

```text
100 DB connections
×
20 Node.js instances
=
2,000 potential DB connections
```

Then consider what happens when all connections are busy.

```text
100K requests
      ↓
Node.js
      ↓
Connection pools
      ↓
DB connections exhausted
      ↓
Requests wait
      ↓
Latency increases
      ↓
Timeouts
      ↓
Retries
```

### Follow-Up

How would you determine the **optimal connection pool size**?

Consider:

* PostgreSQL capacity
* Number of application instances
* Query duration
* CPU
* Memory
* Connection overhead
* Traffic patterns
* Desired concurrency
* Pool wait time

### Key Concept

> **Pool sizing is a system-wide capacity decision, not simply a per-process configuration.**

# 4. Backpressure

A Node.js service reads a **10 GB file** and streams it to another service over HTTP.

### Question

How would you prevent memory usage from growing to several GB?

Explain how:

* Node.js streams
* Chunked processing
* Writable stream buffering
* `highWaterMark`
* `pipe()`
* Backpressure

work together.

Example:

```js
createReadStream(file).pipe(httpRequest);
```

### Key Concept

```text
Fast producer
     ↓
   Stream
     ↓
Slow consumer
     ↓
Backpressure
     ↓
Producer slows down
```

> **The producer should not generate data faster than the consumer can handle it.**

# 5. `Promise.all()` Problem

Consider:

```js
const results = await Promise.all(
  users.map(user => fetchUserData(user))
);
```

There are **500,000 users**.

### Question

Why is this dangerous in an I/O-heavy application even though the operations are asynchronous?

Potential problems:

* 500K Promise objects
* High memory usage
* Too many sockets
* Connection-pool exhaustion
* External API overload
* Increased GC pressure
* Request amplification
* Poor failure isolation

### Better Design

```text
500K jobs
   ↓
Queue
   ↓
Bounded concurrency
   ↓
50 / 100 / N active jobs
   ↓
Results
```

### Key Concept

> **Asynchronous does not mean unlimited concurrency.**

# 6. Slow I/O Dependency

Your service normally responds in:

```text
50 ms
```

But an external API occasionally takes:

```text
30 seconds
```

Thousands of requests begin waiting for that API.

### Question

How can this cause an outage even though Node.js isn't technically blocking?

Think about:

* Pending Promises
* Memory consumption
* Socket exhaustion
* Connection pools
* Request queues
* Timeouts
* Retry storms
* Increased GC pressure
* Increased p99 latency

### Key Concept

> **The event loop can remain responsive while the application runs out of other resources.**

# 7. Timeouts

Design a robust timeout strategy for:

```text
Client
  ↓
Node.js
  ↓
Redis
  ↓
PostgreSQL
  ↓
External API
```

### Question

What timeout should each layer have, and why?

Don't think of timeouts as isolated numbers.

Instead, use an **overall request deadline**.

Example:

```text
Overall deadline: 5 seconds

        5s total
          │
          ├── Redis:       500ms
          ├── PostgreSQL:  2s
          └── External API: 2s
```

The exact numbers depend on the application.

### Important Questions

* What happens when a downstream timeout occurs?
* Should the operation be retried?
* Is the operation idempotent?
* Is there remaining request budget?
* Should the request be cancelled?
* Should the dependency be circuit-broken?

### Key Concept

> **Every I/O operation needs bounded waiting, and downstream timeouts should respect the overall request deadline.**

# 8. CPU vs I/O

Your Node.js server has:

```text
16 CPU cores
1 Node.js process

CPU utilization: 20%
Thousands of requests waiting on I/O
```

### Question

Would adding:

* Cluster?
* Worker Threads?
* More Node.js processes?

improve performance?

### Think About

If the bottleneck is:

```text
Database
External API
Network
Connection pool
```

adding CPU workers doesn't necessarily solve the problem.

### Rule

```text
I/O-heavy
    ↓
Async I/O
+ Connection pools
+ Concurrency control
+ Horizontal scaling


CPU-heavy
    ↓
Worker Threads
or
Multiple processes
```

### Key Concept

> **Scale the bottleneck, not the CPU blindly.**

# 9. File I/O

Compare:

```js
const data = await fs.promises.readFile(file);
```

with:

```js
createReadStream(file);
```

You need to serve a **5 GB file** to multiple concurrent users.

### Question

What happens to:

* Memory?
* Throughput?
* Latency?
* Backpressure?
* GC pressure?

### `readFile()`

```text
5 GB file
   ↓
Read entire file
   ↓
Memory
   ↓
Send response
```

### Stream

```text
5 GB file
   ↓
Chunks
   ↓
Network
   ↓
Consumer speed controls producer
```

### Key Concept

> **For large files and high concurrency, streaming avoids holding entire payloads in memory.**

# 10. Event Loop Lag

Your API has:

```text
Average latency: Good
p99 latency:     Terrible

CPU:             35%
Memory:           50%
DB:               Healthy
Event-loop lag:  800ms
```

### Question

How would you investigate?

Look for:

* Synchronous JavaScript
* CPU-heavy loops
* Large JSON parsing
* Large JSON serialization
* Expensive regular expressions
* Large object manipulation
* Blocking filesystem calls
* Garbage-collection pauses
* Unexpected synchronous dependencies

### Tools / Techniques

Use:

* CPU profiling
* Flame graphs
* APM
* Event-loop monitoring
* Node.js performance hooks
* Heap profiling
* GC metrics

### Key Concept

> **Average latency can look healthy while event-loop stalls destroy p99 latency.**

# 11. Async Does Not Mean Unlimited Concurrency

Consider:

```js
async function process() {
  const tasks = [];

  for (const item of items) {
    tasks.push(doIO(item));
  }

  return Promise.all(tasks);
}
```

### Question

Why can this overload a system?

Because the code effectively does:

```text
Every item
    ↓
Create Promise
    ↓
Start I/O
    ↓
Everything runs concurrently
```

If `items` contains 500,000 elements:

```text
500K operations
       ↓
500K pending operations
       ↓
Memory pressure
       ↓
Sockets / DB pool / API limits
       ↓
Resource exhaustion
```

## Bounded-Concurrency Queue

Instead:

```text
500K jobs
    ↓
   Queue
    ↓
50 active workers
    ↓
Process next job
    ↓
50 active workers
    ↓
...
```

Conceptually:

```js
const limit = 50;

while (queue.length > 0) {
  // Keep at most 50 operations active.
}
```

Or use a proper concurrency-limiting queue/library.

### Key Concept

> **Concurrency should be an explicit capacity decision.**

# 12. Graceful Shutdown

Your Node.js service is processing:

```text
20,000 concurrent I/O operations
```

Kubernetes sends:

```text
SIGTERM
```

### Question

How should the application shut down without:

* Dropping requests?
* Leaving DB transactions open?
* Leaving sockets hanging?
* Losing queued work?

### Expected Flow

```text
SIGTERM
   ↓
Stop accepting new traffic
   ↓
Remove instance from load balancer
   ↓
Stop creating new work
   ↓
Allow in-flight requests to finish
   ↓
Drain queues
   ↓
Finish / cancel outstanding I/O
   ↓
Commit / rollback DB transactions
   ↓
Close DB / Redis connections
   ↓
Exit
```

### Key Concept

> **Graceful shutdown is controlled draining, not simply calling `process.exit()`.**

# 13. Retries & I/O

An external API has a:

```text
1% failure rate
```

You add:

```js
retry(request, 3);
```

Suddenly your system becomes overloaded.

### Question

Why can retries make an outage worse?

Suppose:

```text
10,000 requests
×
3 retries
=
Up to 30,000 additional attempts
```

During a dependency failure:

```text
Failure
   ↓
Retry
   ↓
More traffic
   ↓
More dependency load
   ↓
More failures
   ↓
More retries
```

This creates a **retry storm**.

### Safer Retry Strategy

Use:

* Exponential backoff
* Jitter
* Maximum retry count
* Timeouts
* Overall request deadline
* Circuit breakers
* Idempotency where required
* Retry only appropriate errors

### Key Concept

> **Retries should be treated as additional traffic, not as free recovery.**

# 14. Circuit Breaker

Design a circuit breaker for an external HTTP dependency.

Explain:

### CLOSED

Normal operation.

```text
Requests → Dependency
```

Failures are monitored.

### OPEN

Failure threshold is reached.

```text
Requests
   ↓
Circuit Breaker
   ↓
Fail fast
```

Don't keep calling the unhealthy dependency.

### HALF-OPEN

After a recovery period, allow a limited number of test requests.

```text
          success
             ↓
OPEN → HALF-OPEN → CLOSED

          failure
             ↓
            OPEN
```

### Failure Threshold

Could be based on:

* Consecutive failures
* Error percentage
* Rolling window
* Timeout rate
* Latency threshold

### Recovery Strategy

Consider:

* Open duration
* Half-open probe count
* Exponential recovery delays
* Dependency health
* Failure rate

### Key Concept

> **A circuit breaker protects both your service and the unhealthy dependency.**

# 15. ⭐ Advanced Scenario

You operate a Node.js service handling:

```text
100K requests/sec
```

Each request:

```text
Redis read
   ↓
PostgreSQL query
   ↓
External HTTP call
   ↓
Kafka event
```

Suddenly:

```text
PostgreSQL latency:
10 ms → 2 seconds
```

## Question

Walk through what happens inside your Node.js application over the next **30 seconds**.

Then explain how you would prevent:

* Connection-pool exhaustion
* Request pile-up
* Memory growth
* Timeout storms
* Cascading failure

# ⭐ Think Through the Failure Chain

Initially:

```text
Requests
   ↓
Node.js
   ↓
PostgreSQL
   ↓
10 ms
```

Then PostgreSQL becomes slow:

```text
Requests
   ↓
Node.js
   ↓
PostgreSQL
   ↓
2 seconds
```

Queries remain active much longer.

Therefore:

```text
Longer query duration
       ↓
Connections occupied longer
       ↓
Pool utilization increases
       ↓
New requests wait for connections
       ↓
Request latency increases
       ↓
More requests remain in memory
       ↓
Timeouts increase
       ↓
Retries begin
       ↓
More DB traffic
       ↓
PostgreSQL becomes even more overloaded
```

Eventually:

```text
             PostgreSQL slows
                    ↓
             DB pool exhausted
                    ↓
              Requests queue
                    ↓
              Memory grows
                    ↓
             Timeouts increase
                    ↓
              Retries increase
                    ↓
             DB traffic grows
                    ↓
          PostgreSQL gets slower
                    ↓
            Cascading failure
```

# How Would You Prevent It?

## 1. Connection Pool Limits

Don't allow every request to establish a DB connection.

```text
100K requests
     ↓
Bounded DB pool
     ↓
Controlled DB concurrency
```

## 2. Request Deadlines

Don't allow requests to wait indefinitely.

```text
Request deadline
      ↓
Redis timeout
      ↓
Postgres timeout
      ↓
External API timeout
```

Cancel or fail work once the request's useful time has expired.

## 3. Concurrency Limits

Limit how much work can reach PostgreSQL.

```text
100K requests
      ↓
Concurrency limiter
      ↓
Controlled DB workload
```

## 4. Bulkheads

Isolate resources so one dependency cannot consume everything.

For example:

```text
Redis
 └── Pool / concurrency budget

PostgreSQL
 └── Pool / concurrency budget

External API
 └── Pool / concurrency budget
```

A PostgreSQL incident shouldn't consume every resource in the application.

## 5. Circuit Breaker

If PostgreSQL is severely degraded:

```text
Healthy
   ↓
Failures / latency
   ↓
OPEN
   ↓
Fail fast
```

Don't continuously send traffic to an already overloaded dependency.

## 6. Backpressure / Queues

If work doesn't need to happen synchronously, move it out of the request path.

```text
HTTP Request
     ↓
Queue
     ↓
Workers
     ↓
PostgreSQL
```

This allows the system to control processing rate.

## 7. Caching

Reduce unnecessary PostgreSQL traffic.

```text
Request
   ↓
Cache
 ┌─┴─┐
Hit  Miss
 ↓     ↓
Return PostgreSQL
```

## 8. Safe Retries

If retries are necessary:

```text
Retry
  ↓
Exponential backoff
  ↓
Jitter
  ↓
Maximum attempts
  ↓
Respect request deadline
```

Never blindly retry every failure.

## 9. Load Shedding

When the system is beyond capacity, deliberately reject or defer some work.

```text
Traffic
   ↓
Capacity limit
   ↓
Accept what can be handled
   ↓
Reject / shed excess load
```

### Key Principle

> **Failing a small percentage of requests is often better than allowing the entire system to collapse.**

# 🎯 What This Interview Is Really Testing

Question #15 isn't really about PostgreSQL.

It's testing whether you understand how these concepts interact:

```text
                 ┌── Event Loop
                 │
                 ├── Promises
                 │
                 ├── Connection Pools
                 │
                 ├── Concurrency Limits
                 │
                 ├── Backpressure
                 │
                 ├── Timeouts
                 │
                 ├── Retries
                 │
                 ├── Circuit Breakers
                 │
                 ├── Bulkheads
                 │
                 └── Queues
                        ↓
                 System Stability
```

### Senior/Staff-Level Principle

> **A Node.js application can remain completely non-blocking at the JavaScript level and still fail catastrophically because downstream resources are finite.**

The strongest candidates understand that:

```text
Async I/O
    ≠
Unlimited concurrency
    ≠
Unlimited capacity
```

Instead:

```text
                    CONTROL
                       ↓
Requests → Concurrency → I/O → Downstream
              ↓          ↓
           Backpressure  Timeouts
              ↓          ↓
           Queues      Circuit Breakers
                       ↓
                  Resilient System
```

# ⭐ Final Interview Answer to Memorize

> **"In an I/O-heavy Node.js system, the key is not simply making I/O asynchronous. The system must control concurrency and resource consumption at every downstream boundary. Connection pools, timeouts, backpressure, queues, circuit breakers, bulkheads, and carefully designed retries prevent slow dependencies from turning into connection exhaustion, memory growth, request pile-up, and cascading failure."**----

# Node.js — Senior-Level I/O & Scalability Interview Notes

## 1. Event Loop & I/O

### Answer

Node.js doesn't wait synchronously for I/O operations. It registers the operation and continues executing other work.

When the I/O operation completes, its callback or Promise continuation is scheduled to run through the event loop.

### Key Point

> **I/O is non-blocking, but the code handling the result can still block the event loop if it performs heavy CPU work.**

## 2. Handling 50K Requests/sec + Database + External API

### Answer

At this scale, the goal is to **control downstream work** rather than allowing every incoming request to create unlimited operations.

Use:

* **Connection pooling**
* **Concurrency limits**
* **Request timeouts**
* **Caching**, where appropriate
* **Circuit breakers** for external services
* **Backpressure / queues**
* **Load shedding**, when necessary
* **Monitoring of p95/p99 latency**

### Key Point

> **Don't allow incoming traffic to create unlimited downstream work.**

## 3. Database Connection Pooling

Suppose you have:

```text
20 Node.js instances
×
100 DB connections per instance
=
2,000 PostgreSQL connections
```

This can potentially put **2,000 connections** against PostgreSQL.

### Key Point

> **Pool size must be calculated across all application instances, not per instance in isolation.**

More connections do **not** necessarily mean better performance.

Too many connections can increase:

* Context switching
* Memory usage
* Lock contention
* Database CPU usage

And can actually make PostgreSQL **slower**.

## 4. Backpressure

For large files or streams, don't load the entire payload into memory.

```js
createReadStream(file).pipe(response);
```

The stream mechanism allows the producer to slow down when the consumer cannot keep up.

### Key Point

> **Backpressure means the producer adjusts to the consumer's processing speed.**

This is especially important for:

* Large files
* High-concurrency systems
* Network streams
* Message processing

## 5. `Promise.all()` with 500K Operations

Avoid:

```js
await Promise.all(users.map(fetchUserData));
```

If `users` contains 500,000 items, you may create:

* **500K Promises**
* Huge memory usage
* Too many sockets
* Database pool exhaustion
* External API overload

### Better Approach

```text
500K tasks
    ↓
   Queue
    ↓
50 concurrent tasks
    ↓
 Results
```

For example, process only a bounded number of operations at a time.

### Key Point

> **Async doesn't mean unlimited concurrency.**

## 6. Slow External API

Suppose an external API takes **30 seconds** to respond.

If thousands of Node.js requests are waiting for it, you can eventually experience:

* Memory pressure
* Connection exhaustion
* Request pile-up
* Increased latency
* Timeout storms

### Key Point

> **Non-blocking I/O can still cause resource exhaustion.**

The event loop may remain available while other resources become exhausted.

## 7. Timeouts

Every network boundary should have a timeout.

```text
Client
  ↓ timeout
Node.js
  ↓ timeout
Redis
  ↓ timeout
PostgreSQL
  ↓ timeout
External API
```

### Key Point

> **Never allow an I/O operation to wait forever.**

Timeouts should generally respect an **overall request deadline**.

For example:

```text
Overall request deadline: 5s

Node.js
 ├── Redis:     max 1s
 ├── Postgres:  max 2s
 └── API:       max 2s
```

The exact values depend on the system, but the important concept is **bounded waiting**.

## 8. Cluster vs Worker Threads

If the workload is primarily **I/O-heavy**, adding worker threads usually doesn't solve the primary bottleneck.

Worker threads are most useful for **CPU-heavy operations**.

### Rule of Thumb

```text
I/O-heavy
    ↓
Async I/O + scaling + connection pools


CPU-heavy
    ↓
Worker Threads / Multiple Processes
```

### Examples of CPU-heavy work

* Image processing
* Video encoding
* Large data transformations
* Cryptographic computation
* Complex calculations

### Key Point

> **Use worker threads to move CPU-intensive work away from the event loop.**

## 9. `readFile()` vs `createReadStream()`

### `readFile()`

```js
const data = await fs.promises.readFile(file);
```

This loads the **entire file into memory**.

### `createReadStream()`

```js
createReadStream(file);
```

This processes the file in **chunks**.

### Comparison

| Approach             | Memory Usage | Large Files |
| -------------------- | -----------: | ----------- |
| `readFile()`         |       Higher | Riskier     |
| `createReadStream()` |        Lower | Better      |

### Key Point

> **Large files + high concurrency → streams are usually the safer choice.**

## 10. Event Loop Lag

Suppose monitoring shows:

```text
CPU usage:        35%
Event-loop lag:  800ms
```

Don't assume the system is healthy just because CPU usage is relatively low.

Investigate:

* Synchronous functions
* Large JSON parsing
* Large JSON stringification
* CPU-heavy loops
* Expensive regular expressions
* Large object processing
* Blocking filesystem operations

Use:

* Profiling
* APM
* Event-loop monitoring
* CPU profiles
* Flame graphs

### Key Point

> **Low CPU usage does not guarantee a healthy event loop.**

## 11. Bounded Concurrency

Avoid:

```js
Promise.all(hugeArray);
```

Instead, introduce a concurrency limit.

```text
10,000 jobs
     ↓
   Queue
     ↓
50 workers
     ↓
Database / API
```

This allows you to control the number of active operations.

### Key Point

> **Control how many I/O operations are active at once.**

This protects:

* Databases
* External APIs
* Sockets
* Memory
* Connection pools

## 12. Graceful Shutdown

When the application receives `SIGTERM`:

```text
SIGTERM
   ↓
Stop accepting new requests
   ↓
Finish existing requests
   ↓
Finish / cleanup important I/O
   ↓
Close DB / Redis connections
   ↓
Exit
```

### Key Point

> **Shutdown should drain work, not abruptly kill it.**

This is especially important in:

* Kubernetes
* Containerized deployments
* Load-balanced environments
* Rolling deployments

## 13. Retries

Naive retries can create a feedback loop:

```text
Failure
   ↓
Retry
   ↓
More traffic
   ↓
More failures
   ↓
More retries
   ↓
System overload
```

Use:

* **Exponential backoff**
* **Jitter**
* **Maximum retry count**
* **Timeouts**
* **Circuit breakers**

### Key Point

> **Retries can amplify traffic during failures, so retry carefully.**

## 14. Circuit Breaker

A circuit breaker typically has three states:

```text
       failures
CLOSED ─────────→ OPEN
                    ↓
                  wait
                    ↓
               HALF-OPEN
                 ↙     ↘
             success   failure
                ↓         ↓
             CLOSED      OPEN
```

### States

**CLOSED**

Requests flow normally.

**OPEN**

Requests to the unhealthy dependency are rejected or short-circuited.

**HALF-OPEN**

A limited number of requests are allowed to test whether the dependency has recovered.

### Key Point

> **Stop repeatedly calling an unhealthy dependency and give it time to recover.**

# 15. ⭐ Most Important Scenario: PostgreSQL Latency Increases

Suppose PostgreSQL latency increases from:

```text
10 ms → 2 seconds
```

### Initially

```text
Requests
   ↓
Node.js
   ↓
PostgreSQL
   ↓
10 ms response
```

Everything works normally.

### Database Becomes Slow

```text
100K requests
      ↓
   Node.js
      ↓
PostgreSQL
      ↓
  2 sec response
```

Requests start accumulating.

Eventually:

```text
More requests
      ↓
More DB connections waiting
      ↓
Connection pool exhausted
      ↓
Requests wait
      ↓
Memory grows
      ↓
Timeouts
      ↓
Retries
      ↓
Even more DB traffic
      ↓
Cascading failure
```

## How Do You Prevent It?

Use a combination of:

* **DB connection pool limits**
* **Request timeouts / deadlines**
* **Concurrency limits**
* **Circuit breakers**
* **Bulkheads**
* **Queues / backpressure**
* **Caching**
* **Exponential backoff + jitter**
* **Load shedding**
* **Pool-utilization monitoring**
* **Event-loop-lag monitoring**
* **p95/p99 latency monitoring**

### Important Concept

The goal isn't simply to make PostgreSQL faster.

The goal is to **prevent one slow dependency from consuming all of your application's resources**.

# ⭐ Senior-Level Interview One-Liner

> **"The main principle in an I/O-heavy Node.js system is to control concurrency and apply backpressure at every downstream boundary. Async I/O prevents the event loop from blocking, but without limits, slow dependencies can still exhaust connections, memory, sockets, and queues."**

### Remember This

```text
Async I/O
   ≠
Unlimited concurrency
```

A strong Node.js system is built around:

```text
                ┌── Timeouts
                │
                ├── Concurrency limits
                │
Requests ───────┼── Connection pools
                │
                ├── Backpressure
                │
                ├── Circuit breakers
                │
                └── Queues
                         ↓
              Downstream Services
```

**Core principle:**

> **Control the amount of work entering every downstream system.**
