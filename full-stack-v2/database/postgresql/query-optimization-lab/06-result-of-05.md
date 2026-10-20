Perfect. This is the **exact situation where composite indexes become useful**.

Your current plan tells us something very important:

```text
Bitmap Index Scan
    ↓
idx_orders_user_id
    ↓
10 candidate rows
    ↓
Bitmap Heap Scan
    ↓
apply status = 'PAID'
    ↓
3 rows returned
```

### 1. What PostgreSQL is doing

The index only knows about:

```sql
user_id = 8472
```

So it finds **10 rows**:

```text
actual rows = 10
```

Then PostgreSQL goes to the table and applies:

```sql
status = 'PAID'
```

We can see that here:

```text
Filter: ((status)::text = 'PAID'::text)
Rows Removed by Filter: 7
```

So:

```text
10 rows found using index
        ↓
7 rejected because status != PAID
        ↓
3 returned
```

That means the `status` condition is **not being used by the index**.

---

# 2. Notice the difference between `Index Cond` and `Filter`

This distinction is extremely important in `EXPLAIN`.

You have:

```text
Index Cond: (user_id = 8472)
```

This means:

> PostgreSQL uses the index to locate candidates based on `user_id`.

But:

```text
Filter: (status = 'PAID')
```

means:

> PostgreSQL applies `status` after retrieving those candidates.

That's a useful interview explanation.

---

# 3. Now let's ask the production question

Our application frequently runs:

```sql
SELECT *
FROM orders
WHERE user_id = ?
  AND status = ?;
```

Currently we have:

```text
idx_orders_user_id
```

What if we create:

```sql
CREATE INDEX idx_orders_user_status
ON orders(user_id, status);
```

Now the index contains the combination:

```text
user_id
   +
status
```

So PostgreSQL can potentially locate:

```text
(user_id = 8472, status = 'PAID')
```

directly.

Instead of:

```text
10 candidates
   ↓
check status
   ↓
3 results
```

we can potentially get:

```text
index
  ↓
3 matching rows
```

---

# 4. Let's test it — but don't delete the old index yet

Create:

```sql
CREATE INDEX idx_orders_user_status
ON orders(user_id, status);
```

Then run the **exact same query**:

```sql
EXPLAIN (ANALYZE, BUFFERS)
SELECT *
FROM orders
WHERE user_id = 8472
  AND status = 'PAID';
```

### This time, pay special attention to:

```text
Index Cond:
Filter:
Rows Removed by Filter:
Buffers:
Execution Time:
```

We want to see whether:

```text
status = 'PAID'
```

moves from:

```text
Filter
```

into:

```text
Index Cond
```

That's our next real-world lesson:

> **How to design a composite index from an application's query pattern.**

And after that we'll deliberately create a **bad index** and see why simply adding indexes everywhere is not a production solution.
