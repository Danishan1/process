Perfect. Let's start the **actual `EXPLAIN ANALYZE` lab**.

Assuming you now have:

```text
users    → 10,000
products → 1,000
orders   → 100,000
```

and **no index on `orders.user_id`**.

## Experiment 1 — Find a user's orders

Run:

```sql
EXPLAIN ANALYZE
SELECT *
FROM orders
WHERE user_id = 8472;
```

You should get something similar to:

```text
Seq Scan on orders
  (cost=0.00..XXXX.XX rows=10 width=...)
  (actual time=... rows=... loops=1)
  Filter: (user_id = 8472)
  Rows Removed by Filter: ...
Planning Time: ...
Execution Time: ...
```

### Don't worry about the exact numbers

Your numbers will differ because our data is randomly generated.

Instead, answer these **5 questions from your actual output**:

1. **What scan did PostgreSQL choose?**
2. **How many rows did it estimate?** (`rows=`)
3. **How many rows did it actually find?** (`actual ... rows=`)
4. **How many rows were removed by the filter?**
5. **What was the execution time?**

Paste the complete output here.

Then **you diagnose the query first**. I'll review your reasoning rather than immediately giving you the answer.
