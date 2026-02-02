# The Relational Data Model

> *"The relational model is founded on the simple idea that data is best organized in tables."*
>
> — E.F. Codd, 1970

The relational model, introduced by Edgar F. Codd at IBM in 1970, revolutionized how we think about and manage data. Today, it remains the dominant approach for organizing data in databases. In this chapter, you'll learn the fundamental concepts that underpin every relational database system.

## Learning Objectives

By the end of this chapter, you will be able to:

- Explain the fundamental concepts of the relational model
- Define relations, attributes, tuples, and domains
- Identify different types of keys (primary, foreign, candidate, surrogate)
- Understand and apply relational integrity rules
- Read and interpret database schemas
- Distinguish between logical and physical data models

## The Building Blocks of Relational Databases

The relational model uses a small set of concepts to represent data. Let's explore each one.

### Relations (Tables)

In relational database terminology, a **relation** is what we commonly call a table. A relation is a two-dimensional structure consisting of rows and columns.

```{mermaid}
erDiagram
    EMPLOYEES {
        int employee_id PK
        string first_name
        string last_name
        date hire_date
        decimal salary
    }
```

Key characteristics of a relation:

- Has a unique name
- Each column has a unique name within the table
- All values in a column are of the same data type
- Each row is unique (no duplicate rows)
- The order of rows and columns doesn't matter

### Attributes (Columns)

An **attribute** represents a characteristic or property of an entity. In a table, attributes are the columns.

| Attribute Type | Description | Example |
|---------------|-------------|---------|
| **Simple** | Cannot be subdivided | `employee_id`, `salary` |
| **Composite** | Can be divided into smaller parts | `full_name` → `first_name`, `last_name` |
| **Derived** | Calculated from other attributes | `age` (from `birth_date`) |
| **Key** | Uniquely identifies a row | `employee_id` |
| **Multivalued** | Can have multiple values | `phone_numbers` (should be normalized) |

```{warning}
Multivalued attributes violate First Normal Form (1NF). In a properly designed relational database, each attribute should contain only atomic (indivisible) values.
```

### Tuples (Rows)

A **tuple** is a single row in a relation. Each tuple represents one instance of the entity described by the table.

| employee_id | first_name | last_name | hire_date | salary |
|-------------|------------|-----------|-----------|--------|
| 101 | John | Smith | 2020-03-15 | 55000.00 |
| 102 | Sarah | Johnson | 2019-07-22 | 62000.00 |
| 103 | Michael | Chen | 2021-01-10 | 48000.00 |

In this EMPLOYEES table, each row is a tuple representing one employee.

### Domains

A **domain** is the set of allowable values for an attribute. Domains define:

- The data type (integer, varchar, date, etc.)
- The range of valid values
- The format of the data

**Examples:**

- `employee_id`: Positive integers from 1 to 999999
- `salary`: Decimal numbers from 0 to 9999999.99
- `gender`: One of {'M', 'F', 'N', NULL}
- `email`: Valid email format (contains @ symbol)

## Keys: The Foundation of Relationships

Keys are crucial for identifying rows and establishing relationships between tables. Understanding the different types of keys is essential for database design.

### Primary Key

A **primary key** (PK) is a column or combination of columns that uniquely identifies each row in a table.

**Rules for primary keys:**

1. Must be unique for each row
2. Cannot contain NULL values
3. Should rarely (if ever) change
4. Should be as simple as possible

```sql
CREATE TABLE employees (
    employee_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE
);
```

### Foreign Key

A **foreign key** (FK) is a column that references the primary key of another table, establishing a relationship between the two tables.

```{mermaid}
erDiagram
    DEPARTMENTS ||--o{ EMPLOYEES : "employs"
    DEPARTMENTS {
        int department_id PK
        string department_name
    }
    EMPLOYEES {
        int employee_id PK
        string first_name
        string last_name
        int department_id FK
    }
```

```sql
CREATE TABLE employees (
    employee_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    department_id INTEGER REFERENCES departments(department_id)
);
```

### Candidate Key

A **candidate key** is any column or combination of columns that could serve as a primary key. All candidate keys are unique and non-null.

**Example:** In an EMPLOYEES table, both `employee_id` and `social_security_number` could uniquely identify employees. Both are candidate keys, but we choose one (typically `employee_id`) as the primary key.

### Surrogate Key vs. Natural Key

| Type | Description | Example |
|------|-------------|---------|
| **Natural Key** | A key with business meaning | `social_security_number`, `isbn` |
| **Surrogate Key** | An artificial key with no business meaning | `employee_id` (auto-generated) |

```{tip}
Surrogate keys are often preferred because they:
- Never need to change
- Are simpler (usually a single integer)
- Don't expose sensitive information
- Are database-controlled
```

### Composite Key

A **composite key** (or compound key) is a primary key consisting of two or more columns.

```sql
> — Order line items identified by both order_id AND product_id
CREATE TABLE order_items (
    order_id INTEGER,
    product_id INTEGER,
    quantity INTEGER,
    unit_price DECIMAL(10,2),
    PRIMARY KEY (order_id, product_id)
);
```

## Relationships Between Tables

Tables in a relational database are connected through relationships. There are three types of relationships:

### One-to-One (1:1)

Each row in Table A relates to exactly one row in Table B, and vice versa.

**Example:** Each employee has exactly one parking space, and each parking space is assigned to exactly one employee.

```{mermaid}
erDiagram
    EMPLOYEE ||--|| PARKING_SPACE : "is assigned"
```

