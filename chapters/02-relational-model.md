# The Relational Data Model

```{admonition} Chapter Status
:class: warning
This chapter is under development. Content coming soon!
```

## Learning Objectives

By the end of this chapter, you will be able to:

- Explain the fundamental concepts of the relational model
- Define relations, attributes, tuples, and domains
- Identify keys: primary, foreign, candidate, and surrogate
- Understand relational integrity rules
- Read and interpret database schemas

## Key Concepts Preview

### Relations and Tables

In the relational model, data is organized into **relations** (tables). Each relation consists of:

- **Attributes** (columns): Properties or characteristics
- **Tuples** (rows): Individual records
- **Domain**: Set of allowable values for an attribute

### Types of Keys

:::{glossary}
Primary Key
  A column (or combination of columns) that uniquely identifies each row in a table.

Foreign Key
  A column that references the primary key of another table, creating a relationship between tables.

Candidate Key
  Any column or combination of columns that could serve as the primary key.
:::

### Integrity Constraints

- **Entity Integrity**: Primary keys cannot be null
- **Referential Integrity**: Foreign key values must match existing primary keys (or be null)
- **Domain Integrity**: Values must be within defined domains

## Readings

For this chapter, please review:

- Watt - Chapter 7 (The Relational Data Model)
- Garcia-Molina - Chapter 1 (The Worlds of Database Systems)
- Rob - Chapter 3 (The Relational Database Model)
- Silberschatz - Chapter 2 (Introduction to the Relational Model)

---

**Next:** [Entity-Relationship Modeling](03-er-modeling.md)
