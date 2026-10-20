# Summary: MongoDB — Concise Revision Table

| Concept               | Summary                                     | Key Point                                   |
| --------------------- | ------------------------------------------- | ------------------------------------------- |
| **MongoDB**           | Document-oriented NoSQL database            | Stores data as BSON documents               |
| **Database**          | Container for collections                   | Similar to SQL database                     |
| **Collection**        | Group of documents                          | Similar to SQL table                        |
| **Document**          | JSON-like BSON object                       | Similar to SQL row                          |
| **Field**             | Property inside document                    | Similar to SQL column                       |
| **BSON**              | Binary JSON representation                  | Supports richer data types                  |
| **`_id`**             | Unique document identifier                  | Usually `ObjectId`                          |
| **CRUD**              | Insert, Find, Update, Delete                | Core database operations                    |
| **Embedding**         | Store related data inside document          | Good when data is read together             |
| **Referencing**       | Store relationship using IDs                | Good for large/shared/independent data      |
| **16 MB limit**       | Maximum BSON document size                  | Avoid unbounded embedded arrays             |
| **Index**             | Data structure for faster queries           | Costs memory, disk & write performance      |
| **Compound Index**    | Index on multiple fields                    | Field order matters                         |
| **ESR Rule**          | Equality → Sort → Range                     | Useful for compound-index design            |
| **Unique Index**      | Prevents duplicate values                   | E.g. unique email                           |
| **TTL Index**         | Automatically expires documents             | Sessions/tokens/temp data                   |
| **Multikey Index**    | Indexes array fields                        | Used for array queries                      |
| **Partial Index**     | Indexes only matching documents             | Smaller, targeted index                     |
| **`explain()`**       | Shows query execution plan                  | Look for `IXSCAN` vs `COLLSCAN`             |
| **Aggregation**       | Pipeline for data processing                | `$match → $group → $sort`                   |
| **`$match`**          | Filters documents                           | Prefer early filtering                      |
| **`$group`**          | Groups and calculates values                | Similar to `GROUP BY`                       |
| **`$unwind`**         | Expands array elements                      | One array item → one pipeline document      |
| **`$lookup`**         | Join-like operation                         | Use when appropriate; don't overuse         |
| **Atomicity**         | Single-document operations are atomic       | Major MongoDB design advantage              |
| **Transaction**       | Atomic operations across multiple documents | Use when business consistency requires it   |
| **Write Concern**     | Controls write acknowledgment               | `w: "majority"` for stronger durability     |
| **Read Concern**      | Controls consistency of reads               | E.g. `majority`                             |
| **Read Preference**   | Controls where reads go                     | Primary/secondary/nearest                   |
| **Replica Set**       | Primary + secondaries                       | High availability & replication             |
| **Primary**           | Handles normal writes                       | One primary at a time                       |
| **Secondary**         | Replicates primary data                     | Can serve reads depending on configuration  |
| **Election**          | Chooses new primary after failure           | Majority/quorum based                       |
| **Replication Lag**   | Secondary behind primary                    | Important production metric                 |
| **Sharding**          | Distributes data across shards              | Horizontal scaling                          |
| **Shard Key**         | Determines data distribution                | Must avoid hotspots                         |
| **`mongos`**          | Query router                                | Routes operations in sharded clusters       |
| **Targeted Query**    | Query can identify relevant shard(s)        | More efficient                              |
| **Scatter-Gather**    | Query hits multiple shards                  | Potentially expensive                       |
| **Schema Validation** | Enforces document rules                     | Flexible ≠ schema-less                      |
| **Denormalization**   | Duplicate useful data                       | Faster reads, harder consistency            |
| **Pagination**        | Retrieve data in pages                      | Cursor/range pagination beats huge `skip()` |
| **Connection Pool**   | Reuses DB connections                       | Avoid connection per request                |
| **N+1 Query**         | One query + many follow-up queries          | Fix with batching/modeling/aggregation      |

#### 🔥 MongoDB in 10 lines

