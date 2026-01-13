# Introduction to Database Management

Welcome to **Introduction to Database Management**, an interactive online textbook for IN 251 / IM 751 / LIS 751 at the School of Information Studies.

```{admonition} Course Information
:class: tip
**Instructor:** Dr. Timothy D Bowman  
**Term:** Winter 2025  
**Format:** Hybrid (Online + In-Person Sessions)
```

## About This Textbook

This textbook provides a comprehensive introduction to database concepts, design, and implementation. Whether you're an undergraduate just beginning your journey into data management or a graduate student looking to apply database principles in information retrieval and processing, this book will guide you through the fundamental concepts and practical skills you need.

## What You'll Learn

By the end of this course, you will be able to:

::::{grid} 2

:::{grid-item-card} 📊 Data Modeling
Understand data modeling and database structures, including Entity-Relationship diagrams and normalization techniques.
:::

:::{grid-item-card} 🗄️ RDBMS Implementation
Comprehend relational database management systems and implement them using PostgreSQL.
:::

:::{grid-item-card} 💻 SQL Mastery
Write SQL queries from basic SELECT statements to advanced JOINs, subqueries, triggers, and stored procedures.
:::

:::{grid-item-card} 🌐 Web Integration
Build database-driven web applications using Python and Flask.
:::

::::

## Course Structure

The course is organized into **14 modules** that build upon each other progressively:

| Part | Modules | Topics |
|------|---------|--------|
| **Foundations** | 1-4 | Introduction, Relational Model, ER Modeling, Normalization |
| **SQL in Practice** | 5-8 | SQL Basics, DDL, Advanced SQL, Performance |
| **Applications** | 9-12 | Cloud DBs, Web Integration, CMS, NoSQL |
| **Security** | 13-14 | Database Security and Privacy |

```{note}
This course starts slowly and gradually increases pace. If you have previous database experience, the first modules may be review—use this time to experiment beyond the assignments!
```

## How to Use This Book

This interactive textbook combines:

- **📖 Conceptual Content**: Theory and explanations in readable MyST Markdown
- **💻 Interactive Notebooks**: Hands-on SQL practice with executable code cells
- **📝 Exercises**: Practice problems to reinforce your learning
- **🔗 External Resources**: Links to documentation and supplementary materials

### Running the Code Examples

You have several options for running the interactive notebooks:

1. **Google Colab** (Recommended for beginners): Click the rocket icon → "Colab" on any notebook page
2. **Binder**: Launch a free cloud environment with all dependencies pre-installed
3. **Local Installation**: Install PostgreSQL and Jupyter on your own machine

## Required Software

Throughout this course, you'll use:

- **PostgreSQL** - Our relational database management system
- **DBeaver** - A free database administration tool
- **Jupyter Notebooks** - For interactive SQL practice
- **Python** (later modules) - For web integration

```{seealso}
See [Setting Up Your Environment](chapters/00-setup.md) for detailed installation instructions.
```

## Sample Data

We'll work with several datasets throughout the course, including:

- **Coffee Shop Database** - Transactions, customers, products, and staff from a multi-location coffee chain
- **Company Database** - Employees, projects, and departments for learning JOINs
- **Social Media Data** - Tweet data for real-world data modeling exercises

## Let's Get Started!

Ready to begin your database journey? Head to [Setting Up Your Environment](chapters/00-setup.md) to set up your environment, or jump straight to [Introduction to Databases](chapters/01-intro-databases.md) if you're already set up.

```{tableofcontents}
```

---

*This textbook was created using [Jupyter Book](https://jupyterbook.org/) with [MyST Markdown](https://myst-parser.readthedocs.io/).*
