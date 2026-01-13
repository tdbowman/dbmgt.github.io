# Introduction to SQL

```{epigraph}
SQL is everywhere. It's been around for over 50 years, and it's not going anywhere.

-- Industry Wisdom
```

**Structured Query Language (SQL)** is the standard language for communicating with relational databases. Regardless of which DBMS you use—PostgreSQL, MySQL, Oracle, or SQL Server—you'll use SQL to create, query, and manipulate data.

## Learning Objectives

By the end of this chapter, you will be able to:

- Explain what SQL is and why it's important
- Identify the different categories of SQL commands
- Write basic SELECT statements to retrieve data
- Filter data using WHERE clauses
- Sort results using ORDER BY
- Limit output using LIMIT

## What Is SQL?

:::{glossary}
SQL (Structured Query Language)
  A standardized programming language used to manage and manipulate relational databases. Pronounced "S-Q-L" or "sequel."
:::

SQL was developed at IBM in the 1970s and has since become the universal language for relational databases. While each DBMS has its own extensions, the core SQL syntax is standardized by ANSI/ISO.

### SQL Categories

SQL commands are organized into several categories:

```{mermaid}
flowchart LR
    SQL[SQL] --> DDL[DDL<br/>Data Definition]
    SQL --> DML[DML<br/>Data Manipulation]
    SQL --> DCL[DCL<br/>Data Control]
    SQL --> TCL[TCL<br/>Transaction Control]
    
    DDL --> |CREATE, ALTER, DROP| Tables
    DML --> |SELECT, INSERT, UPDATE, DELETE| Data
    DCL --> |GRANT, REVOKE| Permissions
    TCL --> |COMMIT, ROLLBACK| Transactions
```

| Category | Purpose | Key Commands |
|----------|---------|--------------|
| **DDL** (Data Definition Language) | Define database structure | CREATE, ALTER, DROP, TRUNCATE |
| **DML** (Data Manipulation Language) | Manipulate data | SELECT, INSERT, UPDATE, DELETE |
| **DCL** (Data Control Language) | Control access | GRANT, REVOKE |
| **TCL** (Transaction Control Language) | Manage transactions | COMMIT, ROLLBACK, SAVEPOINT |

```{note}
This chapter focuses on **DML**, specifically the SELECT statement. We'll cover DDL in Chapter 6 and TCL in Chapter 8.
```

## The SELECT Statement

The SELECT statement retrieves data from one or more tables. It's the most commonly used SQL command—you'll write thousands of SELECTs in your career!

### Basic Syntax

```sql
SELECT column1, column2, ...
FROM table_name;
```

### Selecting All Columns

Use `*` (asterisk) to select all columns:

```sql
SELECT *
FROM products;
```

```{warning}
While `SELECT *` is convenient for exploration, avoid it in production code. Explicitly naming columns is faster and makes your code more maintainable.
```

### Selecting Specific Columns

```sql
SELECT product_name, unit_price
FROM products;
```

### Column Aliases

Rename columns in your output using `AS`:

```sql
SELECT 
    product_name AS "Product",
    unit_price AS "Price ($)"
FROM products;
```

## Filtering Data with WHERE

The WHERE clause filters rows based on conditions:

```sql
SELECT product_name, unit_price
FROM products
WHERE unit_price > 3.00;
```

### Comparison Operators

| Operator | Meaning | Example |
|----------|---------|---------|
| `=` | Equal to | `WHERE position = 'Manager'` |
| `<>` or `!=` | Not equal to | `WHERE status <> 'Inactive'` |
| `>` | Greater than | `WHERE price > 5.00` |
| `<` | Less than | `WHERE quantity < 10` |
| `>=` | Greater than or equal | `WHERE salary >= 50000` |
| `<=` | Less than or equal | `WHERE age <= 30` |

### Logical Operators

Combine conditions using AND, OR, and NOT:

```sql
-- Both conditions must be true
SELECT *
FROM sales
WHERE product_category = 'Coffee' 
  AND unit_price > 3.00;

-- Either condition can be true
SELECT *
FROM staff
WHERE position = 'Manager' 
  OR position = 'Assistant Manager';

-- Negates a condition
SELECT *
FROM products
WHERE NOT product_category = 'Food';
```

```{admonition} Operator Precedence
:class: tip

SQL evaluates NOT first, then AND, then OR. Use parentheses to make your intent clear:

```sql
-- Without parentheses (may not do what you expect!)
SELECT * FROM products
WHERE category = 'Beverages' OR category = 'Food' AND price > 5;

-- With parentheses (clear intent)
SELECT * FROM products
WHERE (category = 'Beverages' OR category = 'Food') AND price > 5;
```
```