|  ## | Remember                                                                |
| -: | ----------------------------------------------------------------------- |
|  1 | **MongoDB = document database**                                         |
|  2 | **Database → Collection → Document → Field**                            |
|  3 | **BSON = MongoDB's storage format**                                     |
|  4 | **Embed when data belongs together and is bounded**                     |
|  5 | **Reference when data is large/shared/independent**                     |
|  6 | **Indexes make reads faster but writes more expensive**                 |
|  7 | **`explain()` tells you why a query is slow**                           |
|  8 | **Replica Set = high availability**                                     |
|  9 | **Sharding = horizontal scaling**                                       |
| 10 | **Design MongoDB around access patterns, not relational normalization** |


# MongoDB — Deep Revision

### 1. Core mental model

> **MongoDB is a document-oriented NoSQL database that stores JSON-like documents in BSON format.**

Think:

```text
MongoDB
   │
   ├── Database
   │     └── Collection
   │            └── Document
   │                   └── Fields
   │
   ├── Indexes
   ├── Aggregation
   ├── Replica Set
   └── Sharded Cluster
```

The relational equivalent is roughly:

| SQL         | MongoDB                   |
| ----------- | ------------------------- |
| Database    | Database                  |
| Table       | Collection                |
| Row         | Document                  |
| Column      | Field                     |
| Primary Key | `_id`                     |
| JOIN        | `$lookup` / data modeling |
| SQL query   | MongoDB query             |
| Index       | Index                     |
| Transaction | Transaction               |

## 2. Document

MongoDB stores documents like:

```javascript
{
  _id: ObjectId("..."),
  name: "Alice",
  age: 30,
  email: "alice@example.com",
  skills: ["Java", "Kafka", "MongoDB"]
}
```

Unlike a traditional relational row, a document can contain:

```text
nested objects
arrays
different fields
```

Example:

```javascript
{
  name: "Alice",
  address: {
    city: "Delhi",
    country: "India"
  },
  orders: [
    { id: 101, amount: 500 },
    { id: 102, amount: 800 }
  ]
}
```

This is one of MongoDB's biggest strengths:

> **Store related data together when it is commonly accessed together.**

## 3. BSON

MongoDB stores documents as **BSON**:

> Binary JSON

BSON supports types such as:

```text
String
Integer
Double
Boolean
Date
Array
Embedded Document
ObjectId
Decimal128
Binary
Null
```

Important interview point:

> MongoDB clients often work with JSON-like objects, but MongoDB's actual storage representation is BSON.

## 4. `_id`

Every MongoDB document normally has an `_id`.

Example:

```javascript
{
  _id: ObjectId("64...")
}
```

It uniquely identifies the document within its collection.

If you don't provide `_id`, MongoDB drivers commonly generate an `ObjectId`.

## 5. ObjectId

An `ObjectId` is a BSON identifier designed to be unique and efficient.

Example:

```javascript
ObjectId("64f123...")
```

Don't rely on an ObjectId as a perfect business timestamp or business identifier.

If your business needs:

```text
orderId
customerId
invoiceNumber
```

you may have a separate application-level identifier.

## 6. Database → Collection → Document

Example:

```text
MongoDB
│
└── ecommerce
      │
      ├── users
      │     ├── document
      │     ├── document
      │     └── document
      │
      └── orders
            ├── document
            ├── document
            └── document
```

Unlike SQL tables, MongoDB collections don't require every document to have exactly the same fields.

That's the idea of **flexible schema**.

But:

> Flexible schema does **not** mean "no schema."

You should still enforce sensible application/schema validation.

## 7. CRUD

### Insert

```javascript
db.users.insertOne({
  name: "Alice",
  age: 30
})
```

Multiple:

```javascript
db.users.insertMany([
  { name: "Alice" },
  { name: "Bob" }
])
```

### Find

```javascript
db.users.find({
  age: 30
})
```

One:

```javascript
db.users.findOne({
  name: "Alice"
})
```

### Update

