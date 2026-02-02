# Normalization Basics

## What is Normalization?

**Normalization** is the process of organizing a database to:
- Reduce data redundancy (duplicate data)
- Eliminate anomalies (problems with insert, update, delete)
- Ensure data integrity

Think of it as organizing a messy closet—everything gets its proper place.

---

## Why Normalize?

### The Problem: Anomalies

Consider this poorly designed table:

| order_id | customer_name | customer_email | product | price | qty |
|----------|--------------|----------------|---------|-------|-----|
| 1 | John Smith | john@email.com | Coffee | 3.50 | 2 |
| 2 | John Smith | john@email.com | Muffin | 2.50 | 1 |
| 3 | Jane Doe | jane@email.com | Coffee | 3.50 | 1 |

**Problems:**

#### Insert Anomaly
- Can't add a new customer until they place an order
- Can't add a new product until someone orders it

#### Update Anomaly
- John changes his email? Must update EVERY row
- Coffee price changes? Update every row with Coffee
- Miss one? Inconsistent data!

#### Delete Anomaly
- Delete Jane's only order? Lose her customer information entirely

### The Solution: Normalization

Split into separate tables:

**Customers:**
| customer_id | name | email |
|-------------|------|-------|
| 1 | John Smith | john@email.com |
| 2 | Jane Doe | jane@email.com |

**Products:**
| product_id | name | price |
|------------|------|-------|
| 1 | Coffee | 3.50 |
| 2 | Muffin | 2.50 |

**Orders:**
| order_id | customer_id | product_id | qty |
|----------|-------------|------------|-----|
| 1 | 1 | 1 | 2 |
| 2 | 1 | 2 | 1 |
| 3 | 2 | 1 | 1 |

Now:
- ✅ Add customers without orders
- ✅ Update email in ONE place
- ✅ Delete orders without losing customers

---

## Functional Dependencies

To understand normalization, we need **functional dependencies**.

### Definition

If knowing the value of A tells you the value of B, then:
> **A → B** (A functionally determines B)

### Examples

- **student_id → student_name** (knowing ID gives you name)
- **employee_id → salary** (each employee has one salary)
- **order_id, product_id → quantity** (combination determines quantity)

### Types of Dependencies

**Full Dependency:**
The entire key is needed.
- (order_id, product_id) → quantity ✅

**Partial Dependency:**
Only part of the key is needed.
- (order_id, product_id) → customer_name ❌
- Actually: order_id → customer_name

**Transitive Dependency:**
A → B → C (A determines C through B)
- employee_id → dept_id → dept_name
- Problem: employee_id → dept_name is transitive

---

## Data Redundancy

**Redundancy** = same data stored multiple times

### Symptoms of Redundancy

- Same value appears in many rows
- Storage space wasted
- Updates require changing multiple rows
- Risk of inconsistency

### Example of Redundancy

| emp_id | emp_name | dept_id | dept_name | dept_location |
|--------|----------|---------|-----------|---------------|
| 1 | Alice | 10 | Sales | Building A |
| 2 | Bob | 10 | Sales | Building A |
| 3 | Carol | 20 | Marketing | Building B |
| 4 | Dave | 10 | Sales | Building A |

"Sales" and "Building A" repeated 3 times!

### After Normalization

**Employees:**
| emp_id | emp_name | dept_id |
|--------|----------|---------|
| 1 | Alice | 10 |
| 2 | Bob | 10 |
| 3 | Carol | 20 |
| 4 | Dave | 10 |

**Departments:**
| dept_id | dept_name | dept_location |
|---------|-----------|---------------|
| 10 | Sales | Building A |
| 20 | Marketing | Building B |

Now "Sales" and "Building A" stored only once!

---

## The Normal Forms

Normalization proceeds through **normal forms**, each eliminating specific problems:

| Normal Form | Eliminates |
|-------------|------------|
| 1NF | Repeating groups, multi-valued columns |
| 2NF | Partial dependencies |
| 3NF | Transitive dependencies |
| BCNF | Remaining anomalies from candidate keys |

Each form builds on the previous:
> **1NF → 2NF → 3NF → BCNF**

```{tip}
Most databases aim for **3NF** or **BCNF**. Going further (4NF, 5NF) is rarely necessary.
```

---

## When NOT to Normalize

Sometimes **denormalization** (adding redundancy back) makes sense:

- **Performance:** Joins are expensive; redundancy can speed reads
- **Reporting:** Flat tables are easier for analysis
- **Simplicity:** Over-normalization creates too many tables

The key is understanding the trade-offs:
- More normalization = Better integrity, slower queries
- Less normalization = Faster queries, risk of anomalies

---

## Summary

- Normalization reduces redundancy and anomalies
- Functional dependencies show how attributes relate
- Normal forms (1NF → 2NF → 3NF → BCNF) progressively improve design
- 3NF is the typical target for most databases

---

**Next:** [Normal Forms](normal-forms.md)
