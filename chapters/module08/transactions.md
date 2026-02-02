# Transactions

## What is a Transaction?

A **transaction** is a group of operations that must succeed or fail together.

**Example:** Transferring money between accounts
1. Subtract $100 from Account A
2. Add $100 to Account B

Both must succeed, or neither should happen.

---

## ACID Properties

Transactions guarantee ACID:

### Atomicity
"All or nothing" - Either all operations complete, or none do.

### Consistency  
Database moves from one valid state to another. Rules are never violated.

### Isolation
Concurrent transactions don't interfere with each other.

### Durability
Once committed, changes survive crashes and power failures.

---

## Transaction Commands

### BEGIN / START TRANSACTION
Start a transaction block:
```sql
BEGIN;
-- or
START TRANSACTION;
```

### COMMIT
Save all changes permanently:
```sql
COMMIT;
```

### ROLLBACK
Undo all changes since BEGIN:
```sql
ROLLBACK;
```

---

## Basic Example

```sql
BEGIN;

UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;
UPDATE accounts SET balance = balance + 100 WHERE account_id = 2;

-- Check the results
SELECT * FROM accounts WHERE account_id IN (1, 2);

-- If everything looks good:
COMMIT;

-- If something went wrong:
-- ROLLBACK;
```

---

## Auto-commit Mode

By default, PostgreSQL runs in **auto-commit** mode—each statement is its own transaction.

```sql
-- These are two separate transactions:
UPDATE products SET price = 10 WHERE id = 1;  -- Committed immediately
UPDATE products SET price = 20 WHERE id = 2;  -- Committed immediately
```

Use `BEGIN` to group statements:
```sql
BEGIN;
UPDATE products SET price = 10 WHERE id = 1;
UPDATE products SET price = 20 WHERE id = 2;
COMMIT;  -- Both committed together
```

---

## SAVEPOINT

Create checkpoints within a transaction:

```sql
BEGIN;

UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;
SAVEPOINT after_debit;

UPDATE accounts SET balance = balance + 100 WHERE account_id = 2;
-- Oops, wrong account!

ROLLBACK TO SAVEPOINT after_debit;
-- Now account 2 update is undone, but account 1 update remains

UPDATE accounts SET balance = balance + 100 WHERE account_id = 3;
COMMIT;
```

---

## Isolation Levels

Control how transactions see concurrent changes:

| Level | Dirty Read | Non-repeatable Read | Phantom Read |
|-------|------------|---------------------|--------------|
| Read Uncommitted | Possible | Possible | Possible |
| Read Committed | No | Possible | Possible |
| Repeatable Read | No | No | Possible |
| Serializable | No | No | No |

**PostgreSQL default:** Read Committed

### Setting Isolation Level

```sql
BEGIN;
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
-- ... operations ...
COMMIT;
```

### Common Issues

**Dirty Read:** Reading uncommitted changes from another transaction
**Non-repeatable Read:** Same query returns different results within one transaction
**Phantom Read:** New rows appear that match your query criteria

---

## Practical Example: Order Processing

```sql
BEGIN;

-- Create the order
INSERT INTO orders (customer_id, order_date, total)
VALUES (1, CURRENT_TIMESTAMP, 0)
RETURNING order_id INTO @order_id;

-- Add items
INSERT INTO order_items (order_id, product_id, quantity, price)
VALUES 
    (@order_id, 101, 2, 10.00),
    (@order_id, 102, 1, 15.00);

-- Update inventory
UPDATE products SET stock = stock - 2 WHERE product_id = 101;
UPDATE products SET stock = stock - 1 WHERE product_id = 102;

-- Calculate total
UPDATE orders 
SET total = (
    SELECT SUM(quantity * price) 
    FROM order_items 
    WHERE order_id = @order_id
)
WHERE order_id = @order_id;

-- Verify stock isn't negative
DO $$
BEGIN
    IF EXISTS (SELECT 1 FROM products WHERE stock < 0) THEN
        RAISE EXCEPTION 'Insufficient inventory';
    END IF;
END $$;

COMMIT;
```

---

## Error Handling

If an error occurs, the transaction is aborted:

```sql
BEGIN;
UPDATE products SET price = 10 WHERE id = 1;
UPDATE products SET price = 'invalid';  -- Error!
COMMIT;  -- Won't work - transaction is aborted
```

You must ROLLBACK before continuing:
```sql
ROLLBACK;
```

---

## Deadlocks

When two transactions wait for each other:

**Transaction A:**
```sql
BEGIN;
UPDATE accounts SET balance = 100 WHERE id = 1;  -- Locks row 1
UPDATE accounts SET balance = 200 WHERE id = 2;  -- Waits for B
```

**Transaction B:**
```sql
BEGIN;
UPDATE accounts SET balance = 300 WHERE id = 2;  -- Locks row 2
UPDATE accounts SET balance = 400 WHERE id = 1;  -- Waits for A (DEADLOCK!)
```

PostgreSQL detects and terminates one transaction.

**Prevention:** Always access rows in the same order.

---

## Summary

| Command | Purpose |
|---------|---------|
| `BEGIN` | Start transaction |
| `COMMIT` | Save changes |
| `ROLLBACK` | Undo changes |
| `SAVEPOINT` | Create checkpoint |
| `ROLLBACK TO` | Return to checkpoint |

**ACID:** Atomicity, Consistency, Isolation, Durability

---

**Next:** [Stored Procedures](stored-procedures.md)
