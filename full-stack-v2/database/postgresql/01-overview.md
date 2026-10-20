Absolutely. With **~3 years of development experience**, we should revise PostgreSQL at an **interview + practical production level**, not beginner SQL.

I’d structure this as a **deep rapid revision**, focusing on the questions interviewers actually use to distinguish a 3-year developer from someone who only knows basic CRUD.

# PostgreSQL Interview Revision — 3 YOE

## 1. SQL Fundamentals — must be automatic

You should be comfortable with:

* `SELECT`
* `WHERE`
* `ORDER BY`
* `GROUP BY`
* `HAVING`
* `DISTINCT`
* `LIMIT/OFFSET`
* `CASE`
* `COALESCE`
* `NULLIF`
* `IN`
* `BETWEEN`
* `LIKE` / `ILIKE`
* `EXISTS`
* Subqueries
* CTEs
* Set operations:

  * `UNION`
  * `UNION ALL`
  * `INTERSECT`
  * `EXCEPT`

### Important interview distinction

```sql
WHERE
```

filters **rows before grouping**.

```sql
HAVING
```

filters **groups after aggregation**.

Example:

```sql
SELECT department_id, COUNT(*)
FROM employees
WHERE salary > 50000
GROUP BY department_id
HAVING COUNT(*) > 5;
```

Execution conceptually happens roughly as:

```text
FROM
  ↓
WHERE
  ↓
GROUP BY
  ↓
HAVING
  ↓
SELECT
  ↓
ORDER BY
  ↓
LIMIT
```

Don't confuse this with the exact physical execution plan—the optimizer can execute things differently.

---

# 2. JOINs — extremely important

Know these cold:

### INNER JOIN

Returns matching rows.

```sql
SELECT e.name, d.name
FROM employees e
INNER JOIN departments d
    ON e.department_id = d.id;
```

### LEFT JOIN

Returns **all rows from the left table**, even when there is no match.

```sql
SELECT e.name, d.name
FROM employees e
LEFT JOIN departments d
    ON e.department_id = d.id;
```

If an employee has no department:

```text
e.name     d.name
John       NULL
```

### RIGHT JOIN

Same concept from the other direction.

### FULL OUTER JOIN

Returns matching + unmatched rows from both sides.

### CROSS JOIN

Cartesian product.

If:

```text
A = 5 rows
B = 4 rows
```

then:

```sql
A CROSS JOIN B
```

produces:

```text
5 × 4 = 20 rows
```

---

# 3. The classic LEFT JOIN interview trap

Suppose:

```sql
employees
---------
id
name
department_id
```

You want employees who don't belong to any department.

```sql
SELECT e.*
FROM employees e
LEFT JOIN departments d
    ON e.department_id = d.id
WHERE d.id IS NULL;
```

This is a very common interview question.

But watch this:

```sql
SELECT e.*
FROM employees e
LEFT JOIN departments d
    ON e.department_id = d.id
WHERE d.name = 'Engineering';
```

The `WHERE` condition eliminates the `NULL` rows, making this behave like an inner join for that condition.

A common alternative is:

```sql
LEFT JOIN departments d
    ON e.department_id = d.id
   AND d.name = 'Engineering'
```

Understanding **ON vs WHERE with outer joins** is important at your experience level.

---

# 4. Aggregation

Know:

```sql
COUNT()
SUM()
AVG()
MIN()
MAX()
```

Example:

```sql
SELECT department_id,
       COUNT(*) AS employee_count,
       AVG(salary) AS avg_salary
FROM employees
GROUP BY department_id;
```

### `COUNT(*)` vs `COUNT(column)`

Very important.

Given:

```text
id | name | manager_id
1  | A    | NULL
2  | B    | 1
3  | C    | NULL
```

```sql
COUNT(*)
```

returns:

```text
3
```

while:

```sql
COUNT(manager_id)
```

returns:

```text
1
```

because `COUNT(column)` doesn't count `NULL`.

---

