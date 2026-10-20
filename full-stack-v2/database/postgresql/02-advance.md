Yes. Let's go **deep down**, not just memorize interview definitions.

For **3 years of development experience**, the goal is to understand PostgreSQL at three levels:

> **SQL → PostgreSQL internals → production/debugging**

I'll teach it like an interview revision, with the **"why" behind everything**.

# PostgreSQL Deep Revision — 3 YOE

## 0. First: Understand What PostgreSQL Actually Does

When your application executes:

```sql
SELECT *
FROM users
WHERE email = 'rahul@example.com';
```

there are several layers involved:

```text
Application
    ↓
Connection
    ↓
PostgreSQL Parser
    ↓
Analyzer / Rewriter
    ↓
Query Planner / Optimizer
    ↓
Execution Plan
    ↓
Storage / Buffer Cache
    ↓
Tables + Indexes
    ↓
Result
```

A 3-YOE developer should understand **what happens at each stage**.

---

# 1. Query Lifecycle

Suppose we run:

```sql
SELECT name
FROM users
WHERE age > 25
ORDER BY name;
```

### Step 1 — Parser

PostgreSQL checks whether the SQL syntax is valid.

For example:

```sql
SELECT FROM users
```

is syntactically invalid.

---

### Step 2 — Semantic analysis

PostgreSQL determines:

* Does `users` exist?
* Does `age` exist?
* Does `name` exist?
* Are the types compatible?
* What does each identifier refer to?

---

### Step 3 — Planner

Now PostgreSQL asks:

> "What's the cheapest way to execute this?"

Possible plans:

```text
Sequential Scan
```

or:

```text
Index Scan
```

or:

```text
Bitmap Heap Scan
   ↓
Bitmap Index Scan
```

Then it estimates the cost.

---

### Step 4 — Executor

The selected plan is actually executed.

This distinction is critical:

```text
Planner = decides
Executor = executes
```

---

# 2. Sequential Scan

Suppose:

```text
users = 10 million rows
```

and we execute:

```sql
SELECT *
FROM users
WHERE age = 25;
```

A sequential scan essentially does:

```text
row 1 → check age
row 2 → check age
row 3 → check age
...
row 10,000,000 → check age
```

But don't conclude:

> Sequential scan = bad.

That's a common beginner mistake.

If:

```text
90% of rows have age = 25
```

using an index might actually be worse.

PostgreSQL may reasonably decide:

```text
"Most rows are needed anyway.
I'll just scan the table."
```

---

# 3. Why Indexes Exist

Without an index:

```text
Table
 ↓
scan lots of rows
```

With an appropriate index:

```text
Query
 ↓
Index
 ↓
matching row locations
 ↓
Table
```

For example:

```sql
CREATE INDEX idx_users_email
ON users(email);
```

Now:

```sql
SELECT *
FROM users
WHERE email = 'rahul@example.com';
```

can potentially find the matching row much more efficiently.

---

# 4. But What Is an Index Internally?

This is where we go deeper.

A PostgreSQL B-tree index is conceptually a balanced tree:

```text
                 [50]
               /      \
          [10,20]    [70,90]
          /  |  \     /  |  \
        ... ... ...  ... ... ...
```

It allows PostgreSQL to navigate toward a value instead of examining every table row.

The index contains key information and references to table rows.

Conceptually:

```text
Index

email                  row location
------------------------------------
a@example.com          → table row
b@example.com          → table row
c@example.com          → table row
```

That's why:

```sql
WHERE email = ...
```

can be much faster with a suitable index.

---

# 5. Why Indexes Can Make Writes Slower

Suppose you have:

```sql
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_name ON users(name);
CREATE INDEX idx_users_age ON users(age);
```

Then:

```sql
INSERT INTO users ...
```

doesn't only need to modify the table.

PostgreSQL also needs to maintain those indexes.

Conceptually:

```text
INSERT
 ├── table
 ├── email index
 ├── name index
 └── age index
```

Therefore:

> Indexes improve some reads but introduce write and storage overhead.

