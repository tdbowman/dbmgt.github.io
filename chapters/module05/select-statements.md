# SELECT Statements

## The SELECT Statement

`SELECT` retrieves data from one or more tables. It's the most used SQL command.

### Basic Syntax

```sql
SELECT columns
FROM table
WHERE conditions
ORDER BY columns
LIMIT number;
```

---

## Selecting Columns

### All Columns
```sql
SELECT * FROM products;
```

### Specific Columns
```sql
SELECT product_name, price FROM products;
```

### Expressions and Calculations
```sql
SELECT 
    product_name,
    price,
    price * 1.10 AS price_with_tax
FROM products;
```

### Aliasing Columns
```sql
SELECT 
    product_name AS name,
    unit_price AS price
FROM products;
```

---

## Filtering with WHERE

### Comparison Operators

| Operator | Meaning |
|----------|---------|
| `=` | Equal to |
| `<>` or `!=` | Not equal to |
| `<` | Less than |
| `>` | Greater than |
| `<=` | Less than or equal |
| `>=` | Greater than or equal |

```sql
SELECT * FROM products WHERE price > 5.00;
SELECT * FROM products WHERE category = 'Beverages';
SELECT * FROM products WHERE quantity <> 0;
```

### Logical Operators

**AND** - Both conditions must be true
```sql
SELECT * FROM products 
WHERE category = 'Beverages' AND price < 5.00;
```

**OR** - Either condition can be true
```sql
SELECT * FROM products 
WHERE category = 'Coffee' OR category = 'Tea';
```

**NOT** - Negates a condition
```sql
SELECT * FROM products 
WHERE NOT category = 'Beverages';
```

### Combining Operators
```sql
SELECT * FROM products 
WHERE (category = 'Coffee' OR category = 'Tea') 
  AND price < 5.00;
```

---

## Special Operators

### BETWEEN
```sql
-- Price between 2 and 5 (inclusive)
SELECT * FROM products 
WHERE price BETWEEN 2.00 AND 5.00;

-- Same as:
SELECT * FROM products 
WHERE price >= 2.00 AND price <= 5.00;
```

### IN
```sql
-- Category is one of these values
SELECT * FROM products 
WHERE category IN ('Coffee', 'Tea', 'Pastries');

-- Same as:
SELECT * FROM products 
WHERE category = 'Coffee' 
   OR category = 'Tea' 
   OR category = 'Pastries';
```

### LIKE (Pattern Matching)

| Pattern | Matches |
|---------|---------|
| `%` | Any sequence of characters |
| `_` | Any single character |

```sql
-- Names starting with 'Cof'
SELECT * FROM products WHERE product_name LIKE 'Cof%';

-- Names ending with 'fee'
SELECT * FROM products WHERE product_name LIKE '%fee';

-- Names containing 'Latte'
SELECT * FROM products WHERE product_name LIKE '%Latte%';

-- Names with exactly 6 characters
SELECT * FROM products WHERE product_name LIKE '______';
```

**PostgreSQL bonus: ILIKE** (case-insensitive)
```sql
SELECT * FROM products WHERE product_name ILIKE '%coffee%';
```

### IS NULL / IS NOT NULL
```sql
-- Products without descriptions
SELECT * FROM products WHERE description IS NULL;

-- Products with descriptions
SELECT * FROM products WHERE description IS NOT NULL;
```

---

## Sorting with ORDER BY

### Single Column
```sql
SELECT * FROM products ORDER BY price;        -- Ascending (default)
SELECT * FROM products ORDER BY price ASC;    -- Ascending (explicit)
SELECT * FROM products ORDER BY price DESC;   -- Descending
```

### Multiple Columns
```sql
-- Sort by category, then by price within each category
SELECT * FROM products 
ORDER BY category ASC, price DESC;
```

### By Column Position
```sql
-- Sort by the 3rd column
SELECT product_name, category, price 
FROM products 
ORDER BY 3 DESC;
```

### By Alias
```sql
SELECT product_name, price * quantity AS total
FROM products
ORDER BY total DESC;
```

---

## Limiting Results

### LIMIT
```sql
-- First 10 rows
SELECT * FROM products LIMIT 10;
```

### OFFSET
```sql
-- Skip first 10, get next 10 (pagination)
SELECT * FROM products LIMIT 10 OFFSET 10;
```

### Combined Example
```sql
-- Top 5 most expensive products
SELECT product_name, price
FROM products
ORDER BY price DESC
LIMIT 5;
```

---

## Removing Duplicates with DISTINCT

### Single Column
```sql
-- Unique categories
SELECT DISTINCT category FROM products;
```

### Multiple Columns
```sql
-- Unique category + supplier combinations
SELECT DISTINCT category, supplier FROM products;
```

### With ORDER BY
```sql
SELECT DISTINCT category 
FROM products 
ORDER BY category;
```

---

## Putting It All Together

```sql
-- Find the 5 most expensive beverages under $10
-- that contain "Latte" in the name
SELECT 
    product_name,
    category,
    price
FROM products
WHERE category = 'Beverages'
  AND price < 10.00
  AND product_name LIKE '%Latte%'
ORDER BY price DESC
LIMIT 5;
```

---

## Query Execution Order

SQL processes clauses in this order (not the order written):

1. **FROM** - Which table(s)?
2. **WHERE** - Filter rows
3. **SELECT** - Which columns?
4. **DISTINCT** - Remove duplicates
5. **ORDER BY** - Sort results
6. **LIMIT/OFFSET** - Limit output

This is why you can't use a column alias in WHERE:
```sql
-- ERROR: "total" doesn't exist yet during WHERE
SELECT price * quantity AS total
FROM products
WHERE total > 100;

-- CORRECT: Use the expression
SELECT price * quantity AS total
FROM products
WHERE price * quantity > 100;
```

---

## Summary

| Clause | Purpose |
|--------|---------|
| `SELECT` | Choose columns |
| `FROM` | Choose table |
| `WHERE` | Filter rows |
| `ORDER BY` | Sort results |
| `LIMIT` | Restrict output |
| `DISTINCT` | Remove duplicates |

---

**Next:** [Hands-On SQL Practice](sql-practice.ipynb)
