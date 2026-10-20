Exactly. **This is the ideal result.** You just demonstrated a very important production optimization: **an index can eliminate a sort and allow `LIMIT` to stop the scan early.**

## 1. Compare the plans

### Before

```text
Bitmap Index Scan
      ↓
Bitmap Heap Scan
      ↓
10 rows
      ↓
Sort by created_at DESC
      ↓
LIMIT 5
```

You had:

```text
Sort Method: quicksort
```

### After

```text
Index Scan
      ↓
already ordered by created_at DESC
      ↓
first 5 rows
      ↓
STOP
```

Your new plan:

```text
Limit
  -> Index Scan using idx_orders_user_created
       Index Cond: (user_id = 8472)
```

**The `Sort` node completely disappeared.**

That's the important improvement.

---

# 2. The really interesting part: `rows=5`

Look carefully:

```text
Index Scan
(actual time=0.039..0.045 rows=5.00 loops=1)
```

The planner estimated:

```text
rows=10
```

but the `Index Scan` actually produced:

```text
rows=5
```

Why?

Because of:

```sql
LIMIT 5
```

PostgreSQL doesn't need to return all 10 matching rows.

The index is already ordered:

```text
user_id = 8472

created_at DESC
----------------
newest
   ↓
   ↓
   ↓
   ↓
   ↓
5th newest
```

Once it gets five rows, it can stop.

This is a **huge concept**.

---

# 3. Why the index is perfect for this query

We created:

```sql
CREATE INDEX idx_orders_user_created
ON orders(user_id, created_at DESC);
```

Think of the index logically as:

```text
(user_id, created_at DESC)

8472
 ├── newest order
 ├── second newest
 ├── third newest
 ├── fourth newest
 ├── fifth newest
 ├── ...
```

So this query:

```sql
WHERE user_id = 8472
ORDER BY created_at DESC
LIMIT 5
```

maps almost directly onto the index.

PostgreSQL essentially says:

> Find the section for user 8472, start at the newest entry, return five, and stop.

No separate sorting.

---

# 4. This is much more important than the 0.076 → 0.059 ms improvement

Don't focus on:

```text
0.076 ms → 0.059 ms
```

Our database is tiny, and cache state changes between executions.

Instead focus on the **algorithmic behavior**.

### Before

```text
Find N rows
     ↓
Sort N rows
     ↓
Take 5
```

Roughly:

```text
O(N log N)
```

### After

```text
Find user
     ↓
walk index in desired order
     ↓
take 5
```

Conceptually much closer to:

```text
O(log N + K)
```

where `K = 5`.

That's why this optimization becomes extremely valuable when a user has **millions of orders**.

---

# 5. Now let's understand the index column order

This is one of the most important PostgreSQL interview topics.

We have:

```sql
(user_id, created_at DESC)
```

Why this order?

Because our query has:

```sql
WHERE user_id = ?
ORDER BY created_at DESC
```

So:

```text
filter first
ordering second
```

The index matches the query pattern.

### Compare this:

```sql
(user_id, created_at DESC)
```

with:

```sql
(created_at DESC, user_id)
```

They're **not interchangeable**.

For our query, the first version is generally much more useful because PostgreSQL can locate the `user_id` portion and then walk those entries in timestamp order.

---

# 6. There's another subtle thing you just learned

Previously PostgreSQL chose:

```text
Bitmap Heap Scan
```

Now it chose:

```text
Index Scan
```

Why?

Because an Index Scan gives PostgreSQL something very useful here:

```text
ORDERED ACCESS
```

A bitmap scan is great for finding a set of matching rows efficiently, but it doesn't preserve the index ordering in a way that can simply satisfy the `ORDER BY`.

So PostgreSQL effectively has a choice:

```text
Bitmap Scan
    ↓
good for finding many matching rows
    ↓
but then sort
```

versus:

```text
Index Scan
    ↓
find matching rows
    ↓
already correctly ordered
    ↓
LIMIT 5
    ↓
stop
```

For this query, the second strategy wins.

---

# 7. This gives us a very practical index-design rule

When you see application queries like:

```sql
WHERE A = ?
ORDER BY B DESC
LIMIT K
```

a candidate index is often:

```sql
(A, B DESC)
```

Examples:

```text
(user_id, created_at DESC)
(conversation_id, created_at DESC)
(account_id, updated_at DESC)
(category_id, popularity DESC)
```

This pattern appears constantly in:

* feeds
* order history
* notifications
* chat messages
* audit logs
* dashboards
* activity timelines

---

# 8. But there's a problem with our current database 😄

We now have **three indexes** on `orders`:

```text
PRIMARY KEY (id)

idx_orders_user_id
    (user_id)

idx_orders_user_status
    (user_id, status)

idx_orders_user_created
    (user_id, created_at DESC)
```

And this is where real-world PostgreSQL optimization gets interesting.

We shouldn't just keep creating indexes forever.

Every index means:

```text
INSERT
  ↓
table write
+
index #1 maintenance
+
index #2 maintenance
+
index #3 maintenance
```

So indexes improve reads but increase write/storage/maintenance costs.

---

## Next experiment: let's break an index

We're going to test a query for:

```sql
WHERE status = 'PAID'
```

using our existing indexes.

Run:

```sql
EXPLAIN (ANALYZE, BUFFERS)
SELECT *
FROM orders
WHERE status = 'PAID';
```

**Don't create a status index yet.**

I want you to look at the plan first.

This will teach us the next major concept:

> **Why `(user_id, status)` does not automatically help a query that filters only by `status`.**

After that, we'll deliberately create the appropriate index and compare the plans.
