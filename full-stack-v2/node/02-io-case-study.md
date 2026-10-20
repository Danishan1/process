# Case Study: Building and Stabilizing a High-Concurrency Node.js API

## 1. The System

I was working on a Node.js API responsible for processing customer requests at high traffic.

At peak:

```text
100,000 requests/sec
```

Each request roughly followed this flow:

```text
Client
   ↓
Node.js API
   ↓
Redis
   ↓
PostgreSQL
   ↓
External HTTP API
   ↓
Kafka
```

The important thing was that the Node.js application itself was mostly doing I/O.

It wasn't performing heavy CPU calculations.

For example:

```js
const cached = await redis.get(key);

const data = await postgres.query(...);

const result = await externalApi.call(...);

await kafka.publish(result);
```

At first glance, this looked safe because everything was asynchronous.

But we learned an important lesson:

> **Asynchronous I/O does not mean unlimited concurrency or unlimited capacity.**

The event loop can remain completely responsive while PostgreSQL, Redis, sockets, memory, or external APIs become exhausted.

# 2. The First Question: What Happens With 10,000 Requests?

Suppose 10,000 requests arrive and all of them are waiting for PostgreSQL.

A common misconception is:

> "Node.js has 10,000 requests, so it must have 10,000 threads."

That's not what happens.

Node.js can keep thousands of I/O operations in flight without creating one JavaScript thread per request.

Conceptually:

```text
10,000 HTTP requests
        ↓
Node.js event loop
        ↓
Start asynchronous I/O
        ↓
Requests wait
        ↓
Database/network completes operations
        ↓
Callbacks / Promise continuations become runnable
        ↓
Event loop executes JavaScript
```

While the database is doing the actual I/O work, the JavaScript thread isn't sitting there doing:

```js
while (!databaseFinished) {
}
```

Instead, Node.js registers the asynchronous operation and continues processing other work.

When the operation completes, the corresponding callback/Promise continuation is scheduled for execution.

So:

```text
Waiting for I/O
        ≠
Blocking the event loop
```

But there is another important distinction.

# 3. Three Different Things We Need To Separate

During incident analysis, I always think about three separate concepts.

### A. Event-loop blocking

JavaScript is actively doing CPU work:

```js
while (Date.now() < start + 500) {
  // CPU-heavy work
}
```

Now the Node.js event loop cannot process other JavaScript callbacks efficiently.

This directly increases latency.

### B. Waiting for I/O

Example:

```js
const result = await postgres.query(...);
```

The JavaScript execution pauses for that async function, but the event loop can continue handling other work.

This doesn't necessarily block the event loop.

### C. Waiting for a limited resource

This is where production systems often fail.

Suppose:

```text
PostgreSQL pool = 100 connections
```

Now imagine:

```text
10,000 requests
       ↓
100 DB connections
       ↓
9,900 requests waiting for a connection
```

The event loop can still be healthy.

But the application is overloaded because the **database connection pool is finite**.

That distinction became very important in our design.

# 4. The Hidden Problem With Async Code

One of our early implementations looked like this:

```js
async function processUsers(users) {
  const tasks = users.map(user => processUser(user));

  return Promise.all(tasks);
}
```

It looked elegant.

But imagine:

```text
500,000 users
```

We effectively created:

```text
500,000 operations
        ↓
500,000 Promise objects
        ↓
Huge amount of pending work
        ↓
Many sockets / DB requests
        ↓
Memory pressure
        ↓
External dependency overload
```

The important lesson was:

> **Promise-based code controls how we wait for work, not how much work we start.**

`Promise.all()` is not a concurrency limiter.

# 5. We Introduced Bounded Concurrency

Instead of allowing unlimited work, we introduced explicit concurrency limits.

Conceptually:

```text
500,000 jobs
      ↓
     Queue
      ↓
50 active jobs
      ↓
Complete one
      ↓
Start another
      ↓
50 active jobs
      ↓
...
```

For example:

```js
const limit = pLimit(50);

const tasks = users.map(user =>
  limit(() => processUser(user))
);

const results = await Promise.all(tasks);
```

Now the application could have:

```text
500,000 total jobs
50 active jobs
```

instead of:

```text
500,000 total jobs
500,000 active operations
```

This was a major architectural principle:

> **Concurrency should be an explicit capacity decision.**

# 6. The Database Pool Problem

Our PostgreSQL configuration was approximately:

```text
100 connections per Node.js instance
```

And we had:

```text
20 Node.js instances
```

That means the theoretical maximum was:

```text
100 × 20
= 2,000 database connections
```

This is why connection pooling cannot be designed only at the application-process level.

The database sees the aggregate:

```text
Instance 1 → 100
Instance 2 → 100
Instance 3 → 100
...
Instance 20 → 100

Total → 2,000
```

If PostgreSQL can comfortably handle only a fraction of that, simply increasing the pool size makes the problem worse.

So we treated pool sizing as a **system-wide capacity problem**.

We considered:

* PostgreSQL CPU
* Query duration
* Memory
* Maximum connections
* Number of application instances
* Traffic patterns
* Query concurrency
* Pool wait time
* Desired latency
* Connection overhead

The important metric wasn't:

> "How many connections can we open?"

It was:

> "How much database concurrency can the database actually sustain while meeting our latency target?"

# 7. Then We Had a Production Incident

Normally, our PostgreSQL queries looked like:

```text
10 ms
```

Then suddenly something changed.

PostgreSQL query latency went from:

```text
10 ms
     ↓
2 seconds
```

Nothing in Node.js had changed.

The event loop was still healthy.

But the system started deteriorating rapidly.

# 8. Minute 0: PostgreSQL Becomes Slow

Initially:

```text
Request
   ↓
PostgreSQL
   ↓
10 ms
   ↓
Response
```

After the incident:

```text
Request
   ↓
PostgreSQL
   ↓
2 seconds
   ↓
Response
```

The critical difference is that each connection was now occupied for much longer.

Suppose we have:

```text
100 DB connections
```

Previously:

```text
100 connections × 10 ms
```

Now:

```text
100 connections × 2 seconds
```

The same pool can process dramatically less work per second.

This is a key capacity relationship:

> **Longer service time means fewer requests can be completed with the same concurrency.**

# 9. Connection Pool Starts Filling

The sequence became:

```text
PostgreSQL slows
       ↓
Queries take longer
       ↓
Connections remain occupied
       ↓
Pool utilization increases
       ↓
New requests wait for connections
```

Now the Node.js requests themselves started waiting.

This is an important distinction.

Node.js wasn't necessarily CPU-blocked.

Instead:

```text
Event loop → healthy

Database pool → saturated
```

The application was becoming unhealthy because of a **finite downstream resource**.

# 10. Request Pile-Up

More requests continued arriving.

Suppose:

```text
100,000 requests/sec
```

But PostgreSQL's effective capacity had suddenly dropped.

Now:

```text
Incoming work
      ↓
More DB requests
      ↓
Pool full
      ↓
Requests wait
      ↓
More requests accumulate
```

Pending requests consume resources.

For example:

* Request objects
* Promise state
* Buffers
* Headers
* Context
* Tracing information
* Timers
* Socket state

So memory begins to increase.

The event loop may still be responsive, but the process is now carrying a huge amount of pending work.

# 11. p99 Latency Explodes

Before the incident:

```text
p50 = 20 ms
p95 = 40 ms
p99 = 60 ms
```

During the incident:

```text
p50 = 500 ms
p95 = 2 sec
p99 = 8 sec
```

This is why average latency can be misleading.

If most requests are fast but a small percentage are waiting behind saturated resources, p99 can become terrible.

So we monitored:

```text
p50
p95
p99
event-loop lag
DB pool utilization
DB pool wait time
request concurrency
memory
GC
external API latency
timeout rate
retry rate
```

# 12. Timeouts Become Critical

We didn't want a request to wait forever.

So we introduced request deadlines.

For example:

```text
Overall request deadline = 5 seconds
```

Within that budget:

```text
Request
  │
  ├── Redis
  │
  ├── PostgreSQL
  │
  └── External API
```

Each operation had its own bounded timeout, but the important concept was that the sum had to respect the overall request budget.

We didn't want this:

```text
Redis → 5 sec
Postgres → 5 sec
External API → 5 sec

Total → 15 sec
```

when the client only had a useful request budget of 5 seconds.

Instead:

```text
Request deadline
      ↓
Remaining time
      ↓
Dependency timeout
```

Every downstream operation should understand how much useful time remains.

# 13. Why Timeout Alone Isn't Enough

A timeout doesn't magically cancel all work everywhere.

Suppose Node.js gives up after:

```text
5 seconds
```

But PostgreSQL continues executing a query that is no longer useful.

Now we have:

```text
Client request timed out
        ↓
Node.js no longer needs result
        ↓
Database query may still be running
        ↓
DB resource remains occupied
```

Therefore, timeout design needs to include cancellation/cleanup where supported.

The principle is:

> **Once work is no longer useful, stop spending resources on it.**

# 14. Then Retries Made Things Worse

Some clients and internal services retried failed requests.

We also had application-level retries.

Initially, the logic was roughly:

```text
Failure
  ↓
Retry
  ↓
Retry
  ↓
Retry
```

During normal operation this seemed harmless.

During the PostgreSQL incident it became dangerous.

Suppose:

```text
10,000 original requests
```

and each request could produce multiple attempts.

Now we could generate:

```text
Original traffic
      ↓
Failures
      ↓
Retries
      ↓
Additional database traffic
      ↓
More database load
      ↓
More failures
      ↓
More retries
```

This is a retry storm.

The key realization was:

> **A retry is new traffic.**

It is not free.

# 15. We Changed the Retry Strategy

We introduced:

```text
Maximum retry attempts
        +
Exponential backoff
        +
Jitter
        +
Timeout
        +
Request deadline
        +
Idempotency rules
```

For example:

```text
Attempt 1
   ↓
small delay
   ↓
Attempt 2
   ↓
larger delay
   ↓
Attempt 3
```

With jitter, clients don't all retry at exactly the same moment.

We also became selective about what we retried.

A timeout or transient network failure may be retryable.

A validation error usually isn't.

And non-idempotent operations require much more care.

# 16. We Added a Circuit Breaker

Retries alone weren't enough.

If PostgreSQL or an external API was clearly unhealthy, continuously sending requests was counterproductive.

So we introduced circuit-breaker behavior.

### CLOSED

Normal operation:

```text
Request
   ↓
Circuit
   ↓
Dependency
```

We monitored:

* Error percentage
* Timeout rate
* Consecutive failures
* Rolling failure windows
* Sometimes latency

If the dependency became sufficiently unhealthy:

### OPEN

```text
Request
   ↓
Circuit Breaker
   ↓
Fail fast
```

We stopped sending unnecessary traffic downstream.

After a recovery period:

### HALF-OPEN

We allowed a small number of test requests.

```text
OPEN
  ↓
HALF-OPEN
  ↓
small number of probes
```

If successful:

```text
HALF-OPEN
    ↓
 CLOSED
```

If they failed:

```text
HALF-OPEN
    ↓
 OPEN
```

This protected both systems.

# 17. Bulkheads Prevented One Dependency From Taking Everything Down

We didn't want PostgreSQL to consume every available resource.

So we separated concurrency budgets.

Conceptually:

```text
Node.js
  │
  ├── Redis concurrency budget
  │
  ├── PostgreSQL concurrency budget
  │
  └── External API concurrency budget
```

For example:

```text
Redis       → 100 concurrent operations
PostgreSQL  → 50 concurrent operations
External API → 30 concurrent operations
```

The exact values depend on measured capacity.

The important idea is isolation.

If the external API becomes slow, it shouldn't be able to consume all application capacity and prevent unrelated Redis operations from completing.

That's the purpose of a bulkhead.

