
## 1. Query suddenly becomes slow

**Interviewer:**

> A query that used to take 200 ms now takes 10 seconds. What would you do?

**Weak answer:**

> Add an index.

**Better answer:**

> First I would identify whether the slowdown is caused by the query plan, data growth, locks, I/O, CPU, or statistics. I'd run `EXPLAIN (ANALYZE, BUFFERS)` and compare the current plan with the expected plan. I'd also check `pg_stat_statements`, active queries, locks, and database resource utilization. Only after identifying the bottleneck would I decide whether an index, query rewrite, statistics update, or infrastructure change is appropriate.

**Interviewer:**

> What if there is already an index?

**Answer:**

> Then I'd check whether PostgreSQL is actually using it and whether the index is useful for the query. The problem could be poor selectivity, incorrect column order in a composite index, stale statistics, type mismatches, a function applied to the indexed column, or simply that a sequential scan is cheaper.

# 2. You added an index, but it's still slow

**Interviewer:**

> We added the index. Query is still taking 8 seconds. What now?

**Answer:**

> I wouldn't assume the index solved the problem. I'd inspect `EXPLAIN ANALYZE` again.

Then look for:

```text
Index Scan
Seq Scan
Nested Loop
Hash Join
Sort
Aggregate
Rows Removed by Filter
Buffers
```

For example:

```sql
EXPLAIN (ANALYZE, BUFFERS)
SELECT *
FROM orders
WHERE customer_id = 123;
```

If PostgreSQL scans 10 million rows despite the index, I'd investigate **why**.

Possible causes:

* index isn't selective
* wrong index
* wrong composite-column order
* stale statistics
* query returns too many rows
* implicit type conversion
* function prevents index usage
* planner estimates are wrong

# 3. Query is fast in development but slow in production

**Interviewer:**

> Same query. Dev = 50 ms. Production = 15 seconds. Why?

**Answer:**

> I would compare the environments rather than immediately changing the query.

Check:

```text
Data volume
Indexes
Statistics
PostgreSQL version
Configuration
Hardware
CPU
Memory
Disk I/O
Concurrent queries
Locks
Execution plan
```

The biggest clue is usually:

```sql
EXPLAIN (ANALYZE, BUFFERS)
```

The query may have the same SQL but a **completely different execution plan** because production has much more data or different statistics.

# 4. CPU is 100%

**Interviewer:**

> PostgreSQL CPU is constantly at 100%. What do you do?

**Bad answer:**

> Increase CPU.

**Better answer:**

> First identify which queries are consuming the CPU.

I'd investigate expensive queries through `pg_stat_statements`, then inspect their execution plans.

Typical causes:

```text
Large sequential scans
Expensive joins
Sorting huge datasets
Aggregations
Poor query plans
Repeated queries
Missing/inefficient indexes
```

Only after optimizing the workload would I consider adding CPU.

# 5. Database has 1 billion rows. SELECT is slow.

**Interviewer:**

> We have a billion-row table. Queries are slow. Should we partition it?

**Answer:**

> Maybe, but table size alone isn't enough reason to partition.

I'd first understand the access pattern.

For example, if most queries are:

```sql
WHERE created_at >= '2026-08-01'
```

then time-based partitioning could be useful because PostgreSQL can eliminate irrelevant partitions through **partition pruning**.

But if queries don't filter on the partition key, partitioning may provide little benefit and can add complexity.

# 6. Read traffic is extremely high

**Interviewer:**

> We have 100,000 reads per second. PostgreSQL can't keep up. What do you do?

Don't immediately say:

> Sharding.

Think in layers:

```text
1. Optimize expensive queries
2. Check indexes
3. Connection pooling
4. Cache frequently accessed data
5. Read replicas
6. Database configuration/resources
7. Partitioning where appropriate
8. Sharding if one database cannot handle the workload
```

**Important interview point:**

> Sharding is usually a scaling strategy of last resort, not the first optimization.

# 7. Database is slow, but CPU and memory are fine

This is a great interviewer question.

**Interviewer:**

> PostgreSQL is slow. CPU is 30%, memory looks fine. What could it be?

Think:

### I/O

```text
Disk latency
Random reads
Too many physical reads
Insufficient cache
```

### Locks

```text
Transaction A
     ↓
locks row
     ↓
Transaction B waits
     ↓
Transaction C waits
```

### Connection contention

Too many connections can also hurt performance.

### Long-running transactions

They can prevent cleanup and contribute to table/index bloat.

So I'd investigate:

```text
pg_stat_activity
pg_locks
EXPLAIN (ANALYZE, BUFFERS)
pg_stat_statements
database I/O metrics
```

# 8. SELECT is fast, DELETE is extremely slow

This is a **very good interview trap**.

**Interviewer:**

> `SELECT` is fast, but deleting 1 million rows takes forever. Why?

Possible answer:

> Foreign-key checks, triggers, indexes, cascading deletes, locks, and transaction behavior.

For example, deleting from:

```text
parent
   ↓
child
   ↓
grandchild
```