# 5. Window Functions — HIGH PRIORITY

For 3 YOE, you should absolutely know window functions.

They let you calculate across related rows **without collapsing the rows like GROUP BY does**.

Example:

```sql
SELECT
    employee_id,
    department_id,
    salary,
    AVG(salary) OVER (
        PARTITION BY department_id
    ) AS department_avg
FROM employees;
```

Each employee remains a separate row.

---

## ROW_NUMBER

```sql
ROW_NUMBER() OVER (
    PARTITION BY department_id
    ORDER BY salary DESC
)
```

Produces:

```text
department | employee | salary | row_number
-----------+----------+--------+-----------
IT         | A        | 100000 | 1
IT         | B        | 90000  | 2
IT         | C        | 90000  | 3
```

## RANK

```text
100000 → 1
90000  → 2
90000  → 2
80000  → 4
```

## DENSE_RANK

```text
100000 → 1
90000  → 2
90000  → 2
80000  → 3
```

### Interview question

**Find the second-highest salary in each department.**

```sql
SELECT *
FROM (
    SELECT
        e.*,
        DENSE_RANK() OVER (
            PARTITION BY department_id
            ORDER BY salary DESC
        ) AS rnk
    FROM employees e
) x
WHERE rnk = 2;
```

Know why `DENSE_RANK()` is sometimes preferable to `ROW_NUMBER()`.

---

# 6. CTEs

Common Table Expressions:

```sql
WITH high_salary AS (
    SELECT *
    FROM employees
    WHERE salary > 100000
)
SELECT *
FROM high_salary;
```

Multiple CTEs:

```sql
WITH department_stats AS (
    SELECT department_id, AVG(salary) avg_salary
    FROM employees
    GROUP BY department_id
)
SELECT e.*
FROM employees e
JOIN department_stats d
    ON e.department_id = d.department_id
WHERE e.salary > d.avg_salary;
```

Know that a CTE is primarily a **query organization/composition mechanism**. Modern PostgreSQL can inline eligible CTEs rather than always materializing them.

---

# 7. Subquery vs CTE vs JOIN

Interviewers may ask:

> "How would you solve this?"

Don't memorize one universal answer.

For example:

```sql
SELECT *
FROM employees e
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

could potentially be expressed using a CTE or join.

You should think about:

* readability
* optimizer behavior
* duplicate rows
* whether the subquery is correlated
* whether data needs to be reused
* execution plan

---

# 8. EXISTS vs IN

Very important conceptually.

```sql
SELECT *
FROM customers c
WHERE EXISTS (
    SELECT 1
    FROM orders o
    WHERE o.customer_id = c.id
);
```

This asks:

> Does at least one matching order exist?

`EXISTS` is particularly natural when you only care about existence.

### Important NULL issue

Be careful with:

```sql
NOT IN
```

when the subquery can contain `NULL`.

For anti-joins, this is often safer:

```sql
WHERE NOT EXISTS (
    SELECT 1
    FROM orders o
    WHERE o.customer_id = c.id
)
```

Interviewers love this one.

---

# 9. PostgreSQL Data Types

Know the common types:

### Numeric

```text
smallint
integer
bigint
numeric
decimal
real
double precision
```

### Character

```text
varchar
text
char
```

### Boolean

```text
boolean
```

### Date/time

```text
date
timestamp
timestamptz
time
interval
```

### Other important PostgreSQL types

```text
uuid
json
jsonb
array
enum
inet
```

---

# 10. `timestamp` vs `timestamptz`

Very common PostgreSQL question.

PostgreSQL's:

```sql
timestamp
```

is a timestamp **without time zone**.

```sql
timestamptz
```

represents an instant in time and handles timezone conversion/display according to the session timezone.

For most systems dealing with events such as:

```text
created_at
updated_at
payment_time
login_time
```

`timestamp with time zone` (`timestamptz`) is often the better choice.

---

# 11. JSON vs JSONB

This is a **must know** PostgreSQL-specific topic.

### JSON

Stores JSON text representation.

### JSONB

Stores a decomposed binary representation.

`jsonb` generally provides better querying/indexing capabilities and is the usual choice when you need to query JSON data.

Example:

```sql
CREATE TABLE users (
    id BIGSERIAL PRIMARY KEY,
    profile JSONB
);
```

Data:

```json
{
  "name": "Rahul",
  "skills": ["PostgreSQL", "Java", "Docker"]
}
```

Query:

```sql
SELECT *
FROM users
WHERE profile->>'name' = 'Rahul';
```

Array containment:

```sql
SELECT *
FROM users
WHERE profile @> '{"skills":["PostgreSQL"]}';
```

And you should know that `jsonb` can be indexed, commonly with a GIN index.

---

# 12. Primary Key vs Unique

### Primary key

* Uniquely identifies a row
* Cannot be NULL
* One primary-key constraint per table
* Can be composite

```sql
PRIMARY KEY (user_id)
```

### UNIQUE

Guarantees uniqueness according to the constraint's semantics and can be defined multiple times on a table.

Important PostgreSQL nuance: a normal `UNIQUE` constraint permits multiple `NULL` values because `NULL`s are not considered equal in the usual comparison semantics.

---

# 13. Foreign Keys

Example:

```sql
CREATE TABLE orders (
    id BIGSERIAL PRIMARY KEY,
    customer_id BIGINT REFERENCES customers(id)
);
```

Know:

* referential integrity
* `ON DELETE`
* `ON UPDATE`
* `CASCADE`
* `SET NULL`
* `RESTRICT`
* `NO ACTION`

Example:

```sql
customer_id BIGINT
REFERENCES customers(id)
ON DELETE CASCADE
```

Meaning deleting the customer can delete the related orders.

Be careful with `CASCADE` in production—it can have a large blast radius.

---

# 14. Normalization

Know at least:

### 1NF

Atomic values / no repeating groups.

### 2NF

1NF + no partial dependency on part of a composite key.

### 3NF

2NF + no inappropriate transitive dependencies.

Interview question:

> Why normalize?

Main benefits:

* reduce duplication
* maintain consistency
* avoid update anomalies

But don't say **"always normalize everything."**

Sometimes controlled denormalization improves read performance or simplifies reporting.

---

# 15. Indexes — probably the most important PostgreSQL topic

You need to understand indexes deeply.

An index helps PostgreSQL locate rows without scanning the entire table in cases where the chosen index is useful.

Basic:

```sql
CREATE INDEX idx_users_email
ON users(email);
```

Then:

```sql
SELECT *
FROM users
WHERE email = 'abc@example.com';
```

may use the index.

But:

> An index does NOT automatically make every query faster.

Indexes have costs:

* disk space
* INSERT overhead
* UPDATE overhead
* DELETE overhead
* maintenance
* planner complexity

---

# 16. B-tree indexes

Default index type:

```sql
CREATE INDEX idx_users_email
ON users(email);
```

uses B-tree by default.

Good for many common operations:

```sql
=
<
>
<=
>=
ORDER BY
```

Think of B-tree as the general-purpose index.

---

# 17. Composite indexes

Suppose:

```sql
CREATE INDEX idx_orders_customer_status
ON orders(customer_id, status);
```

The column order matters.

Think:

```text
(customer_id, status)
```

not the same as:

```text
(status, customer_id)
```

The index can be particularly useful when queries constrain the leading column(s).

Example:

```sql
WHERE customer_id = 10
```

Good candidate.

```sql
WHERE customer_id = 10
  AND status = 'PAID'
