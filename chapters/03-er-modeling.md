# Entity-Relationship Modeling

> *"A picture is worth a thousand words—especially when designing databases."*
>
> — Database Design Wisdom

Entity-Relationship (ER) modeling is a technique for visually representing the structure of a database before it's built. An ER diagram serves as a blueprint that helps designers, developers, and stakeholders understand and communicate about data requirements.

## Learning Objectives

By the end of this chapter, you will be able to:

- Explain the purpose and benefits of ER modeling
- Identify entities, attributes, and relationships from business requirements
- Draw ER diagrams using Crow's Foot notation
- Distinguish between different cardinality types (1:1, 1:M, M:N)
- Understand participation constraints (mandatory vs. optional)
- Convert ER diagrams to relational schemas

## What is an Entity-Relationship Diagram?

An **Entity-Relationship Diagram (ERD)** is a visual representation of the data structure for a database. It shows:

- **Entities**: Things we want to store data about
- **Attributes**: Properties that describe entities  
- **Relationships**: How entities are connected to each other

```{mermaid}
erDiagram
    CUSTOMER ||--o{ ORDER : places
    ORDER ||--|{ ORDER_LINE : contains
    PRODUCT ||--o{ ORDER_LINE : "appears in"
    
    CUSTOMER {
        int customer_id PK
        string name
        string email
    }
    ORDER {
        int order_id PK
        date order_date
        int customer_id FK
    }
    ORDER_LINE {
        int order_id PK,FK
        int product_id PK,FK
        int quantity
    }
    PRODUCT {
        int product_id PK
        string name
        decimal price
    }
```

### ER Model vs. ER Diagram vs. Relational Schema

These terms are related but distinct:

| Concept | Description | Level |
|---------|-------------|-------|
| **ER Model** | The logical representation of entities and relationships | Conceptual |
| **ER Diagram** | The visual representation of the ER model | Conceptual |
| **Relational Schema** | The actual table structure in a DBMS | Logical/Physical |

## Entities

An **entity** is a real-world object or concept that can be distinctly identified and about which we want to store data.

### Identifying Entities

Look for nouns in business requirements that represent things you need to track:

- **People**: Customer, Employee, Student, Professor
- **Places**: Store, Warehouse, Campus, Room
- **Things**: Product, Book, Equipment, Vehicle
- **Events**: Order, Transaction, Appointment, Class
- **Concepts**: Account, Department, Course, Project

```{tip}
**Entity Test**: Ask "Do we need to store multiple instances of this?" If yes, it's likely an entity.

- "Customer" → Yes, many customers → Entity ✓
- "Order Date" → No, it's a property of Order → Attribute ✗
```

### Strong vs. Weak Entities

| Type | Description | Example |
|------|-------------|---------|
| **Strong Entity** | Can exist independently; has its own primary key | EMPLOYEE, PRODUCT |
| **Weak Entity** | Depends on another entity for identification | ORDER_LINE (depends on ORDER) |

In Crow's Foot notation, weak entities are sometimes shown with rounded corners or double borders.

## Attributes

**Attributes** are the properties or characteristics that describe an entity.

### Types of Attributes

```{mermaid}
mindmap
  root((Attributes))
    Simple
      employee_id
      salary
    Composite
      name
        first_name
        last_name
      address
        street
        city
        zip
    Derived
      age
      total_amount
    Key
      Primary Key
      Foreign Key
    Multivalued
      phone_numbers
      email_addresses
```

### Attribute Notation in ERDs

In Crow's Foot notation, attributes are typically listed inside the entity box:

```{mermaid}
erDiagram
    EMPLOYEE {
        int employee_id PK "Primary Key"
        string first_name "Required"
        string last_name "Required"
        date birth_date
        decimal salary
        int department_id FK "Foreign Key"
    }
```

**Common Notations:**
- **PK** = Primary Key
- **FK** = Foreign Key
- **NOT NULL** or asterisk (*) = Required
- Derived attributes shown with (D) or calculated notation

