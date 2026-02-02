# Aggregation Functions

## What is Aggregation?

**Aggregation** summarizes multiple rows into single values: totals, averages, counts, etc.

---

## Common Aggregate Functions

| Function | Purpose |
|----------|---------|
| `COUNT()` | Number of rows |
| `SUM()` | Total of values |
| `AVG()` | Average of values |
| `MIN()` | Smallest value |
| `MAX()` | Largest value |

### Basic Examples

```sql
-- Count all products
SELECT COUNT(*) FROM products;

-- Count products with prices
SELECT COUNT(price) FROM products;

-- Total sales
SELECT SUM(total) FROM orders;

-- Average price
SELECT AVG(price) FROM products;

-- Price range
SELECT MIN(price), MAX(price) FROM products;
```

---

## COUNT Variations

```sql
-- Count all rows
SELECT COUNT(*) FROM orders;

-- Count non-NULL values
SELECT COUNT(discount) FROM orders;

-- Count distinct values
SELECT COUNT(DISTINCT customer_id) FROM orders;
```

---

## GROUP BY

Groups rows with the same values, then aggregates each group.

```sql
-- Total sales by customer
SELECT customer_id, SUM(total) AS total_spent
FROM orders
GROUP BY customer_id;
```

**Result:**
| customer_id | total_spent |
|-------------|-------------|
| 1 | 150.00 |
| 2 | 275.00 |
| 3 | 89.50 |

### Multiple Grouping Columns

```sql
-- Sales by customer and month
SELECT 
    customer_id,
    DATE_TRUNC('month', order_date) AS month,
    SUM(total) AS monthly_total
FROM orders
GROUP BY customer_id, DATE_TRUNC('month', order_date);
```

### GROUP BY with JOIN

```sql
-- Total sales by category name
SELECT c.name AS category, SUM(oi.quantity * oi.price) AS total
FROM categories c
JOIN products p ON c.category_id = p.category_id
JOIN order_items oi ON p.product_id = oi.product_id
GROUP BY c.name
ORDER BY total DESC;
```

---

## HAVING

Filters **groups** (after aggregation), unlike WHERE which filters rows (before aggregation).

```sql
-- Categories with more than 10 products
SELECT category_id, COUNT(*) AS product_count
FROM products
GROUP BY category_id
HAVING COUNT(*) > 10;
```

### WHERE vs HAVING

| Clause | Filters | When |
|--------|---------|------|
| WHERE | Individual rows | Before grouping |
| HAVING | Groups | After grouping |

```sql
-- Expensive products, grouped by category, only categories with 5+ items
SELECT category_id, COUNT(*), AVG(price)
FROM products
WHERE price > 10           -- Filter rows first
GROUP BY category_id
HAVING COUNT(*) >= 5;      -- Filter groups after
```

---

## Query Execution Order

1. **FROM** - Get tables
2. **WHERE** - Filter rows
3. **GROUP BY** - Create groups
4. **HAVING** - Filter groups
5. **SELECT** - Calculate columns
6. **ORDER BY** - Sort results
7. **LIMIT** - Restrict output

---

## Aggregate with All Rows

Without GROUP BY, aggregates operate on ALL rows as one group:

```sql
-- Overall statistics
SELECT 
    COUNT(*) AS total_orders,
    SUM(total) AS total_revenue,
    AVG(total) AS avg_order_value,
    MAX(total) AS largest_order
FROM orders;
```

---

## Practical Examples

### Sales Report by Month
```sql
SELECT 
    DATE_TRUNC('month', order_date) AS month,
    COUNT(*) AS num_orders,
    SUM(total) AS revenue,
    AVG(total) AS avg_order
FROM orders
WHERE order_date >= '2024-01-01'
GROUP BY DATE_TRUNC('month', order_date)
ORDER BY month;
```

### Top 5 Customers
```sql
SELECT 
    c.name,
    COUNT(o.order_id) AS order_count,
    SUM(o.total) AS total_spent
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.name
ORDER BY total_spent DESC
LIMIT 5;
```

### Products Never Ordered
```sql
SELECT p.name
FROM products p
LEFT JOIN order_items oi ON p.product_id = oi.product_id
GROUP BY p.product_id, p.name
HAVING COUNT(oi.order_id) = 0;
```

### Category Performance
```sql
SELECT 
    c.name AS category,
    COUNT(DISTINCT p.product_id) AS products,
    COALESCE(SUM(oi.quantity), 0) AS units_sold,
    COALESCE(SUM(oi.quantity * oi.price), 0) AS revenue
FROM categories c
LEFT JOIN products p ON c.category_id = p.category_id
LEFT JOIN order_items oi ON p.product_id = oi.product_id
GROUP BY c.category_id, c.name
ORDER BY revenue DESC;
```

---

## NULL Handling

Aggregates ignore NULL values (except COUNT(*)):

```sql
-- If prices are: 10, 20, NULL, 30
SELECT 
    COUNT(*) AS count_all,     -- 4
    COUNT(price) AS count_price, -- 3 (excludes NULL)
    AVG(price) AS avg_price     -- 20 (ignores NULL)
FROM products;
```

Use COALESCE to replace NULLs:
```sql
SELECT AVG(COALESCE(price, 0)) FROM products;
```

---

## Summary

| Function | Returns |
|----------|---------|
| COUNT(*) | Number of rows |
| COUNT(col) | Non-NULL values |
| SUM(col) | Total |
| AVG(col) | Average |
| MIN(col) | Minimum |
| MAX(col) | Maximum |

| Clause | Purpose |
|--------|---------|
| GROUP BY | Create groups |
| HAVING | Filter groups |

---

**Next Module:** [Advanced SQL II](../module08/overview.md)
