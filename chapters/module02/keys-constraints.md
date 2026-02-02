# Keys and Constraints

## Why Keys Matter

In a relational database, **keys** are how we:
- Uniquely identify each row
- Connect tables together
- Enforce data integrity

Without keys, we'd have no reliable way to find specific records or maintain relationships between tables.

---

## Types of Keys

### Primary Key

A **primary key** uniquely identifies each row in a table.

**Rules for Primary Keys:**
- Must be unique for every row
- Cannot be NULL
- Should rarely (ideally never) change
- One primary key per table

**Example:**

| **employee_id** | first_name | last_name | department |
|-----------------|------------|-----------|------------|
| 101 | Alice | Johnson | Sales |
| 102 | Bob | Smith | Marketing |
| 103 | Carol | Williams | Sales |

Here, `employee_id` is the primary key—each employee has a unique ID.

```sql
CREATE TABLE employees (
    employee_id INTEGER PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    department VARCHAR(50)
);
```

### Foreign Key

A **foreign key** creates a link between two tables. It references the primary key of another table.

**Example:**

**Departments Table:**
| **dept_id** | dept_name | manager_id |
|-------------|-----------|------------|
| 1 | Sales | 101 |
| 2 | Marketing | 102 |

**Employees Table:**
| employee_id | first_name | **dept_id** |
|-------------|------------|-------------|
| 101 | Alice | 1 |
| 102 | Bob | 2 |
| 103 | Carol | 1 |

The `dept_id` in Employees is a **foreign key** referencing Departments.

```sql
CREATE TABLE employees (
    employee_id INTEGER PRIMARY KEY,
    first_name VARCHAR(50),
    dept_id INTEGER REFERENCES departments(dept_id)
);
```

### Candidate Key

A **candidate key** is any column (or combination) that *could* be a primary key—it's unique and not null.

**Example:**
In a Students table, both `student_id` and `email` might be unique. Both are candidate keys, but we choose one as the primary key.

### Composite Key

A **composite key** uses multiple columns together as the primary key.

**Example:** Order Items

| **order_id** | **product_id** | quantity | price |
|--------------|----------------|----------|-------|
| 1001 | 50 | 2 | 19.99 |
| 1001 | 51 | 1 | 29.99 |
| 1002 | 50 | 3 | 19.99 |

Neither `order_id` nor `product_id` alone is unique, but together they form a composite primary key.

```sql
CREATE TABLE order_items (
    order_id INTEGER,
    product_id INTEGER,
    quantity INTEGER,
    price DECIMAL(10,2),
    PRIMARY KEY (order_id, product_id)
);
```

### Natural vs. Surrogate Keys

**Natural Key:** Uses existing, meaningful data
- Email address, Social Security Number, ISBN
- Pro: Already exists, meaningful
- Con: May change, privacy concerns

**Surrogate Key:** System-generated identifier
- Auto-incrementing ID, UUID
- Pro: Never changes, no meaning to leak
- Con: Extra column, no inherent meaning

```{tip}
Most database designers prefer **surrogate keys** (like auto-incrementing IDs) for primary keys because they never need to change.
```

---

## Integrity Constraints

Constraints are rules that enforce data quality.

### Entity Integrity

**Rule:** Primary key values must be unique and not null.

This ensures every row can be uniquely identified.

### Referential Integrity

**Rule:** Foreign key values must match an existing primary key (or be null).

This ensures relationships are valid—you can't assign an employee to a department that doesn't exist.

**What happens when you delete a referenced row?**

| Option | Behavior |
|--------|----------|
| `RESTRICT` | Prevent deletion if referenced |
| `CASCADE` | Delete related rows too |
| `SET NULL` | Set foreign key to NULL |
| `SET DEFAULT` | Set to default value |

```sql
CREATE TABLE employees (
    employee_id INTEGER PRIMARY KEY,
    dept_id INTEGER REFERENCES departments(dept_id)
        ON DELETE SET NULL
        ON UPDATE CASCADE
);
```

### Domain Constraints

**Rule:** Column values must be within the defined domain (data type and restrictions).

Examples:
- `age INTEGER CHECK (age >= 0 AND age <= 150)`
- `email VARCHAR(100) NOT NULL`
- `status VARCHAR(20) CHECK (status IN ('active', 'inactive', 'pending'))`

### Other Constraints

| Constraint | Purpose |
|------------|---------|
| `NOT NULL` | Value required |
| `UNIQUE` | No duplicates allowed |
| `CHECK` | Custom validation rule |
| `DEFAULT` | Automatic value if not provided |

```sql
CREATE TABLE products (
    product_id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    sku VARCHAR(50) UNIQUE,
    price DECIMAL(10,2) CHECK (price > 0),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

## Summary

| Key Type | Purpose |
|----------|---------|
| Primary Key | Uniquely identifies each row |
| Foreign Key | Links to another table |
| Candidate Key | Could be a primary key |
| Composite Key | Multiple columns as key |

| Constraint | Purpose |
|------------|---------|
| Entity Integrity | PKs are unique and not null |
| Referential Integrity | FKs reference valid PKs |
| Domain Constraints | Values within allowed range |

---

**Next Module:** [Data & Database Modeling](../module03/overview.md)