## Relationships

A **relationship** is an association between two or more entities. Relationships are named with verbs that describe the association.

### Relationship Examples

| Entity A | Relationship | Entity B |
|----------|--------------|----------|
| CUSTOMER | places | ORDER |
| EMPLOYEE | works_in | DEPARTMENT |
| STUDENT | enrolls_in | COURSE |
| AUTHOR | writes | BOOK |

### Relationship Degree

The **degree** of a relationship refers to how many entities participate:

- **Unary** (Recursive): One entity relates to itself
- **Binary**: Two entities (most common)
- **Ternary**: Three entities

```{mermaid}
erDiagram
    EMPLOYEE ||--o| EMPLOYEE : "manages"
```

*A recursive relationship: an employee manages other employees*

## Cardinality and Participation

### Cardinality (Connectivity)

**Cardinality** describes the number of instances of one entity that can be associated with instances of another entity.

| Type | Description |
|------|-------------|
| **One-to-One (1:1)** | One instance of A relates to exactly one instance of B |
| **One-to-Many (1:M)** | One instance of A relates to many instances of B |
| **Many-to-Many (M:N)** | Many instances of A relate to many instances of B |

### Crow's Foot Notation Symbols

The Crow's Foot notation uses symbols at the ends of relationship lines:

| Symbol | Meaning | Cardinality |
|--------|---------|-------------|
| `\|` | One | Exactly one |
| `\|\|` | One and only one | Mandatory one |
| `O` | Zero | Optional |
| `<` or crow's foot | Many | Multiple |
| `O<` | Zero or many | Optional many |
| `\|<` | One or many | Mandatory many |

### Reading Crow's Foot Diagrams

Read from each entity's perspective:

```{mermaid}
erDiagram
    DEPARTMENT ||--o{ EMPLOYEE : employs
```

Reading this diagram:
- **From DEPARTMENT**: "One department employs zero or many employees"
- **From EMPLOYEE**: "Each employee works in one and only one department"

### Participation Constraints

**Participation** indicates whether an entity's involvement in a relationship is mandatory or optional.

| Type | Symbol | Meaning |
|------|--------|---------|
| **Mandatory** | `\|` | Must participate (at least one) |
| **Optional** | `O` | May or may not participate (zero allowed) |

**Example:** 
- DEPARTMENT to EMPLOYEE: Optional (a department might have no employees yet)
- EMPLOYEE to DEPARTMENT: Mandatory (every employee must belong to a department)

## Common Cardinality Patterns

### One-to-One (1:1)

```{mermaid}
erDiagram
    EMPLOYEE ||--|| PARKING_SPACE : "is assigned"
    EMPLOYEE {
        int emp_id PK
        string name
    }
    PARKING_SPACE {
        int space_id PK
        string location
        int emp_id FK
    }
```

**Business Rule:** Each employee is assigned exactly one parking space, and each space is assigned to exactly one employee.

### One-to-Many (1:M)

```{mermaid}
erDiagram
    CUSTOMER ||--o{ ORDER : places
    CUSTOMER {
        int customer_id PK
        string name
    }
    ORDER {
        int order_id PK
        date order_date
        int customer_id FK
    }
```

**Business Rule:** A customer can place many orders, but each order belongs to exactly one customer.

### Many-to-Many (M:N)

```{mermaid}
erDiagram
    STUDENT ||--o{ ENROLLMENT : has
    COURSE ||--o{ ENROLLMENT : has
    STUDENT {
        int student_id PK
        string name
    }
    ENROLLMENT {
        int student_id PK,FK
        int course_id PK,FK
        string grade
    }
    COURSE {
        int course_id PK
        string title
    }
```

**Business Rule:** Students can enroll in many courses, and courses can have many students.

```{important}
M:N relationships are always resolved with a **junction table** (ENROLLMENT in this example) that contains foreign keys to both parent tables.
```

