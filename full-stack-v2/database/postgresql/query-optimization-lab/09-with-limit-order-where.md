Perfect. **Now we're hitting a very common production pattern: `WHERE + ORDER BY + LIMIT`.**

Your plan is:

```text
Limit
  ↓
Sort
  ↓
Bitmap Heap Scan
  ↓
Bitmap Index Scan
```

Let's understand exactly what PostgreSQL is doing.

### 1. Current execution

Your query is:

```sql
SELECT *
FROM orders
WHERE user_id = 8472
ORDER BY created_at DESC
LIMIT 5;
```

PostgreSQL currently does:

```text
idx_orders_user_id
        ↓
find 10 orders for user 8472
        ↓
fetch those 10 rows
        ↓
sort by created_at DESC
        ↓
return first 5
```

The key evidence is:

```text
Sort Key: created_at DESC
Sort Method: quicksort
Memory: 25kB
```

So **the database is explicitly sorting the result**.

---

## 2. But notice something important about `LIMIT`

We only want:

```text
5 rows
```

but PostgreSQL first obtains:

```text
10 rows
```

and sorts them.

With our tiny dataset, that's irrelevant:

```text
Execution Time: 0.076 ms
```

But imagine:

```text
user has 2,000,000 orders
```

and we ask:

```sql
ORDER BY created_at DESC
LIMIT 5
```

Now PostgreSQL could potentially have to:

```text
find 2,000,000 rows
        ↓
sort 2,000,000 rows
        ↓
return 5
```

That is obviously wasteful.

---

# 3. This is where index design gets interesting

We could create:

```sql
CREATE INDEX idx_orders_user_created
ON orders(user_id, created_at DESC);
```

Now the index itself is ordered approximately like:

```text
user_id
   ↓
created_at DESC
   ↓
rows
```

For:

```sql
WHERE user_id = 8472
ORDER BY created_at DESC
LIMIT 5
```

PostgreSQL can potentially do:

```text
idx_orders_user_created
        ↓
user_id = 8472
        ↓
already ordered by created_at DESC
        ↓
take first 5
        ↓
STOP
```

No separate sorting operation is necessary.

That's the real power of combining **filtering + ordering in an index**.

---

# 4. This is a production pattern worth remembering

You'll see queries like this everywhere:

### User's latest orders

```sql
WHERE user_id = ?
ORDER BY created_at DESC
LIMIT 20
```

### Latest notifications

```sql
WHERE user_id = ?
ORDER BY created_at DESC
LIMIT 50
```

### Latest messages

```sql
WHERE conversation_id = ?
ORDER BY created_at DESC
LIMIT 50
```

### Latest activity

```sql
WHERE account_id = ?
ORDER BY created_at DESC
LIMIT 20
```

The index often becomes:

```text
(filter_column, ordering_column)
```

For our case:

```text
(user_id, created_at DESC)
```

---

# 5. But don't create it yet

Let's first understand an important SDE-II habit.

Your current plan is **not a performance problem**.

We have:

```text
10 matching rows
25 KB sort memory
0.076 ms execution
```

So if this were production, blindly adding another index would be questionable.

We're deliberately doing it because we're learning how the plan changes.

### Now create:

```sql
CREATE INDEX idx_orders_user_created
ON orders(user_id, created_at DESC);
```

Then rerun:

```sql
EXPLAIN (ANALYZE, BUFFERS)
SELECT *
FROM orders
WHERE user_id = 8472
ORDER BY created_at DESC
LIMIT 5;
```

This time, **don't focus primarily on execution time** because our dataset is too small.

Instead look for whether:

```text
Sort
```

disappears.

And identify what scan PostgreSQL chooses.

That's the next important lesson:

> **Can an index eliminate a sort, and can `LIMIT` allow PostgreSQL to stop reading early?**
