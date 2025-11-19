# MySQL Interview Questions (Beginner → Advanced)

## 1. **Beginner-Level Questions**

### 1. What is MySQL?

Explain: Open-source relational database, uses SQL, ACID compliant, widely used in web apps.

### 2. What is a database, table, row, and column?

Explain definitions and differences.

### 3. What is SQL?

Explain: Structured Query Language for defining and manipulating data.

### 4. What are the major SQL commands?

* DDL (CREATE, ALTER, DROP)
* DML (INSERT, UPDATE, DELETE)
* DQL (SELECT)
* DCL (GRANT, REVOKE)
* TCL (COMMIT, ROLLBACK)

### 5. What is a primary key?

Unique identifier for each row.

### 6. What is a foreign key?

A field that references another table’s primary key.

### 7. What are indexes?

Data structures that improve query speed but slow down writes.

### 8. What is a UNIQUE constraint?

Ensures no duplicate values in a column.

### 9. What is AUTO_INCREMENT?

Automatically increments values for primary keys.

### 10. What is the difference between WHERE and HAVING?

* WHERE filters before grouping.
* HAVING filters after grouping.

---

## 2. **Intermediate-Level Questions**

### 11. What is a JOIN? Explain its types.

* INNER JOIN: matching rows.
* LEFT JOIN: all left + matched right.
* RIGHT JOIN: all right + matched left.
* FULL JOIN: not supported directly but simulated.
* CROSS JOIN: Cartesian product.

### 12. What is normalization? Types?

* 1NF: atomic values
* 2NF: no partial dependency
* 3NF: no transitive dependency

### 13. What is denormalization?

Intentional introduction of redundancy for performance.

### 14. What is an index? Types?

* BTREE (default)
* FULLTEXT
* HASH (in-memory engines)

### 15. What is the difference between CHAR and VARCHAR?

* CHAR: fixed size, faster.
* VARCHAR: variable length, saves space.

### 16. What is a transaction?

A sequence of operations treated as a single unit.

### 17. What are ACID properties?

* Atomicity
* Consistency
* Isolation
* Durability

### 18. What is the difference between DELETE, DROP, TRUNCATE?

* DELETE: row-wise, logged, slow.
* TRUNCATE: removes all rows, fast.
* DROP: removes table entirely.

### 19. What are stored procedures?

Reusable SQL routines executed on the server.

### 20. What are triggers?

Automatic execution of SQL when certain events happen (INSERT/UPDATE/DELETE).

---

## 3. **Advanced-Level Questions**

### 21. Explain query optimization techniques.

* Proper indexing
* Avoid SELECT *
* Use LIMIT for pagination
* Analyze slow queries via EXPLAIN
* Avoid wildcards at string start

### 22. What is EXPLAIN?

Shows query execution plan to optimize performance.

### 23. Difference between InnoDB and MyISAM?

| Feature      | InnoDB | MyISAM         |
| ------------ | ------ | -------------- |
| Transactions | Yes    | No             |
| Foreign Keys | Yes    | No             |
| Locking      | Row    | Table          |
| Speed        | Slower | Faster (reads) |

### 24. What is a deadlock?

Two transactions waiting for each other’s locks → system halts.

### 25. How does MySQL handle concurrency?

Using MVCC (Multiversion Concurrency Control).

### 26. How to handle scaling?

* Read Replicas
* Master-Slave replication
* Sharding
* Partitioning

### 27. What are MySQL partitions?

Splitting tables into smaller logical pieces for performance.

### 28. What is connection pooling?

Reusing database connections to improve performance (important in Node.js).

### 29. What is the difference between BETWEEN vs IN vs LIKE?

Explains performance and use cases.

### 30. Explain SQL Injection and how to prevent it.

Use:

* Prepared statements
* Parameterized queries
* ORM query builders

---

## 4. **Node.js + MySQL Specific Questions**

### 31. How do you connect MySQL with Node.js?

Through libraries:

* mysql2
* sequelize
* knex.js

### 32. How to use prepared statements in Node.js?

`connection.execute("SELECT * FROM users WHERE id=?", [id])`

### 33. What is connection pooling in Node.js?

Maintaining a pool of connections to avoid overhead of creating new ones.

### 34. How to handle transactions in Node.js?

Using:

```js
const conn = await pool.getConnection();
await conn.beginTransaction();
// queries
await conn.commit();
```

### 35. How to structure Node.js backend for MySQL?

* Repository layer
* Models
* Services
* Controllers