This is an extremely important production concept.

---

# 6. Composite Index — Deep Understanding

Consider:

```sql
CREATE INDEX idx_orders
ON orders(customer_id, status, created_at);
```

Think of it conceptually as sorted by:

```text
customer_id
    ↓
status
    ↓
created_at
```

So:

```sql
WHERE customer_id = 100
```

is a natural match.

And:

```sql
WHERE customer_id = 100
  AND status = 'PAID'
```

is also a natural match.

And:

```sql
WHERE customer_id = 100
  AND status = 'PAID'
  AND created_at >= ...
```

can be a strong match.

But:

```sql
WHERE status = 'PAID'
```

doesn't get the same benefit from the leading `customer_id` column.

### Interview question

> Why does index column order matter?

Answer:

Because PostgreSQL's B-tree index is ordered according to the indexed column sequence. Queries that constrain the leading columns can generally exploit the index more effectively.

---

# 7. Selectivity

This is an important word.

**Selectivity = how narrowly a condition identifies rows.**

Suppose:

```text
users = 10 million
```

Query:

```sql
WHERE gender = 'M'
```

Suppose 5 million rows match.

That's low selectivity.

But:

```sql
WHERE email = 'x@example.com'
```

might match one row.

That's highly selective.

Generally:

```text
high selectivity
      ↓
index becomes more attractive
```

But the planner considers many factors, not just selectivity.

---

# 8. EXPLAIN — Your Production Weapon

Never optimize a SQL query purely by guessing.

Use:

```sql
EXPLAIN
SELECT *
FROM users
WHERE email = 'rahul@example.com';
```

You might see:

```text
Index Scan using idx_users_email on users
```

Now use:

```sql
EXPLAIN ANALYZE
SELECT *
FROM users
WHERE email = 'rahul@example.com';
```

This actually executes the query and provides actual execution information.

---

# 9. Estimated Rows vs Actual Rows

Suppose:

```text
estimated rows = 10
actual rows    = 500000
```

That's a huge difference.

The planner thought:

```text
"Only 10 rows."
```

But reality was:

```text
"500,000 rows!"
```

That can cause a poor plan.

Potential causes include:

* stale statistics
* unusual data distribution
* insufficient statistics detail
* correlated columns
* planner estimation limitations

This is why:

```sql
ANALYZE users;
```

matters.

---

# 10. What Does ANALYZE Do?

`ANALYZE` collects statistical information about the table.

The planner uses those statistics to estimate:

```text
How many rows will this condition return?
```

That helps it choose between:

```text
Index Scan
```

and:

```text
Sequential Scan
```

among other plan choices.

---

# 11. VACUUM — Go Deep Here

This is one of the most PostgreSQL-specific concepts.

PostgreSQL uses **MVCC**.

Imagine:

```text
User balance = 100
```

Transaction updates it:

```sql
UPDATE accounts
SET balance = 200
WHERE id = 1;
```

PostgreSQL doesn't simply behave like:

```text
100 → destroyed
200 → stored
```

Conceptually, multiple row versions can exist.

```text
Old version: balance = 100
New version: balance = 200
```

Different transactions may need different versions depending on their snapshots.

Eventually, old versions become unnecessary.

Those are **dead tuples**.

VACUUM helps PostgreSQL deal with this accumulated dead-row-version information and maintain storage/visibility metadata.

---

# 12. Why UPDATE Can Be Expensive

This is a subtle PostgreSQL interview topic.

Suppose:

```sql
UPDATE users
SET name = 'Rahul'
WHERE id = 10;
```

Conceptually, because PostgreSQL uses MVCC, the update creates a new row version rather than simply overwriting the old version in place in the simplistic sense.

That means:

```text
UPDATE
 ↓
new row version
 ↓
old version eventually becomes dead
 ↓
VACUUM helps clean/reclaim
```

Therefore heavy update/delete workloads can create lots of dead tuples.

---

# 13. MVCC

**Multi-Version Concurrency Control**

Imagine two transactions:

