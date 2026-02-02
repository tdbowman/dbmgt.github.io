# Database Security and Privacy

## Why Security Matters

Data breaches can result in:
- Financial losses
- Legal penalties
- Reputation damage
- Loss of customer trust
- Regulatory fines

**Examples of breaches:**
- Equifax (2017): 147 million records
- Marriott (2018): 500 million records
- Facebook (2019): 533 million records

---

## Authentication vs Authorization

### Authentication
**"Who are you?"**
- Verifying identity
- Username/password
- Multi-factor authentication
- Certificates

### Authorization
**"What can you do?"**
- Permissions and privileges
- Role-based access
- Object-level security

---

## PostgreSQL Security

### Creating Users

```sql
-- Create user
CREATE USER app_user WITH PASSWORD 'secure_password';

-- Create role (group)
CREATE ROLE read_only;

-- Assign user to role
GRANT read_only TO app_user;
```

### Granting Privileges

```sql
-- Grant on database
GRANT CONNECT ON DATABASE coffee_shop TO app_user;

-- Grant on schema
GRANT USAGE ON SCHEMA public TO app_user;

-- Grant on table
GRANT SELECT ON products TO app_user;
GRANT SELECT, INSERT, UPDATE ON orders TO app_user;
GRANT ALL PRIVILEGES ON customers TO admin_user;

-- Grant on all tables
GRANT SELECT ON ALL TABLES IN SCHEMA public TO read_only;
```

### Revoking Privileges

```sql
REVOKE INSERT ON orders FROM app_user;
REVOKE ALL PRIVILEGES ON customers FROM app_user;
```

### Column-Level Security

```sql
-- Grant access to specific columns only
GRANT SELECT (customer_id, first_name, last_name) 
ON customers TO support_user;
-- Support can see names but not email, phone, etc.
```

### Row-Level Security

```sql
-- Enable RLS
ALTER TABLE orders ENABLE ROW LEVEL SECURITY;

-- Policy: Users can only see their own orders
CREATE POLICY user_orders ON orders
    FOR SELECT
    USING (customer_id = current_user_id());
```

---

## SQL Injection

### The Problem

SQL injection lets attackers execute malicious SQL.

**Vulnerable code:**
```python
# NEVER DO THIS!
query = f"SELECT * FROM users WHERE username = '{user_input}'"
```

**Attack:**
```
user_input = "admin'; DROP TABLE users; --"
```

**Resulting query:**
```sql
SELECT * FROM users WHERE username = 'admin'; DROP TABLE users; --'
```

### Prevention

**1. Parameterized Queries**
```python
# Python with psycopg2
cur.execute("SELECT * FROM users WHERE username = %s", (user_input,))

# SQLAlchemy
User.query.filter_by(username=user_input).first()
```

**2. Stored Procedures**
```sql
CREATE FUNCTION get_user(p_username VARCHAR)
RETURNS TABLE(user_id INT, username VARCHAR)
AS $$
BEGIN
    RETURN QUERY SELECT u.user_id, u.username 
                 FROM users u 
                 WHERE u.username = p_username;
END;
$$ LANGUAGE plpgsql;
```

**3. Input Validation**
```python
# Validate before using
import re
if not re.match(r'^[a-zA-Z0-9_]+$', username):
    raise ValueError("Invalid username")
```

**4. Least Privilege**
```sql
-- App user can only SELECT, not DROP
GRANT SELECT ON users TO app_user;
-- NOT: GRANT ALL PRIVILEGES
```

---

## Encryption

### Data at Rest
Encrypt stored data:
- Full disk encryption
- Transparent Data Encryption (TDE)
- Column-level encryption

```sql
-- PostgreSQL pgcrypto extension
CREATE EXTENSION pgcrypto;

-- Encrypt sensitive data
INSERT INTO users (email, ssn_encrypted)
VALUES (
    'user@email.com',
    pgp_sym_encrypt('123-45-6789', 'encryption_key')
);

-- Decrypt
SELECT pgp_sym_decrypt(ssn_encrypted, 'encryption_key') 
FROM users;
```

### Data in Transit
Encrypt network connections:
- SSL/TLS for database connections
- HTTPS for web applications

```python
# PostgreSQL with SSL
conn = psycopg2.connect(
    host="localhost",
    database="mydb",
    user="user",
    password="password",
    sslmode="require"
)
```

### Hashing Passwords

**Never store passwords in plain text!**

```python
import bcrypt

# Hash password
password = "user_password"
hashed = bcrypt.hashpw(password.encode(), bcrypt.gensalt())

# Verify password
if bcrypt.checkpw(password.encode(), hashed):
    print("Password correct")
```

---

## Privacy Regulations

### GDPR (General Data Protection Regulation)
European Union regulation requiring:
- Consent for data collection
- Right to access personal data
- Right to be forgotten (deletion)
- Data breach notification
- Data portability

### CCPA (California Consumer Privacy Act)
California law providing:
- Right to know what data is collected
- Right to delete personal data
- Right to opt-out of data sales
- Non-discrimination for exercising rights

### Compliance Requirements

| Requirement | Implementation |
|-------------|----------------|
| Data inventory | Know what data you have |
| Consent management | Track user consent |
| Access requests | Ability to export user data |
| Deletion requests | Ability to delete user data |
| Audit logging | Track who accessed what |
| Breach notification | Detect and report breaches |

---

## Audit Logging

Track database activity:

```sql
-- Create audit table
CREATE TABLE audit_log (
    log_id SERIAL PRIMARY KEY,
    table_name VARCHAR(50),
    operation VARCHAR(10),
    old_data JSONB,
    new_data JSONB,
    changed_by VARCHAR(50),
    changed_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Create trigger function
CREATE OR REPLACE FUNCTION audit_trigger()
RETURNS TRIGGER AS $$
BEGIN
    INSERT INTO audit_log (table_name, operation, old_data, new_data, changed_by)
    VALUES (
        TG_TABLE_NAME,
        TG_OP,
        row_to_json(OLD),
        row_to_json(NEW),
        current_user
    );
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

-- Apply trigger
CREATE TRIGGER customers_audit
AFTER INSERT OR UPDATE OR DELETE ON customers
FOR EACH ROW EXECUTE FUNCTION audit_trigger();
```

---

## Security Best Practices

### Database Level
1. Use strong passwords
2. Limit privileges (least privilege)
3. Enable SSL connections
4. Regular backups
5. Keep software updated
6. Monitor for suspicious activity

### Application Level
1. Parameterized queries always
2. Input validation
3. Hash passwords
4. Encrypt sensitive data
5. HTTPS everywhere
6. Session management

### Organizational Level
1. Security training
2. Access reviews
3. Incident response plan
4. Regular security audits
5. Data classification
6. Vendor assessment

---

## Summary

| Topic | Key Points |
|-------|------------|
| Authentication | Verify identity |
| Authorization | Control access with GRANT/REVOKE |
| SQL Injection | Use parameterized queries |
| Encryption | Protect data at rest and in transit |
| Privacy | Comply with GDPR, CCPA |
| Auditing | Log and monitor activity |

---

**Congratulations!** You've completed the Database Management textbook! 🎉
