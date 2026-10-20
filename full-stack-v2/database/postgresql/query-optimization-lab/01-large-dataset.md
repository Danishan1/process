Yes. Let's build a **small but production-like PostgreSQL dataset** that we'll progressively make larger for `EXPLAIN ANALYZE`.

We'll use an e-commerce model:

```text
users
  │
  └── orders
        │
        └── order_items
                │
                └── products
```

Start with this setup.

### 1. Create database

From terminal:

```bash
psql -U postgres
```

Then:

```sql
CREATE DATABASE explain_lab;
```

Connect:

```sql
\c explain_lab
```

---

## 2. Create `users`

```sql
CREATE TABLE users (
    id BIGSERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    city VARCHAR(100),
    created_at TIMESTAMP NOT NULL DEFAULT NOW()
);
```

Generate **100,000 users**:

```sql
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
FROM generate_series(1, 100000) AS i;
```

Check:

```sql
SELECT COUNT(*) FROM users;
```

---

# 3. Create `products`

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

Generate 10,000 products:

```sql
INSERT INTO products (name, category, price, stock, created_at)
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
FROM generate_series(1, 10000) AS i;
```

---

# 4. Create `orders`

This is going to be our **main performance table**.

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

Now generate **5 million orders**:

```sql
INSERT INTO orders (
    user_id,
    status,
    total_amount,
    created_at
)
SELECT
    FLOOR(random() * 100000 + 1)::BIGINT,
    CASE FLOOR(random() * 4)::INTEGER
        WHEN 0 THEN 'PENDING'
        WHEN 1 THEN 'PAID'
        WHEN 2 THEN 'SHIPPED'
        ELSE 'CANCELLED'
    END,
    ROUND((random() * 50000 + 100)::numeric, 2),
    NOW() - (random() * INTERVAL '1000 days')
FROM generate_series(1, 5000000);
```

This may take some time because we're inserting **5 million rows**.

Check:

```sql
SELECT COUNT(*) FROM orders;
```

---

# 5. Create `order_items`

```sql
CREATE TABLE order_items (
    id BIGSERIAL PRIMARY KEY,
    order_id BIGINT NOT NULL,
    product_id BIGINT NOT NULL,
    quantity INTEGER NOT NULL,
    price NUMERIC(10,2) NOT NULL,

    CONSTRAINT fk_order_items_order
        FOREIGN KEY (order_id)
        REFERENCES orders(id),

    CONSTRAINT fk_order_items_product
        FOREIGN KEY (product_id)
        REFERENCES products(id)
);
```

Generate around **10 million items**:

```sql
INSERT INTO order_items (
    order_id,
    product_id,
    quantity,
    price
)
SELECT
    FLOOR(random() * 5000000 + 1)::BIGINT,
    FLOOR(random() * 10000 + 1)::BIGINT,
    FLOOR(random() * 5 + 1)::INTEGER,
    ROUND((random() * 50000 + 100)::numeric, 2)
FROM generate_series(1, 10000000);
```

---

# 6. Very important: update PostgreSQL statistics

After generating a large amount of data:

```sql
ANALYZE;
```

This is important because PostgreSQL's query planner needs statistics about the tables.

You can think of it as:

```text
Data
 ↓
ANALYZE
 ↓
PostgreSQL learns distribution/cardinality
 ↓
Query Planner makes better decisions
```

We'll later deliberately create **bad statistics/cardinality estimates** to understand why plans go wrong.

---

# 7. Verify the dataset

Run:

```sql
SELECT COUNT(*) FROM users;

SELECT COUNT(*) FROM products;

SELECT COUNT(*) FROM orders;

SELECT COUNT(*) FROM order_items;
```

You should have approximately:

```text
users          100,000
products        10,000
orders       5,000,000
order_items 10,000,000
```

---

# 8. Don't create indexes yet

This is important for our experiment.

**For now, don't create:**

```sql
CREATE INDEX ...
```

We intentionally want:

```text
orders
  ↓
5 million rows
  ↓
Seq Scan
```

Then we'll run:

```sql
EXPLAIN ANALYZE
SELECT *
FROM orders
WHERE user_id = 84729;
```

We'll see the actual problem.

Then we'll create:

```sql
CREATE INDEX ...
```

and run the **same query again**.

That gives us a real:

```text
Before optimization
        ↓
EXPLAIN ANALYZE
        ↓
Identify bottleneck
        ↓
Add index
        ↓
EXPLAIN ANALYZE
        ↓
Compare
```

That's much better for learning than just memorizing what `Index Scan` means.

### One caution

`EXPLAIN ANALYZE` **actually executes the query**. For our `SELECT` experiments that's fine, but remember this when you eventually use it on `UPDATE`, `DELETE`, or other mutating queries.