can cause PostgreSQL to perform significant work checking foreign-key relationships.

So don't just say:

> "Add an index to the SELECT."

You need to investigate the **DELETE execution and triggers**.

# 9. UPDATE is slow even though the WHERE column is indexed

**Interviewer:**

> `UPDATE users SET ... WHERE id = ?` is slow. `id` is the primary key. Why?

Excellent answer:

> The lookup itself may be fast, but the overall update may not be.

Possible causes:

* triggers
* foreign keys
* many indexes
* row contention
* locks
* large transactions
* table/index bloat
* WAL generation
* disk I/O
* updating indexed columns

So:

```text
Fast lookup ≠ fast UPDATE
```

That's an important distinction.

# 10. One query is blocking hundreds of queries

**Interviewer:**

> Hundreds of requests are waiting. You discover one transaction has been running for 30 minutes. What do you do?

First determine:

```text
Who owns the lock?
Who is waiting?
What SQL is running?
Why is the transaction still open?
```

Use PostgreSQL activity/lock information.

Then fix the application behavior.

For example:

```text
BEGIN
   ↓
UPDATE
   ↓
application waits for external API
   ↓
transaction remains open
   ↓
locks remain held
   ↓
other requests wait
```

The correct fix isn't necessarily:

> Increase database resources.

It may be:

> **Don't hold database transactions open while doing slow external work.**

# 11. Caching didn't solve the problem

**Interviewer:**

> We added Redis caching but the database is still slow. Why?

This is another pushback question.

Possible explanation:

> The expensive operation may be writes rather than reads.

Or:

```text
Cache hit = fast
Cache miss = expensive DB query
```

If the application has a low cache-hit ratio, caching won't solve the underlying problem.

Also:

```text
Cache invalidation
stale data
hot keys
cache stampede
large objects
```

can introduce their own problems.

So ask:

> **What percentage of requests are cache hits?**

# 12. Index makes writes slower

**Interviewer:**

> If indexes make reads faster, why not put 20 indexes on every table?

**Answer:**

Because indexes aren't free.

For an INSERT:

```text
INSERT row
   ↓
table write
   ↓
index 1
   ↓
index 2
   ↓
index 3
   ↓
...
```

Every relevant index needs maintenance.

Too many indexes can cause:

* slower INSERT
* slower UPDATE
* slower DELETE
* additional storage
* more memory pressure
* more maintenance

Therefore:

> **Create indexes based on actual query patterns, not because indexes are generally good.**

# 13. OFFSET pagination becomes slow

**Interviewer:**

> This query is getting slower as users go to later pages:

```sql
SELECT *
FROM orders
ORDER BY created_at DESC
LIMIT 50 OFFSET 1000000;
```

What do you do?

**Answer:**

Use **keyset/cursor pagination** where appropriate.

Instead of:

```sql
OFFSET 1000000
```

use something like:

```sql
SELECT *
FROM orders
WHERE created_at < :last_seen
ORDER BY created_at DESC
LIMIT 50;
```

with an appropriate index.

The problem with large OFFSET is that PostgreSQL still has to walk past many rows before returning the requested page.

# 14. `COUNT(*)` is slow

**Interviewer:**

> Why does this take so long?

```sql
SELECT COUNT(*)
FROM orders;
```

**Answer:**

PostgreSQL generally can't treat a normal `COUNT(*)` as a permanently stored exact number because concurrent transactions mean the visible row count depends on the transaction's snapshot.

So for a huge table, an exact count can require substantial work.

Depending on the requirement:

```text
Need exact count?
    ↓
Accept expensive operation

Need approximate count?
    ↓
Use statistics/estimates

Need frequent exact count?
    ↓
Maintain a separate counter carefully
```

# 15. The "everything is slow" question

This is probably the **best one to practice**.

**Interviewer:**

> Users say the entire application is slow. PostgreSQL is involved. What do you do?

Don't immediately say:

> Add indexes.

Give them a diagnostic process:

```text
                    Application slow
                           │
                           ▼
                   Is DB actually slow?
                           │
             ┌─────────────┴─────────────┐
             ▼                           ▼
            YES                          NO
             │                           │
             ▼                           ▼
       Which queries?             Investigate app
             │
             ▼
      pg_stat_statements
             │
             ▼
       EXPLAIN ANALYZE
             │
      ┌──────┼─────────┐
      ▼      ▼         ▼
     CPU    I/O       Locks
      │      │         │
      ▼      ▼         ▼
   Optimize appropriately
             │
             ▼
       Measure again
```

### The golden interview sentence

If you remember only one thing, remember this:

> **"I wouldn't choose caching, indexing, partitioning, or sharding until I identify the actual bottleneck. First I measure, inspect the execution plan and database activity, then apply the smallest change that addresses the measured bottleneck."**

That's the mindset interviewers are often looking for.

If you want to prepare seriously, I can next give you **20 PostgreSQL "interviewer keeps rejecting your answer" scenarios**, with the expected follow-up questions and the answer you should give at each stage.
