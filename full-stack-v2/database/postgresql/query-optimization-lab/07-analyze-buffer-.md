Excellent. **This is exactly what we wanted to prove.** Now you have seen PostgreSQL evolve from a single-column index to a composite index and actually use **both predicates inside the index**.

## 1. Compare the two plans

### Before composite index

With only:

```sql
CREATE INDEX idx_orders_user_id ON orders(user_id);
```

you had:

```text
Bitmap Index Scan
  Index Cond: user_id = 8472
        ↓
10 rows
        ↓
Bitmap Heap Scan
        ↓
Filter: status = 'PAID'
        ↓
7 removed
        ↓
3 returned
```

Execution:

```text
0.058 ms
```

---

### After composite index

You now have:

```text
Bitmap Index Scan on idx_orders_user_status

Index Cond:
  user_id = 8472
  AND status = 'PAID'
```

That's the key improvement.

PostgreSQL can use **both conditions while searching the index**.

Then:

```text
3 index matches
      ↓
3 heap blocks
      ↓
3 rows returned
```

Notice:

```text
Rows Removed by Filter
```

is gone.

That's because there is no separate `status` filter anymore.

---

# 2. The most important line

This:

```text
Index Cond: ((user_id = 8472) AND (status = 'PAID'))
```

is much better than our previous:

```text
Index Cond: (user_id = 8472)

Filter: (status = 'PAID')
```

For this particular query pattern, the composite index matches the access pattern much more closely.

---

# 3. But look at something interesting

You might expect:

```text
Composite index
    ↓
Index Scan
```

But PostgreSQL still chose:

```text
Bitmap Index Scan
        ↓
Bitmap Heap Scan
```

That's completely fine.

**Index design and index access method are two separate decisions.**

We designed the index:

```text
(user_id, status)
```

Then PostgreSQL independently decided:

> "Given the estimated rows and table layout, Bitmap access is cheapest."

Don't confuse:

```text
What is indexed?
```

with:

```text
How will PostgreSQL access that index?
```

---

# 4. Estimated vs actual rows

This is also excellent:

```text
estimated rows = 3
actual rows    = 3
```

PostgreSQL predicted the result size perfectly.

That means the planner has a good understanding of this data distribution.

This becomes extremely important when we later deliberately create **bad cardinality estimates**.

---

# 5. Buffers changed too

Previous query:

```text
Buffers: shared hit=12
```

Now:

```text
Buffers: shared hit=3 read=3
```

Here:

```text
shared hit = pages already in PostgreSQL's buffer cache
shared read = pages had to be read into the buffer cache
```

So this execution required:

```text
3 cached pages
+
3 pages read
```

Don't interpret the `read=3` as a regression by itself.

Your previous execution happened after the pages were already cached, whereas this execution touched different index/table pages and had to bring some into cache.

This is a good lesson:

> **Benchmarking database queries requires controlling for caching and workload conditions.**

A single `EXPLAIN ANALYZE` timing isn't enough to make a production performance claim.

---

# 6. And something even more important: the timing

You got:

```text
Execution Time: 0.094 ms
```

versus:

```text
0.058 ms
```

from the previous query.

It actually became **slightly slower**.

That is completely okay.

This is where you should resist the temptation to say:

> "Composite indexes always make queries faster."

They don't.

Our dataset is tiny:

```text
100,000 orders
```

and we're querying only:

```text
user_id = 8472
AND status = 'PAID'
```

The previous index was already extremely efficient.

The composite index reduces the number of candidate rows, but the absolute difference is so tiny that measurement noise and cache state dominate.

### The important improvement is structural:

```text
Before:

Index → 10 candidates → filter 7 → return 3


After:

Index → 3 candidates → return 3
```

On a much larger table with a much less selective first predicate, that distinction can become significant.

---

# 7. Now comes the BIG composite-index lesson

You created:

```sql
CREATE INDEX idx_orders_user_status
ON orders(user_id, status);
```

The order matters.

This is **not equivalent** to:

```sql
(status, user_id)
```

For B-tree indexes, the leading column matters heavily.

Your index is conceptually organized like:

```text
user_id
   ↓
status
```

So it's particularly useful for queries such as:

```sql
WHERE user_id = ?
```

and:

```sql
WHERE user_id = ?
  AND status = ?
```

But a query such as:

```sql
WHERE status = 'PAID'
```

may not be able to use this index effectively because `status` isn't the leading column.

This is one of the most important things to understand about composite indexes.

---

# 8. Don't create another index yet

We currently have:

```text
PRIMARY KEY
    id

idx_orders_user_id
    user_id

idx_orders_user_status
    user_id, status
```

And now we have an interesting redundancy:

```text
idx_orders_user_id
```

is potentially redundant for queries where the composite index can already serve:

```sql
WHERE user_id = ?
```

because the composite index begins with `user_id`.

But **don't immediately drop it**.

First let's test the hypothesis.

Run:

```sql
EXPLAIN (ANALYZE, BUFFERS)
SELECT *
FROM orders
WHERE user_id = 8472;
```

Now PostgreSQL has both:

```text
idx_orders_user_id
idx_orders_user_status
```

available.

Let's see which one it chooses.

Then we'll ask the production question:

> **Should we keep both indexes, or is one unnecessary?**

And from there we'll move into one of the most important real-world index topics:

**`ORDER BY + LIMIT` and index design.**

That's where you'll start seeing why experienced engineers don't just create indexes based on `WHERE` clauses.
