# Drawing ER Diagrams

## ER Diagram Notations

Several notations exist for ER diagrams. We'll focus on **Crow's Foot notation** because it's:
- Widely used in industry
- Supported by most database tools
- Clear and intuitive once learned

---

## Crow's Foot Notation

### Entities

Entities are represented as **rectangles** with the entity name at the top.

```
┌─────────────┐
│  CUSTOMER   │
├─────────────┤
│ customer_id │ ← Primary Key
│ first_name  │
│ last_name   │
│ email       │
└─────────────┘
```

### Relationships

Relationships are shown as **lines** connecting entities, with symbols at each end showing cardinality.

### Cardinality Symbols

| Symbol | Meaning |
|--------|---------|
| `──────` (single line) | One (and only one) |
| `──────<` (crow's foot) | Many |
| `──○───` (circle) | Zero (optional) |
| `──│───` (vertical bar) | One (mandatory) |

### Combined Symbols

| Notation | Meaning |
|----------|---------|
| `───│──<` | One or many (1 to N, mandatory) |
| `───○──<` | Zero or many (0 to N, optional) |
| `───│──│` | One and only one (1 to 1, mandatory) |
| `───○──│` | Zero or one (0 to 1, optional) |

---

## Reading ER Diagrams

Read relationships from **each side**:

```
┌──────────┐          ┌──────────┐
│DEPARTMENT│──│─────○<│ EMPLOYEE │
└──────────┘          └──────────┘
```

Reading left to right: "A department has zero or many employees"
Reading right to left: "An employee belongs to one (and only one) department"

---

## Common Relationship Patterns

### One-to-Many (1:N)

Most common relationship type.

```
┌──────────┐          ┌──────────┐
│ CUSTOMER │──│─────○<│  ORDER   │
└──────────┘          └──────────┘
```

- One customer can have zero or many orders
- Each order belongs to exactly one customer

### One-to-One (1:1)

Rare, often indicates entities should be merged.

```
┌──────────┐          ┌──────────┐
│ EMPLOYEE │──│─────○││ PARKING  │
└──────────┘          └──────────┘
```

- Each employee has zero or one parking spot
- Each parking spot belongs to exactly one employee

### Many-to-Many (M:N)

Requires a **junction table** in implementation.

```
┌──────────┐          ┌───────────────┐          ┌──────────┐
│ STUDENT  │──│─────○<│  ENROLLMENT   │>○─────│──│  COURSE  │
└──────────┘          ├───────────────┤          └──────────┘
                      │ student_id FK │
                      │ course_id FK  │
                      │ grade         │
                      │ semester      │
                      └───────────────┘
```

The junction table (ENROLLMENT):
- Resolves the M:N relationship
- Can hold additional attributes (grade, semester)
- Has foreign keys to both entities

---

## Primary and Foreign Keys in Diagrams

**Primary Key (PK):** Usually indicated by:
- Bold text
- Underline
- "PK" label
- Key icon

**Foreign Key (FK):** Usually indicated by:
- "FK" label
- Different color or style

```
┌─────────────────────┐
│     ORDER           │
├─────────────────────┤
│ PK  order_id        │
│ FK  customer_id     │
│     order_date      │
│     total_amount    │
└─────────────────────┘
```

---

## Example: Coffee Shop ER Diagram

Let's design a database for our coffee shop:

```
┌─────────────┐          ┌─────────────┐
│   STORE     │──│─────○<│  EMPLOYEE   │
├─────────────┤          ├─────────────┤
│PK store_id  │          │PK emp_id    │
│  address    │          │  first_name │
│  city       │          │  last_name  │
│  phone      │          │FK store_id  │
└─────────────┘          │  hire_date  │
                         └─────────────┘

┌─────────────┐          ┌─────────────┐
│  CATEGORY   │──│─────○<│  PRODUCT    │
├─────────────┤          ├─────────────┤
│PK cat_id    │          │PK product_id│
│  cat_name   │          │  name       │
│  description│          │  price      │
└─────────────┘          │FK cat_id    │
                         └─────────────┘

┌─────────────┐          ┌─────────────┐          ┌─────────────┐
│  CUSTOMER   │──│─────○<│ TRANSACTION │>○─────│──│  EMPLOYEE   │
├─────────────┤          ├─────────────┤          └─────────────┘
│PK cust_id   │          │PK trans_id  │
│  name       │          │FK cust_id   │
│  email      │          │FK emp_id    │
│  loyalty_num│          │  trans_date │
└─────────────┘          │  total      │
                         └─────────────┘

┌─────────────┐          ┌─────────────┐          ┌─────────────┐
│ TRANSACTION │──│─────○<│ TRANS_ITEM  │>○─────│──│  PRODUCT    │
└─────────────┘          ├─────────────┤          └─────────────┘
                         │FK trans_id  │
                         │FK product_id│
                         │  quantity   │
                         │  price      │
                         └─────────────┘
```

---

## Tools for Creating ER Diagrams

| Tool | Type | Notes |
|------|------|-------|
| **DBeaver** | Database tool | Can generate from existing DB |
| **draw.io** | Free online | Good for learning |
| **Lucidchart** | Online (freemium) | Professional quality |
| **MySQL Workbench** | Database tool | MySQL-focused |
| **pgModeler** | PostgreSQL | Open source |
| **ERDPlus** | Free online | Education-focused |

---

## Best Practices

1. **Use consistent naming**
   - Singular entity names (CUSTOMER, not CUSTOMERS)
   - Lowercase with underscores for attributes
   - Clear, descriptive names

2. **Show only essential attributes**
   - Include PKs and FKs
   - Key business attributes
   - Don't clutter with every column

3. **Arrange logically**
   - Group related entities
   - Minimize crossing lines
   - Flow left-to-right or top-to-bottom

4. **Include a legend**
   - Explain notation used
   - Note any conventions

5. **Version your diagrams**
   - Date your diagrams
   - Track changes over time

---

## From ER Diagram to Tables

Each entity becomes a table:
1. Entity name → Table name
2. Attributes → Columns
3. Primary key → PRIMARY KEY constraint
4. Relationships → FOREIGN KEY constraints
5. M:N relationships → Junction tables

---

## Summary

- ER diagrams visualize database structure
- Crow's foot notation shows cardinality clearly
- Entities become tables; relationships become foreign keys
- Many-to-many needs junction tables
- Use tools like DBeaver or draw.io

---

**Next Module:** [Database Normalization](../module04/overview.md)
