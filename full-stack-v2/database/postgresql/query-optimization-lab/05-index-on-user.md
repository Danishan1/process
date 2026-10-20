Excellent — this is exactly the kind of real-world plan we wanted to see. **The index did not produce a plain `Index Scan`; PostgreSQL chose a `Bitmap Heap Scan` + `Bitmap Index Scan`.** That's an important SDE-II concept.

### 1. What changed?

Before the index:

```text
Seq Scan
  → scan all 100,000 orders
  → filter user_id = 8472
  → 5.244 ms
```

After the index:

```text
Bitmap Index Scan
  → find matching row locations in the index
        ↓
Bitmap Heap Scan
  → fetch those rows from the table
```

And the execution time dropped:

```text
5.244 ms  →  0.081 ms
```

That's roughly **65× faster** for this query.

---

## 2. Read the new plan from bottom to top

### Step 1 — Bitmap Index Scan

```text
-> Bitmap Index Scan on idx_orders_user_id
   (cost=0.00..4.37 rows=10 width=0)
   (actual time=0.031..0.031 rows=10.00 loops=1)

   Index Cond: (user_id = 8472)
   Index Searches: 1
   Buffers: shared hit=5
```

PostgreSQL first searches:

```text
idx_orders_user_id
```

for:

```sql
user_id = 8472
```

It finds **10 matching index entries**.

Notice:

```text
estimated rows = 10
actual rows    = 10
```

Again, excellent cardinality estimation.

The index operation itself took only:

```text
0.031 ms
```

---

# 3. Why doesn't it immediately fetch the rows?

Because PostgreSQL chose a **Bitmap** strategy.

Think of it conceptually like:

```text
Index
  ↓
Find matching row locations
  ↓
Build bitmap of table pages/rows
  ↓
Fetch required table pages
```

The bitmap tells PostgreSQL:

> "These are the locations in the `orders` table where the matching rows live."

Then PostgreSQL performs:

```text
Bitmap Heap Scan
```

---

## 4. Bitmap Heap Scan

```text
Bitmap Heap Scan on orders
(cost=4.37..41.33 rows=10 width=39)
(actual time=0.043..0.059 rows=10.00 loops=1)

Recheck Cond: (user_id = 8472)

Heap Blocks: exact=10

Buffers: shared hit=15
```

This is the actual table access.

The important line:

```text
Heap Blocks: exact=10
```

means PostgreSQL needed to access **10 table pages** containing the matching rows.

And:

```text
Buffers: shared hit=15
```

means the required pages were already in PostgreSQL's buffer cache.

Compare this with our previous query:

```text
Before:
Buffers: shared hit=899
```

Now:

```text
After:
Buffers: shared hit=15
```

That's a massive reduction in page accesses.

---

# 5. The really important concept: why Bitmap instead of Index Scan?

This is where you move beyond basic "indexes make queries faster."

PostgreSQL has multiple ways to use an index.

For example:

```text
Index Scan
Bitmap Index Scan
    ↓
Bitmap Heap Scan
Index Only Scan
```

PostgreSQL estimates which one is cheapest.

For our query, it decided:

```text
Bitmap Index Scan
        +
Bitmap Heap Scan
```

was the best strategy.

### Conceptually

An ordinary Index Scan can behave like:

```text
index → row → index → row → index → row
```

If many matching rows are scattered across the table, that can cause lots of random heap access.

Bitmap scanning instead does something closer to:

```text
index
  ↓
collect all matching locations
  ↓
organize them by heap page
  ↓
fetch required pages efficiently
```

That's particularly useful when the query returns **more than a handful of rows**, but still a relatively small fraction of the table.

---

# 6. Look at the costs

Before:

```text
Seq Scan on orders
cost=0.00..2149.00
```

After:

```text
Bitmap Heap Scan
cost=4.37..41.33
```

Huge difference.

But remember:

> **Cost is NOT milliseconds.**

Don't say in an interview:

> "The cost was 2149 ms."

Wrong.

Instead:

> "The planner estimated the indexed plan to be substantially cheaper than the sequential scan."

---

# 7. Actual performance

The strongest evidence is:

```text
Execution Time: 0.081 ms
```

versus:

```text
Execution Time: 5.244 ms
```

So our experiment is:

|                        | Before index |                After index |
| ---------------------- | -----------: | -------------------------: |
| Plan                   |     Seq Scan | Bitmap Heap + Bitmap Index |
| Rows                   |           10 |                         10 |
| Rows examined/filtered |      100,000 |    ~10 relevant rows/pages |
| Buffers                |          899 |                         15 |
| Execution              |     5.244 ms |                   0.081 ms |

This is a very good example of **why indexes matter**.

---

# 8. One subtle interview point

Don't conclude:

> "Bitmap Heap Scan is better than Index Scan."

That's **not** the right conclusion.

The correct conclusion is:

> PostgreSQL chooses between Seq Scan, Index Scan, Bitmap Scan, etc. based on its cost estimates and the expected number/distribution of matching rows.

For example:

```sql
WHERE user_id = 8472
```

might return 10 rows.

But imagine:

```sql
WHERE user_id BETWEEN 1 AND 5000
```

Now potentially tens of thousands of rows could match.

A sequential scan might become cheaper again.

So:

```text
Index exists
      ↓
Does NOT mean
      ↓
Index will always be used
```

That's a **very important production concept**.

---

# 9. And notice something else

Our data is only:

```text
100,000 orders
```

and the original query took only:

```text
5.244 ms
```

So in a real production investigation, we shouldn't say:

> "5 ms is terrible, let's add an index."

Instead we'd ask:

> "Is this query actually a performance problem?"

Suppose production has:

```text
10 million orders
50,000 queries/sec
```

Then the same access pattern could become extremely important.

This is the mindset I want you to develop for SDE-II:

```text
Query
  ↓
EXPLAIN ANALYZE
  ↓
Understand actual workload
  ↓
Identify bottleneck
  ↓
Change
  ↓
Measure again
```

---

## Next experiment: make it more realistic

Now let's introduce a **second filter**, because this is where index design becomes interesting.

Run:

```sql
EXPLAIN (ANALYZE, BUFFERS)
SELECT *
FROM orders
WHERE user_id = 8472
  AND status = 'PAID';
```

**Don't create another index yet.**

Let's see what PostgreSQL does with our existing:

```text
idx_orders_user_id
```

Then we'll use that plan to answer the real-world question:

> **Should we create a single-column index, composite index, or multiple indexes?**

That gets us into actual production index design rather than just "create an index and make the query faster."
