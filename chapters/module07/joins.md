# SQL JOINs

## Why JOINs?

Real databases have multiple related tables. JOINs combine data from these tables based on related columns.

**Example Tables:**

**customers:**
| customer_id | name |
|-------------|------|
| 1 | Alice |
| 2 | Bob |
| 3 | Carol |

**orders:**
| order_id | customer_id | total |
|----------|-------------|-------|
| 101 | 1 | 25.00 |
| 102 | 1 | 35.00 |
| 103 | 2 | 15.00 |

To see "Customer name with their order totals", we need a JOIN.

---

## JOIN Types Overview

```
         INNER JOIN
    ┌───────┬───────┐
    │       │       │
    │   A   │   B   │
    │       │       │
    └───────┴───────┘
    Only matching rows

         LEFT JOIN
    ┌───────────────┐
    │       ┌───────┤
    │   A   │   B   │
    │       └───────┤
    └───────────────┘
    All A + matching B

        RIGHT JOIN
    ┌───────────────┐
    ├───────┐       │
    │   A   │   B   │
    ├───────┘       │
    └───────────────┘
    Matching A + all B

        FULL JOIN
    ┌───────────────┐
    │       │       │
    │   A   │   B   │
    │       │       │
    └───────────────┘
    All A + All B
```

---

## INNER JOIN

Returns only rows with matches in BOTH tables.

```sql
SELECT c.name, o.order_id, o.total
FROM customers c
INNER JOIN orders o ON c.customer_id = o.customer_id;
```

**Result:**
| name | order_id | total |
|------|----------|-------|
| Alice | 101 | 25.00 |
| Alice | 102 | 35.00 |
| Bob | 103 | 15.00 |

Carol has no orders, so she's excluded.

### Multiple Conditions
```sql
SELECT *
FROM orders o
INNER JOIN customers c 
    ON o.customer_id = c.customer_id
    AND c.status = 'active';
```

---

## LEFT JOIN (LEFT OUTER JOIN)

Returns ALL rows from the left table, plus matching rows from right.

```sql
SELECT c.name, o.order_id, o.total
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id;
```

**Result:**
| name | order_id | total |
|------|----------|-------|
| Alice | 101 | 25.00 |
| Alice | 102 | 35.00 |
| Bob | 103 | 15.00 |
| Carol | NULL | NULL |

Carol is included with NULLs.

### Find Non-Matching Rows
```sql
-- Customers with NO orders
SELECT c.name
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
WHERE o.order_id IS NULL;
```

---

## RIGHT JOIN (RIGHT OUTER JOIN)

Returns ALL rows from the right table, plus matching rows from left.

```sql
SELECT c.name, o.order_id, o.total
FROM customers c
RIGHT JOIN orders o ON c.customer_id = o.customer_id;
```

Less common—usually rewrite as LEFT JOIN by swapping table order.

---

## FULL JOIN (FULL OUTER JOIN)

Returns ALL rows from BOTH tables, with NULLs where no match.

```sql
SELECT c.name, o.order_id
FROM customers c
FULL JOIN orders o ON c.customer_id = o.customer_id;
```

**Use case:** Finding unmatched records from either side.

---

## CROSS JOIN

Returns the Cartesian product—every combination.

```sql
SELECT c.name, p.product_name
FROM customers c
CROSS JOIN products p;
```

With 3 customers and 5 products = 15 rows.

**Use case:** Generating combinations (sizes × colors).

---

## Self JOIN

Join a table to itself.

```sql
-- Employees and their managers
SELECT 
    e.name AS employee,
    m.name AS manager
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.employee_id;
```

---

## Joining Multiple Tables

```sql
SELECT 
    c.name AS customer,
    p.name AS product,
    oi.quantity,
    o.order_date
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
JOIN order_items oi ON o.order_id = oi.order_id
JOIN products p ON oi.product_id = p.product_id;
```

---

## JOIN Syntax Variations

### Explicit JOIN (Preferred)
```sql
SELECT * 
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id;
```

### Implicit JOIN (Old Style)
```sql
SELECT * 
FROM customers c, orders o
WHERE c.customer_id = o.customer_id;
```

### USING Clause (When Column Names Match)
```sql
SELECT * 
FROM customers
JOIN orders USING (customer_id);
```

### NATURAL JOIN (Matches All Common Columns)
```sql
-- Danger: unpredictable if columns change
SELECT * 
FROM customers
NATURAL JOIN orders;
```

---

## JOIN Best Practices

1. **Always use table aliases** for clarity
2. **Use explicit JOIN syntax** (not WHERE-based)
3. **Qualify all column names** (table.column)
4. **Index foreign key columns** for performance
5. **Use INNER JOIN** unless you specifically need outers

---

## Summary

| JOIN Type | Returns |
|-----------|---------|
| INNER | Only matching rows |
| LEFT | All left + matching right |
| RIGHT | Matching left + all right |
| FULL | All from both tables |
| CROSS | All combinations |

---

**Next:** [Subqueries](subqueries.md)
