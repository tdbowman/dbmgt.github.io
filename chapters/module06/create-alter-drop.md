# CREATE, ALTER, DROP

## CREATE TABLE

### Basic Syntax
```sql
CREATE TABLE table_name (
    column1 datatype constraints,
    column2 datatype constraints,
    ...
    table_constraints
);
```

### Simple Example
```sql
CREATE TABLE customers (
    customer_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### Complete Example with All Constraint Types
```sql
CREATE TABLE products (
    product_id SERIAL,
    name VARCHAR(100) NOT NULL,
    description TEXT,
    category_id INTEGER NOT NULL,
    price DECIMAL(10,2) NOT NULL,
    stock_quantity INTEGER DEFAULT 0,
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    
    -- Table-level constraints
    CONSTRAINT pk_products PRIMARY KEY (product_id),
    CONSTRAINT fk_category FOREIGN KEY (category_id) 
        REFERENCES categories(category_id)
        ON DELETE RESTRICT
        ON UPDATE CASCADE,
    CONSTRAINT chk_price CHECK (price >= 0),
    CONSTRAINT chk_stock CHECK (stock_quantity >= 0)
);
```

### CREATE TABLE IF NOT EXISTS
Prevents errors if table already exists:
```sql
CREATE TABLE IF NOT EXISTS customers (
    customer_id SERIAL PRIMARY KEY,
    name VARCHAR(100)
);
```

### CREATE TABLE AS (Copy Data)
Create a table from a query:
```sql
-- Copy structure and data
CREATE TABLE orders_2024 AS
SELECT * FROM orders WHERE order_date >= '2024-01-01';

-- Copy structure only (no data)
CREATE TABLE orders_backup AS
SELECT * FROM orders WHERE FALSE;
```

---

## Foreign Key Options

### ON DELETE Options

| Option | Behavior |
|--------|----------|
| `RESTRICT` | Prevent deletion (default) |
| `CASCADE` | Delete related rows |
| `SET NULL` | Set FK to NULL |
| `SET DEFAULT` | Set FK to default value |
| `NO ACTION` | Similar to RESTRICT |

### ON UPDATE Options
Same options as ON DELETE.

### Example
```sql
CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    customer_id INTEGER REFERENCES customers(customer_id)
        ON DELETE SET NULL
        ON UPDATE CASCADE
);
```

---

## ALTER TABLE

### Add a Column
```sql
ALTER TABLE customers 
ADD COLUMN phone VARCHAR(20);

-- With constraints
ALTER TABLE customers 
ADD COLUMN loyalty_points INTEGER DEFAULT 0 NOT NULL;
```

### Drop a Column
```sql
ALTER TABLE customers 
DROP COLUMN phone;

-- Force drop even if constraints exist
ALTER TABLE customers 
DROP COLUMN phone CASCADE;
```

### Rename a Column
```sql
ALTER TABLE customers 
RENAME COLUMN email TO email_address;
```

### Change Data Type
```sql
ALTER TABLE products 
ALTER COLUMN price TYPE NUMERIC(12,2);

-- With conversion
ALTER TABLE products 
ALTER COLUMN price TYPE INTEGER USING price::INTEGER;
```

### Add/Drop NOT NULL
```sql
-- Add NOT NULL
ALTER TABLE customers 
ALTER COLUMN phone SET NOT NULL;

-- Remove NOT NULL
ALTER TABLE customers 
ALTER COLUMN phone DROP NOT NULL;
```

### Add/Drop DEFAULT
```sql
-- Add default
ALTER TABLE customers 
ALTER COLUMN status SET DEFAULT 'active';

-- Remove default
ALTER TABLE customers 
ALTER COLUMN status DROP DEFAULT;
```

### Add Constraints
```sql
-- Add primary key
ALTER TABLE customers 
ADD CONSTRAINT pk_customers PRIMARY KEY (customer_id);

-- Add foreign key
ALTER TABLE orders 
ADD CONSTRAINT fk_customer 
FOREIGN KEY (customer_id) REFERENCES customers(customer_id);

-- Add unique constraint
ALTER TABLE products 
ADD CONSTRAINT uq_sku UNIQUE (sku);

-- Add check constraint
ALTER TABLE products 
ADD CONSTRAINT chk_positive_price CHECK (price > 0);
```

### Drop Constraints
```sql
ALTER TABLE products 
DROP CONSTRAINT chk_positive_price;
```

### Rename Table
```sql
ALTER TABLE customers 
RENAME TO clients;
```

---

## DROP TABLE

### Basic Drop
```sql
DROP TABLE customers;
```

### Drop If Exists
```sql
DROP TABLE IF EXISTS customers;
```

### Drop with Dependencies
```sql
-- Also drops dependent objects (views, FKs)
DROP TABLE customers CASCADE;

-- Refuse if dependencies exist (default)
DROP TABLE customers RESTRICT;
```

### Drop Multiple Tables
```sql
DROP TABLE orders, order_items, customers;
```

---

## TRUNCATE

Quickly remove ALL data from a table (faster than DELETE):

```sql
TRUNCATE TABLE orders;

-- Reset auto-increment counter
TRUNCATE TABLE orders RESTART IDENTITY;

-- With foreign key tables
TRUNCATE TABLE orders CASCADE;
```

### TRUNCATE vs DELETE

| Aspect | TRUNCATE | DELETE |
|--------|----------|--------|
| Speed | Very fast | Slower |
| WHERE clause | No | Yes |
| Triggers | No | Yes |
| Rollback | PostgreSQL: Yes | Yes |
| Reset identity | Optional | No |

---

## Practical Examples

### Create a Complete Schema
```sql
-- Categories table
CREATE TABLE categories (
    category_id SERIAL PRIMARY KEY,
    name VARCHAR(50) NOT NULL UNIQUE,
    description TEXT
);

-- Products table
CREATE TABLE products (
    product_id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    category_id INTEGER REFERENCES categories(category_id),
    price DECIMAL(10,2) NOT NULL CHECK (price > 0),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Customers table
CREATE TABLE customers (
    customer_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL
);

-- Orders table
CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    customer_id INTEGER REFERENCES customers(customer_id),
    order_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    total DECIMAL(10,2)
);

-- Order items (junction table)
CREATE TABLE order_items (
    order_id INTEGER REFERENCES orders(order_id) ON DELETE CASCADE,
    product_id INTEGER REFERENCES products(product_id),
    quantity INTEGER NOT NULL CHECK (quantity > 0),
    unit_price DECIMAL(10,2) NOT NULL,
    PRIMARY KEY (order_id, product_id)
);
```

### Modify Existing Table
```sql
-- Add phone to customers
ALTER TABLE customers ADD COLUMN phone VARCHAR(20);

-- Make it required for new records
ALTER TABLE customers ALTER COLUMN phone SET DEFAULT '';
ALTER TABLE customers ALTER COLUMN phone SET NOT NULL;

-- Add validation
ALTER TABLE customers 
ADD CONSTRAINT chk_phone_format 
CHECK (phone ~ '^\+?[0-9\-\s]+$');
```

---

## Summary

| Command | Purpose | Key Options |
|---------|---------|-------------|
| `CREATE TABLE` | Make new table | `IF NOT EXISTS`, `AS` |
| `ALTER TABLE` | Modify table | `ADD`, `DROP`, `ALTER`, `RENAME` |
| `DROP TABLE` | Delete table | `IF EXISTS`, `CASCADE` |
| `TRUNCATE` | Remove all data | `RESTART IDENTITY`, `CASCADE` |

---

**Next Module:** [Advanced SQL](../module07/overview.md)
