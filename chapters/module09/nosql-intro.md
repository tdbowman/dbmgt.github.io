# Introduction to NoSQL

## What is NoSQL?

**NoSQL** stands for "Not Only SQL"—databases that don't use the traditional relational model.

### Why NoSQL?

Relational databases struggle with:
- Massive scale (billions of records)
- Unstructured/semi-structured data
- Rapid schema changes
- High-velocity data (IoT, real-time)

---

## Types of NoSQL Databases

### 1. Document Databases

Store data as **documents** (usually JSON).

**Example:** MongoDB

```json
{
  "_id": "12345",
  "name": "Alice Smith",
  "email": "alice@email.com",
  "orders": [
    {"product": "Coffee", "quantity": 2},
    {"product": "Muffin", "quantity": 1}
  ]
}
```

**Use Cases:**
- Content management
- User profiles
- Product catalogs

**Popular Options:** MongoDB, CouchDB, Amazon DocumentDB

### 2. Key-Value Stores

Simplest model—just keys and values.

**Example:** Redis

```
user:1001 → {"name": "Alice", "email": "alice@email.com"}
session:abc123 → {"user_id": 1001, "expires": "2024-01-15"}
```

**Use Cases:**
- Caching
- Session storage
- Shopping carts

**Popular Options:** Redis, Amazon DynamoDB, Memcached

### 3. Column-Family Stores

Data organized by columns, not rows.

**Example:** Cassandra

```
Row Key: user_1001
  personal:name = "Alice"
  personal:email = "alice@email.com"
  metrics:logins = 150
  metrics:last_active = "2024-01-15"
```

**Use Cases:**
- Time-series data
- Analytics
- IoT sensor data

**Popular Options:** Apache Cassandra, HBase, ScyllaDB

### 4. Graph Databases

Store entities and their **relationships**.

**Example:** Neo4j

```
(Alice)-[:FRIENDS_WITH]->(Bob)
(Alice)-[:PURCHASED]->(Product1)
(Bob)-[:REVIEWED]->(Product1)
```

**Use Cases:**
- Social networks
- Recommendation engines
- Fraud detection

**Popular Options:** Neo4j, Amazon Neptune, ArangoDB

---

## SQL vs NoSQL Comparison

| Aspect | SQL (Relational) | NoSQL |
|--------|------------------|-------|
| Schema | Fixed, predefined | Flexible, dynamic |
| Scaling | Vertical (bigger server) | Horizontal (more servers) |
| Relationships | Excellent (JOINs) | Limited or different |
| ACID | Full support | Varies (often eventual consistency) |
| Query Language | SQL | Varies by database |
| Best For | Complex queries, transactions | Scale, flexibility |

---

## CAP Theorem

Distributed databases can only guarantee 2 of 3:

- **Consistency** - All nodes see the same data
- **Availability** - System always responds
- **Partition Tolerance** - Works despite network issues

| Database Type | Typically Prioritizes |
|---------------|----------------------|
| Relational | Consistency + Availability |
| Many NoSQL | Availability + Partition Tolerance |

---

## When to Use NoSQL

**Good fit:**
- Rapidly changing requirements
- Massive scale needed
- Unstructured or varied data
- High write throughput
- Geographic distribution

**Not ideal:**
- Complex transactions
- Complex reporting/analytics
- Strong consistency required
- Small to medium data volume

---

## MongoDB Example

**Connecting:**
```python
from pymongo import MongoClient

client = MongoClient("mongodb://localhost:27017")
db = client["coffee_shop"]
customers = db["customers"]
```

**Insert:**
```python
customer = {
    "name": "Alice Smith",
    "email": "alice@email.com",
    "loyalty_points": 150
}
customers.insert_one(customer)
```

**Query:**
```python
# Find one
alice = customers.find_one({"email": "alice@email.com"})

# Find many
loyal_customers = customers.find({"loyalty_points": {"$gte": 100}})
```

---

## Summary

| Type | Model | Example | Use Case |
|------|-------|---------|----------|
| Document | JSON documents | MongoDB | Flexible content |
| Key-Value | Key → Value | Redis | Caching |
| Column-Family | Column groups | Cassandra | Time-series |
| Graph | Nodes + Edges | Neo4j | Relationships |

**Choose based on:**
- Data structure
- Query patterns
- Scale requirements
- Consistency needs

---

**Next Module:** [Python & PostgreSQL Integration](../module10/overview.md)
