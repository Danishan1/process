Excellent. This is exactly the kind of output we wanted. Let's read **your actual plan**, not a theoretical one.

## 1. What PostgreSQL did

```text
Seq Scan on orders
```

PostgreSQL chose a **Sequential Scan**.

Conceptually:

```text
orders (100,000 rows)
        ↓
read rows one by one
        ↓
check user_id = 8472
        ↓
10 match
```

---

## 2. Estimated vs actual rows

Your plan says:

```text
rows=10
```

and:

```text
actual ... rows=10.00
```

This is actually **very good**.

PostgreSQL estimated:

> "I'll find approximately 10 rows."

Reality:

> "I found exactly 10."

So the planner's statistics are accurate here.

This is important because **`rows` doesn't mean rows scanned**.

It means:

```text
rows=10
```

→ estimated rows **returned by this node**.

---

## 3. The interesting number

This:

```text
Rows Removed by Filter: 99990
```

is telling us:

```text
100,000 total rows
        ↓
99,990 rejected
        ↓
10 returned
```

So PostgreSQL examined essentially the entire table to find those 10 rows.

That's our inefficiency.

---

# 4. Now let's understand `Buffers`

You got:

```text
Buffers: shared hit=899
```

This is extremely useful.

It means PostgreSQL accessed **899 shared buffers/pages**, and they were already present in PostgreSQL's buffer cache.

Conceptually:

```text
Database pages
      ↓
PostgreSQL shared_buffers
      ↓
899 pages already in memory
      ↓
"shared hit"
```

So this particular execution didn't need to read those pages from disk.

That's one reason your query is only:

```text
Execution Time: 5.244 ms
```

even though it scanned 100K rows.

**Don't conclude that a sequential scan is always slow.**

100K rows is small enough that a sequential scan can be perfectly reasonable.

---

# 5. `Planning Time` vs `Execution Time`

You have:

```text
Planning Time: 1.381 ms
Execution Time: 5.244 ms
```

These are different phases:

```text
SQL
 ↓
Planning
 ↓ 1.381 ms
Execution
 ↓ 5.244 ms
Result
```

For this query, execution dominates.

---

# 6. Why did PostgreSQL choose Seq Scan?

This is the important question.

We have:

```text
100,000 rows
        ↓
only 10 match
```

You might think:

> "Obviously PostgreSQL should use an index."

But **there is no index on `user_id`**.

The only relevant index automatically created for `orders` is the primary-key index:

```text
orders_pkey
       ↓
orders.id
```

Our query is:

```sql
WHERE user_id = 8472
```

That index can't help.

So PostgreSQL effectively has:

```text
Option A
Sequentially scan orders

Option B
?
No useful user_id index exists
```

Therefore:

```text
Seq Scan
```

is the obvious choice.

---

# 7. Now let's make the experiment interesting 🔥

Create the index:

```sql
CREATE INDEX idx_orders_user_id
ON orders(user_id);
```

Then run **the exact same query again**:

```sql
EXPLAIN ANALYZE
SELECT *
FROM orders
WHERE user_id = 8472;
```

Don't change anything else.

Then paste the new output.

We'll compare:

```text
BEFORE

Seq Scan
100,000 rows examined
10 returned
899 buffers
~5 ms


              ↓ INDEX


AFTER

???
???
???
```

And this time, I want you to identify **exactly what changed** in the execution plan.
