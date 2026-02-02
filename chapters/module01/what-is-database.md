# What is a Database?

## The Problem: Data Chaos

Imagine you run a coffee shop. You need to track:
- **Products** - What you sell, prices, inventory
- **Employees** - Who works for you, their schedules, pay rates
- **Customers** - Loyalty programs, purchase history
- **Transactions** - Every sale, every day

In the early days of computing, all this data was stored in **files**. Each department might have their own files:
- `products.txt`
- `employees.xlsx`
- `sales_jan.csv`, `sales_feb.csv`, ...

### Problems with File-Based Systems

This approach leads to serious issues:

| Problem | Description |
|---------|-------------|
| **Data Redundancy** | Same data stored in multiple places |
| **Data Inconsistency** | Different versions disagree |
| **Difficult Updates** | Change one file, forget another |
| **No Data Sharing** | Files locked to one user |
| **Security Issues** | Hard to control who sees what |
| **Lack of Standards** | Everyone uses different formats |

```{admonition} Real-World Example
:class: tip

A customer changes their phone number. In a file-based system, you might need to update 5 different files across 3 departments. Miss one, and you have inconsistent data!
```

---

## The Solution: Databases

A **database** is an organized collection of structured data, stored and accessed electronically.

But more importantly, a **Database Management System (DBMS)** is software that:
- Stores data in an organized way
- Allows multiple users to access data simultaneously
- Maintains data integrity and consistency
- Provides security controls
- Enables efficient searching and retrieval

### Popular Database Management Systems

| DBMS | Type | Common Uses |
|------|------|-------------|
| **PostgreSQL** | Relational | Web apps, analytics, GIS |
| **MySQL** | Relational | Web apps, WordPress |
| **Microsoft SQL Server** | Relational | Enterprise applications |
| **Oracle** | Relational | Large enterprises, banking |
| **MongoDB** | NoSQL (Document) | Flexible data, rapid development |
| **SQLite** | Relational (embedded) | Mobile apps, browsers |

```{note}
In this course, we use **PostgreSQL** because it's powerful, free, open-source, and follows SQL standards closely.
```

---

## Advantages of DBMS

### 1. Data Independence
Applications don't need to know how data is physically stored. Change the storage structure without breaking applications.

### 2. Efficient Data Access
DBMS use sophisticated techniques to store and retrieve data quickly, even with millions of records.

### 3. Data Integrity
Rules (constraints) ensure data accuracy:
- A price can't be negative
- An employee must have a department
- A transaction must have a valid customer

### 4. Concurrent Access
Multiple users can work with the same data simultaneously without conflicts.

### 5. Security
Control exactly who can see and modify what data:
- Cashiers can view prices but not change them
- Managers can update inventory
- Only HR can see salary information

### 6. Backup and Recovery
DBMS provide tools to back up data and recover from failures.

---

## Database Terminology

Let's establish some vocabulary:

**Database (DB)**
: A collection of related data organized for efficient retrieval

**Database Management System (DBMS)**
: Software that manages databases (PostgreSQL, MySQL, etc.)

**Schema**
: The structure or design of a database (tables, columns, relationships)

**Table** (or Relation)
: A collection of related data organized in rows and columns

**Row** (or Record, Tuple)
: A single entry in a table (one customer, one product, one transaction)

**Column** (or Field, Attribute)
: A single piece of information about each record (name, price, date)

**Query**
: A request for data from a database (using SQL)

---

## Databases in Everyday Life

You interact with databases constantly:

- **Banking** - Your account balance, transaction history
- **Shopping** - Product catalogs, inventory, orders
- **Social Media** - Posts, friends, likes, comments
- **Healthcare** - Medical records, appointments, prescriptions
- **Education** - Student records, grades, course registrations
- **Streaming** - Movies, playlists, watch history

```{admonition} Think About It
:class: tip

How many databases have you interacted with today? Your email, your phone contacts, any website you visited, your transit card, your coffee shop loyalty app...
```

---

## Types of Databases

### Relational Databases
- Organize data into **tables** with rows and columns
- Tables are connected through **relationships**
- Use **SQL** (Structured Query Language)
- Examples: PostgreSQL, MySQL, Oracle

### NoSQL Databases
- Don't use traditional table structure
- Better for unstructured or rapidly changing data
- Various types: document, key-value, graph, column-family
- Examples: MongoDB, Redis, Neo4j

### In This Course
We focus on **relational databases** because:
- They're the most widely used in business
- SQL skills are highly valued
- Understanding relational concepts helps you learn other types
- PostgreSQL is free and professional-grade

---

## Summary

- Databases solve the problems of file-based data storage
- A DBMS provides organized storage, security, and efficient access
- Databases are everywhere in modern life
- This course focuses on relational databases and PostgreSQL

---

**Next:** [Setting Up PostgreSQL](setup-postgresql.md)
