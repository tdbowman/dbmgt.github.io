# Python and PostgreSQL

## Connecting Python to Databases

Python can connect to PostgreSQL using several libraries:

| Library | Description |
|---------|-------------|
| **psycopg2** | Most popular, low-level |
| **SQLAlchemy** | ORM with high-level features |
| **asyncpg** | Async support |

---

## psycopg2 Basics

### Installation

```bash
pip install psycopg2-binary
```

### Connecting

```python
import psycopg2

# Connect to database
conn = psycopg2.connect(
    host="localhost",
    database="coffee_shop_db",
    user="postgres",
    password="yourpassword"
)

# Create cursor
cur = conn.cursor()
```

### Executing Queries

```python
# Simple query
cur.execute("SELECT * FROM products")
products = cur.fetchall()

for product in products:
    print(product)
```

### Fetching Results

```python
# Fetch one row
cur.execute("SELECT * FROM products WHERE product_id = 1")
product = cur.fetchone()

# Fetch all rows
cur.execute("SELECT * FROM products")
all_products = cur.fetchall()

# Fetch many rows
cur.execute("SELECT * FROM products")
some_products = cur.fetchmany(10)
```

### Parameterized Queries (IMPORTANT!)

**Never use string formatting—it's vulnerable to SQL injection!**

```python
# WRONG - SQL Injection risk!
cur.execute(f"SELECT * FROM products WHERE name = '{user_input}'")

# CORRECT - Use parameters
cur.execute("SELECT * FROM products WHERE name = %s", (user_input,))
```

### Insert, Update, Delete

```python
# Insert
cur.execute(
    "INSERT INTO products (name, price) VALUES (%s, %s)",
    ("New Coffee", 4.50)
)
conn.commit()  # Don't forget!

# Update
cur.execute(
    "UPDATE products SET price = %s WHERE product_id = %s",
    (5.00, 1)
)
conn.commit()

# Delete
cur.execute("DELETE FROM products WHERE product_id = %s", (1,))
conn.commit()
```

### Closing Connections

```python
cur.close()
conn.close()

# Or use context managers
with psycopg2.connect(...) as conn:
    with conn.cursor() as cur:
        cur.execute("SELECT * FROM products")
        # Connection closes automatically
```

---

## SQLAlchemy

SQLAlchemy provides both raw SQL and ORM capabilities.

### Installation

```bash
pip install sqlalchemy psycopg2-binary
```

### Engine and Connection

```python
from sqlalchemy import create_engine, text

# Create engine
engine = create_engine("postgresql://postgres:password@localhost/coffee_shop_db")

# Execute query
with engine.connect() as conn:
    result = conn.execute(text("SELECT * FROM products"))
    for row in result:
        print(row)
```

### ORM Basics

```python
from sqlalchemy import Column, Integer, String, Numeric
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

Base = declarative_base()

# Define model
class Product(Base):
    __tablename__ = 'products'
    
    product_id = Column(Integer, primary_key=True)
    name = Column(String(100))
    price = Column(Numeric(10, 2))

# Create session
Session = sessionmaker(bind=engine)
session = Session()

# Query
products = session.query(Product).filter(Product.price > 5).all()

for p in products:
    print(f"{p.name}: ${p.price}")

# Insert
new_product = Product(name="Espresso", price=3.50)
session.add(new_product)
session.commit()
```

---

## Practical Example: Product Manager

```python
import psycopg2
from contextlib import contextmanager

@contextmanager
def get_db_connection():
    conn = psycopg2.connect(
        host="localhost",
        database="coffee_shop_db",
        user="postgres",
        password="yourpassword"
    )
    try:
        yield conn
    finally:
        conn.close()

def get_all_products():
    with get_db_connection() as conn:
        with conn.cursor() as cur:
            cur.execute("SELECT product_id, name, price FROM products")
            return cur.fetchall()

def get_product(product_id):
    with get_db_connection() as conn:
        with conn.cursor() as cur:
            cur.execute(
                "SELECT * FROM products WHERE product_id = %s",
                (product_id,)
            )
            return cur.fetchone()

def add_product(name, price):
    with get_db_connection() as conn:
        with conn.cursor() as cur:
            cur.execute(
                "INSERT INTO products (name, price) VALUES (%s, %s) RETURNING product_id",
                (name, price)
            )
            product_id = cur.fetchone()[0]
            conn.commit()
            return product_id

def update_product(product_id, name=None, price=None):
    with get_db_connection() as conn:
        with conn.cursor() as cur:
            if name:
                cur.execute(
                    "UPDATE products SET name = %s WHERE product_id = %s",
                    (name, product_id)
                )
            if price:
                cur.execute(
                    "UPDATE products SET price = %s WHERE product_id = %s",
                    (price, product_id)
                )
            conn.commit()

def delete_product(product_id):
    with get_db_connection() as conn:
        with conn.cursor() as cur:
            cur.execute(
                "DELETE FROM products WHERE product_id = %s",
                (product_id,)
            )
            conn.commit()

# Usage
if __name__ == "__main__":
    # List products
    for product in get_all_products():
        print(product)
    
    # Add a product
    new_id = add_product("Latte", 4.75)
    print(f"Added product with ID: {new_id}")
```

---

## Error Handling

```python
import psycopg2
from psycopg2 import OperationalError, IntegrityError

try:
    conn = psycopg2.connect(...)
    cur = conn.cursor()
    cur.execute("INSERT INTO products (name, price) VALUES (%s, %s)", ("Test", 1.00))
    conn.commit()
except OperationalError as e:
    print(f"Connection error: {e}")
except IntegrityError as e:
    print(f"Data error: {e}")
    conn.rollback()
finally:
    if cur:
        cur.close()
    if conn:
        conn.close()
```

---

## Summary

| Library | Best For |
|---------|----------|
| psycopg2 | Direct SQL, full control |
| SQLAlchemy Core | SQL with Python conveniences |
| SQLAlchemy ORM | Object-oriented database access |

**Key Points:**
- Always use parameterized queries
- Close connections when done
- Handle errors appropriately
- Use context managers

---

**Next:** [Flask Web Framework](flask-basics.md)
