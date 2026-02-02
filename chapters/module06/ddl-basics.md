# DDL Basics

## What is DDL?

**DDL (Data Definition Language)** commands define and modify database structures:

| Command | Purpose |
|---------|---------|
| `CREATE` | Make new objects (tables, databases, indexes) |
| `ALTER` | Modify existing objects |
| `DROP` | Delete objects |
| `TRUNCATE` | Remove all data from a table |

---

## PostgreSQL Data Types

### Numeric Types

| Type | Description | Range/Size |
|------|-------------|------------|
| `SMALLINT` | Small integer | -32,768 to 32,767 |
| `INTEGER` | Standard integer | -2.1 billion to 2.1 billion |
| `BIGINT` | Large integer | Very large numbers |
| `DECIMAL(p,s)` | Exact decimal | p total digits, s after decimal |
| `NUMERIC(p,s)` | Same as DECIMAL | Alias |
| `REAL` | 6 decimal precision | Approximate |
| `DOUBLE PRECISION` | 15 decimal precision | Approximate |
| `SERIAL` | Auto-increment int | 1 to 2.1 billion |
| `BIGSERIAL` | Auto-increment bigint | Very large |

```sql
price DECIMAL(10,2)  -- Up to 99999999.99
quantity INTEGER
id SERIAL            -- Auto-incrementing
```

### Character Types

| Type | Description |
|------|-------------|
| `CHAR(n)` | Fixed-length, padded with spaces |
| `VARCHAR(n)` | Variable-length, max n characters |
| `TEXT` | Unlimited length |

```sql
state CHAR(2)        -- Always 2 characters
email VARCHAR(255)   -- Up to 255 characters
description TEXT     -- Any length
```

### Date/Time Types

| Type | Description | Example |
|------|-------------|---------|
| `DATE` | Date only | '2024-01-15' |
| `TIME` | Time only | '14:30:00' |
| `TIMESTAMP` | Date and time | '2024-01-15 14:30:00' |
| `TIMESTAMPTZ` | With timezone | '2024-01-15 14:30:00-05' |
| `INTERVAL` | Time span | '1 day', '2 hours' |

```sql
birth_date DATE
created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
```

### Boolean Type

| Type | Values |
|------|--------|
| `BOOLEAN` | TRUE, FALSE, NULL |

```sql
is_active BOOLEAN DEFAULT TRUE
```

### Other Useful Types

| Type | Description |
|------|-------------|
| `UUID` | Universally unique identifier |
| `JSON` | JSON data |
| `JSONB` | Binary JSON (faster) |
| `ARRAY` | Array of values |

---

## Constraints

Constraints enforce rules on data:

### NOT NULL
Column must have a value.
```sql
email VARCHAR(255) NOT NULL
```

### UNIQUE
Values must be unique across all rows.
```sql
email VARCHAR(255) UNIQUE
```

### PRIMARY KEY
Uniquely identifies each row (NOT NULL + UNIQUE).
```sql
customer_id SERIAL PRIMARY KEY
```

### FOREIGN KEY
References a primary key in another table.
```sql
customer_id INTEGER REFERENCES customers(customer_id)
```

### CHECK
Custom validation rule.
```sql
price DECIMAL(10,2) CHECK (price > 0)
age INTEGER CHECK (age >= 0 AND age <= 150)
```

### DEFAULT
Automatic value when none provided.
```sql
created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
status VARCHAR(20) DEFAULT 'pending'
```

---

## Constraint Naming

Name your constraints for clearer error messages:

```sql
CREATE TABLE products (
    product_id SERIAL,
    name VARCHAR(100) NOT NULL,
    price DECIMAL(10,2),
    
    CONSTRAINT pk_products PRIMARY KEY (product_id),
    CONSTRAINT chk_price_positive CHECK (price > 0),
    CONSTRAINT uq_product_name UNIQUE (name)
);
```

---

## Creating Databases

### Create a Database
```sql
CREATE DATABASE coffee_shop;
```

### Create with Options
```sql
CREATE DATABASE coffee_shop
    ENCODING 'UTF8'
    LC_COLLATE 'en_US.UTF-8'
    LC_CTYPE 'en_US.UTF-8';
```

### Connect to Database
```sql
\c coffee_shop
```

### Drop a Database
```sql
DROP DATABASE coffee_shop;
```

---

## Creating Schemas

Schemas organize objects within a database:

```sql
-- Create schema
CREATE SCHEMA sales;

-- Create table in schema
CREATE TABLE sales.orders (
    order_id SERIAL PRIMARY KEY,
    total DECIMAL(10,2)
);

-- Query with schema
SELECT * FROM sales.orders;
```

---

## Summary

- DDL defines database structure
- Choose appropriate data types for your data
- Use constraints to enforce data integrity
- Name constraints for clearer error messages

---

**Next:** [CREATE, ALTER, DROP](create-alter-drop.md)