```javascript
db.users.updateOne(
  { name: "Alice" },
  { $set: { age: 31 } }
)
```

Increment:

```javascript
db.users.updateOne(
  { name: "Alice" },
  { $inc: { age: 1 } }
)
```

### Delete

```javascript
db.users.deleteOne({
  name: "Alice"
})
```

## 8. Query operators

Common operators:

```text
$gt
$gte
$lt
$lte
$eq
$ne
$in
$nin
$and
$or
$not
$exists
```

Example:

```javascript
db.users.find({
  age: { $gte: 18, $lt: 30 }
})
```

Equivalent idea:

```text
18 <= age < 30
```

## 9. Arrays

Document:

```javascript
{
  name: "Alice",
  skills: ["Java", "MongoDB", "Kafka"]
}
```

Query:

```javascript
db.users.find({
  skills: "Kafka"
})
```

MongoDB can match an array containing the value.

## 10. Embedded documents

Example:

```javascript
{
  name: "Alice",
  address: {
    city: "Delhi",
    country: "India"
  }
}
```

Query nested field:

```javascript
db.users.find({
  "address.city": "Delhi"
})
```

This is called **dot notation**.

## 11. Projection

Suppose document contains:

```javascript
{
  name: "Alice",
  age: 30,
  email: "alice@example.com"
}
```

Only return name:

```javascript
db.users.find(
  {},
  { name: 1 }
)
```

Exclude email:

```javascript
db.users.find(
  {},
  { email: 0 }
)
```

Projection reduces unnecessary data transfer.

## 12. Sorting

```javascript
db.users.find().sort({
  age: 1
})
```

Ascending:

```text
1
```

Descending:

```text
-1
```

Example:

```javascript
db.users.find().sort({
  age: -1
})
```

## 13. Limit and skip

```javascript
db.users.find()
  .sort({ age: -1 })
  .limit(10)
```

Pagination with `skip`:

```javascript
db.users.find()
  .skip(100)
  .limit(20)
```

But:

> Large `skip()` values can become inefficient.

For large datasets, **range/cursor-based pagination** is generally preferable.

## 14. Indexes — THE most important MongoDB topic

Without an appropriate index:

```text
Query
 ↓
scan many/all documents
```

This is a collection scan.

With an index:

```text
Query
 ↓
Index
 ↓
matching documents
```

Indexes can dramatically improve reads.

But indexes aren't free.

They consume:

```text
disk
RAM
write performance
maintenance overhead
```

## 15. Single-field index

```javascript
db.users.createIndex({
  email: 1
})
```

Now:

```javascript
db.users.find({
  email: "alice@example.com"
})
```

can use the index.

## 16. Compound index

Suppose queries frequently use:

```javascript
{
  status: "ACTIVE",
  age: { $gt: 30 }
}
```

Potential index:

```javascript
db.users.createIndex({
  status: 1,
  age: 1
})
```

Order matters.

## 17. Compound index prefix rule

Index:

```text
(status, age, city)
```

Can efficiently support queries involving the prefix:

```text
status
status + age
status + age + city
```

But not necessarily:

```text
age alone
city alone
```

This is the **ESR rule** territory.

## 18. ESR rule

For compound indexes, a useful design heuristic is:

> **Equality → Sort → Range**

Example query:

```javascript
db.orders.find({
  customerId: "C1",
  status: "PAID",
  amount: { $gt: 100 }
}).sort({
  createdAt: -1
})
```

A potential index:

```javascript
{
  customerId: 1,
  status: 1,
  createdAt: -1,
  amount: 1
}
```

The exact optimal index depends on query shape and workload, but the ESR principle is a strong starting point.

## 19. Explain

Never blindly create indexes.

Use:

```javascript
db.users.find({
  email: "alice@example.com"
}).explain("executionStats")
```

Look for things such as:

```text
IXSCAN
COLLSCAN
nReturned
totalKeysExamined
totalDocsExamined
executionTimeMillis
```

#### `COLLSCAN`

Collection scan.