```text
Transaction A
Transaction B
```

A reads:

```text
balance = 100
```

B updates the balance:

```text
balance = 200
```

A's snapshot can continue to provide the version appropriate to its transaction visibility rules.

This allows PostgreSQL to support high concurrency without requiring readers and writers to block each other in the simplistic way a lock-only system would.

---

# 14. Transactions

A transaction is a logical unit of work.

Example money transfer:

```sql
BEGIN;

UPDATE accounts
SET balance = balance - 100
WHERE id = 1;

UPDATE accounts
SET balance = balance + 100
WHERE id = 2;

COMMIT;
```

If something fails:

```sql
ROLLBACK;
```

We don't want:

```text
Account A: -100
Account B: +0
```

We want both changes to succeed or neither to happen.

---

# 15. ACID — But Don't Give the Wikipedia Answer

### Atomicity

Transaction is all-or-nothing.

### Consistency

Database constraints/invariants remain valid.

### Isolation

Concurrent transactions are controlled according to the isolation level.

### Durability

Committed changes survive crashes according to PostgreSQL's durability mechanisms/configuration.

Interview follow-up:

> "Which PostgreSQL feature is important for durability?"

Think:

```text
WAL
```

---

# 16. WAL — Deep Concept

**Write-Ahead Log**

The basic principle:

> Record the required change information in WAL before relying on the corresponding data-page changes being durable.

Conceptually:

```text
Application
    ↓
UPDATE
    ↓
WAL record
    ↓
data pages
```

If PostgreSQL crashes, WAL helps PostgreSQL recover the database into a consistent state.

WAL is also fundamental to replication.

---

# 17. Isolation Levels

PostgreSQL commonly uses:

```text
READ COMMITTED
REPEATABLE READ
SERIALIZABLE
```

### READ COMMITTED

Default.

Each statement gets its own snapshot.

Consider:

```text
BEGIN;

SELECT balance FROM accounts WHERE id = 1;

-- another transaction commits an update

SELECT balance FROM accounts WHERE id = 1;
```

The two statements can see different committed versions.

---

# 18. REPEATABLE READ

The transaction gets a consistent snapshot for its reads.

Conceptually:

```text
BEGIN
 ↓
snapshot
 ↓
SELECT
 ↓
other transactions commit
 ↓
SELECT again
 ↓
same snapshot perspective
```

PostgreSQL's implementation can still abort a transaction in certain concurrent-update situations.

---

# 19. SERIALIZABLE

The goal is:

> Concurrent transactions should behave as though they executed serially.

This provides stronger correctness guarantees, but it can result in:

```text
serialization failure
```

So application code may need:

```text
retry transaction
```

This is a very good production-level interview point.

---

# 20. Row Locks

Suppose two users try to buy the last item.

You might use:

```sql
BEGIN;

SELECT *
FROM inventory
WHERE product_id = 10
FOR UPDATE;

UPDATE inventory
SET quantity = quantity - 1
WHERE product_id = 10;

COMMIT;
```

`FOR UPDATE` asks PostgreSQL to lock the selected rows against conflicting updates until the transaction ends.

This is fundamentally different from simply doing:

```sql
SELECT quantity ...
```

and then updating later without considering concurrency.

---

# 21. Race Condition

Imagine stock:

```text
quantity = 1
```

Two requests:

```text
Request A             Request B
---------             ---------
read quantity = 1     read quantity = 1

buy                   buy

quantity = 0         quantity = 0
```

You may accidentally sell two units.

The database transaction/locking strategy must prevent this.

This is where PostgreSQL knowledge becomes **backend engineering**, not just SQL.

---

# 22. Deadlocks

Transaction A:

```text
lock user 1
wait for user 2
```

Transaction B:

```text
lock user 2
wait for user 1
```

Graph:

```text
A → B
↑   ↓
└───┘
```

Deadlock.

PostgreSQL detects it and aborts one transaction.

### Prevention

Use consistent lock ordering:

```text
always lock lower ID first
```

For example:

