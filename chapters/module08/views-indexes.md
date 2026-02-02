# Views and Indexes

## Views

A **view** is a saved query that acts like a virtual table.

### Why Use Views?

- **Simplify complex queries** - Write once, use many times
- **Security** - Hide sensitive columns
- **Abstraction** - Present data differently than stored
- **Consistency** - Ensure everyone uses the same logic

### Creating Views

```sql
CREATE VIEW active_customers AS
SELECT customer_id, name, email
FROM customers
WHERE status = 'active';
```

### Using Views

Views work like tables:
```sql
SELECT * FROM active_customers;

SELECT name FROM active_customers WHERE email LIKE '%@gmail.com';
```

### View with JOIN

```sql
CREATE VIEW order_summary AS
SELECT 
    o.order_id,
    c.name AS customer,
    o.order_date,
    o.total,
    COUNT(oi.product_id) AS items
FROM orders o
JOIN customers c ON o.customer_id = c.customer_id
JOIN order_items oi ON o.order_id = oi.order_id
GROUP BY o.order_id, c.name, o.order_date, o.total;
```

### Replacing Views

```sql
CREATE OR REPLACE VIEW active_customers AS
SELECT customer_id, name, email, phone
FROM customers
WHERE status = 'active';
```

### Dropping Views

```sql
DROP VIEW active_customers;
DROP VIEW IF EXISTS active_customers;
```

### Updatable Views

Simple views can sometimes be updated:
```sql
-- This may work for simple views
UPDATE active_customers SET email = 'new@email.com' WHERE customer_id = 1;
```

Rules for updatable views:
- Based on single table
- No aggregates, DISTINCT, GROUP BY
- No subqueries in SELECT

---

## Indexes

An **index** is a data structure that speeds up queries (like a book's index).

### How Indexes Work

Without index: Database scans ALL rows (slow)
With index: Database jumps directly to matching rows (fast)

### Creating Indexes

```sql
-- Single column
CREATE INDEX idx_customer_email ON customers(email);

-- Multiple columns
CREATE INDEX idx_order_date_customer ON orders(order_date, customer_id);

-- Unique index
CREATE UNIQUE INDEX idx_product_sku ON products(sku);
```

### When to Create Indexes

**Good candidates:**
- Primary keys (automatic)
- Foreign keys
- Columns in WHERE clauses
- Columns in JOIN conditions
- Columns in ORDER BY

**Bad candidates:**
- Columns rarely queried
- Columns with few unique values
- Small tables
- Frequently updated columns

### Index Types in PostgreSQL

| Type | Use Case |
|------|----------|
| B-tree | Default, most queries |
| Hash | Exact equality only |
| GIN | Arrays, full-text search |
| GiST | Geometric, spatial data |

```sql
-- Specify index type
CREATE INDEX idx_name ON table USING HASH (column);
```

### Viewing Indexes

```sql
-- List indexes on a table
\di products

-- Using system catalog
SELECT indexname, indexdef 
FROM pg_indexes 
WHERE tablename = 'products';
```

### Dropping Indexes

```sql
DROP INDEX idx_customer_email;
DROP INDEX IF EXISTS idx_customer_email;
```

### Index Trade-offs

| Benefit | Cost |
|---------|------|
| Faster SELECT | Slower INSERT/UPDATE/DELETE |
| Faster WHERE | More disk space |
| Faster JOIN | Maintenance overhead |

### EXPLAIN for Query Analysis

See if your index is being used:

```sql
EXPLAIN SELECT * FROM customers WHERE email = 'test@email.com';

EXPLAIN ANALYZE SELECT * FROM customers WHERE email = 'test@email.com';
```

Look for:
- "Index Scan" - Using index ✓
- "Seq Scan" - Full table scan ✗

---

## Summary

**Views:**
- Virtual tables from saved queries
- Simplify complex logic
- Can provide security layer
- `CREATE VIEW`, `DROP VIEW`

**Indexes:**
- Speed up data retrieval
- Trade-off with write performance
- Create on frequently queried columns
- Use `EXPLAIN` to verify usage

---

**Next:** [Transactions](transactions.md)
