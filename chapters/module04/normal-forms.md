# Normal Forms

## Overview

Normal forms are a series of tests for database tables. Each form eliminates specific types of redundancy.

```
Unnormalized → 1NF → 2NF → 3NF → BCNF
                ↓      ↓      ↓
            Atomic  No Partial  No Transitive
            Values  Dependencies Dependencies
```

---

## First Normal Form (1NF)

### Rule
> Each cell contains only **atomic** (indivisible) values, and each record is unique.

### Violations of 1NF

**Multi-valued attributes:**
| order_id | products |
|----------|----------|
| 1 | Coffee, Muffin, Scone |

**Repeating groups:**
| student_id | course1 | grade1 | course2 | grade2 |
|------------|---------|--------|---------|--------|
| 101 | Math | A | English | B |

### Converting to 1NF

**Multi-valued → separate rows:**
| order_id | product |
|----------|---------|
| 1 | Coffee |
| 1 | Muffin |
| 1 | Scone |

**Repeating groups → separate table:**

**Students:**
| student_id | name |
|------------|------|
| 101 | Alice |

**Enrollments:**
| student_id | course | grade |
|------------|--------|-------|
| 101 | Math | A |
| 101 | English | B |

### 1NF Checklist
- ✅ All columns have atomic values
- ✅ No repeating groups
- ✅ Each row is unique (has a primary key)

---

## Second Normal Form (2NF)

### Rule
> Must be in 1NF AND have no **partial dependencies**.

A partial dependency exists when a non-key column depends on only PART of a composite primary key.

### When Does 2NF Apply?
Only relevant when you have a **composite primary key** (multiple columns).

### Example Violation

**OrderItems** (PK: order_id, product_id)

| order_id | product_id | quantity | **product_name** | **product_price** |
|----------|------------|----------|------------------|-------------------|
| 1 | 101 | 2 | Coffee | 3.50 |
| 1 | 102 | 1 | Muffin | 2.50 |
| 2 | 101 | 1 | Coffee | 3.50 |

**Problem:**
- product_name depends only on product_id, not the full PK
- product_price depends only on product_id

### Converting to 2NF

**OrderItems:**
| order_id | product_id | quantity |
|----------|------------|----------|
| 1 | 101 | 2 |
| 1 | 102 | 1 |
| 2 | 101 | 1 |

**Products:**
| product_id | product_name | product_price |
|------------|--------------|---------------|
| 101 | Coffee | 3.50 |
| 102 | Muffin | 2.50 |

### 2NF Checklist
- ✅ Is in 1NF
- ✅ All non-key columns depend on the ENTIRE primary key
- ✅ (Only matters with composite keys)

---

## Third Normal Form (3NF)

### Rule
> Must be in 2NF AND have no **transitive dependencies**.

A transitive dependency: A → B → C where A is the PK, and C depends on B rather than directly on A.

### Example Violation

**Employees** (PK: emp_id)

| emp_id | emp_name | dept_id | **dept_name** | **dept_location** |
|--------|----------|---------|---------------|-------------------|
| 1 | Alice | 10 | Sales | Building A |
| 2 | Bob | 10 | Sales | Building A |
| 3 | Carol | 20 | Marketing | Building B |

**Problem:**
- emp_id → dept_id → dept_name (transitive!)
- emp_id → dept_id → dept_location (transitive!)

dept_name and dept_location depend on dept_id, not directly on emp_id.

### Converting to 3NF

**Employees:**
| emp_id | emp_name | dept_id |
|--------|----------|---------|
| 1 | Alice | 10 |
| 2 | Bob | 10 |
| 3 | Carol | 20 |

**Departments:**
| dept_id | dept_name | dept_location |
|---------|-----------|---------------|
| 10 | Sales | Building A |
| 20 | Marketing | Building B |

### 3NF Checklist
- ✅ Is in 2NF
- ✅ No non-key column depends on another non-key column
- ✅ All non-key columns depend DIRECTLY on the primary key

```{tip}
**Memory aid for 3NF:** "Every non-key attribute must provide a fact about the key, the whole key, and nothing but the key."
```

---

## Boyce-Codd Normal Form (BCNF)

### Rule
> For every functional dependency A → B, A must be a **superkey**.

BCNF is stricter than 3NF. It handles edge cases with multiple candidate keys.

### When Does BCNF Matter?

When you have:
- Multiple candidate keys
- Overlapping candidate keys
- Determinants that aren't candidate keys

### Example Violation

**CourseInstructors**

| student | course | instructor |
|---------|--------|------------|
| Alice | Database | Dr. Smith |
| Bob | Database | Dr. Smith |
| Alice | Python | Dr. Jones |
| Carol | Python | Dr. Brown |

**Constraints:**
- Each student takes each course once
- Each instructor teaches only ONE course
- Courses can have multiple instructors

**Candidate keys:** (student, course) and (student, instructor)

**Problem:** instructor → course, but instructor is not a superkey

### Converting to BCNF

**StudentInstructors:**
| student | instructor |
|---------|------------|
| Alice | Dr. Smith |
| Bob | Dr. Smith |
| Alice | Dr. Jones |
| Carol | Dr. Brown |

**InstructorCourses:**
| instructor | course |
|------------|--------|
| Dr. Smith | Database |
| Dr. Jones | Python |
| Dr. Brown | Python |

### BCNF vs 3NF

| Aspect | 3NF | BCNF |
|--------|-----|------|
| Strictness | Less strict | More strict |
| Preserves all dependencies | Always | Sometimes not |
| Practical use | Most common | When needed |

---

## Summary Table

| NF | Rule | Eliminates |
|----|------|------------|
| **1NF** | Atomic values, unique rows | Repeating groups |
| **2NF** | 1NF + no partial dependencies | Partial key dependencies |
| **3NF** | 2NF + no transitive dependencies | Non-key dependencies |
| **BCNF** | Every determinant is a superkey | Remaining anomalies |

---

## Practical Normalization Steps

1. **Start with all data in one table** (unnormalized)
2. **Apply 1NF:** Remove multi-valued and repeating groups
3. **Identify all functional dependencies**
4. **Apply 2NF:** Remove partial dependencies (create new tables)
5. **Apply 3NF:** Remove transitive dependencies (create new tables)
6. **Check BCNF:** If issues remain with candidate keys, decompose further

---

## Quick Decision Guide

```
Is data atomic? ─No→ Fix for 1NF
        │
       Yes
        ↓
Composite PK with partial dependencies? ─Yes→ Fix for 2NF
        │
       No
        ↓
Transitive dependencies? ─Yes→ Fix for 3NF
        │
       No
        ↓
Determinants that aren't superkeys? ─Yes→ Fix for BCNF
        │
       No
        ↓
     Done! ✓
```

---

**Next Module:** [SQL Statements](../module05/overview.md)