```text
user 1 → user 2
```

rather than sometimes:

```text
1 → 2
```

and elsewhere:

```text
2 → 1
```

---

# 23. `SELECT FOR UPDATE` vs `SELECT`

This is a favorite practical question.

Normal:

```sql
SELECT *
FROM accounts
WHERE id = 1;
```

means:

> Read this row.

With:

```sql
SELECT *
FROM accounts
WHERE id = 1
FOR UPDATE;
```

you're saying, conceptually:

> I'm going to modify this row; protect me from conflicting concurrent updates according to the lock semantics.

---

# 24. `FOR UPDATE SKIP LOCKED`

This is an advanced but valuable PostgreSQL feature.

Imagine a job queue:

```text
jobs
----
1
2
3
4
5
```

Multiple workers need jobs.

You can use:

```sql
SELECT *
FROM jobs
WHERE status = 'pending'
ORDER BY id
FOR UPDATE SKIP LOCKED
LIMIT 1;
```

Instead of waiting on a locked job, another worker can skip it and take another available job.

Very useful for queue-like workloads.

---

# 25. `FOR UPDATE NOWAIT`

Another option:

```sql
FOR UPDATE NOWAIT
```

Instead of waiting for a conflicting lock, PostgreSQL errors immediately if the row cannot be locked.

Useful when:

> "I don't want to wait."

---

# 26. UPSERT — Production Example

Suppose an email must be unique.

```sql
CREATE TABLE users (
    id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    email TEXT UNIQUE,
    name TEXT
);
```

Then:

```sql
INSERT INTO users(email, name)
VALUES ('a@example.com', 'Rahul')
ON CONFLICT (email)
DO UPDATE
SET name = EXCLUDED.name;
```

`EXCLUDED` means:

> the row that was proposed for insertion.

This is much better than naïvely doing:

```text
SELECT whether exists
       ↓
if no:
INSERT
```

because that can have race conditions under concurrency.

---

# 27. Why `SELECT THEN INSERT` Can Be Dangerous

Bad concurrency pattern:

```text
Request A                  Request B

SELECT email              SELECT email
not found                 not found

INSERT                    INSERT
```

Both may think:

```text
"It doesn't exist."
```

A unique constraint plus `ON CONFLICT` lets PostgreSQL enforce the invariant safely.

This is a crucial principle:

> **Use database constraints for database invariants.**

Don't rely only on application-side checks.

---

# 28. Constraints — Very Deep Interview Principle

Suppose:

```sql
email TEXT UNIQUE
```

Why have the database enforce it?

Because your application can have:

```text
10 application servers
100 concurrent requests
```

All of them interact with the same database.

The database is the final authority on the constraint.

Important constraints:

```text
PRIMARY KEY
UNIQUE
FOREIGN KEY
CHECK
NOT NULL
EXCLUSION constraints
```

---

# 29. CHECK Constraints

Example:

```sql
CREATE TABLE products (
    price NUMERIC CHECK (price >= 0)
);
```

Now:

```sql
INSERT INTO products(price)
VALUES (-100);
```

fails.

This is better than relying exclusively on application code.

---

# 30. Foreign Keys — Deeply

Suppose:

```text
customers
---------
id

orders
------
id
customer_id
```

Foreign key:

```sql
FOREIGN KEY (customer_id)
REFERENCES customers(id)
```

means:

> An order cannot reference a nonexistent customer, subject to the configured constraint behavior and transaction state.

Possible delete actions:

```text
CASCADE
SET NULL
RESTRICT
NO ACTION
```

Know the difference conceptually.

---

# 31. NULL — Interview Trap

`NULL` does not mean:

```text
0
```

or:

```text
''
```

or:

```text
false
```

It means:

> unknown / missing / not applicable depending on the data model.

Therefore:

```sql
WHERE name = NULL
```

is wrong.

Use:

```sql
WHERE name IS NULL
```

---

# 32. Three-Valued Logic

SQL has:

```text
TRUE
FALSE
UNKNOWN
```

For example:

