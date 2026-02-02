# Setting Up PostgreSQL

## What is PostgreSQL?

PostgreSQL (often called "Postgres") is a powerful, open-source relational database management system. It's used by companies like Apple, Netflix, Spotify, and Instagram.

### Why PostgreSQL?

- **Free and Open Source** - No licensing fees
- **Standards Compliant** - Follows SQL standards closely
- **Feature Rich** - Supports advanced features like JSON, full-text search, and GIS
- **Reliable** - Over 30 years of development
- **Widely Used** - Skills transfer to any job

---

## Installation Guide

### Windows

1. **Download the Installer**
   - Go to [postgresql.org/download/windows](https://www.postgresql.org/download/windows/)
   - Click "Download the installer"
   - Select the latest version (PostgreSQL 15 or 16)

2. **Run the Installer**
   - Double-click the downloaded `.exe` file
   - Click "Next" through the welcome screens

3. **Choose Installation Directory**
   - Default is usually fine: `C:\Program Files\PostgreSQL\16`
   - Click "Next"

4. **Select Components**
   - Keep all components selected:
     - ✅ PostgreSQL Server
     - ✅ pgAdmin 4
     - ✅ Stack Builder
     - ✅ Command Line Tools
   - Click "Next"

5. **Choose Data Directory**
   - Default is fine
   - Click "Next"

6. **Set Password**
   - **IMPORTANT:** Choose a password you'll remember!
   - This is the password for the `postgres` superuser
   - Write it down somewhere safe
   - Click "Next"

7. **Choose Port**
   - Default port: `5432`
   - Keep the default unless you have a conflict
   - Click "Next"

8. **Complete Installation**
   - Click "Next" through remaining screens
   - Click "Finish"

```{warning}
Remember your password! You'll need it to connect to PostgreSQL.
```

### macOS

#### Option 1: Postgres.app (Recommended)

1. **Download Postgres.app**
   - Go to [postgresapp.com](https://postgresapp.com/)
   - Click "Download"
   - Move to your Applications folder

2. **Launch Postgres.app**
   - Open from Applications
   - Click "Initialize" to create a new server
   - Click "Start" to run the server

3. **Configure Command Line (Optional)**
   ```bash
   sudo mkdir -p /etc/paths.d && echo /Applications/Postgres.app/Contents/Versions/latest/bin | sudo tee /etc/paths.d/postgresapp
   ```

#### Option 2: Homebrew

```bash
# Install Homebrew first if needed: https://brew.sh
brew install postgresql@16
brew services start postgresql@16
```

### Linux (Ubuntu/Debian)

```bash
# Update package list
sudo apt update

# Install PostgreSQL
sudo apt install postgresql postgresql-contrib

# Start the service
sudo systemctl start postgresql
sudo systemctl enable postgresql

# Set a password for the postgres user
sudo -u postgres psql
ALTER USER postgres PASSWORD 'your_password';
\q
```

---

## Verifying Your Installation

### Windows
1. Open "SQL Shell (psql)" from the Start menu
2. Press Enter to accept defaults for Server, Database, Port, Username
3. Enter your password when prompted
4. You should see the `postgres=#` prompt

### macOS/Linux
1. Open Terminal
2. Type: `psql -U postgres`
3. Enter your password
4. You should see the `postgres=#` prompt

### Test Commands

Once connected, try these commands:

```sql
-- Show PostgreSQL version
SELECT version();

-- List all databases
\l

-- Quit
\q
```

---

## Creating Your First Database

Let's create a database for this course:

```sql
-- Connect as postgres user
psql -U postgres

-- Create a new database
CREATE DATABASE coffee_shop_db;

-- List databases to confirm
\l

-- Connect to the new database
\c coffee_shop_db

-- You should see: You are now connected to database "coffee_shop_db"
```

---

## Common Connection Settings

When connecting to PostgreSQL, you'll typically need:

| Setting | Default Value |
|---------|--------------|
| Host | `localhost` |
| Port | `5432` |
| Database | `postgres` (default) or your database name |
| Username | `postgres` |
| Password | What you set during installation |

---

## Troubleshooting

### "Connection refused" error
- Make sure PostgreSQL service is running
- Windows: Check Services app for "postgresql-x64-16"
- macOS: Make sure Postgres.app is running
- Linux: `sudo systemctl status postgresql`

### "Password authentication failed"
- Double-check your password
- Make sure you're using the correct username (`postgres`)

### "Port already in use"
- Another application is using port 5432
- Either stop that application or use a different port

### Need to Reset Password?

**Windows:**
1. Edit `pg_hba.conf` (in PostgreSQL data directory)
2. Change `md5` to `trust` temporarily
3. Restart PostgreSQL service
4. Connect and set new password: `ALTER USER postgres PASSWORD 'newpassword';`
5. Change `pg_hba.conf` back to `md5`
6. Restart PostgreSQL

---

## Summary

You've installed PostgreSQL and can:
- ✅ Start the PostgreSQL server
- ✅ Connect using psql
- ✅ Create a new database

---

**Next:** [Setting Up DBeaver](setup-dbeaver.md)
