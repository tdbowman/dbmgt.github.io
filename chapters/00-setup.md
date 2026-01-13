# Setting Up Your Environment

Before we dive into database concepts, let's ensure you have all the necessary tools installed on your computer.

```{admonition} Time Required
:class: tip
Plan for approximately **30-45 minutes** to complete the full setup.
```

## Required Software

You'll need to install two main pieces of software:

1. **PostgreSQL** - The relational database management system we'll use throughout this course
2. **DBeaver** - A free, cross-platform database administration tool

## Installing PostgreSQL

PostgreSQL (often called "Postgres") is a powerful, open-source relational database system with over 35 years of active development.

### Windows Installation

1. Visit the [PostgreSQL Downloads page](https://www.postgresql.org/download/windows/)
2. Click "Download the installer" to get the EnterpriseDB installer
3. Run the installer and follow these steps:
   - Accept the default installation directory
   - Select all components (PostgreSQL Server, pgAdmin 4, Stack Builder, Command Line Tools)
   - Choose a data directory (default is fine)
   - **Important:** Set a password for the `postgres` superuser—remember this!
   - Keep the default port (5432)
   - Select your locale
   - Complete the installation

```{warning}
Write down your PostgreSQL password! You'll need it to connect to the database.
```

### macOS Installation

The easiest method is using Homebrew:

```bash
# Install Homebrew if you don't have it
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install PostgreSQL
brew install postgresql@15

# Start PostgreSQL service
brew services start postgresql@15
```

Alternatively, download [Postgres.app](https://postgresapp.com/) for a simple click-to-run installation.

### Linux (Ubuntu/Debian)

```bash
# Update package list
sudo apt update

# Install PostgreSQL
sudo apt install postgresql postgresql-contrib

# Start the service
sudo systemctl start postgresql

# Enable auto-start on boot
sudo systemctl enable postgresql
```

## Verifying PostgreSQL Installation

Open a terminal (or Command Prompt on Windows) and run:

```bash
psql --version
```

You should see output like:
```
psql (PostgreSQL) 15.4
```

## Installing DBeaver

DBeaver is a universal database tool that works with PostgreSQL, MySQL, SQLite, and many other databases.

### All Platforms

1. Visit [DBeaver Downloads](https://dbeaver.io/download/)
2. Download the Community Edition for your operating system
3. Run the installer and follow the prompts
4. Launch DBeaver after installation

## Connecting DBeaver to PostgreSQL

Once both tools are installed, let's create your first database connection:

1. **Open DBeaver**
2. Click **Database** → **New Database Connection** (or the plug icon with a + sign)
3. Select **PostgreSQL** and click **Next**
4. Enter connection details:
   - **Host:** `localhost`
   - **Port:** `5432`
   - **Database:** `postgres`
   - **Username:** `postgres`
   - **Password:** (the password you set during installation)
5. Click **Test Connection** to verify
6. Click **Finish**

```{figure} ../images/dbeaver-connection.png
:name: dbeaver-connection
:alt: DBeaver connection dialog

DBeaver's New Connection dialog for PostgreSQL.
```

## Creating Your First Database

Let's create a database for our course exercises:

1. In DBeaver, right-click on **Databases** under your connection
2. Select **Create New Database**
3. Name it `course_db`
4. Click **OK**

Or use SQL:

```sql
CREATE DATABASE course_db;
```

## Setting Up Python Environment (Optional)

For the later modules on web integration, you'll need Python:

```bash
# Create a virtual environment
python -m venv dbenv

# Activate it (Windows)
dbenv\Scripts\activate

# Activate it (macOS/Linux)
source dbenv/bin/activate

# Install required packages
pip install jupyter ipython-sql psycopg2-binary sqlalchemy pandas flask
```

## Testing Your Jupyter SQL Setup

Create a new Jupyter notebook and test the SQL connection:

```python
# Load the SQL extension
%load_ext sql

# Connect to PostgreSQL
%sql postgresql://postgres:yourpassword@localhost/course_db
```

```python
# Test with a simple query
%sql SELECT version();
```

You should see your PostgreSQL version information.

## Troubleshooting

```{admonition} Common Issues
:class: warning

**"Connection refused" error**
- Make sure PostgreSQL service is running
- Check that the port is 5432
- Verify firewall settings

**"Password authentication failed"**
- Double-check your password
- On macOS/Linux, you may need to edit `pg_hba.conf`

**DBeaver can't find driver**
- Click "Download" when prompted to get the PostgreSQL JDBC driver
```

## Summary

You now have:

- ✅ PostgreSQL installed and running
- ✅ DBeaver connected to your database
- ✅ A `course_db` database ready for exercises
- ✅ (Optional) Python/Jupyter configured for SQL

```{seealso}
If you encounter issues, check:
- [PostgreSQL Documentation](https://www.postgresql.org/docs/)
- [DBeaver Wiki](https://github.com/dbeaver/dbeaver/wiki)
- Post questions in the course discussion forum!
```

---

**Next:** [Introduction to Databases](01-intro-databases.md)