```

Good candidate.

But:

```sql
WHERE status = 'PAID'
```

alone generally cannot exploit this B-tree index as effectively as a query using the leading `customer_id` column.

This is the **leftmost-prefix** idea you should understand.

---

# 18. Index Scan vs Sequential Scan

Interview question:

> "Why is PostgreSQL doing a sequential scan even though I created an index?"

Possible reasons:

* table is small
* condition matches a large percentage of rows
* index isn't selective enough
* statistics are stale/inaccurate
* expression doesn't match the index
* data distribution
* planner estimates index access as more expensive

Example:

```sql
EXPLAIN ANALYZE
SELECT *
FROM users
WHERE email = 'x@example.com';
```

You need to know how to read:

```text
Seq Scan
Index Scan
Bitmap Index Scan
Bitmap Heap Scan
```

---

# 19. EXPLAIN / EXPLAIN ANALYZE

This is **mandatory for a 3-year backend developer**.

```sql
EXPLAIN
SELECT *
FROM orders
WHERE customer_id = 100;
```

And:

```sql
EXPLAIN ANALYZE
SELECT *
FROM orders
WHERE customer_id = 100;
```

### Difference

`EXPLAIN` gives the **planned execution**.

`EXPLAIN ANALYZE` actually executes the query and reports actual runtime information.

Look at:

```text
cost
actual time
rows
loops
```

Most importantly, compare:

```text
estimated rows
vs
actual rows
```

Large discrepancies can indicate poor cardinality estimates/statistics.

---

# 20. Query Optimization

Suppose someone writes:

```sql
SELECT *
FROM users
WHERE LOWER(email) = 'abc@example.com';
```

An ordinary index:

```sql
CREATE INDEX idx_users_email
ON users(email);
```

may not directly support that expression efficiently.

You can consider an expression index:

```sql
CREATE INDEX idx_users_lower_email
ON users(LOWER(email));
```

Then:

```sql
WHERE LOWER(email) = 'abc@example.com'
```

can potentially use it.

---

# 21. Partial Index

Very useful PostgreSQL feature.

Suppose only active users are queried frequently:

```sql
CREATE INDEX idx_active_users_email
ON users(email)
WHERE active = true;
```

This index only covers rows satisfying the predicate.

Benefits can include:

* smaller index
* less maintenance than a full index
* better targeted access

---

# 22. Covering Index / INCLUDE

PostgreSQL supports:

```sql
CREATE INDEX idx_users_email
ON users(email)
INCLUDE (name, created_at);
```

This can allow PostgreSQL to obtain additional columns from the index and, in suitable circumstances, use an **index-only scan**.

But don't claim an index-only scan is guaranteed—visibility information and the query plan matter.

---

# 23. Transactions

Know ACID.

### Atomicity

All or nothing.

### Consistency

Transaction preserves database constraints/invariants.

### Isolation

Concurrent transactions don't improperly interfere with each other according to the selected isolation level.

### Durability

Committed data survives failures according to PostgreSQL's durability guarantees/configuration.

---

# 24. PostgreSQL Isolation Levels

Know:

```text
Read Committed
Repeatable Read
Serializable
```

PostgreSQL also accepts:

```text
Read Uncommitted
```

but effectively treats it as **Read Committed**.

### Read Committed

Default PostgreSQL isolation level.

Each statement sees a snapshot as of the start of that statement.

This means two SELECTs in the same transaction can potentially see different committed data.

### Repeatable Read

Transaction-level snapshot semantics prevent the transaction from seeing changes committed after its snapshot.

### Serializable

Strongest standard isolation level.

PostgreSQL uses Serializable Snapshot Isolation (SSI), and transactions may fail with serialization errors that the application should be prepared to retry.

---

# 25. Dirty Read / Non-repeatable Read / Phantom Read

Know the definitions.

### Dirty read

Reading uncommitted data.

PostgreSQL doesn't allow normal dirty reads.

### Non-repeatable read

Read the same row twice and get different committed values.

### Phantom read

Repeated query finds a different set of matching rows.

You should understand how PostgreSQL's MVCC and isolation levels affect these phenomena.

---

# 26. MVCC — VERY IMPORTANT

PostgreSQL uses **Multi-Version Concurrency Control**.

Instead of simply overwriting one row version, PostgreSQL maintains row versions so concurrent transactions can operate using snapshots.

Conceptually:

```text
Transaction A reads row
          ↓
