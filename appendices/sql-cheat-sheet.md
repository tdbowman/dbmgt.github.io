# SQL Cheat Sheet

A quick reference guide to common SQL commands in PostgreSQL.

## Data Definition Language (DDL)

### CREATE TABLE

```sql
CREATE TABLE table_name (
    column1 datatype constraints,
    column2 datatype constraints,
    ...
    PRIMARY KEY (column1),
    FOREIGN KEY (column2) REFERENCES other_table(column)
);
```

### Common Data Types

| Type | Description | Example |
|------|-------------|---------|
| `INTEGER` / `INT` | Whole numbers | `42` |
| `SERIAL` | Auto-incrementing integer | `1, 2, 3...` |
| `DECIMAL(p,s)` | Exact numeric | `19.99` |
| `VARCHAR(n)` | Variable-length string | `'Hello'` |
| `TEXT` | Unlimited string | Long text |
| `DATE` | Date only | `'2024-01-15'` |
| `TIME` | Time only | `'14:30:00'` |
| `TIMESTAMP` | Date and time | `'2024-01-15 14:30:00'` |
| `BOOLEAN` | True/False | `TRUE`, `FALSE` |

### Common Constraints

| Constraint | Description |
|------------|-------------|
| `PRIMARY KEY` | Unique identifier for the row |
| `FOREIGN KEY` | References another table |
| `NOT NULL` | Cannot be empty |
| `UNIQUE` | Must be unique across all rows |
| `CHECK` | Must satisfy a condition |
| `DEFAULT` | Default value if none provided |

### ALTER TABLE

```sql
-- Add column
ALTER TABLE table_name ADD column_name datatype;

-- Drop column
ALTER TABLE table_name DROP COLUMN column_name;

-- Rename column
ALTER TABLE table_name RENAME COLUMN old_name TO new_name;

-- Add constraint
ALTER TABLE table_name ADD CONSTRAINT constraint_name CHECK (condition);
```

### DROP TABLE

```sql
DROP TABLE table_name;
DROP TABLE IF EXISTS table_name;
```

---

## Data Manipulation Language (DML)

### SELECT

```sql
SELECT [DISTINCT] column1, column2, ...
FROM table_name
[WHERE condition]
[GROUP BY column]
[HAVING condition]
[ORDER BY column [ASC|DESC]]
[LIMIT n [OFFSET m]];
```

### INSERT

```sql
-- Single row
INSERT INTO table_name (column1, column2)
VALUES (value1, value2);

-- Multiple rows
INSERT INTO table_name (column1, column2)
VALUES 
    (value1a, value2a),
    (value1b, value2b);
```

### UPDATE

```sql
UPDATE table_name
SET column1 = value1, column2 = value2
WHERE condition;
```

### DELETE

```sql
DELETE FROM table_name
WHERE condition;
```

---

## WHERE Clause Operators

### Comparison Operators

| Operator | Meaning |
|----------|---------|
| `=` | Equal |
| `<>` or `!=` | Not equal |
| `<` | Less than |
| `>` | Greater than |
| `<=` | Less than or equal |
| `>=` | Greater than or equal |

### Logical Operators

```sql
WHERE condition1 AND condition2
WHERE condition1 OR condition2
WHERE NOT condition
```

### Special Operators

```sql
-- Range (inclusive)
WHERE column BETWEEN value1 AND value2

-- List membership
WHERE column IN (value1, value2, value3)

-- Pattern matching (% = any chars, _ = single char)
WHERE column LIKE 'pattern%'
WHERE column ILIKE 'pattern%'  -- case-insensitive

-- Null check
WHERE column IS NULL
WHERE column IS NOT NULL
```

---

## JOIN Operations

```sql
-- Inner Join (matching rows only)
SELECT * FROM table1
INNER JOIN table2 ON table1.col = table2.col;

-- Left Join (all from left, matching from right)
SELECT * FROM table1
LEFT JOIN table2 ON table1.col = table2.col;

-- Right Join (all from right, matching from left)
SELECT * FROM table1
RIGHT JOIN table2 ON table1.col = table2.col;

-- Full Outer Join (all from both)
SELECT * FROM table1
FULL OUTER JOIN table2 ON table1.col = table2.col;
```

---

## Aggregate Functions

| Function | Description |
|----------|-------------|
| `COUNT(*)` | Number of rows |
| `COUNT(column)` | Number of non-null values |
| `SUM(column)` | Sum of values |
| `AVG(column)` | Average of values |
| `MIN(column)` | Minimum value |
| `MAX(column)` | Maximum value |

```sql
SELECT 
    category,
    COUNT(*) as num_products,
    AVG(price) as avg_price
FROM products
GROUP BY category
HAVING COUNT(*) > 5;
```

---

## Subqueries

```sql
-- In WHERE clause
SELECT * FROM products
WHERE price > (SELECT AVG(price) FROM products);

-- In FROM clause
SELECT * FROM (SELECT * FROM products WHERE active = true) AS active_products;

-- In SELECT clause
SELECT 
    product_name,
    (SELECT AVG(price) FROM products) as avg_price
FROM products;
```

---

## Common Table Expressions (CTEs)

```sql
WITH cte_name AS (
    SELECT column1, column2
    FROM table_name
    WHERE condition
)
SELECT * FROM cte_name;
```

---

## Transaction Control

```sql
BEGIN;  -- Start transaction
-- SQL statements here
COMMIT;  -- Save changes

-- Or
ROLLBACK;  -- Undo changes
```

---

## Useful PostgreSQL Commands

```sql
-- List all tables
\dt

-- Describe table structure
\d table_name

-- List databases
\l

-- Connect to database
\c database_name

-- Show current user
SELECT current_user;

-- Show current database
SELECT current_database();
```
