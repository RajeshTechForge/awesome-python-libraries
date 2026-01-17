<div align="center">

# Awesome Psycopg3 [![Awesome](https://cdn.rawgit.com/sindresorhus/awesome/d7305f38d29fed78fa85652e3a63e154dd8e8829/media/badge.svg)](https://github.com/sindresorhus/awesome)

 🐘 **The Ultimate Guide to PostgreSQL with Python (2026 Edition)** - Modern, Fast, Async-Ready!

<br>
<br>
</div>

Welcome to the most comprehensive Psycopg3 tutorial on the internet! This guide will transform you from a PostgreSQL novice to a production-ready Python database expert. Whether you're building web apps, data pipelines, or high-performance systems, this tutorial has everything you need.

**✨ What's New in 2026:**
- **Psycopg 3.3+** with Python 3.14 template string support
- **Async-first** design patterns
- **Connection pooling** for production apps
- **Type-safe** queries with modern Python
- **Server-side cursors** for big data
- **Pipeline mode** for ultra-high performance
- **Best practices** from 10+ years of psycopg2 experience

*Inspired by [awesome-python](https://github.com/vinta/awesome-python)* ✨


## 📋 Quick Navigation

- **[Introduction](#-introduction)** - What & Why Psycopg3
- **[Getting Started](#-getting-started)** - Installation & Setup
- **[Core Concepts](#-core-concepts)** - Connections & Basics
- **[CRUD Operations](#-crud-operations)** - Create, Read, Update, Delete
- **[Advanced Querying](#-advanced-querying)** - Parameters & SQL Composition
- **[Async Operations](#-async-operations)** - AsyncIO Integration
- **[Connection Pooling](#-connection-pooling)** - Production-Ready Pools
- **[Row Factories](#-row-factories)** - Custom Result Types
- **[Performance](#-performance)** - Optimization & Best Practices
- **[Production Patterns](#-production-patterns)** - Real-World Examples
- **[Resources](#-additional-resources)** - Learn More

---

## 🎯 Introduction

### What is PostgreSQL?

[PostgreSQL](https://www.postgresql.org/) (often called "Postgres") is the world's most advanced open-source relational database. It's:

- **🔒 ACID Compliant** - Reliable transactions
- **📊 Feature-Rich** - JSON, Arrays, Full-text search, GIS
- **⚡ High Performance** - Handles millions of rows
- **🔧 Extensible** - Custom types, functions, operators
- **🌍 Battle-Tested** - Powers major companies worldwide

**Why PostgreSQL?**
```
Traditional SQL:         PostgreSQL Adds:
┌─────────────┐         ┌──────────────┐
│ Tables      │    +    │ JSON/JSONB   │
│ Indexes     │         │ Arrays       │
│ Constraints │         │ Full-Text    │
│ Transactions│         │ GIS (PostGIS)│
└─────────────┘         │ Custom Types │
                        └──────────────┘
```

### What is Psycopg3?

[Psycopg3](https://www.psycopg.org/psycopg3/) is the **most popular** and **most reliable** PostgreSQL adapter for Python. It's a complete rewrite of psycopg2, designed for modern Python and PostgreSQL.

```python
# Simple, elegant, powerful
import psycopg

# Sync version
with psycopg.connect("dbname=mydb") as conn:
    with conn.cursor() as cur:
        cur.execute("SELECT * FROM users WHERE id = %s", (1,))
        print(cur.fetchone())

# Async version
import asyncio

async with await psycopg.AsyncConnection.connect("dbname=mydb") as conn:
    async with conn.cursor() as cur:
        await cur.execute("SELECT * FROM users WHERE id = %s", (1,))
        print(await cur.fetchone())
```

### Why Psycopg3 Rocks in 2026

**🚀 Major Improvements Over Psycopg2:**

1. **⚡ Async Native** - First-class asyncio support
2. **🎯 Server-Side Binding** - Better performance & security
3. **🔧 Automatic Prepared Statements** - No manual management
4. **📦 Connection Pooling** - Built-in pool implementation
5. **🎨 Row Factories** - Flexible result types (dict, dataclass, etc.)
6. **⏱️ Pipeline Mode** - Batch operations for speed
7. **🛡️ Type Safety** - Full type hints support
8. **🔐 Modern Security** - SCRAM-SHA-256 auth

**Latest Version:** Psycopg 3.3.2+
- ✅ Python 3.10+ support (3.14 template strings!)
- ✅ PostgreSQL 12-17 support
- ✅ Binary packages available
- ✅ Production-ready and stable

**Key Differences from Psycopg2:**
| Feature | Psycopg2 | Psycopg3 |
|---------|----------|----------|
| Async Support | Limited | Native |
| Prepared Statements | Manual | Automatic |
| Connection Pool | External | Built-in |
| Parameter Binding | Client-side | Server-side |
| Row Factories | Basic | Advanced |
| Type Hints | No | Full |
| Maintenance | Maintenance mode | Active |


## 🚀 Getting Started

### Prerequisites

- [ ] Python 3.10 or higher
- [ ] PostgreSQL 12+ server
- [ ] Basic Python & SQL knowledge

### Installing PostgreSQL

**macOS (Homebrew):**
```bash
brew install postgresql@17
brew services start postgresql@17
```

**Ubuntu/Debian:**
```bash
sudo apt update
sudo apt install postgresql postgresql-contrib
sudo systemctl start postgresql
sudo systemctl enable postgresql
```

**Windows:**
Download from [PostgreSQL Downloads](https://www.postgresql.org/download/windows/) or use [EnterpriseDB installer](https://www.enterprisedb.com/downloads/postgres-postgresql-downloads).

**Docker (Quick Start):**
```bash
docker run --name postgres-dev \
  -e POSTGRES_PASSWORD=mysecretpassword \
  -e POSTGRES_DB=mydb \
  -p 5432:5432 \
  -d postgres:17
```

**Verify Installation:**
```bash
psql --version
```

### Installing Psycopg3

```bash
# Pure Python implementation (works everywhere)
pip install psycopg

# C extension for better performance (recommended)
pip install "psycopg[binary]"

# With connection pool support
pip install "psycopg[pool]"

# All optional dependencies
pip install "psycopg[binary,pool,c]"

# Verify installation
python -c "import psycopg; print(psycopg.__version__)"

```

**🎓 Virtual Environment (Recommended):**

```bash
# Create project
mkdir awesome_postgres_app
cd awesome_postgres_app

# Create virtual environment
python -m venv venv

# Activate
source venv/bin/activate  # Linux/macOS
venv\Scripts\activate     # Windows

# Install psycopg3
pip install "psycopg[binary,pool]"

```

**Package Variants:**

| Package | Use Case |
|---------|----------|
| `psycopg` | Pure Python, works everywhere |
| `psycopg[binary]` | Pre-compiled binary (fastest install) |
| `psycopg[c]` | Compile from source (best performance) |
| `psycopg[pool]` | Include connection pooling |


## 🔧 Core Concepts

### Connecting to PostgreSQL

#### Quick Connect (Local)

```python
import psycopg

# Simple connection
conn = psycopg.connect("dbname=mydb user=postgres")

# Using connection string
conn = psycopg.connect("postgresql://user:password@localhost/mydb")

# Using keyword arguments
conn = psycopg.connect(
    dbname="mydb",
    user="postgres",
    password="secret",
    host="localhost",
    port=5432
)

# Test connection
print(conn.info.status)  # ConnectionStatus.OK
```

#### Context Manager (Best Practice!)

```python
import psycopg

# Automatic commit/rollback and cleanup
with psycopg.connect("dbname=mydb") as conn:
    with conn.cursor() as cur:
        cur.execute("SELECT version()")
        version = cur.fetchone()
        print(version[0])
# Connection automatically closed, transaction committed

# Handle errors gracefully
try:
    with psycopg.connect("dbname=mydb") as conn:
        with conn.cursor() as cur:
            cur.execute("SELECT * FROM users WHERE id = %s", (1,))
            user = cur.fetchone()
except psycopg.Error as e:
    print(f"Database error: {e}")
```

#### Production-Ready Connection

```python
import psycopg
from psycopg import Connection
import logging

logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

def get_connection() -> Connection:
    """
    Create a production-grade PostgreSQL connection.
    
    Returns:
        Connection: Configured database connection
    """
    try:
        conn = psycopg.connect(
            # Connection string
            "postgresql://user:password@localhost:5432/mydb",
            
            # Connection options
            connect_timeout=10,           # Connection timeout (seconds)
            options="-c statement_timeout=30000",  # Query timeout (ms)
            
            # Auto-prepare frequently used queries
            prepare_threshold=5,          # Prepare after 5 executions
            
            # Application identifier
            application_name="my-awesome-app",
            
            # Row factory (we'll cover this later)
            # row_factory=dict_row
        )
        
        logger.info(f"✅ Connected to PostgreSQL {conn.info.server_version}")
        return conn
        
    except psycopg.OperationalError as e:
        logger.error(f"❌ Connection failed: {e}")
        raise
    except Exception as e:
        logger.error(f"❌ Unexpected error: {e}")
        raise

# Usage
conn = get_connection()

# Check connection info
print(f"Database: {conn.info.dbname}")
print(f"User: {conn.info.user}")
print(f"Host: {conn.info.host}")
print(f"Encoding: {conn.info.encoding}")
```

#### Connection String Formats

```python
# PostgreSQL URL format (recommended)
"postgresql://user:password@localhost:5432/mydb"
"postgresql://user:password@localhost/mydb"  # Default port 5432

# With SSL
"postgresql://user:password@host/db?sslmode=require"

# Unix socket
"postgresql:///mydb"  # Uses Unix socket

# Multiple hosts (failover)
"postgresql://host1,host2,host3/mydb"

# All options
conn_string = (
    "postgresql://user:password@localhost:5432/mydb"
    "?connect_timeout=10"
    "&application_name=myapp"
    "&options=-c%20statement_timeout=5000"
)
```

### Transaction Management

**Default Behavior:**
Psycopg3 connections start in **non-autocommit** mode by default. This means:
- Every query runs in a transaction
- You must explicitly `commit()` or `rollback()`
- Context managers auto-commit on exit (if no error)

```python
# Manual transaction management
conn = psycopg.connect("dbname=mydb")

try:
    cur = conn.cursor()
    cur.execute("INSERT INTO users (name) VALUES (%s)", ("Alice",))
    cur.execute("INSERT INTO orders (user_id) VALUES (%s)", (1,))
    
    conn.commit()  # Commit transaction
    print("✅ Transaction committed")
    
except Exception as e:
    conn.rollback()  # Rollback on error
    print(f"❌ Transaction rolled back: {e}")
finally:
    conn.close()

# Context manager (automatic commit/rollback)
with psycopg.connect("dbname=mydb") as conn:
    with conn.cursor() as cur:
        cur.execute("INSERT INTO users (name) VALUES (%s)", ("Bob",))
        # Automatically commits on successful exit
        # Automatically rolls back on exception
```

**Autocommit Mode:**

```python
# Enable autocommit (each statement commits immediately)
conn = psycopg.connect("dbname=mydb", autocommit=True)

# Or set it later
conn.autocommit = True

# Now each statement commits automatically
cur = conn.cursor()
cur.execute("INSERT INTO users (name) VALUES ('Charlie')")
# No need to call commit()

# Useful for:
# - DDL statements (CREATE TABLE, ALTER, etc.)
# - Long-running scripts
# - LISTEN/NOTIFY operations
```

**Transaction Blocks:**

```python
# Explicit transaction blocks (even in autocommit mode)
conn = psycopg.connect("dbname=mydb", autocommit=True)

with conn.transaction():
    cur = conn.cursor()
    cur.execute("INSERT INTO users (name) VALUES (%s)", ("Diana",))
    cur.execute("INSERT INTO orders (user_id) VALUES (%s)", (1,))
    # Commits on successful exit, rolls back on exception

# Nested transactions (savepoints)
with conn.transaction():
    cur.execute("INSERT INTO users (name) VALUES (%s)", ("Eve",))
    
    try:
        with conn.transaction():  # Nested transaction (savepoint)
            cur.execute("INSERT INTO invalid_data ...")
    except:
        # Inner transaction rolled back, outer continues
        pass
    
    cur.execute("INSERT INTO users (name) VALUES (%s)", ("Frank",))
# Both Eve and Frank inserted
```

### Executing Queries

```python
import psycopg

with psycopg.connect("dbname=mydb") as conn:
    cur = conn.cursor()
    
    # Simple query
    cur.execute("SELECT * FROM users")
    
    # Query with parameters (SECURE - prevents SQL injection!)
    user_id = 1
    cur.execute("SELECT * FROM users WHERE id = %s", (user_id,))
    
    # Named parameters
    cur.execute(
        "SELECT * FROM users WHERE name = %(name)s AND age > %(age)s",
        {"name": "Alice", "age": 25}
    )
    
    # Multiple statements (no parameters)
    cur.execute("""
        CREATE TABLE IF NOT EXISTS temp_table (id int);
        INSERT INTO temp_table VALUES (1), (2), (3);
        DROP TABLE temp_table;
    """)
    
    # Get query result
    result = cur.fetchone()   # First row
    results = cur.fetchall()  # All rows
    results = cur.fetchmany(10)  # Next 10 rows
```

**⚠️ Important Security Note:**

```python
# ✅ CORRECT - Use parameters
user_input = "Alice"
cur.execute("SELECT * FROM users WHERE name = %s", (user_input,))

# ❌ WRONG - SQL Injection vulnerability!
user_input = "Alice'; DROP TABLE users; --"
cur.execute(f"SELECT * FROM users WHERE name = '{user_input}'")

# ❌ ALSO WRONG - String formatting
cur.execute("SELECT * FROM users WHERE name = '%s'" % user_input)

# ❌ STILL WRONG - f-strings
cur.execute(f"SELECT * FROM users WHERE name = '{user_input}'")
```

### Python 3.14 Template Strings (NEW!)

Psycopg 3.3+ supports Python 3.14's new template strings for safe, expressive queries:

```python
from psycopg import connect, t  # t is the template string marker

conn = connect("dbname=mydb")

# Template strings (Python 3.14+)
name = "Alice"
age = 30

# Variables are automatically handled safely!
result = conn.execute(
    t"SELECT * FROM users WHERE name = {name} AND age > {age}"
).fetchall()

# Equivalent to:
result = conn.execute(
    "SELECT * FROM users WHERE name = %s AND age > %s",
    (name, age)
).fetchall()

# Works with complex expressions too
min_age = 25
max_age = 35
result = conn.execute(
    t"SELECT * FROM users WHERE age BETWEEN {min_age} AND {max_age}"
).fetchall()
```

**🎯 Pro Tip:** Template strings combine the convenience of f-strings with the safety of parameterized queries!


## 📝 CRUD Operations

### Create: Inserting Data

#### Insert Single Row

```python
import psycopg
from datetime import datetime

with psycopg.connect("dbname=mydb") as conn:
    cur = conn.cursor()
    
    # Simple insert
    cur.execute(
        "INSERT INTO users (name, email, age) VALUES (%s, %s, %s)",
        ("Alice", "alice@example.com", 28)
    )
    
    # Insert with RETURNING (get inserted ID)
    cur.execute(
        "INSERT INTO users (name, email) VALUES (%s, %s) RETURNING id",
        ("Bob", "bob@example.com")
    )
    user_id = cur.fetchone()[0]
    print(f"✅ Inserted user with ID: {user_id}")
    
    # Insert with named parameters
    user_data = {
        "name": "Charlie",
        "email": "charlie@example.com",
        "age": 35,
        "created_at": datetime.now()
    }
    
    cur.execute(
        """
        INSERT INTO users (name, email, age, created_at)
        VALUES (%(name)s, %(email)s, %(age)s, %(created_at)s)
        RETURNING id
        """,
        user_data
    )
    
    # Insert with DEFAULT values
    cur.execute(
        "INSERT INTO users (name, email) VALUES (%s, %s)",
        ("Diana", "diana@example.com")
    )
    # age and created_at use DEFAULT values
```

#### Insert Multiple Rows

```python
# Method 1: executemany() - Multiple separate inserts
users = [
    ("Eve", "eve@example.com", 24),
    ("Frank", "frank@example.com", 31),
    ("Grace", "grace@example.com", 27)
]

cur.executemany(
    "INSERT INTO users (name, email, age) VALUES (%s, %s, %s)",
    users
)

print(f"✅ Inserted {cur.rowcount} users")

# Method 2: Single INSERT with multiple VALUES
users = [
    ("Henry", "henry@example.com", 29),
    ("Ivy", "ivy@example.com", 26),
    ("Jack", "jack@example.com", 33)
]

# Build multi-value INSERT
from psycopg import sql

values = sql.SQL(',').join(
    sql.Literal(user) for user in users
)

cur.execute(
    sql.SQL("INSERT INTO users (name, email, age) VALUES {}").format(values)
)

# Method 3: COPY (fastest for bulk inserts!)
# We'll cover this in the Performance section
```

#### Insert with ON CONFLICT (Upsert)

```python
# Insert or update if exists
cur.execute(
    """
    INSERT INTO users (email, name, age)
    VALUES (%s, %s, %s)
    ON CONFLICT (email)
    DO UPDATE SET
        name = EXCLUDED.name,
        age = EXCLUDED.age,
        updated_at = NOW()
    RETURNING id, (xmax = 0) AS inserted
    """,
    ("alice@example.com", "Alice Updated", 29)
)

user_id, was_inserted = cur.fetchone()
if was_inserted:
    print(f"✅ Inserted new user: {user_id}")
else:
    print(f"✅ Updated existing user: {user_id}")

# Insert or do nothing
cur.execute(
    """
    INSERT INTO users (email, name)
    VALUES (%s, %s)
    ON CONFLICT (email) DO NOTHING
    """,
    ("bob@example.com", "Bob")
)
```

### Read: Querying Data

#### Basic Queries

```python
cur = conn.cursor()

# Fetch one row
cur.execute("SELECT * FROM users WHERE id = %s", (1,))
user = cur.fetchone()
print(user)  # (1, 'Alice', 'alice@example.com', 28)

# Fetch all rows
cur.execute("SELECT * FROM users WHERE age > %s", (25,))
users = cur.fetchall()
for user in users:
    print(user)

# Fetch in batches (memory efficient)
cur.execute("SELECT * FROM large_table")
while True:
    rows = cur.fetchmany(1000)  # Fetch 1000 at a time
    if not rows:
        break
    process_batch(rows)

# Iterate through results
cur.execute("SELECT * FROM users")
for user in cur:
    print(user)  # Lazy iteration - memory efficient!

# Count rows
cur.execute("SELECT COUNT(*) FROM users WHERE age > %s", (30,))
count = cur.fetchone()[0]
print(f"Users over 30: {count}")
```

#### Working with Results

```python
# Check if query returned results
cur.execute("SELECT * FROM users WHERE id = %s", (999,))
user = cur.fetchone()

if user is None:
    print("User not found")
else:
    print(f"Found user: {user}")

# Get column names
cur.execute("SELECT id, name, email FROM users LIMIT 1")
columns = [desc[0] for desc in cur.description]
print(columns)  # ['id', 'name', 'email']

# Build dict from row
cur.execute("SELECT id, name, email FROM users WHERE id = %s", (1,))
row = cur.fetchone()
if row:
    user_dict = dict(zip([desc[0] for desc in cur.description], row))
    print(user_dict)  # {'id': 1, 'name': 'Alice', 'email': '...'}

# Or use dict_row factory (easier!)
from psycopg.rows import dict_row

cur = conn.cursor(row_factory=dict_row)
cur.execute("SELECT * FROM users WHERE id = %s", (1,))
user = cur.fetchone()
print(user)  # {'id': 1, 'name': 'Alice', ...}
print(user['name'])  # 'Alice'
```

### Update: Modifying Data

```python
# Simple update
cur.execute(
    "UPDATE users SET age = %s WHERE id = %s",
    (30, 1)
)
print(f"✅ Updated {cur.rowcount} rows")

# Update multiple fields
cur.execute(
    """
    UPDATE users
    SET name = %s, email = %s, updated_at = NOW()
    WHERE id = %s
    """,
    ("Alice Updated", "alice.new@example.com", 1)
)

# Update with RETURNING
cur.execute(
    """
    UPDATE users
    SET age = age + 1
    WHERE id = %s
    RETURNING id, name, age
    """,
    (1,)
)
updated_user = cur.fetchone()
print(f"Updated: {updated_user}")

# Conditional update
cur.execute(
    """
    UPDATE users
    SET status = 'active'
    WHERE last_login > NOW() - INTERVAL '30 days'
    """)

# Update with subquery
cur.execute(
    """
    UPDATE orders
    SET total = (
        SELECT SUM(price * quantity)
        FROM order_items
        WHERE order_items.order_id = orders.id
    )
    WHERE id = %s
    """,
    (123,)
)

# Bulk update (multiple rows)
updates = [
    ("Premium", 1),
    ("Premium", 2),
    ("Basic", 3)
]

cur.executemany(
    "UPDATE users SET plan = %s WHERE id = %s",
    updates
)
```

### Delete: Removing Data

```python
# Delete single row
cur.execute("DELETE FROM users WHERE id = %s", (1,))
print(f"✅ Deleted {cur.rowcount} rows")

# Delete with condition
cur.execute(
    "DELETE FROM users WHERE status = %s AND created_at < %s",
    ("inactive", datetime(2020, 1, 1))
)

# Delete with RETURNING
cur.execute(
    "DELETE FROM users WHERE id = %s RETURNING *",
    (2,)
)
deleted_user = cur.fetchone()
if deleted_user:
    print(f"Deleted user: {deleted_user}")

# Delete all rows (DANGEROUS!)
cur.execute("DELETE FROM temp_table")

# Soft delete (recommended for important data)
cur.execute(
    """
    UPDATE users
    SET deleted_at = NOW(), is_deleted = TRUE
    WHERE id = %s
    """,
    (3,)
)

# Cascade delete (if foreign keys configured)
cur.execute("DELETE FROM users WHERE id = %s", (4,))
# Related orders, etc. automatically deleted
```


## 🔍 Advanced Querying

### Parameter Styles

Psycopg3 uses **server-side parameter binding** for better security and performance.

```python
# Positional parameters (recommended)
cur.execute(
    "SELECT * FROM users WHERE name = %s AND age > %s",
    ("Alice", 25)
)

# Named parameters (more readable)
cur.execute(
    "SELECT * FROM users WHERE name = %(name)s AND age > %(age)s",
    {"name": "Alice", "age": 25}
)

# Reusing named parameters
cur.execute(
    """
    SELECT * FROM users
    WHERE created_at BETWEEN %(start)s AND %(end)s
       OR updated_at BETWEEN %(start)s AND %(end)s
    """,
    {"start": start_date, "end": end_date}
)
```

**⚠️ Server-Side Binding Limitations:**

Server-side binding works for DML (SELECT, INSERT, UPDATE, DELETE) but not for:
- DDL statements (CREATE, ALTER, DROP)
- SET commands
- LISTEN/NOTIFY

```python
# ❌ This won't work (server-side binding limitation)
cur.execute("SET timezone TO %s", ("UTC",))
# Error: syntax error at or near "$1"

# ✅ Workarounds:

# Option 1: Use PostgreSQL functions
cur.execute("SELECT set_config('timezone', %s, false)", ("UTC",))

# Option 2: Use psycopg.sql for client-side composition
from psycopg import sql

cur.execute(
    sql.SQL("SET timezone TO {}").format(sql.Literal("UTC"))
)

# Option 3: Use ClientCursor for client-side binding
from psycopg import ClientCursor

cur = conn.cursor(cursor_factory=ClientCursor)
cur.execute("SET timezone TO %s", ("UTC",))
```

### SQL Composition (psycopg.sql)

For dynamic SQL construction (table names, column names, etc.), use `psycopg.sql`:

```python
from psycopg import sql

# Dynamic table name
table_name = "users"
cur.execute(
    sql.SQL("SELECT * FROM {} WHERE age > %s").format(
        sql.Identifier(table_name)
    ),
    (25,)
)

# Dynamic column name
column = "email"
cur.execute(
    sql.SQL("SELECT {} FROM users WHERE id = %s").format(
        sql.Identifier(column)
    ),
    (1,)
)

# Multiple identifiers
cur.execute(
    sql.SQL("SELECT {col1}, {col2} FROM {table}").format(
        col1=sql.Identifier("name"),
        col2=sql.Identifier("email"),
        table=sql.Identifier("users")
    )
)

# Build INSERT dynamically
columns = ["name", "email", "age"]
values = ["Alice", "alice@example.com", 28]

cur.execute(
    sql.SQL("INSERT INTO users ({}) VALUES ({})").format(
        sql.SQL(', ').join(map(sql.Identifier, columns)),
        sql.SQL(', ').join(sql.Placeholder() * len(values))
    ),
    values
)

# Complex query building
def build_filter_query(filters):
    """Build dynamic WHERE clause."""
    conditions = []
    params = {}
    
    for field, value in filters.items():
        conditions.append(
            sql.SQL("{} = {}").format(
                sql.Identifier(field),
                sql.Placeholder(field)
            )
        )
        params[field] = value
    
    query = sql.SQL("SELECT * FROM users WHERE {}").format(
        sql.SQL(" AND ").join(conditions)
    )
    
    return query, params

# Usage
filters = {"age": 28, "status": "active"}
query, params = build_filter_query(filters)
cur.execute(query, params)
```

### Working with JSON

PostgreSQL has excellent JSON support, and Psycopg3 makes it easy:

```python
import json

# Insert JSON data
user_metadata = {
    "preferences": {"theme": "dark", "language": "en"},
    "tags": ["premium", "verified"],
    "settings": {"notifications": True}
}

cur.execute(
    "INSERT INTO users (name, metadata) VALUES (%s, %s)",
    ("Alice", json.dumps(user_metadata))
)

# Or use psycopg's Json adapter
from psycopg.types.json import Jsonb

cur.execute(
    "INSERT INTO users (name, metadata) VALUES (%s, %s)",
    ("Bob", Jsonb(user_metadata))
)

# Query JSON fields
cur.execute(
    """
    SELECT name, metadata->>'theme' as theme
    FROM users
    WHERE metadata->>'language' = %s
    """,
    ("en",)
)

# Update JSON field
cur.execute(
    """
    UPDATE users
    SET metadata = metadata || %s
    WHERE id = %s
    """,
    (json.dumps({"new_field": "value"}), 1)
)

# Query nested JSON
cur.execute(
    """
    SELECT *
    FROM users
    WHERE metadata->'preferences'->>'theme' = %s
    """,
    ("dark",)
)
```

### Working with Arrays

```python
# Insert array
tags = ["python", "postgresql", "database"]
cur.execute(
    "INSERT INTO posts (title, tags) VALUES (%s, %s)",
    ("My Post", tags)
)

# Query array contains value
cur.execute(
    "SELECT * FROM posts WHERE %s = ANY(tags)",
    ("python",)
)

# Query array overlaps
cur.execute(
    "SELECT * FROM posts WHERE tags && %s",
    (["python", "sql"],)
)

# Array aggregation
cur.execute(
    """
    SELECT author, array_agg(title) as posts
    FROM posts
    GROUP BY author
    """
)
```


## ⚡ Async Operations

Psycopg3 has **native async support** - not an afterthought!

### Basic Async Connection

```python
import asyncio
import psycopg

async def main():
    # Connect (note the double async)
    conn = await psycopg.AsyncConnection.connect(
        "postgresql://user:password@localhost/mydb"
    )
    
    try:
        # Create cursor
        cur = conn.cursor()
        
        # Execute query
        await cur.execute("SELECT * FROM users WHERE id = %s", (1,))
        
        # Fetch results
        user = await cur.fetchone()
        print(user)
        
        # Commit transaction
        await conn.commit()
        
    finally:
        # Close connection
        await conn.close()

# Run async function
asyncio.run(main())
```

### Async Context Managers

```python
async def get_user(user_id: int):
    """Fetch user with async context managers."""
    # Double async with!
    async with await psycopg.AsyncConnection.connect(
        "dbname=mydb"
    ) as conn:
        async with conn.cursor() as cur:
            await cur.execute(
                "SELECT * FROM users WHERE id = %s",
                (user_id,)
            )
            return await cur.fetchone()

# Run
user = asyncio.run(get_user(1))
```

### Async Transactions

```python
async def transfer_funds(from_id: int, to_id: int, amount: float):
    """Transfer money between accounts (atomic transaction)."""
    async with await psycopg.AsyncConnection.connect("dbname=bank") as conn:
        try:
            # Start transaction
            async with conn.transaction():
                cur = conn.cursor()
                
                # Debit from account
                await cur.execute(
                    """
                    UPDATE accounts
                    SET balance = balance - %s
                    WHERE id = %s AND balance >= %s
                    """,
                    (amount, from_id, amount)
                )
                
                if cur.rowcount == 0:
                    raise ValueError("Insufficient funds")
                
                # Credit to account
                await cur.execute(
                    "UPDATE accounts SET balance = balance + %s WHERE id = %s",
                    (amount, to_id)
                )
                
                print(f"✅ Transferred ${amount} from {from_id} to {to_id}")
                
        except Exception as e:
            print(f"❌ Transaction failed: {e}")
            raise

# Run
asyncio.run(transfer_funds(1, 2, 100.00))
```

### Concurrent Queries

```python
async def get_user(conn, user_id):
    """Fetch a single user."""
    async with conn.cursor() as cur:
        await cur.execute("SELECT * FROM users WHERE id = %s", (user_id,))
        return await cur.fetchone()

async def get_multiple_users(user_ids):
    """Fetch multiple users concurrently."""
    async with await psycopg.AsyncConnection.connect("dbname=mydb") as conn:
        # Create tasks
        tasks = [get_user(conn, uid) for uid in user_ids]
        
        # Run concurrently
        users = await asyncio.gather(*tasks)
        return users

# Fetch 100 users concurrently!
user_ids = range(1, 101)
users = asyncio.run(get_multiple_users(user_ids))
print(f"✅ Fetched {len(users)} users concurrently")
```

### Async Row Factories

```python
from psycopg.rows import dict_row

async def get_users_as_dicts():
    """Fetch users as dictionaries."""
    async with await psycopg.AsyncConnection.connect(
        "dbname=mydb",
        row_factory=dict_row  # Set row factory on connection
    ) as conn:
        async with conn.cursor() as cur:
            await cur.execute("SELECT * FROM users LIMIT 10")
            users = await cur.fetchall()
            
            for user in users:
                print(f"{user['name']} - {user['email']}")

asyncio.run(get_users_as_dicts())
```


## 🏊 Connection Pooling

Connection pooling is **essential** for production applications!

### Why Connection Pools?

- **⚡ Performance** - Reuse connections instead of creating new ones
- **📊 Resource Management** - Limit concurrent connections
- **🛡️ Stability** - Prevent connection exhaustion
- **🎯 Scalability** - Handle many concurrent requests

**Connection Creation Cost:**
```
Creating new connection: ~20-50ms
Using pooled connection: <1ms
```

### Installing the Pool

```bash
pip install "psycopg[pool]"
# or
pip install psycopg-pool
```

### Basic Connection Pool

```python
from psycopg_pool import ConnectionPool

# Create pool
pool = ConnectionPool(
    conninfo="postgresql://user:password@localhost/mydb",
    min_size=2,      # Minimum connections
    max_size=10,     # Maximum connections
    timeout=30.0,    # Wait timeout for getting connection
    max_idle=300.0,  # Close idle connections after 5 minutes
    max_lifetime=3600.0  # Recreate connections after 1 hour
)

# Use pool
with pool.connection() as conn:
    with conn.cursor() as cur:
        cur.execute("SELECT * FROM users WHERE id = %s", (1,))
        user = cur.fetchone()

# Close pool when done
pool.close()
```

### Production Pool Setup

```python
from psycopg_pool import ConnectionPool
from psycopg.rows import dict_row
import logging

logger = logging.getLogger(__name__)

# Global pool instance
pool = None

def init_pool():
    """Initialize database connection pool."""
    global pool
    
    pool = ConnectionPool(
        # Connection string
        conninfo="postgresql://user:password@localhost/mydb",
        
        # Pool size
        min_size=5,          # Always maintain 5 connections
        max_size=20,         # Max 20 concurrent connections
        
        # Timeouts
        timeout=30.0,        # Wait 30s for connection
        max_wait=60.0,       # Max wait in queue
        
        # Connection lifecycle
        max_idle=300.0,      # Close idle after 5 min
        max_lifetime=3600.0, # Recreate after 1 hour
        
        # Connection parameters
        kwargs={
            "row_factory": dict_row,  # Return dicts
            "autocommit": False,       # Manual transactions
            "prepare_threshold": 5,    # Auto-prepare queries
        },
        
        # Callbacks
        configure=configure_connection,  # Called on new connections
        check=check_connection,          # Health check
        reset=reset_connection,          # Reset before returning to pool
        
        # Don't open immediately
        open=False
    )
    
    # Open pool
    pool.open()
    logger.info(f"✅ Connection pool opened: {pool.name}")

def configure_connection(conn):
    """Configure new connections."""
    with conn.cursor() as cur:
        cur.execute("SET application_name = 'my-app'")
        cur.execute("SET statement_timeout = '30s'")
        cur.execute("SET timezone = 'UTC'")

def check_connection(conn):
    """Health check for pooled connections."""
    with conn.cursor() as cur:
        cur.execute("SELECT 1")

def reset_connection(conn):
    """Reset connection before returning to pool."""
    # Rollback any uncommitted transaction
    if not conn.autocommit:
        conn.rollback()

def close_pool():
    """Close the pool."""
    if pool:
        pool.close()
        logger.info("✅ Connection pool closed")

# Initialize on app startup
init_pool()

# Usage throughout application
def get_user(user_id):
    with pool.connection() as conn:
        with conn.cursor() as cur:
            cur.execute("SELECT * FROM users WHERE id = %s", (user_id,))
            return cur.fetchone()

# Close on shutdown
# close_pool()
```

### Framework Integration

**FastAPI Example:**

```python
from fastapi import FastAPI
from contextlib import asynccontextmanager
from psycopg_pool import AsyncConnectionPool

# Global pool
pool: AsyncConnectionPool = None

@asynccontextmanager
async def lifespan(app: FastAPI):
    """Manage pool lifecycle."""
    global pool
    
    # Startup
    pool = AsyncConnectionPool(
        conninfo="postgresql://user:password@localhost/mydb",
        min_size=5,
        max_size=20,
        open=False
    )
    await pool.open()
    print("✅ Database pool opened")
    
    yield  # App runs
    
    # Shutdown
    await pool.close()
    print("✅ Database pool closed")

# Create FastAPI app
app = FastAPI(lifespan=lifespan)

@app.get("/users/{user_id}")
async def get_user(user_id: int):
    async with pool.connection() as conn:
        async with conn.cursor() as cur:
            await cur.execute(
                "SELECT * FROM users WHERE id = %s",
                (user_id,)
            )
            user = await cur.fetchone()
            return user
```

**Flask Example:**

```python
from flask import Flask, g
from psycopg_pool import ConnectionPool

app = Flask(__name__)

# Create pool on app startup
pool = ConnectionPool(
    conninfo="postgresql://user:password@localhost/mydb",
    min_size=5,
    max_size=20
)

@app.teardown_appcontext
def close_pool(error):
    """Close pool on shutdown."""
    pool.close()

def get_db():
    """Get connection from pool."""
    if 'db' not in g:
        g.db = pool.connection()
    return g.db

@app.route('/users/<int:user_id>')
def get_user(user_id):
    conn = get_db()
    with conn.cursor() as cur:
        cur.execute("SELECT * FROM users WHERE id = %s", (user_id,))
        user = cur.fetchone()
        return user
```

### Async Connection Pool

```python
from psycopg_pool import AsyncConnectionPool
import asyncio

async def main():
    # Create async pool
    pool = AsyncConnectionPool(
        conninfo="postgresql://user:password@localhost/mydb",
        min_size=5,
        max_size=20,
        open=False  # Don't open in constructor
    )
    
    # Open pool
    await pool.open()
    
    try:
        # Use pool
        async with pool.connection() as conn:
            async with conn.cursor() as cur:
                await cur.execute("SELECT * FROM users")
                users = await cur.fetchall()
                print(f"Found {len(users)} users")
    
    finally:
        # Close pool
        await pool.close()

asyncio.run(main())
```

### Pool Monitoring

```python
# Get pool stats
stats = pool.get_stats()
print(f"Pool size: {stats['pool_size']}")
print(f"Pool available: {stats['pool_available']}")
print(f"Requests waiting: {stats['requests_waiting']}")

# Check pool health
info = pool.get_stats()
if info['pool_available'] == 0:
    logger.warning("⚠️ No connections available!")

# Configure pool monitoring
import time

def monitor_pool():
    """Monitor pool health."""
    while True:
        stats = pool.get_stats()
        logger.info(
            f"Pool: {stats['pool_size']} total, "
            f"{stats['pool_available']} available, "
            f"{stats['requests_waiting']} waiting"
        )
        time.sleep(60)  # Check every minute

# Run in background thread
import threading
monitor_thread = threading.Thread(target=monitor_pool, daemon=True)
monitor_thread.start()
```


## 🎨 Row Factories

Row factories let you customize how query results are returned!

### Built-in Row Factories

```python
from psycopg import connect
from psycopg.rows import (
    tuple_row,      # Default - returns tuples
    dict_row,       # Returns dictionaries
    namedtuple_row, # Returns named tuples
    class_row,      # Returns custom class instances
    scalar_row      # Returns single value (first column)
)

# Tuple row (default)
with connect("dbname=mydb") as conn:
    cur = conn.cursor(row_factory=tuple_row)
    cur.execute("SELECT id, name, email FROM users WHERE id = 1")
    user = cur.fetchone()
    print(user)  # (1, 'Alice', 'alice@example.com')
    print(user[0])  # 1

# Dictionary row (most common)
with connect("dbname=mydb") as conn:
    cur = conn.cursor(row_factory=dict_row)
    cur.execute("SELECT id, name, email FROM users WHERE id = 1")
    user = cur.fetchone()
    print(user)  # {'id': 1, 'name': 'Alice', 'email': 'alice@example.com'}
    print(user['name'])  # 'Alice'

# Named tuple row
with connect("dbname=mydb") as conn:
    cur = conn.cursor(row_factory=namedtuple_row)
    cur.execute("SELECT id, name, email FROM users WHERE id = 1")
    user = cur.fetchone()
    print(user)  # Row(id=1, name='Alice', email='alice@example.com')
    print(user.name)  # 'Alice'

# Scalar row (single value)
with connect("dbname=mydb") as conn:
    cur = conn.cursor(row_factory=scalar_row)
    cur.execute("SELECT COUNT(*) FROM users")
    count = cur.fetchone()
    print(count)  # 42 (just the number, not a tuple)
```

### Class Row Factory

```python
from dataclasses import dataclass
from psycopg.rows import class_row

@dataclass
class User:
    id: int
    name: str
    email: str
    age: int = None

# Use class_row factory
with connect("dbname=mydb") as conn:
    cur = conn.cursor(row_factory=class_row(User))
    cur.execute("SELECT id, name, email, age FROM users WHERE id = 1")
    user = cur.fetchone()
    
    print(user)  # User(id=1, name='Alice', email='alice@example.com', age=28)
    print(type(user))  # <class '__main__.User'>
    print(user.name)  # 'Alice'

# Works with fetchall too!
with connect("dbname=mydb") as conn:
    cur = conn.cursor(row_factory=class_row(User))
    cur.execute("SELECT id, name, email, age FROM users")
    users = cur.fetchall()
    
    for user in users:
        print(f"{user.name} ({user.age} years old)")
```

### Custom Row Factory

```python
def custom_row_factory(cursor):
    """Custom row factory example."""
    column_names = [desc[0] for desc in cursor.description]
    
    def make_row(values):
        # Create custom object
        row = {}
        for name, value in zip(column_names, values):
            # Convert to uppercase
            row[name.upper()] = value
        return row
    
    return make_row

# Use custom factory
with connect("dbname=mydb") as conn:
    cur = conn.cursor(row_factory=custom_row_factory)
    cur.execute("SELECT id, name, email FROM users WHERE id = 1")
    user = cur.fetchone()
    print(user)  # {'ID': 1, 'NAME': 'Alice', 'EMAIL': 'alice@example.com'}
```

### Setting Row Factory on Connection

```python
from psycopg.rows import dict_row

# Set for entire connection
conn = connect("dbname=mydb", row_factory=dict_row)

# All cursors will return dicts
cur1 = conn.cursor()
cur2 = conn.cursor()

cur1.execute("SELECT * FROM users WHERE id = 1")
user = cur1.fetchone()
print(type(user))  # <class 'dict'>

# Override for specific cursor
from psycopg.rows import namedtuple_row

cur3 = conn.cursor(row_factory=namedtuple_row)
cur3.execute("SELECT * FROM users WHERE id = 1")
user = cur3.fetchone()
print(type(user))  # <class 'Row'>
```

---

## 🚄 Performance Optimization

### COPY for Bulk Operations

COPY is the **fastest** way to load data into PostgreSQL!

```python
from psycopg import connect
from io import StringIO

# COPY FROM (bulk insert)
data = StringIO("""1,Alice,alice@example.com,28
2,Bob,bob@example.com,35
3,Charlie,charlie@example.com,42
""")

with connect("dbname=mydb") as conn:
    with conn.cursor() as cur:
        # COPY FROM StringIO
        with cur.copy("COPY users (id, name, email, age) FROM STDIN") as copy:
            copy.write(data.read())
        
        print(f"✅ Copied {cur.rowcount} rows")

# COPY FROM with Python data
users = [
    (4, "Diana", "diana@example.com", 30),
    (5, "Eve", "eve@example.com", 25),
    (6, "Frank", "frank@example.com", 33)
]

with connect("dbname=mydb") as conn:
    with conn.cursor() as cur:
        with cur.copy("COPY users (id, name, email, age) FROM STDIN") as copy:
            for user in users:
                # Write tab-separated values
                copy.write_row(user)

# COPY TO (export data)
with connect("dbname=mydb") as conn:
    with conn.cursor() as cur:
        with cur.copy("COPY users TO STDOUT") as copy:
            for row in copy:
                print(row)  # Tab-separated values

# COPY with format
with connect("dbname=mydb") as conn:
    with conn.cursor() as cur:
        # CSV format
        with cur.copy(
            "COPY users TO STDOUT (FORMAT CSV, HEADER TRUE)"
        ) as copy:
            for row in copy:
                print(row)
```

**Performance Comparison:**
```
Method              | Rows/Second
--------------------|-------------
INSERT (single)     | ~1,000
executemany()       | ~10,000
COPY               | ~100,000+
```

### Prepared Statements (Automatic!)

Psycopg3 automatically prepares frequently-used queries!

```python
conn = connect("dbname=mydb", prepare_threshold=5)

# First 4 executions: normal
# 5th execution: prepared!
# Future executions: use prepared statement

for i in range(10):
    with conn.cursor() as cur:
        cur.execute("SELECT * FROM users WHERE id = %s", (i,))
        user = cur.fetchone()

# Manual prepare
with conn.cursor() as cur:
    # Force prepare immediately
    cur.execute(
        "SELECT * FROM users WHERE id = %s",
        (1,),
        prepare=True  # Prepare on first execution!
    )
    
    # Don't prepare
    cur.execute(
        "SELECT * FROM temp_table WHERE id = %s",
        (1,),
        prepare=False  # Never prepare
    )

# Disable auto-prepare globally
conn = connect("dbname=mydb", prepare_threshold=None)
```

### Pipeline Mode (Batching)

Pipeline mode lets you send multiple queries at once!

```python
# Regular mode (slow - round trip for each query)
with connect("dbname=mydb") as conn:
    cur = conn.cursor()
    
    for i in range(1000):
        cur.execute("INSERT INTO logs (message) VALUES (%s)", (f"Log {i}",))
    # 1000 round trips!

# Pipeline mode (fast - batched)
with connect("dbname=mydb") as conn:
    with conn.pipeline():
        cur = conn.cursor()
        
        for i in range(1000):
            cur.execute("INSERT INTO logs (message) VALUES (%s)", (f"Log {i}",))
        
        # All sent together - much faster!

# Pipeline with results
with connect("dbname=mydb") as conn:
    with conn.pipeline():
        cur = conn.cursor()
        
        # Queue multiple queries
        for i in range(10):
            cur.execute("SELECT * FROM users WHERE id = %s", (i,))
        
    # Fetch all results (after pipeline closes)
    for i in range(10):
        result = cur.fetchone()
        print(result)
```

**Performance gain:** 2-10x faster for bulk operations!

### Indexing Best Practices

```sql
-- Create indexes for frequently queried columns
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_created_at ON users(created_at);

-- Composite indexes
CREATE INDEX idx_users_status_created ON users(status, created_at);

-- Partial indexes (smaller, faster)
CREATE INDEX idx_active_users ON users(id) WHERE status = 'active';

-- Expression indexes
CREATE INDEX idx_users_lower_email ON users(LOWER(email));

-- EXPLAIN to check index usage
EXPLAIN ANALYZE SELECT * FROM users WHERE email = 'alice@example.com';
```

### Query Optimization Tips

```python
# ✅ Use LIMIT
cur.execute("SELECT * FROM users ORDER BY created_at DESC LIMIT 100")

# ✅ Select only needed columns
cur.execute("SELECT id, name FROM users")  # Not SELECT *

# ✅ Use indexes
cur.execute("SELECT * FROM users WHERE email = %s", ("alice@example.com",))

# ✅ Batch operations
users = [(f"user{i}", f"user{i}@example.com") for i in range(1000)]
cur.executemany("INSERT INTO users (name, email) VALUES (%s, %s)", users)

# ✅ Use transactions
with conn.transaction():
    for i in range(1000):
        cur.execute("INSERT INTO logs (msg) VALUES (%s)", (f"Log {i}",))
# Commit once

# ❌ Avoid N+1 queries
# Bad
for user_id in user_ids:
    cur.execute("SELECT * FROM users WHERE id = %s", (user_id,))

# Good
cur.execute("SELECT * FROM users WHERE id = ANY(%s)", (user_ids,))
```


*Final sections coming up...*

## 🏭 Production Patterns

### Database Migrations

```python
# Simple migration system
import psycopg

def run_migration(conn, migration_sql):
    """Run a database migration."""
    with conn.transaction():
        cur = conn.cursor()
        cur.execute(migration_sql)
        print("✅ Migration completed")

# Example migration
migration_001 = """
CREATE TABLE IF NOT EXISTS schema_migrations (
    version INT PRIMARY KEY,
    applied_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE IF NOT EXISTS users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    created_at TIMESTAMP DEFAULT NOW()
);
"""

with psycopg.connect("dbname=mydb") as conn:
    run_migration(conn, migration_001)

# Check which migrations have run
def get_applied_migrations(conn):
    cur = conn.cursor()
    cur.execute("SELECT version FROM schema_migrations ORDER BY version")
    return [row[0] for row in cur.fetchall()]

# Apply pending migrations
def apply_migrations(conn, migrations):
    applied = get_applied_migrations(conn)
    
    for version, sql in migrations.items():
        if version not in applied:
            print(f"Applying migration {version}...")
            with conn.transaction():
                cur = conn.cursor()
                cur.execute(sql)
                cur.execute(
                    "INSERT INTO schema_migrations (version) VALUES (%s)",
                    (version,)
                )
            print(f"✅ Migration {version} applied")
```

### Health Checks

```python
def health_check(conn):
    """Check database health."""
    try:
        cur = conn.cursor()
        cur.execute("SELECT 1")
        result = cur.fetchone()
        return result[0] == 1
    except Exception as e:
        print(f"❌ Health check failed: {e}")
        return False

# FastAPI health endpoint
from fastapi import FastAPI, status

app = FastAPI()

@app.get("/health")
async def health():
    async with pool.connection() as conn:
        if await health_check(conn):
            return {"status": "healthy"}
        else:
            return {"status": "unhealthy"}, status.HTTP_503_SERVICE_UNAVAILABLE
```

### Retry Logic

```python
import time
from psycopg import OperationalError

def execute_with_retry(conn, query, params, max_retries=3):
    """Execute query with automatic retry on transient failures."""
    for attempt in range(max_retries):
        try:
            cur = conn.cursor()
            cur.execute(query, params)
            return cur.fetchall()
        except OperationalError as e:
            if attempt < max_retries - 1:
                wait_time = 2 ** attempt  # Exponential backoff
                print(f"⚠️ Attempt {attempt + 1} failed, retrying in {wait_time}s...")
                time.sleep(wait_time)
            else:
                print(f"❌ All {max_retries} attempts failed")
                raise

# Usage
with connect("dbname=mydb") as conn:
    results = execute_with_retry(
        conn,
        "SELECT * FROM users WHERE id = %s",
        (1,)
    )
```

### LISTEN/NOTIFY

Real-time pub/sub with PostgreSQL!

```python
import psycopg

# Publisher
def send_notification(channel, payload):
    """Send notification to channel."""
    with psycopg.connect("dbname=mydb", autocommit=True) as conn:
        cur = conn.cursor()
        # Use pg_notify() for parameterized notification
        cur.execute("SELECT pg_notify(%s, %s)", (channel, payload))
        print(f"📢 Sent: {payload}")

# Subscriber
def listen_for_notifications(channel):
    """Listen for notifications on channel."""
    with psycopg.connect("dbname=mydb", autocommit=True) as conn:
        cur = conn.cursor()
        cur.execute(f"LISTEN {channel}")
        
        print(f"👂 Listening on channel: {channel}")
        
        # Iterate through notifications
        for notify in conn.notifies():
            print(f"📬 Received: {notify.channel} - {notify.payload}")
            
            # Process notification
            if notify.payload == "stop":
                print("🛑 Stopping listener")
                break

# Run in separate processes/threads
import threading

listener = threading.Thread(
    target=listen_for_notifications,
    args=("events",),
    daemon=True
)
listener.start()

# Send notifications
time.sleep(1)
send_notification("events", "Hello!")
send_notification("events", "World!")
send_notification("events", "stop")

listener.join()
```

### Server-Side Cursors (Large Results)

```python
# Named cursor = server-side cursor
# Perfect for large result sets!

with psycopg.connect("dbname=mydb") as conn:
    # Create server-side cursor
    with conn.cursor(name="large_query") as cur:
        cur.execute("SELECT * FROM huge_table")
        
        # Fetch in batches
        while True:
            rows = cur.fetchmany(1000)  # Fetch 1000 at a time
            if not rows:
                break
            
            # Process batch
            process_batch(rows)

# Scrollable server cursor
with psycopg.connect("dbname=mydb") as conn:
    with conn.cursor(name="scrollable", scrollable=True) as cur:
        cur.execute("SELECT * FROM users")
        
        # Forward
        cur.scroll(10)  # Skip 10 rows
        row = cur.fetchone()
        
        # Backward
        cur.scroll(-5)  # Go back 5 rows
        row = cur.fetchone()
```


## 📚 Additional Resources

### Official Documentation
- **Psycopg3 Docs**: https://www.psycopg.org/psycopg3/docs/
- **PostgreSQL Docs**: https://www.postgresql.org/docs/
- **Python DB-API**: https://peps.python.org/pep-0249/

### Best Practices Checklist

✅ **Connection Management**
- Use connection pools in production
- Always use context managers
- Set appropriate timeouts
- Enable autocommit for DDL/LISTEN

✅ **Security**
- Always use parameterized queries
- Never concatenate user input into SQL
- Use least-privilege database users
- Enable SSL for remote connections

✅ **Performance**
- Use COPY for bulk inserts
- Create appropriate indexes
- Use LIMIT for large result sets
- Leverage prepared statements
- Use pipeline mode for batches

✅ **Reliability**
- Implement retry logic
- Use transactions appropriately
- Handle errors gracefully
- Monitor connection pool health
- Set up health checks

✅ **Code Quality**
- Use type hints
- Use row factories for clean code
- Keep queries in dedicated files
- Write database tests
- Document complex queries


## 🎉 Conclusion

Congratulations! You've mastered Psycopg3! You now know:

- ✅ How to connect to PostgreSQL efficiently
- ✅ CRUD operations and advanced querying
- ✅ Async operations with asyncio
- ✅ Connection pooling for production
- ✅ Performance optimization techniques
- ✅ Real-world production patterns

**Next Steps:**
1. Build a project with Psycopg3
2. Explore PostgreSQL advanced features (JSONB, Full-Text Search, PostGIS)
3. Learn about ORMs (SQLAlchemy, etc.)
4. Set up monitoring and logging
5. Optimize your database schema

**Happy Coding! 🐘✨**

---

## 💝 Contributing

Found an error? Have a suggestion? Contributions welcome!

1. Fork the repository
2. Create your feature branch
3. Commit changes
4. Create a Pull Request


## Quick Reference

```python
# Connection
import psycopg
conn = psycopg.connect("dbname=mydb")

# Cursor
cur = conn.cursor()

# Execute
cur.execute("SELECT * FROM users WHERE id = %s", (1,))

# Fetch
user = cur.fetchone()
users = cur.fetchall()

# Insert
cur.execute(
    "INSERT INTO users (name) VALUES (%s) RETURNING id",
    ("Alice",)
)
user_id = cur.fetchone()[0]

# Update
cur.execute("UPDATE users SET age = %s WHERE id = %s", (30, 1))

# Delete
cur.execute("DELETE FROM users WHERE id = %s", (1,))

# Transaction
with conn.transaction():
    cur.execute("...")
    cur.execute("...")

# Async
async with await psycopg.AsyncConnection.connect("dbname=mydb") as conn:
    async with conn.cursor() as cur:
        await cur.execute("SELECT * FROM users")
        users = await cur.fetchall()

# Pool
from psycopg_pool import ConnectionPool
pool = ConnectionPool("dbname=mydb", min_size=5, max_size=20)
with pool.connection() as conn:
    with conn.cursor() as cur:
        cur.execute("SELECT * FROM users")
```

---

<div align="center">
  
**Made with ❤️ for the Python Community by @RajeshTechForge**

</div>