Potential warning sign for a selective query.

#### `IXSCAN`

Index scan.

Generally desirable when an appropriate index can efficiently satisfy the query.

## 20. Selectivity

Suppose:

```text
gender = "M"
```

matches:

```text
50% of documents
```

Not highly selective.

But:

```text
email = "alice@example.com"
```

matches:

```text
1 document
```

Highly selective.

Highly selective predicates often benefit strongly from indexes.

## 21. Unique index

```javascript
db.users.createIndex(
  { email: 1 },
  { unique: true }
)
```

Now duplicate emails are rejected.

This is useful for business constraints such as:

```text
email
username
externalId
```

## 22. TTL index

TTL = Time To Live.

Useful for:

```text
sessions
temporary tokens
logs
cache-like documents
```

Example:

```javascript
db.sessions.createIndex(
  { expiresAt: 1 },
  { expireAfterSeconds: 0 }
)
```

MongoDB's TTL monitor removes expired documents asynchronously; don't treat expiration as an exact-to-the-second deletion mechanism.

## 23. Multikey index

If a field contains an array:

```javascript
{
  name: "Alice",
  skills: ["Java", "Kafka", "MongoDB"]
}
```

an index on:

```javascript
{ skills: 1 }
```

becomes a **multikey index**.

MongoDB indexes array elements for matching.

## 24. Partial index

Instead of indexing every document:

```javascript
db.users.createIndex(
  { email: 1 },
  {
    partialFilterExpression: {
      active: true
    }
  }
)
```

Useful when only a subset of documents matters.

Benefits:

```text
smaller index
less maintenance
potentially better performance
```

## 25. Sparse index

A sparse index only contains documents where the indexed field exists.

Conceptually:

```text
field exists → indexed
field missing → not indexed
```

Partial indexes are often more expressive and are generally preferable when you need conditional indexing.

## 26. Covered query

A covered query can be answered entirely from the index without fetching the full documents.

Suppose index:

```javascript
{
  email: 1,
  name: 1
}
```

Query:

```javascript
db.users.find(
  { email: "alice@example.com" },
  { name: 1, _id: 0 }
)
```

The index contains everything needed.

Potentially:

```text
Index
 ↓
Result
```

instead of:

```text
Index
 ↓
Document fetch
 ↓
Result
```

## 27. Aggregation Framework

MongoDB's aggregation pipeline is extremely important.

Think:

```text
Documents
   ↓
Stage 1
   ↓
Stage 2
   ↓
Stage 3
   ↓
Result
```

Example:

```javascript
db.orders.aggregate([
  { $match: { status: "PAID" } },
  { $group: {
      _id: "$customerId",
      total: { $sum: "$amount" }
  }},
  { $sort: { total: -1 } }
])
```

## 28. Important aggregation stages

Know these:

```text
$match
$project
$set
$unset
$group
$sort
$limit
$skip
$unwind
$lookup
$facet
$count
$replaceWith
```

## 29. `$match`

Filters documents.

```javascript
{
  $match: {
    status: "PAID"
  }
}
```

Important optimization:

> Put selective `$match` stages early when possible.

This reduces documents flowing through later stages.

## 30. `$project`

Controls output fields.

```javascript
{
  $project: {
    customerId: 1,
    amount: 1
  }
}
```

Can also calculate values.

## 31. `$group`

Groups documents.

Example:

```javascript
{
  $group: {
    _id: "$customerId",
    totalOrders: { $sum: 1 },
    totalAmount: { $sum: "$amount" }
  }
}
```

Output:

```javascript
{
  _id: "C1",
  totalOrders: 10,
  totalAmount: 5000
}
```

## 32. `$unwind`

Suppose:

```javascript
{
  customer: "Alice",
  items: [
    { product: "A", price: 100 },
    { product: "B", price: 200 }
  ]
}
```

`$unwind` turns it conceptually into:

```text
Alice + A
Alice + B
```

Useful for array-based aggregation.

## 33. `$lookup`