### One-to-Many (1:M)

One row in Table A can relate to many rows in Table B, but each row in Table B relates to only one row in Table A.

**Example:** One department has many employees, but each employee belongs to only one department.

```{mermaid}
erDiagram
    DEPARTMENT ||--o{ EMPLOYEE : "employs"
```

This is the most common type of relationship in relational databases.

### Many-to-Many (M:N)

Rows in Table A can relate to many rows in Table B, and rows in Table B can relate to many rows in Table A.

**Example:** Students enroll in many courses, and each course has many students.

```{mermaid}
erDiagram
    STUDENT }o--o{ COURSE : "enrolls in"
```

```{important}
Many-to-many relationships cannot be directly implemented in a relational database. They must be resolved using a **junction table** (also called a bridge table or associative entity).
```

```{mermaid}
erDiagram
    STUDENT ||--o{ ENROLLMENT : "has"
    COURSE ||--o{ ENROLLMENT : "has"
    STUDENT {
        int student_id PK
        string name
    }
    ENROLLMENT {
        int student_id FK
        int course_id FK
        date enrollment_date
        string grade
    }
    COURSE {
        int course_id PK
        string course_name
    }
```

## Relational Integrity Rules

Integrity rules ensure the accuracy and consistency of data in a database.

### Entity Integrity

> **Rule:** No primary key attribute can be NULL.

The primary key must uniquely identify each row. A NULL value would make identification impossible.

```sql
> — This will fail because primary key cannot be NULL
INSERT INTO employees (employee_id, first_name) VALUES (NULL, 'John');
> — ERROR: null value in column "employee_id" violates not-null constraint
```

### Referential Integrity

> **Rule:** A foreign key must either match a primary key value in the related table or be NULL.

This ensures that relationships between tables remain valid.

```sql
> — If department_id 999 doesn't exist in departments table, this fails
INSERT INTO employees (employee_id, first_name, department_id) 
VALUES (1, 'John', 999);
> — ERROR: insert or update on table "employees" violates foreign key constraint
```

### Domain Integrity

> **Rule:** All values in a column must come from the defined domain.

```sql
> — This will fail if salary must be positive
INSERT INTO employees (employee_id, first_name, salary) 
VALUES (1, 'John', -5000);
> — ERROR: new row violates check constraint "salary_positive"
```

## Reading a Relational Schema

A **relational schema** describes the structure of a database. It's typically written as:

```
TABLE_NAME (column1, column2, column3, ...)
```

With primary keys underlined and foreign keys marked:

```
DEPARTMENT (department_id, department_name, manager_id)
           ─────────────                     FK→EMPLOYEE

EMPLOYEE (employee_id, first_name, last_name, department_id, salary)
         ───────────                          FK→DEPARTMENT
```

### Schema Notation Conventions

| Convention | Meaning |
|------------|---------|
| Underline | Primary Key |
| FK→TABLE | Foreign Key reference |
| * | NOT NULL constraint |
| (italics) | Optional/can be NULL |

## From ER Model to Relational Model

Converting an Entity-Relationship diagram to a relational schema follows specific rules:

1. **Each entity** becomes a table
2. **Each attribute** becomes a column
3. **The primary key** of the entity becomes the primary key of the table
4. **1:M relationships** are implemented by adding a foreign key to the "many" side
5. **M:N relationships** require a junction table
6. **1:1 relationships** can be implemented by adding a foreign key to either table

## Advantages of the Relational Model

The relational model has remained dominant for over 50 years because of its:

- **Structural Independence**: Programs don't depend on physical storage
- **Conceptual Simplicity**: Easy to understand tables and relationships
- **Ad-hoc Query Capability**: SQL allows flexible querying
- **Data Independence**: Logical changes don't require application changes
- **Normalization**: Systematic approach to reducing redundancy
- **ACID Transactions**: Reliable data operations

## Summary

In this chapter, we learned:

- The relational model organizes data into **relations** (tables) with **attributes** (columns) and **tuples** (rows)
- **Keys** identify rows and establish relationships: primary keys, foreign keys, candidate keys, and composite keys
- **Relationships** between tables can be 1:1, 1:M, or M:N
- **Integrity rules** (entity, referential, domain) ensure data consistency
- The relational model provides data independence, flexibility, and reliability

## Exercises

```{exercise}
:label: ex-rel-1

Given the following business rules, identify the appropriate relationship type (1:1, 1:M, or M:N):
1. A customer can place many orders
2. Each order is placed by exactly one customer
3. A book can have multiple authors
4. An author can write multiple books
5. Each employee has exactly one office
6. Each office is assigned to exactly one employee
```

```{exercise}
:label: ex-rel-2

For the following table, identify:
- The primary key
- Any candidate keys
- Potential foreign keys

| student_id | ssn | email | first_name | last_name | major_id |
|------------|-----|-------|------------|-----------|----------|
| 1001 | 123-45-6789 | john@univ.edu | John | Doe | CS |
| 1002 | 234-56-7890 | jane@univ.edu | Jane | Smith | MATH |
```

```{exercise}
:label: ex-rel-3

Write the relational schema for a library database that tracks:
- Books (ISBN, title, publication year)
- Authors (author ID, name)
- The many-to-many relationship between books and authors
- Members (member ID, name, email)
- Loans (which member borrowed which book, and when)
```

---

**Next:** [Entity-Relationship Modeling](03-er-modeling.md)