```sql
5 = NULL
```

is:

```text
UNKNOWN
```

not false in the ordinary Boolean sense.

This is why:

```sql
WHERE x = NULL
```

doesn't work as people expect.

This also explains the famous:

```sql
NOT IN
```

* `NULL` trap.

---

# 33. `NOT IN` vs `NOT EXISTS`

Suppose:

```sql
WHERE customer_id NOT IN (
    SELECT customer_id
    FROM orders
)
```

If the subquery contains `NULL`, three-valued logic can produce surprising results.

Often safer:

```sql
WHERE NOT EXISTS (
    SELECT 1
    FROM orders o
    WHERE o.customer_id = c.id
)
```

This is a classic interview question.

---

# 34. Window Functions — Deep

Suppose:

```text
employees

id | department | salary
1  | IT         | 100
2  | IT         | 90
3  | IT         | 90
4  | HR         | 80
```

You want each employee plus department ranking.

```sql
SELECT *,
       RANK() OVER (
           PARTITION BY department
           ORDER BY salary DESC
       ) AS salary_rank
FROM employees;
```

Result conceptually:

```text
1 | IT | 100 | 1
2 | IT | 90  | 2
3 | IT | 90  | 2
4 | HR | 80  | 1
```

### Important distinction

`GROUP BY`:

```text
many rows
   ↓
one row per group
```

Window function:

```text
many rows
   ↓
same number of rows + calculated information
```

---

# 35. ROW_NUMBER vs RANK vs DENSE_RANK

Given:

```text
100
90
90
80
```

### ROW_NUMBER

```text
100 → 1
90  → 2
90  → 3
80  → 4
```

### RANK

```text
100 → 1
90  → 2
90  → 2
80  → 4
```

### DENSE_RANK

```text
100 → 1
90  → 2
90  → 2
80  → 3
```

Interview question:

> Find the second-highest distinct salary.

`DENSE_RANK()` is often a natural solution.

---

# 36. Top N Per Group

One of the most useful SQL interview problems.

> Find the top 3 employees by salary in every department.

```sql
SELECT *
FROM (
    SELECT
        e.*,
        ROW_NUMBER() OVER (
            PARTITION BY department_id
            ORDER BY salary DESC
        ) AS rn
    FROM employees e
) x
WHERE rn <= 3;
```

If ties should all be included, consider:

```sql
RANK()
```

or:

```sql
DENSE_RANK()
```

depending on the required semantics.

---

# 37. Pagination — Very Important

Basic pagination:

```sql
SELECT *
FROM products
ORDER BY id
LIMIT 20 OFFSET 100000;
```

Problem:

Large offsets can become increasingly expensive because PostgreSQL may need to walk/skip many rows before returning the requested page.

---

# 38. Keyset Pagination

Instead:

```sql
SELECT *
FROM products
WHERE id > 100000
ORDER BY id
LIMIT 20;
```

This is often called:

```text
cursor pagination
keyset pagination
```

It can scale much better for large datasets when the ordering/filtering is designed appropriately.

This is a very good backend interview topic.

---

# 39. JSONB Deep Dive

Suppose:

```sql
CREATE TABLE users (
    id BIGINT,
    metadata JSONB
);
```

Data:

```json
{
  "name": "Rahul",
  "age": 28,
  "skills": ["Java", "PostgreSQL"]
}
```

Access:

```sql
metadata->'skills'
```

returns JSON.

While:

```sql
metadata->>'name'
```

returns text.

This distinction is important.

---

# 40. JSONB Indexing

Suppose you frequently query JSONB:

```sql
WHERE metadata @> '{"role":"admin"}'
```

A GIN index can be appropriate:

```sql
CREATE INDEX idx_users_metadata
ON users
USING GIN (metadata);
```

But don't say:

> "GIN is always better than B-tree."

They solve different access patterns.

---

# 41. B-tree vs GIN

### B-tree

General-purpose.

Good for things such as:

```text
=
<
>
<=
>=
ORDER BY
```

### GIN

