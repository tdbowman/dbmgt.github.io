# Entity-Relationship Modeling

## What is ER Modeling?

**Entity-Relationship (ER) modeling** is a technique for designing databases visually before implementation. It helps you:

- Understand the data requirements
- Identify entities and their relationships
- Communicate designs with stakeholders
- Avoid costly redesigns later

Think of it as creating a blueprint before building a house.

---

## Core Concepts

### Entities

An **entity** is a "thing" we want to store data about.

**Examples:**
- Customer
- Product
- Order
- Employee
- Store

**Entity Instance:** A specific occurrence of an entity
- Entity: Customer
- Instance: "John Smith, customer #1234"

### Attributes

**Attributes** are properties that describe an entity.

**Example - Customer Entity:**
- customer_id
- first_name
- last_name
- email
- phone
- address

#### Types of Attributes

| Type | Description | Example |
|------|-------------|---------|
| **Simple** | Cannot be divided | first_name |
| **Composite** | Can be divided into parts | address → street, city, state, zip |
| **Derived** | Calculated from other attributes | age (from birthdate) |
| **Multivalued** | Can have multiple values | phone_numbers |

```{note}
In relational databases, we typically avoid multivalued attributes by creating separate tables.
```

### Relationships

A **relationship** shows how entities are connected.

**Examples:**
- Customer **places** Order
- Employee **works in** Department
- Product **belongs to** Category

#### Relationship Degree

| Degree | Entities Involved | Example |
|--------|-------------------|---------|
| Unary | 1 (self-referencing) | Employee **manages** Employee |
| Binary | 2 | Customer **places** Order |
| Ternary | 3 | Doctor **prescribes** Medicine **to** Patient |

Most relationships are binary.

---

## Cardinality

**Cardinality** defines how many instances of one entity relate to another.

### One-to-One (1:1)

One instance of A relates to exactly one instance of B.

**Example:** Each employee has one parking spot; each parking spot belongs to one employee.

```
[Employee] 1 -------- 1 [ParkingSpot]
```

### One-to-Many (1:N)

One instance of A relates to many instances of B.

**Example:** One department has many employees; each employee belongs to one department.

```
[Department] 1 -------- N [Employee]
```

### Many-to-Many (M:N)

Many instances of A relate to many instances of B.

**Example:** Students enroll in many courses; courses have many students.

```
[Student] M -------- N [Course]
```

```{important}
Many-to-many relationships require a **junction table** (also called bridge table or associative entity) when implemented in a database.
```

---

## Participation

**Participation** defines whether all instances must participate in a relationship.

### Total Participation (Mandatory)

Every instance MUST participate in the relationship.

**Example:** Every order must have a customer.

### Partial Participation (Optional)

Instances MAY participate in the relationship.

**Example:** An employee may or may not manage other employees.

---

## Identifying vs. Non-Identifying Relationships

### Identifying Relationship

The child entity's existence depends on the parent. The parent's key becomes part of the child's primary key.

**Example:** Order and OrderItem
- An OrderItem cannot exist without an Order
- OrderItem's PK might be (order_id, line_number)

### Non-Identifying Relationship

The child entity can exist independently. The parent's key is just a foreign key.

**Example:** Employee and Department
- An employee references a department
- But employee_id alone identifies the employee

---

## Steps for ER Modeling

1. **Identify Entities**
   - What "things" do we need to track?
   - Look for nouns in requirements

2. **Identify Attributes**
   - What do we need to know about each entity?
   - Determine the primary key

3. **Identify Relationships**
   - How are entities connected?
   - Look for verbs in requirements

4. **Determine Cardinality**
   - How many of each?
   - 1:1, 1:N, or M:N?

5. **Determine Participation**
   - Is participation required or optional?

6. **Draw the Diagram**
   - Use standard notation
   - Review with stakeholders

---

## Example: Coffee Shop

**Business Requirements:**
- We have multiple stores in different cities
- Each store has employees who work there
- Employees have a home store but may work at others
- We sell products organized into categories
- Customers make purchases (transactions)
- Some customers have loyalty cards

**Entities Identified:**
- Store
- Employee
- Product
- Category
- Customer
- Transaction

**Relationships:**
- Store **employs** Employee (1:N)
- Product **belongs to** Category (N:1)
- Customer **makes** Transaction (1:N)
- Transaction **includes** Product (M:N → needs junction table)

---

**Next:** [Drawing ER Diagrams](er-diagrams.md)