MongoDB's equivalent of a join-like operation.

Example:

```javascript
{
  $lookup: {
    from: "users",
    localField: "userId",
    foreignField: "_id",
    as: "user"
  }
}
```

But don't automatically model everything relationally and then use `$lookup` everywhere.

MongoDB encourages choosing a document model appropriate to access patterns.

## 34. Embedding vs referencing

One of the biggest MongoDB design decisions.

### Embed

```javascript
{
  customer: "Alice",
  address: {
    city: "Delhi",
    country: "India"
  }
}
```

Use embedding when:

```text
data belongs together
read together
bounded in size
updated together
```

Advantages:

```text
one read
simple access
often no join
```

## 35. Reference

Instead:

```javascript
{
  customerId: ObjectId("...")
}
```

and another collection:

```text
customers
```

Use references when:

```text
data is shared
relationship is large
child data grows without bound
independent lifecycle
```

## 36. The key MongoDB modeling question

Don't ask:

> "How would I normalize this?"

Ask:

> **"How will this data be accessed?"**

MongoDB schema design is **query/access-pattern driven**.

## 37. Unbounded arrays — danger

Bad:

```javascript
{
  user: "Alice",
  events: [
    event1,
    event2,
    event3,
    ...
    millions
  ]
}
```

The document can grow indefinitely.

Problems:

```text
large documents
large updates
memory pressure
16 MB document limit
```

Better:

```text
users
events
```

with a reference:

```javascript
{
  userId: ...
}
```

## 38. MongoDB 16 MB document limit

A BSON document has a maximum size of approximately:

> **16 MB**

Therefore don't embed unlimited:

```text
logs
comments
events
transactions
```

inside one document.

This is a classic interview question.

## 39. Atomicity

MongoDB guarantees atomicity at the **single-document level**.

Example:

```javascript
{
  balance: 1000,
  status: "ACTIVE"
}
```

Updating multiple fields in one document can be atomic.

This is one reason MongoDB encourages embedding related data when appropriate.

## 40. Transactions

MongoDB also supports multi-document ACID transactions.

Conceptually:

```text
BEGIN
  update document A
  update document B
  insert document C
COMMIT
```

or:

```text
ABORT
```

But don't automatically use transactions for everything.

Transactions add:

```text
coordination
latency
resource cost
operational complexity
```

Good MongoDB design often minimizes the need for multi-document transactions.

## 41. ACID

MongoDB supports ACID transactions.

#### Atomicity

All transactional operations succeed or roll back.

#### Consistency

Data moves between valid states according to application/database constraints.

#### Isolation

Concurrent operations don't observe an invalid intermediate transactional state according to the transaction isolation semantics.

#### Durability

Committed data is persisted according to write concern and deployment/storage guarantees.

## 42. Write concern

Write concern controls how strongly MongoDB acknowledges writes.

Conceptually:

```text
w: 1
```

means acknowledgment from the primary.

```text
w: "majority"
```

requests acknowledgment after a majority of voting members have durably acknowledged the write according to the configured semantics.

You can also control journaling-related behavior depending on deployment/version.

The key idea:

> **Write concern = how much confirmation you require before considering a write successful.**

## 43. Read concern

Read concern controls consistency/isolation characteristics of reads.

Common concepts include:

```text
local
available
majority
linearizable
snapshot
```

You don't need to memorize every nuance initially.

Think:

> **Read concern = what level of committed/consistent data should my read observe?**

## 44. Read preference

Read preference decides **where reads go** in a replica set.

Common modes:

```text
primary
primaryPreferred
secondary
secondaryPreferred
nearest
```

Example:

```text
primary
```

means read from primary.

```text
secondaryPreferred
```

allows secondaries to serve reads when appropriate.

Important:

> Read preference is not the same thing as read concern.

## 45. Replica Set

A replica set provides high availability.

Example:

```text
              Replica Set
                  |
        ┌─────────┼─────────┐
        ↓         ↓         ↓
     Primary   Secondary Secondary
```

