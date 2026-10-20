Yes. Since this is just our practice database, let's **cancel the current batch and start clean**.

### 1. Stop the running query

From your **second `psql` terminal** where you ran `pg_stat_activity`:

```sql
SELECT pg_cancel_backend(21988);
```

You should get:

```text
 pg_cancel_backend
-------------------
 t
```

Wait a few seconds and check:

```sql
SELECT pid, state, query
FROM pg_stat_activity
WHERE datname = 'temp_test';
```

If PID `21988` is still active, forcefully terminate it:

```sql
SELECT pg_terminate_backend(21988);
```

---

### 2. Connect to `temp_test`

```sql
\c temp_test
```

Check what was created:

```sql
\dt
```

---

### 3. Drop everything and start fresh

Since this is our lab, easiest is:

```sql
DROP TABLE IF EXISTS order_items;
DROP TABLE IF EXISTS orders;
DROP TABLE IF EXISTS products;
DROP TABLE IF EXISTS users;
```

Because of foreign keys, **drop in this order**.

Then:

```sql
\dt
```

You should see:

```text
Did not find any relations.
```

---

# 4. Start with a much smaller dataset

Let's not jump to 5 million rows yet.

We'll use:

```text
users          10,000
products        1,000
orders        100,000
order_items   200,000
```

That's already enough to learn `EXPLAIN ANALYZE`.

For example:

```sql
CREATE TABLE users (
    id BIGSERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    city VARCHAR(100),
    created_at TIMESTAMP NOT NULL DEFAULT NOW()
);

INSERT INTO users (name, email, city, created_at)
SELECT
    'User ' || i,
    'user' || i || '@example.com',
    CASE (i % 5)
        WHEN 0 THEN 'Delhi'
        WHEN 1 THEN 'Mumbai'
        WHEN 2 THEN 'Bangalore'
        WHEN 3 THEN 'Hyderabad'
        ELSE 'Pune'
    END,
    NOW() - (random() * INTERVAL '1000 days')
FROM generate_series(1, 10000) AS i;
```

Then we'll create the other tables **one at a time**, checking each step.

This time we won't send a giant 15-million-row script to PostgreSQL. 😄

Once `users` finishes, run:

```sql
SELECT COUNT(*) FROM users;
```

Great. Now let's create the **`products` table and 1,000 rows**.

Run this in `temp_test`:

```sql
CREATE TABLE products (
    id BIGSERIAL PRIMARY KEY,
    name VARCHAR(200) NOT NULL,
    category VARCHAR(100) NOT NULL,
    price NUMERIC(10,2) NOT NULL,
    stock INTEGER NOT NULL,
    created_at TIMESTAMP NOT NULL DEFAULT NOW()
);
```

Then insert the data:

```sql
INSERT INTO products (
    name,
    category,
    price,
    stock,
    created_at
)
SELECT
    'Product ' || i,
    CASE (i % 5)
        WHEN 0 THEN 'Electronics'
        WHEN 1 THEN 'Clothing'
        WHEN 2 THEN 'Books'
        WHEN 3 THEN 'Home'
        ELSE 'Sports'
    END,
    ROUND((random() * 50000 + 100)::numeric, 2),
    FLOOR(random() * 1000)::INTEGER,
    NOW() - (random() * INTERVAL '1000 days')
FROM generate_series(1, 1000) AS i;
```

Verify:

```sql
SELECT COUNT(*) FROM products;
```

Expected:

```text
 count
-------
 1000
```

And let's inspect a few:

```sql
SELECT *
FROM products
LIMIT 10;
```

### Then create `orders`

```sql
CREATE TABLE orders (
    id BIGSERIAL PRIMARY KEY,
    user_id BIGINT NOT NULL,
    status VARCHAR(20) NOT NULL,
    total_amount NUMERIC(12,2) NOT NULL,
    created_at TIMESTAMP NOT NULL DEFAULT NOW(),

    CONSTRAINT fk_orders_user
        FOREIGN KEY (user_id)
        REFERENCES users(id)
);
```

Insert **100,000 orders**:

```sql
INSERT INTO orders (
    user_id,
    status,
    total_amount,
    created_at
)
SELECT
    FLOOR(random() * 10000 + 1)::BIGINT,
    CASE FLOOR(random() * 4)::INTEGER
        WHEN 0 THEN 'PENDING'
        WHEN 1 THEN 'PAID'
        WHEN 2 THEN 'SHIPPED'
        ELSE 'CANCELLED'
    END,
    ROUND((random() * 50000 + 100)::numeric, 2),
    NOW() - (random() * INTERVAL '1000 days')
FROM generate_series(1, 100000);
```

Then:

```sql
SELECT COUNT(*) FROM orders;
```

Expected:

```text
100000
```

**Don't create any indexes yet** except the primary keys/unique constraint automatically created by PostgreSQL.

After this, we'll do something interesting: **run our first `EXPLAIN ANALYZE` on `orders` before adding an index**, so you can see the `Seq Scan` for yourself.