### Special Operators

**BETWEEN** - Range of values (inclusive):
```sql
SELECT *
FROM transactions
WHERE unit_price BETWEEN 2.50 AND 4.00;
```

**IN** - Match any value in a list:
```sql
SELECT *
FROM stores
WHERE store_city IN ('New York', 'Long Island City', 'Brooklyn');
```

**LIKE** - Pattern matching:
```sql
-- % matches any sequence of characters
SELECT *
FROM products
WHERE product_name LIKE '%Coffee%';

-- _ matches a single character
SELECT *
FROM products
WHERE product_name LIKE 'Latte _g';  -- Matches 'Latte Lg', 'Latte Rg'
```

**IS NULL** - Check for null values:
```sql
SELECT *
FROM customers
WHERE loyalty_card_number IS NULL;
```

## Sorting Results with ORDER BY

The ORDER BY clause sorts your results:

```sql
SELECT product_name, unit_price
FROM products
ORDER BY unit_price;  -- Ascending by default
```

### Ascending vs. Descending

```sql
-- Ascending (A to Z, 1 to 100)
ORDER BY product_name ASC;

-- Descending (Z to A, 100 to 1)
ORDER BY unit_price DESC;
```

### Multiple Sort Columns

```sql
SELECT product_category, product_name, unit_price
FROM products
ORDER BY product_category ASC, unit_price DESC;
```

This sorts first by category (A to Z), then within each category by price (highest to lowest).

## Limiting Results with LIMIT

LIMIT restricts the number of rows returned:

```sql
-- Get the 10 most expensive products
SELECT product_name, unit_price
FROM products
ORDER BY unit_price DESC
LIMIT 10;
```

### OFFSET for Pagination

Skip rows before returning results:

```sql
-- Get products 11-20 (for pagination)
SELECT product_name, unit_price
FROM products
ORDER BY unit_price DESC
LIMIT 10 OFFSET 10;
```

## Removing Duplicates with DISTINCT

DISTINCT removes duplicate rows:

```sql
-- List all unique product categories
SELECT DISTINCT product_category
FROM products;

-- Unique combinations
SELECT DISTINCT store_city, store_state_province
FROM stores;
```

## Putting It All Together

A complete SELECT statement can use all these clauses:

```sql
SELECT DISTINCT 
    product_category,
    product_type,
    product_name AS "Product",
    unit_price AS "Price"
FROM products
WHERE product_category = 'Beverages'
  AND unit_price BETWEEN 2.00 AND 5.00
ORDER BY unit_price DESC, product_name ASC
LIMIT 20;
```

```{admonition} Clause Order Matters!
:class: warning

SQL clauses must appear in this specific order:

1. SELECT
2. FROM
3. WHERE
4. GROUP BY (Chapter 7)
5. HAVING (Chapter 7)
6. ORDER BY
7. LIMIT
```

## SQL Execution Order

The order you write clauses isn't the order SQL processes them:

```{mermaid}
flowchart LR
    A[FROM] --> B[WHERE]
    B --> C[GROUP BY]
    C --> D[HAVING]
    D --> E[SELECT]
    E --> F[DISTINCT]
    F --> G[ORDER BY]
    G --> H[LIMIT]
```

Understanding this helps explain why you can't use column aliases in WHERE but can in ORDER BY.

## Summary

In this chapter, we learned the fundamentals of SQL queries:

- **SELECT** specifies which columns to retrieve
- **FROM** specifies which table(s)
- **WHERE** filters rows based on conditions
- **ORDER BY** sorts the results
- **LIMIT** restricts the number of rows
- **DISTINCT** removes duplicates

These building blocks will be the foundation for everything we do with SQL!

## Quick Reference

```sql
-- Complete SELECT syntax covered in this chapter
SELECT [DISTINCT] column1 [AS alias], column2, ...
FROM table_name
WHERE condition1 [AND|OR condition2 ...]
ORDER BY column1 [ASC|DESC], column2 [ASC|DESC], ...
LIMIT n [OFFSET m];
```

## Practice Time!

Ready to try it yourself? Head to the [Hands-On SQL Basics](../notebooks/05-sql-basics-practice.ipynb) notebook for hands-on exercises with our coffee shop database!

```{exercise}
:label: ex-sql-1

Write a query to find all transactions where the quantity is greater than 2.
```

```{exercise}
:label: ex-sql-2

Write a query to list all unique product types in the 'Food' category.
```

```{exercise}
:label: ex-sql-3

Write a query to find the top 5 highest-priced beverages, showing only the product name and price.
```

---

**Next:** [Hands-On SQL Basics](../notebooks/05-sql-basics-practice.ipynb)