Writes normally go to the primary.

Secondaries replicate the primary's data.

## 46. Primary failure

Suppose:

```text
Primary
   X
   |
Secondary 1
Secondary 2
```

The remaining eligible members hold an election.

One becomes:

```text
New Primary
```

Applications using appropriate connection settings can reconnect and discover the new primary.

This provides automatic failover.

## 47. Replica set election

Important concepts:

```text
voting members
majority
election
heartbeat
term
```

A primary needs a majority-based election to avoid split-brain scenarios.

This is why odd numbers of voting members are often useful:

```text
3 nodes → majority = 2
5 nodes → majority = 3
```

## 48. Why 3 nodes instead of 2?

With:

```text
Node A
Node B
```

if network connectivity breaks:

```text
A ↔ X ↔ B
```

who should become primary?

Neither side can safely establish a majority.

With:

```text
A
B
C
```

one side can potentially have:

```text
A + B = majority
```

and continue.

That's the purpose of quorum.

## 49. Arbiter

An arbiter participates in elections but does not store the data.

Conceptually:

```text
Primary
Secondary
Arbiter
```

It can help establish voting majority with fewer data-bearing nodes, but it should be used carefully and is not a substitute for a proper redundant data-bearing topology.

## 50. Replication lag

Suppose:

```text
Primary:
100000 operations

Secondary:
95000
```

Lag exists.

Causes:

```text
slow disk
network
heavy writes
secondary overload
resource contention
```

Replication lag matters because it can affect:

```text
read freshness
failover recovery point
secondary read behavior
```

## 51. Sharding

Replica sets solve:

> **availability**

Sharding solves:

> **horizontal scaling / distributing data and workload**

Conceptually:

```text
                MongoDB Cluster
                      |
        ┌─────────────┼─────────────┐
        ↓             ↓             ↓
     Shard 1       Shard 2       Shard 3
```

Each shard is typically itself a replica set in production.

## 52. Sharded cluster

Modern conceptual architecture:

```text
Application
     |
     v
mongos
     |
     ├──────────> Shard 1
     ├──────────> Shard 2
     └──────────> Shard 3

Config Servers
      |
 cluster metadata
```

#### `mongos`

Query router.

#### Config servers

Store cluster metadata/configuration.

#### Shards

Store the actual data.

## 53. Shard key

This is one of the most important scaling decisions.

Example:

```javascript
{
  customerId: "C123"
}
```

might be part of a shard key.

A good shard key should generally provide:

```text
high cardinality
reasonable distribution
good query targeting
appropriate write distribution
```

## 54. Bad shard key

Suppose:

```text
status = "ACTIVE"
```

Only two values:

```text
ACTIVE
INACTIVE
```

Low cardinality.

Data can become concentrated.

Bad distribution:

```text
Shard 1 -> 90%
Shard 2 -> 5%
Shard 3 -> 5%
```

That's a hotspot.

## 55. Monotonically increasing shard key

Suppose shard key is:

```text
timestamp
```

and every new write has a larger timestamp.

New writes may concentrate toward the newest chunk/shard region depending on the sharding strategy.

This can create a write hotspot.

A hashed strategy can distribute writes more evenly, but may reduce the ability to target range queries efficiently.

## 56. Sharding tradeoff

#### Range-based sharding

Good for:

```text
range queries
```

Potential problem:

```text
hotspots
```

#### Hashed sharding

Good for:

```text
uniform distribution
```

Potential problem:

```text
range queries become less naturally targeted
```

Again:

> There is no universally best shard key.

## 57. Targeted vs scatter-gather query

Suppose:

```text
Shard 1
Shard 2
Shard 3
```

Query includes shard key:

```javascript
{
  customerId: "C123"
}
```

Router can potentially target the relevant shard(s).

That's a:

> **targeted query**

Without shard-key information:

```javascript
{
  email: "alice@example.com"
}
```

MongoDB may need to query multiple shards.

That's:

> **scatter-gather**

Potentially much more expensive.

