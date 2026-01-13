# Entity-Relationship Modeling

```{admonition} Chapter Status
:class: warning
This chapter is under development. Content coming soon!
```

## Learning Objectives

By the end of this chapter, you will be able to:

- Explain the purpose of data modeling
- Identify entities, attributes, and relationships
- Draw Entity-Relationship Diagrams (ERDs) using Crow's Foot notation
- Distinguish between cardinality types (1:1, 1:M, M:N)
- Convert business requirements into ER models

## What is an Entity-Relationship Diagram?

An **Entity-Relationship Diagram (ERD)** is a visual representation of the data structure for a database. It shows:

- **Entities**: Things we want to store data about (like Customers, Products, Orders)
- **Attributes**: Properties of entities (like customer_name, product_price)
- **Relationships**: How entities are connected (like "Customer places Order")

```{figure} ../images/sample-erd.png
:name: sample-erd
:alt: Sample ERD showing customers, orders, and products

A simple ERD showing the relationship between Customers, Orders, and Products.
```

## Cardinality and Participation

Understanding how entities relate to each other:

| Cardinality | Meaning | Example |
|-------------|---------|---------|
| 1:1 | One to One | Person - Passport |
| 1:M | One to Many | Customer - Orders |
| M:N | Many to Many | Students - Courses |

## Readings

For this chapter, please review:

- Rob - Chapter 4 (ER Modeling)
- Vidhya - Chapter 2 (Data Models)
- Visual Paradigm Guide to ERD

## Exercises

```{exercise}
:label: ex-erd-1

Draw an ERD for a library system that tracks:
- Books (with ISBN, title, author, publication year)
- Members (with member ID, name, address, phone)
- Loans (tracking which member borrowed which book and when)
```

---

**Next:** [Database Normalization](04-normalization.md)
