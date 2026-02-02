# Setting Up DBeaver

## What is DBeaver?

DBeaver is a free, universal database management tool. While you can interact with PostgreSQL using the command line (`psql`), DBeaver provides a graphical interface that makes it easier to:

- Browse database structures
- Write and run SQL queries
- View and edit data
- Export results
- Manage multiple databases

---

## Installation

### Download DBeaver

1. Go to [dbeaver.io/download](https://dbeaver.io/download/)
2. Download **DBeaver Community** (the free version)
3. Choose the installer for your operating system

### Windows Installation

1. Run the downloaded `.exe` file
2. Follow the installation wizard
3. Accept the license agreement
4. Choose installation location (default is fine)
5. Complete the installation
6. Launch DBeaver

### macOS Installation

1. Open the downloaded `.dmg` file
2. Drag DBeaver to your Applications folder
3. Launch DBeaver from Applications
4. If prompted about security, go to System Preferences → Security & Privacy and click "Open Anyway"

### Linux Installation

**Ubuntu/Debian:**
```bash
# Add repository
sudo add-apt-repository ppa:serge-rider/dbeaver-ce

# Update and install
sudo apt update
sudo apt install dbeaver-ce
```

**Or download the .deb file:**
```bash
# Download and install
wget https://dbeaver.io/files/dbeaver-ce_latest_amd64.deb
sudo dpkg -i dbeaver-ce_latest_amd64.deb
```

---

## Connecting to PostgreSQL

### Step 1: Create a New Connection

1. Launch DBeaver
2. Click **Database** → **New Database Connection** (or click the plug icon)
3. Select **PostgreSQL** from the list
4. Click **Next**

### Step 2: Enter Connection Details

Fill in the connection settings:

| Field | Value |
|-------|-------|
| Host | `localhost` |
| Port | `5432` |
| Database | `postgres` (or `coffee_shop_db` if created) |
| Username | `postgres` |
| Password | Your PostgreSQL password |

```{tip}
Check "Save password locally" so you don't have to enter it every time.
```

### Step 3: Test the Connection

1. Click **Test Connection**
2. If prompted to download drivers, click **Download**
3. You should see "Connected" message
4. Click **Finish**

### Step 4: Explore the Connection

In the left panel, you'll see your connection. Expand it to see:
- **Databases** - List of all databases
- **Schemas** - Organization within databases
- **Tables** - Your data tables
- **Views** - Virtual tables

---

## DBeaver Interface Tour

### Database Navigator (Left Panel)
- Browse all your database objects
- Right-click for context menus
- Drag tables to see relationships

### SQL Editor (Center)
- Write SQL queries
- Execute with Ctrl+Enter (Cmd+Enter on Mac)
- Multiple tabs for different queries

### Results Panel (Bottom)
- View query results
- Edit data directly
- Export to CSV, Excel, etc.

### Properties Panel (Right, if enabled)
- View object details
- See table structures
- Check constraints

---

## Running Your First Query

### Create a SQL Script

1. Right-click on your database connection
2. Select **SQL Editor** → **New SQL Script**
3. A new editor tab opens

### Write and Execute SQL

Type this in the editor:

```sql
-- Show all databases
SELECT datname FROM pg_database;
```

Execute by:
- Pressing **Ctrl+Enter** (Cmd+Enter on Mac)
- Or clicking the **Execute** button (play icon)

### View Results

The results appear in the panel below the editor.

---

## Useful DBeaver Features

### Execute Multiple Statements

Write multiple queries separated by semicolons:

```sql
SELECT 1 + 1 AS sum;
SELECT NOW() AS current_time;
SELECT 'Hello, Database!' AS greeting;
```

- **Ctrl+Enter**: Execute statement at cursor
- **Alt+X**: Execute all statements

### Auto-Complete

As you type, DBeaver suggests:
- Table names
- Column names
- SQL keywords
- Functions

Press **Tab** to accept a suggestion.

### Format SQL

Messy SQL? Select your code and press **Ctrl+Shift+F** to format it nicely.

### Export Results

After running a query:
1. Right-click in the results panel
2. Select **Export Data**
3. Choose format (CSV, Excel, SQL, etc.)
4. Follow the wizard

---

## Creating Tables with DBeaver

Besides writing SQL, you can create tables visually:

1. Right-click on **Tables** in the navigator
2. Select **Create New Table**
3. Add columns with the GUI
4. Set data types, constraints
5. Click **Save** to create the table

```{note}
We'll use both methods in this course. Writing SQL is faster once you know it, but the GUI helps you learn the structure.
```

---

## Keyboard Shortcuts

| Action | Windows/Linux | macOS |
|--------|---------------|-------|
| Execute statement | Ctrl+Enter | Cmd+Enter |
| Execute all | Alt+X | Option+X |
| New SQL script | Ctrl+] | Cmd+] |
| Format SQL | Ctrl+Shift+F | Cmd+Shift+F |
| Auto-complete | Ctrl+Space | Ctrl+Space |
| Save | Ctrl+S | Cmd+S |
| Find | Ctrl+F | Cmd+F |

---

## Troubleshooting

### "Cannot connect to database"
- Make sure PostgreSQL is running
- Verify host, port, username, password
- Check if firewall is blocking the connection

### "Driver not found"
- Click "Download" when prompted
- Or go to **Database** → **Driver Manager** → **PostgreSQL** → **Download**

### Connection Timeout
- Increase timeout in connection settings
- Check network connectivity

---

## Summary

You've set up DBeaver and can:
- ✅ Connect to PostgreSQL
- ✅ Navigate the database structure
- ✅ Write and execute SQL queries
- ✅ View and export results

---

## What's Next?

Now that your tools are set up, you're ready to learn about the relational data model in [Module 2](../module02/overview.md)!

But first, complete **Exercise 1** on Canvas to practice what you've learned.