## 58. MongoDB indexes + sharding

Indexes are still extremely important in sharded MongoDB.

You need to consider:

```text
query shape
shard key
local indexes
sort
selectivity
distribution
```

A query can be:

```text
well-indexed but poorly targeted
```

or:

```text
well-targeted but poorly indexed
```

You need both dimensions.

## 59. Aggregation in sharded cluster

Aggregation may execute partly on shards and partly through the router/merging process.

A selective:

```text
$match
```

that can be pushed down is generally better than forcing huge amounts of data to move across the cluster.

So:

> Filter early and reduce data movement.

## 60. MongoDB transactions in sharded clusters

MongoDB supports distributed transactions across multiple documents and, where supported by the deployment/version, across shards.

But distributed transactions are more expensive than single-document atomic operations.

Design principle:

> **Prefer good data modeling first; use transactions when the business invariant actually requires them.**

## 61. MongoDB vs SQL

| MongoDB              | SQL                                |
| -------------------- | ---------------------------------- |
| Document             | Row                                |
| Collection           | Table                              |
| BSON                 | Relational columns/types           |
| Flexible schema      | Usually predefined schema          |
| Embed related data   | Normalize/relations                |
| `$lookup`            | JOIN                               |
| Aggregation pipeline | GROUP BY / window/query operations |
| Replica set          | Replication/HA                     |
| Sharding             | Horizontal partitioning            |
| `_id`                | Primary key                        |

Neither is universally better.

Choose based on:

```text
data model
relationships
query patterns
consistency requirements
scaling requirements
team expertise
```

## 62. MongoDB vs Redis

Very simplified:

#### MongoDB

```text
primary persistent document database
```

#### Redis

```text
in-memory data structure store
```

Redis is excellent for:

```text
cache
sessions
counters
queues
fast ephemeral state
```

MongoDB is more naturally used as:

```text
primary application database
```

They are often used **together**.

## 63. MongoDB performance checklist

When a query is slow:

```text
1. What is the query shape?
2. Does an appropriate index exist?
3. What does explain() say?
4. Is it doing COLLSCAN?
5. How many documents/keys are examined?
6. Is sorting happening in memory?
7. Is the working set in RAM?
8. Is the query hitting the right shard?
9. Is there a hot shard?
10. Is the database or application actually the bottleneck?
```

## 64. `explain()` mental model

Suppose:

```javascript
db.orders.find({
  customerId: "C1"
}).explain("executionStats")
```

You want to understand:

```text
IXSCAN?
COLLSCAN?
keys examined?
documents examined?
documents returned?
execution time?
```

Bad pattern:

```text
1,000,000 documents examined
10 returned
```

Potentially means the query is doing far more work than necessary.

## 65. N+1 problem

Application:

```text
get 100 users
```

Then:

```text
query orders for user 1
query orders for user 2
query orders for user 3
...
```

100+ queries.

This is the classic:

> **N+1 query problem**

Solutions may include:

```text
better data modeling
batch queries
aggregation
$lookup
denormalization
```

depending on the access pattern.

## 66. Pagination

Bad for huge collections:

```javascript
skip(1000000)
limit(20)
```

MongoDB may need to walk past many records.

Better:

```text
cursor/range pagination
```

For example:

```javascript
{
  createdAt: { $lt: lastSeenCreatedAt }
}
```

with a suitable index:

```javascript
{
  createdAt: -1,
  _id: -1
}
```

This scales much better for deep pagination.

## 67. Schema validation

Flexible schema doesn't mean uncontrolled data.

MongoDB can enforce validation rules.

For example:

```text
email → string
age → number
status → allowed values
```

Think:

```text
application flexibility
+
database guardrails
```

## 68. Denormalization

Instead of:

```text
orders
   |
   -> users
```

you might store:

```javascript
{
  orderId: 123,
  customerId: 1,
  customerName: "Alice"
}
```

Why?

Because order screens may always need:

```text
customerName
```

and avoiding a lookup can improve read performance.

Tradeoff:

> duplicated data requires update consistency.

## 69. Embedded vs normalized example

#### Embed

```javascript
{
  orderId: 100,
  customer: {
    id: 1,
    name: "Alice"
  }
}
```

Good if customer snapshot belongs with order.

#### Reference

```javascript
{
  orderId: 100,
  customerId: 1
}
```

Good if customer data changes independently and is shared widely.

## 70. Write-heavy workload

If writes are extremely heavy:

Watch:

```text
indexes
write concern
document size
shard distribution
hot keys
replication lag
disk throughput
```

Every additional index generally increases write maintenance.

So:

> **Don't create indexes for every field just because indexes make reads faster.**

## 71. Read-heavy workload

Focus on:

```text
indexes
working set
projection
read preference
replica set scaling
caching
query shape
```

Potential architecture:

```text
                Primary
               /       \
              /         \
        Secondary     Secondary
          reads         reads
```

But don't send consistency-sensitive reads to potentially stale secondaries.

## 72. Connection pooling

Applications should generally use connection pools rather than creating a new database connection for every request.

Conceptually:

```text
Application
    |
 Connection Pool
 ┌──┼──┬──┬──┐
 C1 C2 C3 C4 ...
    |
 MongoDB
```

Too few connections:

```text
requests wait
```

Too many:

```text
resource exhaustion
```

Tune based on workload.

## 73. MongoDB architecture cheat sheet

```text
                 Application
                      |
                      v
                 MongoDB Driver
                      |
               Connection Pool
                      |
          ┌───────────┴───────────┐
          ↓                       ↓
     Replica Set              Sharded Cluster
          |                       |
   ┌──────┼──────┐          ┌─────┴─────┐
   ↓      ↓      ↓          ↓           ↓
Primary Secondary Secondary Shards    Config
   |                       |
 writes                    data
```

## 74. The 20 MongoDB concepts you MUST know

|  ## | Concept                                |
| -: | -------------------------------------- |
|  1 | Document / BSON                        |
|  2 | Collection                             |
|  3 | `_id` / ObjectId                       |
|  4 | CRUD                                   |
|  5 | Query operators                        |
|  6 | Arrays / embedded documents            |
|  7 | Indexes                                |
|  8 | Compound indexes                       |
|  9 | ESR rule                               |
| 10 | `explain()`                            |
| 11 | Aggregation                            |
| 12 | `$lookup` / `$unwind`                  |
| 13 | Embedding vs referencing               |
| 14 | Single-document atomicity              |
| 15 | Transactions                           |
| 16 | Replica sets                           |
| 17 | Read/write concerns                    |
| 18 | Sharding / shard key                   |
| 19 | Pagination / performance               |
| 20 | Schema design based on access patterns |

## 75. Final MongoDB mental model

Remember this chain:

```text
                    MongoDB
                       |
             ┌─────────┴─────────┐
             ↓                   ↓
        Data Modeling         Scaling
             |                   |
       Document/BSON         Replica Set
             |                   |
       Embed/Reference       High Availability
             |
         Indexes
             |
        Fast Queries
             |
       Aggregation
             |
        Data Processing
             |
       Sharding
             |
     Horizontal Scaling
```

And the **one sentence** to remember:

> **MongoDB stores BSON documents in collections; you design those documents around access patterns, use indexes for efficient queries, replica sets for high availability, and sharding for horizontal scale.**

#### 🔥 Interview golden rules

```text
Embed when → data is read/updated together and bounded.

Reference when → data is shared, large, or independently managed.

Index when → a query is important and selective enough to justify it.

Don't index everything → indexes slow writes and consume resources.

Use transactions when → a business invariant spans multiple documents.

Use replica sets when → you need high availability.

Use sharding when → one replica set can no longer handle data/workload scale.

Choose shard key carefully → distribution + query targeting matter.

Use explain() → never guess why a query is slow.

Use cursor pagination → avoid huge skip() for deep pages.

Design MongoDB around queries → not around normalized relational tables.
```