## ER Modeling Process

Follow these steps to create an ER diagram:

### Step 1: Identify Entities

Read the business requirements and identify nouns that represent things to track.

**Example Scenario:** "A coffee shop needs to track its products, sales transactions, employees, and customers. Each transaction is processed by one employee and can include multiple products."

**Entities identified:** PRODUCT, TRANSACTION, EMPLOYEE, CUSTOMER

### Step 2: Identify Relationships

Identify how entities are connected using verbs.

- EMPLOYEE processes TRANSACTION
- TRANSACTION includes PRODUCT
- CUSTOMER makes TRANSACTION

### Step 3: Determine Cardinality

For each relationship, determine the cardinality from both perspectives:

| Relationship | From A | From B |
|--------------|--------|--------|
| EMPLOYEE processes TRANSACTION | One-to-Many | Many-to-One |
| TRANSACTION includes PRODUCT | Many-to-Many | Many-to-Many |

### Step 4: Add Attributes

List the important properties of each entity:

- **PRODUCT**: product_id, name, price, category
- **EMPLOYEE**: employee_id, first_name, last_name, position
- **TRANSACTION**: transaction_id, date, time, total
- **CUSTOMER**: customer_id, name, email, loyalty_number

### Step 5: Identify Keys

Determine primary keys and foreign keys:

- Primary keys: product_id, employee_id, transaction_id, customer_id
- Foreign keys: Added to implement relationships

### Step 6: Draw the Diagram

Create the visual representation using your chosen notation.

## Converting ERD to Relational Schema

Once you have an ER diagram, convert it to a relational schema:

1. **Each entity → Table**
2. **Each attribute → Column**
3. **Primary key → PRIMARY KEY constraint**
4. **1:M relationship → Foreign key on the "many" side**
5. **M:N relationship → Junction table with composite primary key**

**Example Conversion:**

```sql
> — From ERD to SQL
CREATE TABLE departments (
    department_id SERIAL PRIMARY KEY,
    department_name VARCHAR(100) NOT NULL
);

CREATE TABLE employees (
    employee_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    department_id INTEGER REFERENCES departments(department_id)
);
```

## Common ER Modeling Mistakes

```{warning}
Avoid these common errors:

1. **Missing relationships** - Forgetting to connect related entities
2. **Wrong cardinality** - Not carefully analyzing business rules
3. **Redundant relationships** - Adding unnecessary direct connections
4. **Missing keys** - Not identifying primary keys for each entity
5. **Unresolved M:N** - Forgetting junction tables for many-to-many relationships
```

## Summary

In this chapter, we learned:

- **ER diagrams** visually represent database structure before implementation
- **Entities** are things we store data about; **attributes** describe them
- **Relationships** connect entities and have **cardinality** (1:1, 1:M, M:N)
- **Crow's Foot notation** uses symbols to show cardinality and participation
- **Junction tables** resolve many-to-many relationships
- The **ER modeling process** moves from requirements to implementation

## Exercises

```{exercise}
:label: ex-erd-1

Draw an ER diagram for a library system with these requirements:
- The library has many books
- Each book has one or more authors
- Each author can write multiple books
- Members can borrow books
- Track which member borrowed which book and when it was returned
```

```{exercise}
:label: ex-erd-2

A university needs a database to track:
- Students and their enrollment
- Courses offered each semester
- Professors who teach courses
- Classrooms where courses meet

Identify the entities, relationships, and cardinalities.
```

```{exercise}
:label: ex-erd-3

Convert this ER diagram description to a relational schema (CREATE TABLE statements):
- ARTIST (artist_id PK, name, country)
- ALBUM (album_id PK, title, release_year, artist_id FK)
- SONG (song_id PK, title, duration, album_id FK)
- PLAYLIST can contain many SONGs, and SONGs can appear in many PLAYLISTs
```

---

**Next:** [Database Normalization](04-normalization.md)
