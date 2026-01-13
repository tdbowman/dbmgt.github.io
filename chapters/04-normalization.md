# Database Normalization

```{epigraph}
The key, the whole key, and nothing but the key—so help me Codd.

-- Database Designer's Oath (paraphrased from E.F. Codd's principles)
```

Normalization is the process of organizing a database to reduce redundancy and improve data integrity. In this chapter, you'll learn why normalization matters and how to apply normal forms to create efficient database designs.

## Learning Objectives

By the end of this chapter, you will be able to:

- Explain normalization and its role in database design
- Identify each normal form: 1NF, 2NF, 3NF, BCNF
- Recognize data anomalies (insertion, update, deletion)
- Transform tables from lower to higher normal forms
- Know when denormalization might be appropriate

## Why Normalize?

Normalization addresses several problems that arise from poor table design:

::::{grid} 3

:::{grid-item-card} 🔄 Update Anomaly
Changing data in one place but not others leads to inconsistent data.
:::

:::{grid-item-card} ➕ Insertion Anomaly
Can't add certain data without adding unrelated data.
:::

:::{grid-item-card} ➖ Deletion Anomaly
Deleting data accidentally removes other important information.
:::

::::

### Example: A Poorly Designed Table

Consider this unnormalized SALES table:

| OrderID | Customer | CustomerPhone | Product | Category | Price | Qty |
|---------|----------|---------------|---------|----------|-------|-----|
| 1001 | John Smith | 555-1234 | Latte | Beverages | 4.50 | 2 |
| 1001 | John Smith | 555-1234 | Muffin | Food | 3.25 | 1 |
| 1002 | Jane Doe | 555-5678 | Latte | Beverages | 4.50 | 1 |

**Problems:**
- John's phone number is stored twice (redundancy)
- If John changes his phone, we must update multiple rows (update anomaly)
- If we delete order 1002, we lose Jane Doe's contact info (deletion anomaly)
- Latte's price is stored multiple times (redundancy)

## Functional Dependencies

Before diving into normal forms, we need to understand **functional dependencies**.

:::{glossary}
Functional Dependency
  A relationship where one attribute uniquely determines another. Written as A → B, meaning "A determines B."
:::

**Examples:**
- `employee_id → employee_name` (ID determines name)
- `product_id → product_name, price` (product ID determines name and price)
- `order_id, product_id → quantity` (composite key determines quantity)

## The Normal Forms

```{mermaid}
flowchart LR
    UNF[Unnormalized] --> 1NF[First Normal Form]
    1NF --> 2NF[Second Normal Form]
    2NF --> 3NF[Third Normal Form]
    3NF --> BCNF[Boyce-Codd NF]
    BCNF --> 4NF[Fourth Normal Form]
    
    style 3NF fill:#90EE90
```

For most practical purposes, **Third Normal Form (3NF)** is sufficient. Let's examine each level.

## First Normal Form (1NF)

A table is in **1NF** if:

1. All columns contain only **atomic** (indivisible) values
2. Each column contains values of a **single type**
3. Each row is **unique** (has a primary key)
4. No **repeating groups** of columns

### Violation Example

| OrderID | Customer | Products |
|---------|----------|----------|
| 1001 | John | Latte, Muffin, Coffee |

The `Products` column contains multiple values—not atomic!

### 1NF Solution

| OrderID | Customer | Product |
|---------|----------|---------|
| 1001 | John | Latte |
| 1001 | John | Muffin |
| 1001 | John | Coffee |

Now each cell contains a single value.

## Second Normal Form (2NF)

A table is in **2NF** if:

1. It is in 1NF
2. All non-key attributes are **fully functionally dependent** on the entire primary key (no partial dependencies)

This only applies to tables with **composite primary keys**.

### Violation Example

Primary Key: (OrderID, ProductID)

| OrderID | ProductID | CustomerName | ProductPrice | Quantity |
|---------|-----------|--------------|--------------|----------|
| 1001 | P1 | John | 4.50 | 2 |
| 1001 | P2 | John | 3.25 | 1 |

