# Introduction to Databases

```{epigraph}
Data is the new oil.

-- Clive Humby, 2006
```

In today's information-driven world, databases are everywhere. Every time you check your bank balance, search for a book at the library, or scroll through social media, you're interacting with databases. This chapter introduces the fundamental concepts that will form the foundation for everything we learn in this course.

## Learning Objectives

By the end of this chapter, you will be able to:

- Define what a database is and explain its purpose
- Distinguish between data, information, and knowledge
- Identify the key components of a Database Management System (DBMS)
- Explain the advantages of using a DBMS over file-based systems
- Describe different types of database users and their roles

## What Is a Database?

:::{glossary}
Database
  A shared, integrated computer structure that stores a collection of **end-user data** (raw facts of interest to the end user) and **metadata** (data about data, describing the characteristics of the data).
:::

Think of a database as an organized digital filing cabinet. But unlike a physical cabinet, a database can:

- Store millions of records efficiently
- Find specific information in milliseconds
- Allow multiple users to access data simultaneously
- Maintain data integrity and security
- Handle complex relationships between different types of data

```{admonition} Real-World Example: Coffee Shop
:class: tip

Consider our sample coffee shop data. A single transaction involves:
- **Customer information** (name, email, loyalty card)
- **Product details** (name, category, price)
- **Staff information** (who made the sale)
- **Store location** (address, neighborhood)
- **Transaction details** (date, time, quantity)

A database organizes all these related pieces of information so we can answer questions like "What's our best-selling product in Manhattan?" or "Which employee processed the most sales last month?"
```

## Data vs. Information vs. Knowledge

Understanding the hierarchy of data is crucial:

```{mermaid}
flowchart BT
    A[Data] --> B[Information]
    B --> C[Knowledge]
    C --> D[Wisdom]
    
    style A fill:#e1f5fe
    style B fill:#b3e5fc
    style C fill:#81d4fa
    style D fill:#4fc3f7
```

Data
: Raw facts without context. Example: `3.75`, `Latte`, `2019-04-13`

Information
: Data processed to be meaningful. Example: "A Latte sold for $3.75 on April 13, 2019"

Knowledge
: Information combined with experience and judgment. Example: "Latte sales increase 40% on cold mornings—we should stock more milk in winter"

Wisdom
: The ability to apply knowledge appropriately. Example: Knowing when to run a latte promotion vs. when to introduce a new seasonal drink

## The Database Management System (DBMS)

A **Database Management System (DBMS)** is software that enables users to create, maintain, and access databases. Think of it as the intermediary between users and the stored data.

### Key DBMS Functions

::::{grid} 2

:::{grid-item-card} 📊 Data Definition
Create and modify database structure (tables, relationships, constraints)
:::

:::{grid-item-card} 🔍 Data Manipulation
Insert, update, delete, and query data
:::

:::{grid-item-card} 🔒 Security Management
Control who can access what data and what operations they can perform
:::

:::{grid-item-card} 🔄 Concurrency Control
Handle multiple users accessing data simultaneously
:::

:::{grid-item-card} 💾 Backup & Recovery
Protect data against loss and restore from failures
:::

:::{grid-item-card} 📈 Performance Optimization
Index data and optimize queries for fast retrieval
:::

::::

### Popular DBMS Examples

| DBMS | Type | Common Use Cases |
|------|------|-----------------|
| PostgreSQL | Relational | Web applications, GIS, analytics |
| MySQL | Relational | Web applications, WordPress |
| Oracle | Relational | Enterprise applications, banking |
| Microsoft SQL Server | Relational | Windows environments, business apps |
| MongoDB | Document | Content management, real-time analytics |
| Redis | Key-Value | Caching, session management |

```{note}
In this course, we focus on **PostgreSQL**—a powerful, open-source relational DBMS known for its standards compliance, extensibility, and strong community support.
```

## Why Use a DBMS? The File System Problems

Before databases, organizations stored data in flat files. This approach had serious limitations:

### Data Redundancy and Inconsistency

```{figure} ../images/redundancy-example.png
:name: redundancy
:alt: Data redundancy across files

Without a DBMS, the same customer information might be stored in multiple files, leading to inconsistencies.
```

When customer addresses are stored in separate files for Sales, Shipping, and Marketing, updating an address means changing it in multiple places. Miss one, and you have inconsistent data.

### Data Isolation

Data scattered across multiple files in different formats makes it difficult to write programs that retrieve appropriate data.

### Integrity Problems

Ensuring data accuracy (like "account balance must be positive") is difficult to implement and maintain across separate files.

### Atomicity Problems

Consider a bank transfer: debit one account and credit another. In a file system, if the system crashes between these operations, money could be "lost." Databases handle this with **transactions**.

### Security Problems

File systems have limited ability to control which users can access which data.

## The DBMS Advantage

```{admonition} DBMS Benefits
:class: important

1. **Data Independence**: Programs don't depend on physical data storage
2. **Efficient Data Access**: Sophisticated techniques for storing and retrieving data
3. **Data Integrity and Security**: Constraints and access controls
4. **Data Administration**: Centralized management
5. **Concurrent Access and Crash Recovery**: Multiple users, reliable transactions
6. **Reduced Application Development Time**: SQL provides a high-level interface
```

## Types of Database Users

Different people interact with databases in different ways:

```{mermaid}
mindmap
  root((Database<br/>Users))
    End Users
      Casual Users
      Parametric Users
      Sophisticated Users
    Application Programmers
    Database Administrators
      System Admin
      Data Admin
```

**End Users**
: People who use applications that access the database. They may not even know a database is involved.

**Application Programmers**
: Write programs that access the database using languages like Python, Java, or PHP.

**Database Administrators (DBAs)**
: Responsible for managing the database system, including security, performance, backup, and recovery.

## Historical Context

The evolution of database technology:

| Era | Technology | Characteristics |
|-----|-----------|-----------------|
| 1960s | File Systems | Flat files, application-dependent |
| 1970s | Hierarchical/Network | First true DBMS, complex navigation |
| 1980s | Relational | SQL, data independence (we are here!) |
| 1990s | Object-Oriented | Complex data types |
| 2000s | XML/Web | Internet integration |
| 2010s+ | NoSQL/NewSQL | Big data, distributed systems |

```{note}
While newer technologies exist, **relational databases remain dominant** for most business applications. Understanding them is essential before exploring alternatives.
```

## Summary

In this chapter, we learned that:

- A **database** is an organized collection of related data
- A **DBMS** provides tools to create, maintain, and access databases
- DBMSs solve critical problems with file-based systems: redundancy, inconsistency, isolation, integrity, and security
- Different **users** interact with databases at different levels
- **Relational databases** (using SQL) remain the industry standard

```{admonition} Key Terms
:class: seealso

- Database
- DBMS (Database Management System)
- Data, Information, Knowledge
- Data Redundancy
- Data Integrity
- Transaction
```

## Exercises

```{exercise}
:label: ex-intro-1

Identify three databases you interact with in your daily life. For each one:
1. What data do you think they store?
2. Who are the different types of users?
3. What problems might occur if they used a file-based system instead?
```

```{exercise}
:label: ex-intro-2

A small library currently tracks books using an Excel spreadsheet. What problems might they encounter? What advantages would a DBMS provide?
```

```{exercise}
:label: ex-intro-3

Consider our coffee shop dataset. List five questions a manager might want to answer using this data. What makes a database better suited than spreadsheets for answering these questions?
```

---

**Next:** [The Relational Data Model](02-relational-model.md)
