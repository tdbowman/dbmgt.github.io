# Business Intelligence

## What is Business Intelligence?

**Business Intelligence (BI)** transforms raw data into actionable insights for better decision-making.

Components of BI:
- Data collection and storage
- Data analysis and mining
- Reporting and visualization
- Decision support

---

## OLTP vs OLAP

### OLTP (Online Transaction Processing)
- Day-to-day operations
- INSERT, UPDATE, DELETE heavy
- Current data
- Normalized design
- Fast individual transactions

**Example:** Processing a sale, updating inventory

### OLAP (Online Analytical Processing)
- Analysis and reporting
- SELECT heavy (complex queries)
- Historical data
- Denormalized design
- Fast aggregations

**Example:** "What were total sales by region last quarter?"

| Aspect | OLTP | OLAP |
|--------|------|------|
| Purpose | Operations | Analysis |
| Queries | Simple, fast | Complex, analytical |
| Data | Current | Historical |
| Users | Many (clerks) | Few (analysts) |
| Design | Normalized | Denormalized |

---

## Data Warehouses

A **data warehouse** is a centralized repository optimized for analysis.

### Characteristics
- Subject-oriented (organized by business area)
- Integrated (consistent data from multiple sources)
- Time-variant (historical data)
- Non-volatile (stable, not frequently updated)

### Architecture

```
Operational    →    ETL    →    Data         →    BI Tools
Systems             Process      Warehouse
(OLTP)                          (OLAP)

- POS                           - Reports
- CRM             Extract       - Dashboards
- ERP             Transform     - Analysis
- Web             Load
```

---

## ETL Process

**ETL = Extract, Transform, Load**

### Extract
Pull data from source systems:
- Databases
- Files (CSV, Excel)
- APIs
- Web scraping

### Transform
Clean and prepare data:
- Remove duplicates
- Fix inconsistencies
- Apply business rules
- Aggregate data
- Join from multiple sources

### Load
Insert into data warehouse:
- Full load (replace all)
- Incremental load (new/changed only)
- Scheduled jobs (nightly, hourly)

```sql
-- Example: Simple ETL to aggregate daily sales
INSERT INTO sales_summary (sale_date, total_sales, order_count)
SELECT 
    DATE(order_date) AS sale_date,
    SUM(total) AS total_sales,
    COUNT(*) AS order_count
FROM orders
WHERE order_date >= CURRENT_DATE - INTERVAL '1 day'
GROUP BY DATE(order_date);
```

---

## Dimensional Modeling

Data warehouses often use **star schema** design.

### Fact Tables
- Contain measurements/metrics
- Foreign keys to dimensions
- Example: sales_facts (sale_id, date_id, product_id, amount, quantity)

### Dimension Tables
- Describe the "who, what, where, when"
- Denormalized for query performance
- Example: dim_product (product_id, name, category, subcategory)

### Star Schema Example

```
                    dim_date
                       ↑
dim_product → sales_facts ← dim_customer
                       ↓
                   dim_store
```

```sql
-- Fact table
CREATE TABLE sales_facts (
    sale_id SERIAL PRIMARY KEY,
    date_id INTEGER REFERENCES dim_date(date_id),
    product_id INTEGER REFERENCES dim_product(product_id),
    customer_id INTEGER REFERENCES dim_customer(customer_id),
    store_id INTEGER REFERENCES dim_store(store_id),
    quantity INTEGER,
    amount DECIMAL(10,2)
);

-- Dimension table
CREATE TABLE dim_date (
    date_id SERIAL PRIMARY KEY,
    full_date DATE,
    year INTEGER,
    quarter INTEGER,
    month INTEGER,
    month_name VARCHAR(20),
    day_of_week INTEGER,
    day_name VARCHAR(20),
    is_weekend BOOLEAN,
    is_holiday BOOLEAN
);
```

---

## BI Queries

### Time-Based Analysis
```sql
-- Sales by month
SELECT 
    d.year,
    d.month_name,
    SUM(f.amount) AS total_sales
FROM sales_facts f
JOIN dim_date d ON f.date_id = d.date_id
GROUP BY d.year, d.month, d.month_name
ORDER BY d.year, d.month;
```

### Comparative Analysis
```sql
-- This year vs last year
SELECT 
    d.month_name,
    SUM(CASE WHEN d.year = 2024 THEN f.amount END) AS sales_2024,
    SUM(CASE WHEN d.year = 2023 THEN f.amount END) AS sales_2023,
    SUM(CASE WHEN d.year = 2024 THEN f.amount END) - 
    SUM(CASE WHEN d.year = 2023 THEN f.amount END) AS difference
FROM sales_facts f
JOIN dim_date d ON f.date_id = d.date_id
WHERE d.year IN (2023, 2024)
GROUP BY d.month, d.month_name
ORDER BY d.month;
```

### Top N Analysis
```sql
-- Top 10 products by revenue
SELECT 
    p.name,
    p.category,
    SUM(f.amount) AS revenue
FROM sales_facts f
JOIN dim_product p ON f.product_id = p.product_id
GROUP BY p.product_id, p.name, p.category
ORDER BY revenue DESC
LIMIT 10;
```

---

## BI Tools

### Reporting Tools
- **Tableau** - Visual analytics
- **Power BI** - Microsoft ecosystem
- **Looker** - Google Cloud
- **Metabase** - Open source

### What They Provide
- Interactive dashboards
- Drag-and-drop visualization
- Scheduled reports
- Drill-down capabilities
- Data exploration

---

## Summary

| Concept | Purpose |
|---------|---------|
| OLTP | Day-to-day transactions |
| OLAP | Analysis and reporting |
| Data Warehouse | Centralized analytical repository |
| ETL | Data integration process |
| Star Schema | Optimized analytical design |
| BI Tools | Visualization and reporting |

---

**Next Module:** [Security & Privacy](../module14/overview.md)