# 18. We Added Load Shedding

Eventually, every system has a capacity limit.

If:

```text
Incoming traffic > Processing capacity
```

something has to give.

Instead of allowing:

```text
100%
traffic
   ↓
system saturation
   ↓
everything becomes slow
   ↓
everything times out
   ↓
system collapses
```

we deliberately rejected or deferred some work.

For example:

```text
Incoming traffic
       ↓
Capacity check
       ↓
 ┌─────┴─────┐
 │           │
Accept      Reject
 │           │
Process     429/503
```

It can be better to reject 10% of traffic quickly than to make 100% of requests take 30 seconds and eventually fail.

# 19. We Moved Non-Critical Work to Kafka

Some operations did not need to happen synchronously.

For example, after successfully processing the request, we needed to publish an analytics/event operation.

Instead of making the client wait:

```text
HTTP
 ↓
Redis
 ↓
Postgres
 ↓
External API
 ↓
Kafka
 ↓
Response
```

we moved non-critical work out of the request path.

Conceptually:

```text
HTTP Request
      ↓
Validate / process critical work
      ↓
Publish job/event
      ↓
Return response
      ↓
Kafka
      ↓
Workers
      ↓
PostgreSQL / downstream systems
```

Now Kafka acted as a buffer between producers and consumers.

Workers could process at a controlled rate.

That gave us another form of backpressure.

# 20. Backpressure With Streams

We had another problem involving large files.

One service needed to send a 10 GB file to another HTTP service.

The dangerous implementation would be:

```js
const data = await fs.promises.readFile("10GB-file");
```

Now we potentially hold a huge payload in memory.

With multiple concurrent requests:

```text
10 GB
×
multiple requests
=
massive memory pressure
```

Instead, we streamed:

```js
createReadStream(file).pipe(httpRequest);
```

Conceptually:

```text
Disk
 ↓
Readable Stream
 ↓
HTTP Writable
 ↓
Network
```

The producer shouldn't continuously generate data when the consumer can't keep up.

Node.js streams provide buffering and backpressure mechanisms.

`highWaterMark` influences how much data a stream buffers before applying pressure.

The important concept isn't memorizing the exact default number.

It's understanding:

```text
Fast producer
      ↓
Buffer
      ↓
Slow consumer
      ↓
Backpressure
      ↓
Producer slows down
```

This prevents us from turning a slow network connection into unlimited memory growth.

# 21. Large File Serving: `readFile()` vs Streaming

For a 5 GB file:

### `readFile()`

```text
5 GB file
   ↓
Read entire file
   ↓
Memory
   ↓
Send
```

This can create enormous memory pressure.

### Streaming

```text
5 GB file
   ↓
small chunks
   ↓
network
   ↓
consumer controls pace
```

Streaming gives us much better memory characteristics under concurrency.

The file size no longer directly translates into the amount of JavaScript heap we need to hold.

# 22. Then We Investigated Event-Loop Lag

During another performance investigation we saw:

```text
CPU = 35%
Memory = 50%
DB = healthy

Average latency = good
p99 latency = terrible

Event-loop lag = 800 ms
```

This immediately told us:

> The problem might not be downstream I/O.

We looked for JavaScript that was blocking the event loop.

Examples included:

```text
Large JSON.parse()
Large JSON.stringify()
CPU-heavy loops
Synchronous filesystem operations
Expensive regular expressions
Large object transformations
Unexpected synchronous dependencies
```

We used:

* CPU profiling
* Flame graphs
* APM traces
* Event-loop monitoring
* Node.js performance APIs
* Heap profiling
* GC metrics

The key lesson:

> **Async I/O doesn't make CPU-bound JavaScript asynchronous.**

If a Promise resolves and its continuation does:

```js
for (let i = 0; i < hugeNumber; i++) {
  expensiveCalculation();
}
```

that work executes on the JavaScript thread.

# 23. What If 100 I/O Operations Finish Together?

Suppose 100 database requests complete almost simultaneously.

Their callbacks/Promise continuations become ready.