Transaction B updates row
          ↓
A can continue using its snapshot
```

This is a major reason PostgreSQL can provide strong concurrency without readers constantly blocking writers.

But old row versions create cleanup requirements.

---

# 27. VACUUM

When rows are updated/deleted, old row versions can become dead tuples.

PostgreSQL uses:

```sql
VACUUM
```

to help reclaim/reuse storage and maintain table health.

### VACUUM FULL

More aggressive.

It rewrites the table and generally requires a stronger lock, so it is **not** the routine maintenance command you casually run in production.

### Autovacuum

PostgreSQL normally handles routine vacuum/analyze work automatically through autovacuum.

You should know why autovacuum matters.

---

# 28. ANALYZE

```sql
ANALYZE users;
```

collects statistics that the planner uses to estimate things such as:

```text
How many rows match this condition?
```

If estimates are bad, the optimizer can choose a poor plan.

---

# 29. Locks

Know the difference between:

* row-level locks
* table-level locks
* explicit locking
* blocking
* deadlocks

Example:

```sql
SELECT *
FROM accounts
WHERE id = 1
FOR UPDATE;
```

This locks the selected row(s) for update within the transaction.

Classic use:

```text
Read balance
   ↓
modify balance
   ↓
commit
```

when you need to prevent conflicting concurrent updates.

---

# 30. Deadlocks

Example:

Transaction A:

```text
locks row 1
waits for row 2
```

Transaction B:

```text
locks row 2
waits for row 1
```

Now:

```text
A → waiting for B
B → waiting for A
```

Deadlock.

PostgreSQL detects deadlocks and aborts one transaction.

### How to reduce them?

A very common strategy:

> Acquire locks in a consistent order.

For example, always lock:

```text
account 1
then account 2
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

# 31. UPSERT

PostgreSQL has excellent support for upsert.

```sql
INSERT INTO users (email, name)
VALUES ('a@example.com', 'A')
ON CONFLICT (email)
DO UPDATE SET name = EXCLUDED.name;
```

`EXCLUDED` refers to the row you attempted to insert.

You should know:

```sql
ON CONFLICT DO NOTHING
```

and:

```sql
ON CONFLICT (...) DO UPDATE
```

---

# 32. RETURNING

PostgreSQL-specific and extremely useful.

```sql
INSERT INTO users(name)
VALUES ('Rahul')
RETURNING id;
```

Instead of:

```text
INSERT
↓
SELECT generated ID
```

you can retrieve it directly.

Also:

```sql
UPDATE users
SET name = 'A'
WHERE id = 10
RETURNING *;
```

---

# 33. Sequences / Identity / SERIAL

You should understand:

```sql
SERIAL
BIGSERIAL
```

and modern identity columns:

```sql
GENERATED ALWAYS AS IDENTITY
```

For new schemas, identity columns are generally preferable to relying on the older `serial` shorthand.

Also understand:

> Sequence values are not guaranteed to be gapless.

If IDs are:

```text
100
101
103
```

it doesn't necessarily mean something is broken. Sequence values can be consumed by rolled-back transactions, caching, etc.

---

# 34. DELETE vs TRUNCATE

### DELETE

```sql
DELETE FROM users;
```

* row-level operation
* can use `WHERE`
* produces row-level effects
* can fire applicable triggers
* transactional

### TRUNCATE

```sql
TRUNCATE users;
```

* removes all rows quickly
* cannot use `WHERE`
* takes stronger locks
* can restart identity depending on options
* transactional in PostgreSQL

Know:

```sql
TRUNCATE users RESTART IDENTITY;
```

---

# 35. DELETE vs DROP

```sql
DELETE FROM users;
```

removes rows.

```sql
DROP TABLE users;
```

removes the table definition and its data.

Obvious concept, but interviewers sometimes use rapid-fire questions.

---

# 36. Views

```sql
CREATE VIEW active_users AS
SELECT *
FROM users
WHERE active = true;
```