Generalized Inverted Index.

Useful for structures such as:

```text
JSONB
arrays
full-text-search-related workloads
```

depending on the operator class and query.

Interview answer should be:

> Choose the index type according to the operators and data structure your workload needs.

---

# 42. Partial Index — Real Scenario

Suppose:

```text
orders = 100 million
pending orders = 50,000
```

Your application constantly queries:

```sql
WHERE status = 'PENDING'
```

Potentially:

```sql
CREATE INDEX idx_pending_orders
ON orders(created_at)
WHERE status = 'PENDING';
```

Now you're indexing only the relevant subset.

This can be dramatically smaller than indexing all 100 million rows.

---

# 43. Expression Index

Query:

```sql
SELECT *
FROM users
WHERE LOWER(email) = 'abc@example.com';
```

Index:

```sql
CREATE INDEX idx_users_lower_email
ON users (LOWER(email));
```

This is useful because the indexed expression matches the query expression.

---

# 44. Why `SELECT *` Is Often Bad

Imagine:

```text
users has 50 columns
```

but API needs:

```text
id
name
email
```

Don't blindly do:

```sql
SELECT *
```

Reasons:

* more data transferred
* more memory
* potentially more I/O
* can prevent some index-only scan opportunities
* couples query to schema changes

Prefer:

```sql
SELECT id, name, email
FROM users;
```

---

# 45. Connection Pooling

Suppose:

```text
1000 requests/sec
```

You don't want:

```text
request
 ↓
create DB connection
 ↓
query
 ↓
destroy connection
```

for every request.

Instead:

```text
Application
     ↓
Connection Pool
 ┌───┼───┼───┐
 DB  DB  DB  DB
```

Connections are reused.

But there's a trap:

> Bigger pool ≠ always better.

Too many concurrent DB connections can cause:

```text
CPU contention
memory pressure
context switching
lock contention
```

The database itself has finite resources.

---

# 46. N+1 Query

Suppose API:

```text
GET /users
```

runs:

```text
1 query → users
```

Then for every user:

```text
1 query → orders
```

For 100 users:

```text
1 + 100 = 101 queries
```

That's N+1.

Possible solutions:

```text
JOIN
batch query
IN (...)
eager loading
```

But again, don't blindly use joins if the resulting row multiplication becomes enormous.

---

# 47. Slow Query Investigation

This is the answer you should be able to give in an interview:

> "How would you debug a slow PostgreSQL query?"

### Step 1

Get the exact SQL.

Don't optimize:

```text
"the API is slow"
```

You need:

```text
actual query
parameters
frequency
duration
```

### Step 2

Run:

```sql
EXPLAIN (ANALYZE, BUFFERS)
...
```

### Step 3

Check:

```text
Seq Scan?
Index Scan?
Bitmap Scan?
Sort?
Hash Join?
Nested Loop?
```

### Step 4

Compare:

```text
estimated rows
actual rows
```

### Step 5

Check:

```text
indexes
statistics
locks
I/O
CPU
connection pool
```

### Step 6

Fix.

### Step 7

Measure again.

Never stop at:

> "I added an index."

---

# 48. JOIN Algorithms

This is a level deeper.

PostgreSQL can use different join strategies.

### Nested Loop

Conceptually:

```text
for each row in A:
    find matching rows in B
```

Can be excellent when one side is small and the other has an efficient index/access path.

---

### Hash Join

Conceptually:

```text
build hash structure for one side
            ↓
probe with rows from other side
```

Often useful for equality joins.

---

### Merge Join

Conceptually:

```text
sorted A
   +
sorted B
   ↓
merge
```

Useful when inputs are appropriately sorted/ordered.

---

# 49. Why Query Plans Matter

Suppose:

```sql
SELECT *
FROM orders o
JOIN customers c
  ON c.id = o.customer_id;
```

You shouldn't assume:

```text
"PostgreSQL will always use an index."
```

The planner could choose:

```text
Nested Loop
```

or:

```text
Hash Join
```

or:

```text
Merge Join
```