If each continuation performs:

```text
500 ms CPU work
```

we now have a major event-loop problem.

Conceptually:

```text
100 completions
      ↓
100 callbacks
      ↓
CPU-heavy JavaScript
      ↓
Event loop busy
      ↓
Other requests wait
      ↓
p99 latency increases
```

Even though the original operation was asynchronous, the work after it isn't magically parallel.

This is why we distinguish:

```text
I/O-bound
vs
CPU-bound
```

# 24. CPU Scaling vs I/O Scaling

At one point we had:

```text
16 CPU cores
1 Node.js process

CPU = 20%
Thousands of I/O operations
```

Someone suggested:

> "Let's add worker threads."

But if the bottleneck is:

```text
PostgreSQL
```

worker threads won't make PostgreSQL faster.

If the bottleneck is:

```text
External API
```

more CPU doesn't solve it.

If the bottleneck is:

```text
DB connection pool
```

adding CPU doesn't solve it.

So our rule became:

```text
I/O bottleneck
    ↓
Connection pools
Concurrency limits
Caching
Horizontal scaling
Backpressure

CPU bottleneck
    ↓
Worker Threads
Multiple processes
Horizontal CPU scaling
```

The principle:

> **Scale the bottleneck, not the CPU blindly.**

# 25. Graceful Shutdown

We also had to think about deployment.

Kubernetes sends:

```text
SIGTERM
```

while the instance may have:

```text
20,000 in-flight I/O operations
```

We didn't want to simply do:

```js
process.exit(0);
```

because that could terminate active work.

Instead:

```text
SIGTERM
   ↓
Stop accepting new traffic
   ↓
Remove instance from load balancer
   ↓
Stop starting new work
   ↓
Allow in-flight requests to finish
   ↓
Drain queues
   ↓
Cancel work that exceeded deadlines
   ↓
Commit / rollback transactions
   ↓
Close Redis / DB connections
   ↓
Exit
```

This is controlled draining.

Graceful shutdown is essentially:

> **Stop taking new work and safely drain existing work.**

# 26. The Complete Failure Chain

Now let's put the entire incident together.

PostgreSQL latency changes:

```text
10 ms
  ↓
2 seconds
```

That causes:

```text
Queries take longer
       ↓
DB connections remain occupied longer
       ↓
Connection pool saturates
       ↓
Requests wait
       ↓
Request concurrency increases
       ↓
Memory usage increases
       ↓
Timeouts begin
       ↓
Retries begin
       ↓
Database receives more traffic
       ↓
Database becomes even slower
       ↓
More timeouts
       ↓
More retries
       ↓
Cascading failure
```

This is the most important chain to understand.

# 27. Our Protection Layers

We eventually designed the system with multiple control points.

```text
                    Incoming Traffic
                           │
                           ↓
                  ┌─────────────────┐
                  │ Load Shedding   │
                  └────────┬────────┘
                           ↓
                  ┌─────────────────┐
                  │ Rate Limiting   │
                  └────────┬────────┘
                           ↓
                  ┌─────────────────┐
                  │ Request Deadline│
                  └────────┬────────┘
                           ↓
                  ┌─────────────────┐
                  │ Concurrency     │
                  │ Limits          │
                  └────────┬────────┘
                           ↓
                ┌─────────────────────┐
                │ Dependency Controls │
                └──────────┬──────────┘
                           │
             ┌─────────────┼─────────────┐
             ↓             ↓             ↓
          Redis        PostgreSQL    External API
             │             │             │
          timeout       timeout       timeout
             │             │             │
          breaker       breaker       breaker
             │             │             │
             └─────────────┼─────────────┘
                           ↓
                    Retry Carefully
                           ↓
                    Queue / Kafka
                           ↓
                       Workers
```

Each layer exists for a different reason.

# 28. The Mental Model I Use in Interviews

When an interviewer gives me an I/O-heavy Node.js problem, I don't start by talking about Promises.

I ask:

### 1. What is the bottleneck?