A normal view stores the query definition, not a separate copy of the result.

### Materialized view

```sql
CREATE MATERIALIZED VIEW user_stats AS
SELECT ...
```

Stores the result physically and can be refreshed:

```sql
REFRESH MATERIALIZED VIEW user_stats;
```

Useful for expensive read-heavy reporting/aggregation workloads where slightly stale data is acceptable.

---

# 37. Functions vs Procedures

PostgreSQL supports both.

Functions:

```sql
CREATE FUNCTION ...
```

can return values and are usable in SQL expressions.

Procedures:

```sql
CREATE PROCEDURE ...
```

are invoked with:

```sql
CALL ...
```

You don't need to become a PL/pgSQL expert unless your role specifically requires database-side programming, but understand the distinction.

---

# 38. PostgreSQL-specific advanced topics

For a strong 3-YOE interview, I'd also revise:

### Arrays

```sql
text[]
integer[]
```

Example:

```sql
SELECT *
FROM users
WHERE 'Java' = ANY(skills);
```

### UUID

```sql
id UUID
```

Understand why UUIDs can be useful for distributed systems, but also think about index locality/storage characteristics depending on the UUID generation strategy.

### ENUM

```sql
CREATE TYPE order_status AS ENUM (
    'pending',
    'paid',
    'cancelled'
);
```

Know the tradeoffs versus lookup/reference tables.

### Extensions

Examples:

```sql
CREATE EXTENSION ...
```

You should at least know PostgreSQL has an extension ecosystem and that some deployments use extensions such as PostGIS.

---

# 39. Partitioning

At 3 YOE, know the concept even if you haven't implemented it.

Instead of one huge logical table:

```text
orders
```

you can partition by:

```text
date
range
list
hash
```

Example concept:

```text
orders
 ├── orders_2026_01
 ├── orders_2026_02
 ├── orders_2026_03
 └── ...
```

PostgreSQL supports:

* RANGE
* LIST
* HASH

Partitioning can help with very large tables, maintenance, and partition pruning, but **partitioning is not automatically a performance improvement**.

---

# 40. Replication

At least understand:

```text
Primary
   ↓
Standby / Replica
```

PostgreSQL supports streaming replication and WAL-based replication mechanisms.

Know the terminology:

* WAL
* primary
* standby
* replication
* synchronous replication
* asynchronous replication
* read replica
* replication lag

### Important

A read replica can be behind the primary.

Therefore:

```text
WRITE → primary
READ  → replica
```

can create consistency surprises immediately after a write.

This becomes a real backend interview question.

---

# 41. WAL

**Write-Ahead Logging**

Core idea:

> Changes are recorded in WAL before the corresponding data pages are considered durably written.

WAL is fundamental to:

* crash recovery
* durability
* replication
* point-in-time recovery

You don't need to memorize internals initially, but understand the concept very well.

---

# 42. Connection Pooling

A 3-YOE backend developer should absolutely know this.

Imagine:

```text
1000 requests
```

and every request creates a new PostgreSQL connection.

That's bad.

Instead:

```text
Application
     ↓
Connection Pool
 ┌───┼───┐
 ↓   ↓   ↓
 DB connections
```

Common concepts:

* pool size
* connection timeout
* idle connections
* max connections
* transaction duration

Too many database connections can actually **hurt** PostgreSQL rather than improve performance.

---

# 43. N+1 Query Problem

Classic backend interview question.

Suppose:

```text
1 query → fetch 100 users

then

100 queries → fetch orders for each user
```

Total:

```text
101 queries
```

That's the N+1 problem.

Possible solutions:

* JOIN
* batch queries
* `IN`
* appropriate ORM eager loading
* carefully designed data fetching

Example:

```sql
SELECT u.*, o.*
FROM users u
LEFT JOIN orders o
    ON o.user_id = u.id
WHERE u.id IN (...);
```

But don't blindly replace every N+1 with a huge join; result multiplication and data volume matter.

---

