# Subqueries

## What is a Subquery?

A **subquery** (or nested query) is a query inside another query. The inner query runs first, and its result is used by the outer query.

```sql
SELECT * FROM products
WHERE price > (SELECT AVG(price) FROM products);
```

---

## Types of Subqueries

### 1. Scalar Subquery
Returns a **single value** (one row, one column).

```sql
-- Products above average price
SELECT name, price
FROM products
WHERE price > (SELECT AVG(price) FROM products);
```

```sql
-- Most expensive product's name
SELECT name
FROM products
WHERE price = (SELECT MAX(price) FROM products);
```

### 2. Column Subquery
Returns a **single column** (multiple rows).

```sql
-- Products in categories with 'Coffee' in name
SELECT name, price
FROM products
WHERE category_id IN (
    SELECT category_id 
    FROM categories 
    WHERE name LIKE '%Coffee%'
);
```

### 3. Table Subquery
Returns a **table** (multiple rows and columns).

```sql
-- Average order value by customer
SELECT c.name, order_totals.avg_total
FROM customers c
JOIN (
    SELECT customer_id, AVG(total) AS avg_total
    FROM orders
    GROUP BY customer_id
) AS order_totals ON c.customer_id = order_totals.customer_id;
```

---

## Subquery Locations

### In WHERE Clause
```sql
SELECT * FROM products
WHERE price > (SELECT AVG(price) FROM products);
```

### In SELECT Clause
```sql
SELECT 
    name,
    price,
    (SELECT AVG(price) FROM products) AS avg_price,
    price - (SELECT AVG(price) FROM products) AS diff_from_avg
FROM products;
```

### In FROM Clause (Derived Table)
```sql
SELECT dept_name, avg_salary
FROM (
    SELECT department_id, AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department_id
) AS dept_stats
JOIN departments d ON dept_stats.department_id = d.department_id;
```

### In HAVING Clause
```sql
SELECT category_id, AVG(price) AS avg_price
FROM products
GROUP BY category_id
HAVING AVG(price) > (SELECT AVG(price) FROM products);
```

---

## Subquery Operators

### IN / NOT IN
```sql
-- Customers who have placed orders
SELECT name FROM customers
WHERE customer_id IN (SELECT customer_id FROM orders);

-- Customers who have NOT placed orders
SELECT name FROM customers
WHERE customer_id NOT IN (SELECT customer_id FROM orders);
```

### EXISTS / NOT EXISTS
Tests whether subquery returns any rows (often faster than IN).

```sql
-- Customers who have placed orders
SELECT c.name 
FROM customers c
WHERE EXISTS (
    SELECT 1 FROM orders o 
    WHERE o.customer_id = c.customer_id
);

-- Customers with no orders
SELECT c.name 
FROM customers c
WHERE NOT EXISTS (
    SELECT 1 FROM orders o 
    WHERE o.customer_id = c.customer_id
);
```

### ANY / SOME
True if condition is true for ANY row.

```sql
-- Products cheaper than ANY product in 'Premium' category
SELECT name, price
FROM products
WHERE price < ANY (
    SELECT price FROM products 
    WHERE category = 'Premium'
);
```

### ALL
True if condition is true for ALL rows.

```sql
-- Products cheaper than ALL products in 'Premium' category
SELECT name, price
FROM products
WHERE price < ALL (
    SELECT price FROM products 
    WHERE category = 'Premium'
);
```

---

## Correlated Subqueries

A **correlated subquery** references the outer query. It runs once per outer row.

```sql
-- Products with price above their category's average
SELECT p1.name, p1.price, p1.category_id
FROM products p1
WHERE p1.price > (
    SELECT AVG(p2.price)
    FROM products p2
    WHERE p2.category_id = p1.category_id  -- References outer query!
);
```

```sql
-- Latest order for each customer
SELECT *
FROM orders o1
WHERE order_date = (
    SELECT MAX(order_date)
    FROM orders o2
    WHERE o2.customer_id = o1.customer_id
);
```

---

## Subquery vs JOIN

Often you can write the same logic either way:

**Subquery:**
```sql
SELECT name FROM customers
WHERE customer_id IN (
    SELECT customer_id FROM orders 
    WHERE total > 100
);
```

**JOIN:**
```sql
SELECT DISTINCT c.name 
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
WHERE o.total > 100;
```

### When to Use Each

| Use Subquery When | Use JOIN When |
|-------------------|---------------|
| Only need data from outer table | Need data from both tables |
| Simple existence check | Complex relationships |
| Aggregating for comparison | Better readability |

---

## Common Subquery Patterns

### Find Max/Min Row
```sql
-- Highest-paid employee
SELECT * FROM employees
WHERE salary = (SELECT MAX(salary) FROM employees);
```

### Top N Per Group
```sql
-- Top 3 products per category
SELECT * FROM products p1
WHERE (
    SELECT COUNT(*) FROM products p2
    WHERE p2.category_id = p1.category_id
      AND p2.price > p1.price
) < 3;
```

### Running Total (Pre-Window Functions)
```sql
SELECT 
    order_date,
    total,
    (SELECT SUM(total) FROM orders o2 
     WHERE o2.order_date <= o1.order_date) AS running_total
FROM orders o1;
```

---

## Summary

| Subquery Type | Returns | Use With |
|---------------|---------|----------|
| Scalar | Single value | =, >, <, etc. |
| Column | Single column | IN, ANY, ALL |
| Table | Multiple columns | FROM, JOIN |
| Correlated | Depends on outer | EXISTS, comparisons |

---

**Next:** [Aggregation Functions](aggregation.md)