based on estimated cost.

This is why memorizing:

> "JOIN = index"

is incorrect.

---

# 50. Production Scenario — Inventory

Interview question:

> "Two users buy the last product simultaneously. How do you prevent overselling?"

Strong answer:

```text
BEGIN

lock/read the inventory row appropriately

check quantity

if quantity > 0:
    decrement quantity

COMMIT
```

For example:

```sql
BEGIN;

SELECT quantity
FROM inventory
WHERE product_id = 10
FOR UPDATE;

UPDATE inventory
SET quantity = quantity - 1
WHERE product_id = 10
  AND quantity > 0;

COMMIT;
```

But a very important refinement:

You should check the affected row count and design the transaction carefully. The exact pattern depends on whether you need to return the updated row, handle retries, and enforce business invariants.

---

# 51. Better Atomic Update Pattern

Sometimes you can avoid a separate read:

```sql
UPDATE inventory
SET quantity = quantity - 1
WHERE product_id = 10
  AND quantity > 0
RETURNING quantity;
```

Then:

```text
row returned
    ↓
success

no row returned
    ↓
out of stock
```

This can be a very elegant concurrency-safe approach when it matches the business logic.

That's the kind of PostgreSQL thinking interviewers like.

---

# 52. PostgreSQL Questions You Should Be Able To Explain

At 3 YOE, I would expect you to explain these **without memorized definitions**:

### Internals

* What is MVCC?
* Why do dead tuples exist?
* What does VACUUM do?
* Why is autovacuum important?
* What is WAL?
* What happens during crash recovery?
* What is a snapshot?
* How does PostgreSQL handle concurrent reads/writes?

### Query performance

* How does an index work?
* B-tree vs GIN?
* Composite index ordering?
* Partial index?
* Expression index?
* Why can PostgreSQL ignore an index?
* What does `EXPLAIN ANALYZE` tell you?
* What are estimated vs actual rows?
* What is a bitmap heap scan?
* Nested loop vs hash join?

### Concurrency

* What is a transaction?
* Isolation levels?
* Row locks?
* `FOR UPDATE`?
* Deadlocks?
* Race conditions?
* `SKIP LOCKED`?
* Serialization failure?

### SQL

* JOINs
* CTEs
* subqueries
* window functions
* `EXISTS`
* `NOT EXISTS`
* `NULL`
* `GROUP BY`
* `HAVING`
* top-N-per-group
* keyset pagination

### Production

* connection pooling
* replication
* replication lag
* read/write split
* migrations
* indexes on large tables
* slow-query debugging
* N+1
* transactions around external services

---

# The Mental Model I Want You To Remember

Don't think of PostgreSQL as:

```text
"Somewhere I write SQL."
```

Think:

```text
                    PostgreSQL
                         │
          ┌──────────────┼──────────────┐
          ↓              ↓              ↓
        SQL           Concurrency     Storage
          │              │              │
          ↓              ↓              ↓
       Planner          MVCC           Pages
          │              │              │
          ↓              ↓              ↓
      EXPLAIN           Locks           WAL
          │              │              │
          ↓              ↓              ↓
      Indexes        Transactions     VACUUM
          │              │              │
          └──────────────┼──────────────┘
                         ↓
                    Performance
```

If you understand that picture, you're moving from **"I know PostgreSQL syntax"** to **"I understand PostgreSQL as a backend engineer."**

## 🔥 Next: Interview Mode

Let's make this genuinely useful instead of giving you another huge theory dump.

I'll act as a **senior PostgreSQL interviewer** and take you through progressively harder questions:

**Level 1 → SQL**
**Level 2 → Advanced SQL**
**Level 3 → Indexes & EXPLAIN**
**Level 4 → Transactions & MVCC**
**Level 5 → Locks & concurrency**
**Level 6 → Production debugging**
**Level 7 → System-design-style PostgreSQL scenarios**

And I won't give you the answer first. I'll ask **one question at a time**, evaluate your answer like an actual interview, correct it, and drill into follow-ups.