# 44. ORM vs PostgreSQL

If you're using something like:

```text
Hibernate
JPA
Django ORM
SQLAlchemy
Prisma
Entity Framework
```

the interviewer may ask:

> "If you're using an ORM, why do you need SQL?"

Answer:

Because production debugging often requires understanding:

```text
SQL generated by ORM
indexes
query plans
transactions
locks
constraints
isolation
```

An ORM does not eliminate database knowledge.

---

# 45. Questions I would expect at 3 YOE

You should be able to answer these **without Googling**:

### SQL

1. Difference between `WHERE` and `HAVING`?
2. `INNER JOIN` vs `LEFT JOIN`?
3. `UNION` vs `UNION ALL`?
4. `EXISTS` vs `IN`?
5. What is a correlated subquery?
6. CTE vs subquery?
7. Find duplicate records.
8. Find the second-highest salary.
9. Find top 3 salaries per department.
10. Find customers who have never placed an order.

### PostgreSQL

11. What is MVCC?
12. What is VACUUM?
13. What does autovacuum do?
14. What is WAL?
15. What is `EXPLAIN ANALYZE`?
16. Why might PostgreSQL ignore an index?
17. B-tree vs GIN?
18. What is a composite index?
19. Why does index column order matter?
20. What is a partial index?
21. What is an expression index?
22. What is an index-only scan?
23. What is `jsonb`?
24. `timestamp` vs `timestamptz`?
25. `DELETE` vs `TRUNCATE`?
26. `SERIAL` vs `IDENTITY`?
27. What is `RETURNING`?
28. What is `ON CONFLICT`?
29. What are isolation levels?
30. What causes deadlocks?
31. How do you investigate a slow query?
32. How does connection pooling work?
33. What is replication lag?
34. What happens when a transaction is rolled back?
35. What happens internally when an UPDATE occurs?

---

# 46. The most important practical scenario

Suppose the interviewer says:

> **"Our API endpoint became slow. It was 100ms yesterday and is now taking 5 seconds. The PostgreSQL table has 20 million rows. What do you do?"**

Don't immediately say:

> "Add an index."

A strong 3-YOE answer is:

```text
1. Reproduce/measure the slow query.
             ↓
2. Check application logs / generated SQL.
             ↓
3. Run EXPLAIN ANALYZE.
             ↓
4. Check whether the planner chooses Seq Scan / Index Scan.
             ↓
5. Compare estimated vs actual rows.
             ↓
6. Check indexes and their column order.
             ↓
7. Check whether the predicate can use the index.
             ↓
8. Check table statistics / ANALYZE.
             ↓
9. Check locks / blocking.
             ↓
10. Check CPU, I/O, memory and connection pressure.
             ↓
11. Fix query/index/schema if necessary.
             ↓
12. Re-run EXPLAIN ANALYZE and benchmark.
```

**That's the level of thinking I want you to develop for the interview.**

---

# 47. Our fastest revision strategy

Rather than reading 50 topics passively, I'd do this as an **interview drill**:

### Round 1 — SQL

* joins
* aggregation
* subqueries
* CTE
* window functions
* `EXISTS`
* tricky SQL problems

### Round 2 — PostgreSQL internals

* MVCC
* transactions
* isolation
* locks
* deadlocks
* VACUUM
* WAL

### Round 3 — Performance

* indexes
* composite indexes
* partial indexes
* GIN
* `EXPLAIN ANALYZE`
* slow-query debugging

### Round 4 — Real backend scenarios

* transactions
* concurrency
* order/payment systems
* inventory
* connection pooling
* pagination
* N+1
* replication

### Round 5 — Mock interview

I'll ask you questions **one at a time**, starting easy and progressively going to **3–4 YOE level**. You answer, and I'll tell you:

**✅ Correct**
**⚠️ Partially correct**
**❌ Wrong**

Then I'll give you the **interview-quality answer** and a follow-up question.

**Let's start with the highest-value topic: JOINs + tricky SQL.**
