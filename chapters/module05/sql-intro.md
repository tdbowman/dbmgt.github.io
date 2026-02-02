# Introduction to SQL

## What is SQL?

**SQL (Structured Query Language)** is the standard language for communicating with relational databases. Pronounced "sequel" or "S-Q-L".

With SQL, you can:
- Retrieve data (queries)
- Insert, update, and delete data
- Create and modify database structures
- Control access and permissions

---

## A Brief History

| Year | Milestone |
|------|-----------|
| 1970 | Codd proposes relational model |
| 1974 | IBM develops SEQUEL |
| 1979 | Oracle releases first commercial SQL database |
| 1986 | SQL becomes ANSI standard |
| 1987 | SQL becomes ISO standard |
| Today | SQL powers most business applications |

---

## SQL Categories

SQL commands are grouped into categories:

### DDL (Data Definition Language)
**Defines database structure**
- `CREATE` - Create tables, databases, indexes
- `ALTER` - Modify existing structures
- `DROP` - Delete structures
- `TRUNCATE` - Remove all data from table

### DML (Data Manipulation Language)
**Works with data**
- `SELECT` - Retrieve data
- `INSERT` - Add new data
- `UPDATE` - Modify existing data
- `DELETE` - Remove data

### DCL (Data Control Language)
**Controls access**
- `GRANT` - Give permissions
- `REVOKE` - Remove permissions

### TCL (Transaction Control Language)
**Manages transactions**
- `BEGIN` - Start transaction
- `COMMIT` - Save changes
- `ROLLBACK` - Undo changes

---

## SQL is Declarative

SQL is **declarative**—you describe WHAT you want, not HOW to get it.

**Imperative (like Python):**
```python
results = []
for row in table:
    if row.price > 5:
        results.append(row)
```

**Declarative (SQL):**
```sql
SELECT * FROM products WHERE price > 5;
```

The database figures out HOW to get your data efficiently.

---

## SQL Syntax Basics

### Case Insensitivity
SQL keywords are case-insensitive:
```sql
SELECT * FROM products;
select * from products;
SeLeCt * FrOm PrOdUcTs;  -- All work!
```

**Convention:** UPPERCASE for keywords, lowercase for names

### Statements End with Semicolons
```sql
SELECT * FROM products;
SELECT * FROM customers;
```

### Comments
```sql
-- This is a single-line comment

/* This is a
   multi-line comment */

SELECT * FROM products;  -- Inline comment
```

### Whitespace Doesn't Matter
```sql
-- These are identical:
SELECT * FROM products WHERE price > 5;

SELECT *
FROM products
WHERE price > 5;

SELECT    *    FROM    products    WHERE    price > 5;
```

---

## PostgreSQL vs. Standard SQL

PostgreSQL follows SQL standards closely but has some extensions:

### PostgreSQL-Specific Features
- `SERIAL` data type for auto-increment
- `ILIKE` for case-insensitive matching
- Array data types
- JSON/JSONB support
- Rich string functions

### Example Differences

**Standard SQL:**
```sql
CREATE TABLE products (
    id INTEGER GENERATED ALWAYS AS IDENTITY PRIMARY KEY
);
```

**PostgreSQL shorthand:**
```sql
CREATE TABLE products (
    id SERIAL PRIMARY KEY
);
```

---

## Your First Queries

### Connect to PostgreSQL
```bash
psql -U postgres -d coffee_shop_db
```

### See All Tables
```sql
\dt
```

### Describe a Table
```sql
\d products
```

### Basic Query
```sql
SELECT * FROM products;
```

### Count Rows
```sql
SELECT COUNT(*) FROM products;
```

---

## SQL Best Practices

1. **Use meaningful names**
   - `customer_email` not `ce` or `x1`

2. **Be consistent with case**
   - Keywords: UPPERCASE
   - Identifiers: lowercase_with_underscores

3. **Format for readability**
   ```sql
   SELECT 
       first_name,
       last_name,
       email
   FROM customers
   WHERE status = 'active'
   ORDER BY last_name;
   ```

4. **Comment complex queries**
   ```sql
   -- Get top 10 customers by total purchases
   SELECT customer_id, SUM(amount) AS total
   FROM orders
   GROUP BY customer_id
   ORDER BY total DESC
   LIMIT 10;
   ```

5. **Use aliases for clarity**
   ```sql
   SELECT c.name, o.total
   FROM customers AS c
   JOIN orders AS o ON c.id = o.customer_id;
   ```

---

## Common Errors

| Error | Cause | Fix |
|-------|-------|-----|
| `relation does not exist` | Wrong table name | Check spelling, use `\dt` |
| `column does not exist` | Wrong column name | Use `\d tablename` |
| `syntax error` | Missing comma, quote | Check punctuation |
| `permission denied` | Insufficient access | Contact admin |

---

## Summary

- SQL is the standard language for relational databases
- DDL defines structure; DML manipulates data
- SQL is declarative—describe what, not how
- PostgreSQL extends standard SQL with useful features

---

**Next:** [SELECT Statements](select-statements.md)