```text
CPU?
DB?
Network?
External API?
Memory?
Connections?
```

### 2. How much concurrency can each dependency handle?

```text
DB → 100?
API → 500?
Redis → 1,000?
```

### 3. Where is the queue?

Every system has queues somewhere.

It might be:

```text
Connection pool
Socket buffer
Node.js memory
Kafka
Database
Load balancer
```

If I don't explicitly control the queue, the system will create one somewhere else.

### 4. What happens when the dependency becomes slow?

This is the most important question.

```text
Normal latency
     ↓
Slow dependency
     ↓
Longer resource occupancy
     ↓
Queue grows
     ↓
Latency grows
     ↓
Timeouts
     ↓
Retries
     ↓
More load
```

### 5. How do we stop the chain?

We use:

```text
Timeouts
Concurrency limits
Backpressure
Bulkheads
Circuit breakers
Caching
Queues
Load shedding
Safe retries
```

# 29. The Final Senior-Level Answer

If the interviewer asks:

> "What is the biggest lesson from this architecture?"

I would answer:

> Node.js gives us an efficient model for handling large amounts of I/O because the JavaScript event loop doesn't need to block while external I/O is in progress. But that does not mean the system has unlimited capacity.
>
> Every downstream dependency has finite resources: database connections, sockets, CPU, memory, API quotas, queues, and throughput.
>
> The real challenge in a high-concurrency Node.js system is controlling how much work we allow into those finite resources.
>
> So I design around bounded concurrency, connection pools, request deadlines, timeouts, backpressure, bulkheads, circuit breakers, safe retries, caching, queues, and load shedding.
>
> I also distinguish I/O-bound work from CPU-bound JavaScript because asynchronous I/O doesn't make CPU-heavy JavaScript asynchronous.
>
> Ultimately, the goal isn't to process every request at any cost. The goal is to keep the system stable under both normal traffic and degraded dependencies.

# 30. The One Diagram to Remember

If I had only 30 seconds before an interview, I'd remember this:

```text
                         TRAFFIC
                            │
                            ↓
                    ┌──────────────┐
                    │ Rate Limit   │
                    └──────┬───────┘
                           ↓
                    ┌──────────────┐
                    │ Concurrency  │
                    │   Limit      │
                    └──────┬───────┘
                           ↓
                    ┌──────────────┐
                    │   Deadline   │
                    │   Timeout    │
                    └──────┬───────┘
                           ↓
              ┌────────────┼────────────┐
              ↓            ↓            ↓
            Redis        Postgres    External API
              │            │            │
              ↓            ↓            ↓
           Pool/Limit   Pool/Limit   Pool/Limit
              │            │            │
           Timeout      Timeout      Timeout
              │            │            │
          Breaker       Breaker      Breaker
              │            │            │
              └────────────┼────────────┘
                           ↓
                    Retry Carefully
                           ↓
                    Queue / Kafka
                           ↓
                       Workers
                           ↓
                    Controlled Load
```

And around the entire system:

```text
             ┌─────────────────────────┐
             │     BACKPRESSURE         │
             │     LOAD SHEDDING        │
             │     OBSERVABILITY        │
             │     GRACEFUL SHUTDOWN    │
             └─────────────────────────┘
```

The final mental model is:

```text
ASYNC I/O
   ≠
UNLIMITED CONCURRENCY
   ≠
UNLIMITED CAPACITY

Instead:

REQUESTS
   ↓
CONTROL CONCURRENCY
   ↓
BOUND RESOURCES
   ↓
APPLY BACKPRESSURE
   ↓
SET DEADLINES
   ↓
FAIL FAST WHEN NECESSARY
   ↓
QUEUE NON-CRITICAL WORK
   ↓
PROTECT DEPENDENCIES
   ↓
KEEP THE SYSTEM STABLE
```

**That's the Staff-level Node.js mindset:** don't just ask *"How do I make this operation asynchronous?"* Ask *"How much work can the entire system safely handle, where will it queue, and what happens when the slowest dependency becomes 100× slower?"*