**Problem:** CustomerName depends only on OrderID, not on the full key.

### 2NF Solution

Split into two tables:

**Orders Table:**
| OrderID | CustomerName |
|---------|--------------|
| 1001 | John |

**OrderDetails Table:**
| OrderID | ProductID | ProductPrice | Quantity |
|---------|-----------|--------------|----------|
| 1001 | P1 | 4.50 | 2 |
| 1001 | P2 | 3.25 | 1 |

## Third Normal Form (3NF)

A table is in **3NF** if:

1. It is in 2NF
2. No **transitive dependencies** (non-key columns depending on other non-key columns)

### Violation Example

| EmployeeID | DepartmentID | DepartmentName |
|------------|--------------|----------------|
| E1 | D1 | Sales |
| E2 | D1 | Sales |
| E3 | D2 | Marketing |

**Problem:** DepartmentName depends on DepartmentID, not directly on EmployeeID.

Dependency chain: EmployeeID → DepartmentID → DepartmentName

### 3NF Solution

**Employees Table:**
| EmployeeID | DepartmentID |
|------------|--------------|
| E1 | D1 |
| E2 | D1 |
| E3 | D2 |

**Departments Table:**
| DepartmentID | DepartmentName |
|--------------|----------------|
| D1 | Sales |
| D2 | Marketing |

## The 3NF Test

Remember this mnemonic for 3NF:

> Every non-key attribute must depend on **the key**, **the whole key**, and **nothing but the key**.

- **The key**: Attribute is dependent on primary key (1NF)
- **The whole key**: No partial dependencies (2NF)
- **Nothing but the key**: No transitive dependencies (3NF)

## Boyce-Codd Normal Form (BCNF)

A stricter version of 3NF. A table is in BCNF if:

- For every functional dependency A → B, A is a superkey

Most 3NF tables are automatically in BCNF. The distinction matters mainly for tables with multiple overlapping candidate keys.

## Normalization Process Summary

```{mermaid}
flowchart TD
    A[Start with unnormalized data] --> B[Remove repeating groups]
    B --> C[1NF: Atomic values, unique rows]
    C --> D[Remove partial dependencies]
    D --> E[2NF: Full functional dependency on PK]
    E --> F[Remove transitive dependencies]
    F --> G[3NF: No non-key dependencies]
```

## When to Denormalize

While normalization reduces redundancy, sometimes **denormalization** is beneficial:

- **Performance**: Joining many tables can be slow
- **Reporting**: Analytical queries often need pre-joined data
- **Read-heavy workloads**: Trading write efficiency for read speed

```{warning}
Only denormalize after careful analysis! The benefits of normalization usually outweigh performance concerns, especially with modern databases and indexing.
```

## Summary

- **Normalization** reduces redundancy and prevents data anomalies
- **1NF**: Atomic values, no repeating groups
- **2NF**: No partial dependencies (for composite keys)
- **3NF**: No transitive dependencies
- Most databases should be in at least **3NF**
- **Denormalization** may be appropriate for specific performance needs

## Readings

- Silberschatz - Chapter 6 (Normalization of Database Tables)
- Watt - Chapter 12 (Normalization)
- Rob - Chapter 6 (Normalization)

## Exercises

```{exercise}
:label: ex-norm-1

Identify the normal form of this table and explain what violations exist:

| StudentID | CourseID | StudentName | CourseName | InstructorID | InstructorName | Grade |
|-----------|----------|-------------|------------|--------------|----------------|-------|
```

```{exercise}
:label: ex-norm-2

Normalize the following table to 3NF:

| InvoiceNo | CustomerID | CustomerName | CustomerCity | ProductID | ProductName | Qty | Price |
|-----------|------------|--------------|--------------|-----------|-------------|-----|-------|
```

---

**Next:** [Introduction to SQL](05-intro-sql.md)
