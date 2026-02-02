# Stored Procedures and Functions

## What Are They?

**Stored procedures** and **functions** are reusable code blocks stored in the database.

| Feature | Function | Procedure |
|---------|----------|-----------|
| Returns value | Yes (required) | Optional |
| Use in SELECT | Yes | No |
| Transaction control | Limited | Full (COMMIT/ROLLBACK) |
| Called with | SELECT | CALL |

---

## Creating Functions

### Basic Syntax

```sql
CREATE OR REPLACE FUNCTION function_name(parameters)
RETURNS return_type
LANGUAGE plpgsql
AS $$
BEGIN
    -- Code here
    RETURN value;
END;
$$;
```

### Simple Example

```sql
CREATE OR REPLACE FUNCTION get_product_count()
RETURNS INTEGER
LANGUAGE plpgsql
AS $$
DECLARE
    product_count INTEGER;
BEGIN
    SELECT COUNT(*) INTO product_count FROM products;
    RETURN product_count;
END;
$$;

-- Use it
SELECT get_product_count();
```

### Function with Parameters

```sql
CREATE OR REPLACE FUNCTION calculate_tax(price DECIMAL, tax_rate DECIMAL DEFAULT 0.08)
RETURNS DECIMAL
LANGUAGE plpgsql
AS $$
BEGIN
    RETURN price * tax_rate;
END;
$$;

-- Use it
SELECT calculate_tax(100);        -- Returns 8.00
SELECT calculate_tax(100, 0.10);  -- Returns 10.00
```

### Function Returning a Table

```sql
CREATE OR REPLACE FUNCTION get_expensive_products(min_price DECIMAL)
RETURNS TABLE(name VARCHAR, price DECIMAL)
LANGUAGE plpgsql
AS $$
BEGIN
    RETURN QUERY
    SELECT p.name, p.price
    FROM products p
    WHERE p.price >= min_price;
END;
$$;

-- Use it
SELECT * FROM get_expensive_products(50);
```

---

## Creating Procedures

```sql
CREATE OR REPLACE PROCEDURE transfer_funds(
    from_account INTEGER,
    to_account INTEGER,
    amount DECIMAL
)
LANGUAGE plpgsql
AS $$
BEGIN
    -- Deduct from source
    UPDATE accounts SET balance = balance - amount
    WHERE account_id = from_account;
    
    -- Add to destination
    UPDATE accounts SET balance = balance + amount
    WHERE account_id = to_account;
    
    COMMIT;
END;
$$;

-- Call it
CALL transfer_funds(1, 2, 100.00);
```

---

## Variables and Control Flow

### Declaring Variables

```sql
DECLARE
    customer_count INTEGER;
    total_sales DECIMAL(10,2) := 0;
    customer_name VARCHAR(100);
```

### IF Statements

```sql
IF condition THEN
    -- statements
ELSIF other_condition THEN
    -- statements
ELSE
    -- statements
END IF;
```

**Example:**
```sql
CREATE OR REPLACE FUNCTION get_discount(total DECIMAL)
RETURNS DECIMAL
LANGUAGE plpgsql
AS $$
BEGIN
    IF total >= 1000 THEN
        RETURN 0.15;
    ELSIF total >= 500 THEN
        RETURN 0.10;
    ELSIF total >= 100 THEN
        RETURN 0.05;
    ELSE
        RETURN 0;
    END IF;
END;
$$;
```

### CASE Statements

```sql
CASE expression
    WHEN value1 THEN result1
    WHEN value2 THEN result2
    ELSE default_result
END CASE;
```

### Loops

**Simple Loop:**
```sql
LOOP
    -- statements
    EXIT WHEN condition;
END LOOP;
```

**FOR Loop:**
```sql
FOR i IN 1..10 LOOP
    -- statements
END LOOP;
```

**FOR Loop with Query:**
```sql
FOR record IN SELECT * FROM products LOOP
    -- Use record.column_name
END LOOP;
```

**WHILE Loop:**
```sql
WHILE condition LOOP
    -- statements
END LOOP;
```

---

## Error Handling

```sql
CREATE OR REPLACE FUNCTION safe_divide(a DECIMAL, b DECIMAL)
RETURNS DECIMAL
LANGUAGE plpgsql
AS $$
BEGIN
    IF b = 0 THEN
        RAISE EXCEPTION 'Division by zero';
    END IF;
    RETURN a / b;
EXCEPTION
    WHEN division_by_zero THEN
        RETURN NULL;
    WHEN OTHERS THEN
        RAISE NOTICE 'Error: %', SQLERRM;
        RETURN NULL;
END;
$$;
```

---

## Practical Example

### Complete Order Processing Function

```sql
CREATE OR REPLACE FUNCTION process_order(
    p_customer_id INTEGER,
    p_product_id INTEGER,
    p_quantity INTEGER
)
RETURNS INTEGER  -- Returns order_id
LANGUAGE plpgsql
AS $$
DECLARE
    v_order_id INTEGER;
    v_price DECIMAL;
    v_stock INTEGER;
BEGIN
    -- Check stock
    SELECT stock, price INTO v_stock, v_price
    FROM products WHERE product_id = p_product_id;
    
    IF v_stock < p_quantity THEN
        RAISE EXCEPTION 'Insufficient stock: % available', v_stock;
    END IF;
    
    -- Create order
    INSERT INTO orders (customer_id, order_date, total)
    VALUES (p_customer_id, CURRENT_TIMESTAMP, v_price * p_quantity)
    RETURNING order_id INTO v_order_id;
    
    -- Add order item
    INSERT INTO order_items (order_id, product_id, quantity, price)
    VALUES (v_order_id, p_product_id, p_quantity, v_price);
    
    -- Update stock
    UPDATE products 
    SET stock = stock - p_quantity
    WHERE product_id = p_product_id;
    
    RETURN v_order_id;
END;
$$;

-- Use it
SELECT process_order(1, 101, 2);
```

---

## Managing Functions

```sql
-- List functions
\df

-- View function definition
\df+ function_name

-- Drop function
DROP FUNCTION function_name(parameter_types);
DROP FUNCTION IF EXISTS function_name(parameter_types);
```

---

## Summary

| Feature | Function | Procedure |
|---------|----------|-----------|
| Create | `CREATE FUNCTION` | `CREATE PROCEDURE` |
| Call | `SELECT func()` | `CALL proc()` |
| Return | Required | Optional |
| Use in queries | Yes | No |

**Key Elements:**
- `DECLARE` for variables
- `IF/ELSIF/ELSE` for conditions
- `LOOP/FOR/WHILE` for iteration
- `EXCEPTION` for error handling
- `RETURN` to send results

---

**Next Module:** [Cloud DBs & Open CMS](../module09/overview.md)
