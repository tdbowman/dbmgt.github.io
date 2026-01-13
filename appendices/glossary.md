# Glossary

Key terms and definitions used throughout this textbook.

```{glossary}
Attribute
  A column in a relation (table) that represents a property or characteristic of an entity. Also called a field.

ACID
  A set of properties that guarantee database transactions are processed reliably: **A**tomicity, **C**onsistency, **I**solation, **D**urability.

Atomicity
  The property that ensures a transaction is treated as a single, indivisible unit—either all operations complete successfully, or none do.

Boyce-Codd Normal Form (BCNF)
  A stricter version of Third Normal Form where every determinant is a candidate key.

Candidate Key
  A column or combination of columns that could uniquely identify rows in a table. One candidate key is chosen as the primary key.

Cardinality
  The number of elements in a set; in databases, often refers to the number of rows in a table or the nature of relationships (1:1, 1:M, M:N).

Composite Key
  A primary key consisting of two or more columns.

Concurrency
  The ability of a database to handle multiple transactions simultaneously.

Constraint
  A rule that restricts the values that can be stored in a table (e.g., NOT NULL, UNIQUE, CHECK).

Data Definition Language (DDL)
  SQL commands used to define and modify database structure (CREATE, ALTER, DROP).

Data Manipulation Language (DML)
  SQL commands used to query and modify data (SELECT, INSERT, UPDATE, DELETE).

Database
  An organized collection of structured data, typically stored and accessed electronically.

Database Management System (DBMS)
  Software that provides an interface for users and applications to interact with databases.

Denormalization
  The process of intentionally adding redundancy to a database to improve read performance.

Entity
  A real-world object or concept that can be distinctly identified; represented as a table in a relational database.

Entity-Relationship Diagram (ERD)
  A visual representation of entities and their relationships, used in database design.

First Normal Form (1NF)
  A table is in 1NF if all columns contain atomic values, there are no repeating groups, and each row is unique.

Foreign Key
  A column that references the primary key of another table, establishing a relationship between tables.

Functional Dependency
  A relationship where one attribute uniquely determines another (A → B).

Index
  A data structure that improves the speed of data retrieval operations on a database table.

Inner Join
  A join that returns only rows where there is a match in both tables.

Integrity
  The accuracy and consistency of data throughout its lifecycle.

Join
  An operation that combines rows from two or more tables based on related columns.

Metadata
  Data about data—information that describes the structure, properties, and characteristics of the stored data.

Normalization
  The process of organizing data to reduce redundancy and improve data integrity.

Null
  A special marker indicating that a data value does not exist or is unknown.

Outer Join
  A join that returns all rows from one or both tables, with NULL values where there is no match.

Partial Dependency
  When a non-key attribute depends on only part of a composite primary key (violates 2NF).

Primary Key
  A column or combination of columns that uniquely identifies each row in a table.

Query
  A request for data from a database, typically written in SQL.

Redundancy
  Unnecessary duplication of data within a database.

Relation
  A table in a relational database, consisting of rows (tuples) and columns (attributes).

Relational Database
  A database organized according to the relational model, where data is stored in tables with relationships between them.

Schema
  The structure or organization of a database, including tables, columns, data types, and relationships.

Second Normal Form (2NF)
  A table is in 2NF if it is in 1NF and has no partial dependencies.

SQL (Structured Query Language)
  The standard programming language for managing and manipulating relational databases.

Stored Procedure
  A prepared SQL code that can be saved and reused, often accepting parameters.

Subquery
  A query nested inside another query.

Surrogate Key
  An artificial key, typically an auto-generated number, used as a primary key.

Third Normal Form (3NF)
  A table is in 3NF if it is in 2NF and has no transitive dependencies.

Transaction
  A sequence of database operations that are treated as a single logical unit of work.

Transitive Dependency
  When a non-key attribute depends on another non-key attribute (violates 3NF).

Trigger
  A stored procedure that automatically executes in response to certain events on a table.

Tuple
  A row in a relation (table); a single record.

View
  A virtual table based on the result of a stored query.
```
