# Creating and Modifying Tables

```{admonition} Chapter Status
:class: warning
This chapter is under development. Full content coming soon!
```

## Learning Objectives

- Write CREATE TABLE statements
- Define data types appropriate for PostgreSQL
- Implement constraints (PRIMARY KEY, FOREIGN KEY, NOT NULL, UNIQUE, CHECK)
- Modify tables with ALTER TABLE
- Safely remove tables with DROP TABLE

## Preview: DDL Commands

```sql
-- Creating a table
CREATE TABLE customers (
    customer_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Adding a column
ALTER TABLE customers ADD COLUMN phone VARCHAR(20);

-- Adding a foreign key
ALTER TABLE orders 
ADD CONSTRAINT fk_customer 
FOREIGN KEY (customer_id) REFERENCES customers(customer_id);

-- Dropping a table
DROP TABLE IF EXISTS old_customers;
```

---

**Next:** [Hands-On DDL Practice](../notebooks/06-ddl-practice.ipynb)
