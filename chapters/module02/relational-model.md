# The Relational Model

## Introduction

The **relational model** was introduced by Edgar F. Codd in 1970 and revolutionized how we organize and access data. Today, relational databases power most business applications worldwide.

## Key Concepts

### Tables (Relations)

A **table** (formally called a **relation**) is a two-dimensional structure with:
- **Rows** (tuples) - individual records
- **Columns** (attributes) - properties of each record

### Example: Employees Table

| employee_id | first_name | last_name | department | salary |
|-------------|------------|-----------|------------|--------|
| 101 | Alice | Johnson | Sales | 55000 |
| 102 | Bob | Smith | Marketing | 52000 |
| 103 | Carol | Williams | Sales | 58000 |

### Terminology

| Informal Term | Formal Term | Description |
|---------------|-------------|-------------|
| Table | Relation | Collection of related data |
| Row | Tuple | Single record |
| Column | Attribute | Property of a record |
| Column type | Domain | Set of allowed values |

---

## Properties of Relations

1. **Each row is unique** - No duplicate rows
2. **Order doesn't matter** - Rows and columns have no inherent order
3. **Each cell contains one value** - No arrays or lists in a cell (atomic values)
4. **Each column has a unique name** - No duplicate column names

---

## Data Types (Domains)

Each column has a **domain**—the set of legal values. In PostgreSQL:

| Data Type | Description | Example |
|-----------|-------------|---------|
| `INTEGER` | Whole numbers | 42, -17, 0 |
| `DECIMAL(p,s)` | Exact numbers | 19.99, 3.14159 |
| `VARCHAR(n)` | Variable-length text | 'Hello', 'Database' |
| `DATE` | Calendar date | '2024-01-15' |
| `BOOLEAN` | True/false | TRUE, FALSE |

---

**Next:** [Keys and Constraints](keys-constraints.md)
