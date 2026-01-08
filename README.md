<div align="center">

# Awesome PyMongo [![Awesome](https://cdn.rawgit.com/sindresorhus/awesome/d7305f38d29fed78fa85652e3a63e154dd8e8829/media/badge.svg)](https://github.com/sindresorhus/awesome)

 🚀 **The Ultimate Guide to Mastering MongoDB with Python (2026 Edition)**

<br>
<br>
</div>

Welcome to the most comprehensive, up-to-date PyMongo tutorial! This guide will take you from MongoDB novice to production-ready developer, with practical examples, best practices, and modern patterns for 2026.

**✨ What's New in This Edition:**
- Updated for **PyMongo 4.15.5+** and **MongoDB 8.0**
- Modern **Python 3.9+** features and type hints
- Production-ready patterns and best practices
- Performance optimization techniques  
- Real-world examples and use cases
- Comprehensive aggregation pipeline guide
- Advanced indexing strategies

*Inspired by [awesome-python](https://github.com/vinta/awesome-python)* ✨


## 📋 Quick Navigation

- **[Introduction](#-introduction)** - What & Why
- **[Getting Started](#-getting-started)** - Installation & Setup
- **[Core Concepts](#-core-concepts)** - Connecting & Basics
- **[CRUD Operations](#-crud-operations)** - Create, Read, Update, Delete
- **[Advanced Querying](#-advanced-querying)** - Operators & Filters
- **[Aggregation Pipeline](#-aggregation-pipeline)** - Data Processing & Analysis
- **[Indexing & Performance](#-indexing--performance)** - Speed Optimization
- **[Connection Management](#-connection-management)** - Pooling & Error Handling
- **[Production Patterns](#-production-patterns)** - Real-world Applications
- **[Resources](#-additional-resources)** - Learn More

---

## 🎯 Introduction

### What is MongoDB?

[MongoDB](https://www.mongodb.com/) is a powerful NoSQL database that stores data in flexible, JSON-like documents (BSON format). Unlike traditional SQL databases:

- **Flexible Schema**: No rigid table structures
- **Document-Oriented**: Store related data together
- **Scalable**: Horizontal scaling with sharding
- **High Performance**: Optimized for modern applications

**Think of it like this:**
```
Traditional SQL:         MongoDB:
┌──────────┐             ┌──────────┐
│  Tables  │             │ Database │
├──────────┤             ├──────────┤
│   Rows   │    vs       │Collection│
├──────────┤             ├──────────┤
│ Columns  │             │ Documents│
└──────────┘             └──────────┘
```

### What is PyMongo?

[PyMongo](https://pymongo.readthedocs.io/) is the **official** Python driver for MongoDB. It's your bridge between Python code and MongoDB databases.

```python
# Simple, Pythonic, Powerful
from pymongo import MongoClient

client = MongoClient('mongodb://localhost:27017/')
db = client.my_database
users = db.users

# That's it! You're connected 🎸
```

### Why PyMongo Rocks in 2026

1. **🎨 Pythonic API** - Natural, intuitive syntax
2. **⚡ High Performance** - C-optimized core components
3. **🔒 Auto Connection Pooling** - Built-in connection management
4. **🔄 Async Ready** - Works with asyncio/Motor
5. **🛡️ Type-Safe** - Full type hint support
6. **📦 GridFS** - Handle large files easily
7. **🔐 Enterprise Security** - Authentication & encryption
8. **🎯 Latest Support** - MongoDB 4.2 through 8.0

**Current Version:** PyMongo 4.15.5+
- ✅ Python 3.9+ required
- ✅ Free-threaded Python support
- ✅ Enhanced connection pooling


## 🚀 Getting Started

### Prerequisites Checklist

- [ ] Python 3.9 or higher
- [ ] MongoDB Server or Atlas account
- [ ] Basic Python & database knowledge

### Installing MongoDB

**Option 1: Local Installation**

**macOS:**
```bash
brew tap mongodb/brew
brew install mongodb-community
brew services start mongodb-community
```

**Ubuntu/Debian:**
```bash
wget -qO - https://www.mongodb.org/static/pgp/server-7.0.asc | sudo apt-key add -
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu $(lsb_release -cs)/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
sudo apt-get update
sudo apt-get install -y mongodb-org
sudo systemctl start mongod
```

**Windows:**
Download from [MongoDB Download Center](https://www.mongodb.com/try/download/community) and follow the installer.

**Option 2: MongoDB Atlas (Recommended for Beginners)**

☁️ **Free cloud database - no credit card required!**

1. Sign up at [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
2. Create a free cluster (~5-10 minutes)
3. Create database user & whitelist IP
4. Get connection string
5. Done! 🎉

**Atlas Benefits:**
- ✅ Free tier (512MB storage)
- ✅ Automatic backups
- ✅ Built-in monitoring
- ✅ Global deployment
- ✅ Zero maintenance

### Installing PyMongo

```bash
# Basic installation
pip install pymongo

# With optional dependencies
pip install pymongo[encryption,aws,srv,snappy,zstd]

# Verify installation
python -c "import pymongo; print(f'PyMongo {pymongo.version}')"
```

**🎓 Pro Tip: Use Virtual Environments!**

```bash
# Create project
mkdir awesome_mongo_app
cd awesome_mongo_app

# Create & activate venv
python -m venv venv
source venv/bin/activate  # Linux/macOS
# venv\Scripts\activate    # Windows

# Install PyMongo
pip install pymongo

# Save dependencies
pip freeze > requirements.txt
```


## 🔧 Core Concepts

### Connecting to MongoDB

#### Quick Connect (Local)

```python
from pymongo import MongoClient

# Simple connection
client = MongoClient()  # localhost:27017

# Explicit connection
client = MongoClient('mongodb://localhost:27017/')

# Test connection
try:
    client.admin.command('ping')
    print("✅ Connected to MongoDB!")
except Exception as e:
    print(f"❌ Connection failed: {e}")
```

#### Cloud Connect (Atlas)

```python
from pymongo import MongoClient

# Atlas connection string
uri = "mongodb+srv://user:password@cluster.mongodb.net/?retryWrites=true&w=majority"

client = MongoClient(uri)
print("✅ Connected to MongoDB Atlas!")
```

#### Production-Ready Connection

```python
from pymongo import MongoClient
from pymongo.errors import ConnectionFailure
import logging

logger = logging.getLogger(__name__)

def get_mongo_client():
    """Production-grade MongoDB client with full configuration."""
    try:
        client = MongoClient(
            'mongodb://localhost:27017/',
            # Timeouts
            serverSelectionTimeoutMS=5000,
            connectTimeoutMS=10000,
            socketTimeoutMS=20000,
            # Connection Pool
            maxPoolSize=50,
            minPoolSize=10,
            maxIdleTimeMS=45000,
            waitQueueTimeoutMS=5000,
            # Reliability
            retryWrites=True,
            w='majority',
            # Application identifier
            appName='my-awesome-app'
        )
        
        # Verify connection
        client.admin.command('ping')
        logger.info("✅ MongoDB connection established")
        return client
        
    except ConnectionFailure as e:
        logger.error(f"❌ MongoDB connection failed: {e}")
        raise

# Usage
client = get_mongo_client()
db = client.my_database
```

#### Context Manager (Best Practice!)

```python
from pymongo import MongoClient

# Automatic resource cleanup
with MongoClient('mongodb://localhost:27017/') as client:
    db = client.my_database
    users = db.users
    
    # Do your operations
    user = users.find_one({'name': 'Alice'})
    print(user)
    
# Connection automatically closed!
```

**🎯 Connection Best Practices:**

1. ✅ Use **one MongoClient per application** (thread-safe!)
2. ✅ Use **context managers** for automatic cleanup
3. ✅ Configure **appropriate timeouts**
4. ✅ Handle **connection errors** gracefully
5. ✅ Use **connection pooling** (automatic in PyMongo)

### Database & Collection Basics

**MongoDB Structure:**
```
Server
└── Database (my_app)
    ├── Collection (users)
    │   ├── Document { _id: 1, name: "Alice" }
    │   ├── Document { _id: 2, name: "Bob" }
    │   └── Document { _id: 3, name: "Charlie" }
    └── Collection (products)
        ├── Document { _id: 1, name: "Widget" }
        └── Document { _id: 2, name: "Gadget" }
```

**Accessing Databases & Collections:**

```python
from pymongo import MongoClient

client = MongoClient('mongodb://localhost:27017/')

# Dictionary-style (recommended for dynamic names)
db = client['my_database']
users = db['users']

# Attribute-style (cleaner for static names)
db = client.my_database
users = db.users

# Using methods
db = client.get_database('my_database')
users = db.get_collection('users')
```

**Important:** 🚨 Databases and collections are created **lazily** (on first write)!

```python
# This doesn't create anything yet
db = client.new_database
collection = db.new_collection

# Created here when first document is inserted!
collection.insert_one({'hello': 'world'})
```

**Listing & Managing:**

```python
# List all databases
print(client.list_database_names())

# List collections in a database
print(db.list_collection_names())

# Check existence
if 'users' in db.list_collection_names():
    print("✅ Collection exists!")

# Get stats
stats = db.command('dbStats')
print(f"Database size: {stats['dataSize']} bytes")

# Drop (use with caution!)
db.old_collection.drop()
client.drop_database('old_database')
```

### Understanding Document IDs

Every MongoDB document has a unique `_id` field.

**Auto-Generated ObjectIds:**

```python
from bson.objectid import ObjectId
from datetime import datetime

# Auto-generated by MongoDB
doc = {'name': 'Alice', 'age': 30}
result = collection.insert_one(doc)

print(f"ID: {result.inserted_id}")  # ObjectId('...')

# ObjectIds contain timestamps!
obj_id = result.inserted_id
creation_time = obj_id.generation_time
print(f"Created: {creation_time}")
```

**ObjectId Structure:**
- 12 bytes total
- 4 bytes: timestamp
- 5 bytes: random value  
- 3 bytes: counter

**Custom IDs:**

```python
# String ID
custom_doc = {
    '_id': 'user_alice_123',
    'name': 'Alice',
    'email': 'alice@example.com'
}
collection.insert_one(custom_doc)

# Integer ID
numbered_doc = {
    '_id': 12345,
    'name': 'Bob'
}
collection.insert_one(numbered_doc)

# UUID ID
import uuid
uuid_doc = {
    '_id': str(uuid.uuid4()),
    'name': 'Charlie'
}
collection.insert_one(uuid_doc)
```

**⚠️ Important:** IDs must be unique! Duplicates raise `DuplicateKeyError`.

**Working with ObjectIds:**

```python
from bson.objectid import ObjectId
from datetime import datetime, timedelta

# Create ObjectId
obj_id = ObjectId()

# Convert string to ObjectId
obj_id = ObjectId("507f1f77bcf86cd799439011")

# Find by ObjectId
user = collection.find_one({'_id': obj_id})

# Find documents from last hour
one_hour_ago = datetime.now() - timedelta(hours=1)
recent_id = ObjectId.from_datetime(one_hour_ago)
recent_docs = collection.find({'_id': {'$gte': recent_id}})

# Validate ObjectId string
def is_valid_object_id(id_string):
    try:
        ObjectId(id_string)
        return True
    except:
        return False
```


## 📝 CRUD Operations

### Create: Inserting Documents

#### Insert One

```python
from datetime import datetime

# Single document
user = {
    'name': 'Alice Johnson',
    'email': 'alice@example.com',
    'age': 28,
    'role': 'developer',
    'skills': ['Python', 'MongoDB', 'FastAPI'],
    'address': {
        'city': 'New York',
        'zip': '10001'
    },
    'created_at': datetime.now(),
    'is_active': True
}

result = users.insert_one(user)
print(f"✅ Inserted ID: {result.inserted_id}")

# Document now has _id
print(user['_id'])
```

#### Insert Many

```python
# Multiple documents
new_users = [
    {
        'name': 'Bob Smith',
        'email': 'bob@example.com',
        'age': 35,
        'role': 'designer',
        'skills': ['UI/UX', 'Figma'],
        'created_at': datetime.now()
    },
    {
        'name': 'Charlie Brown',
        'email': 'charlie@example.com',
        'age': 42,
        'role': 'manager',
        'skills': ['Leadership', 'Agile'],
        'created_at': datetime.now()
    },
    {
        'name': 'Diana Prince',
        'email': 'diana@example.com',
        'age': 30,
        'role': 'developer',
        'skills': ['JavaScript', 'React'],
        'created_at': datetime.now()
    }
]

result = users.insert_many(new_users)
print(f"✅ Inserted {len(result.inserted_ids)} documents")
print(f"IDs: {result.inserted_ids}")
```

#### Ordered vs Unordered

```python
# Ordered (default) - Stops on first error
try:
    users.insert_many([
        {'_id': 1, 'name': 'User 1'},
        {'_id': 2, 'name': 'User 2'},
        {'_id': 1, 'name': 'Duplicate!'},  # Fails here
        {'_id': 3, 'name': 'User 3'}       # Not inserted
    ], ordered=True)
except Exception as e:
    print("❌ Ordered insert stopped at error")

# Unordered - Continues despite errors
try:
    users.insert_many([
        {'_id': 10, 'name': 'User 10'},
        {'_id': 20, 'name': 'User 20'},
        {'_id': 10, 'name': 'Duplicate!'},  # Skipped
        {'_id': 30, 'name': 'User 30'}      # Still inserted!
    ], ordered=False)
except Exception as e:
    print("⚠️ Unordered insert completed partial insertions")
```

**🎯 Pro Tip:** Use `ordered=False` for bulk inserts to maximize successful insertions.

### Read: Finding Documents

#### Find One

```python
# Find first match
user = users.find_one({'name': 'Alice Johnson'})
if user:
    print(f"{user['name']} - {user['email']}")

# Find by _id
user = users.find_one({'_id': ObjectId('507f...')})

# Multiple conditions
developer = users.find_one({
    'role': 'developer',
    'age': {'$gte': 25},
    'is_active': True
})

# Returns None if not found
result = users.find_one({'name': 'Nobody'})
print(result)  # None
```

#### Find Many

```python
# Find all
for user in users.find():
    print(user['name'])

# Find with filter
developers = users.find({'role': 'developer'})
for dev in developers:
    print(f"{dev['name']} - {dev['skills']}")

# Complex query
experienced = users.find({
    'age': {'$gte': 30},
    'role': {'$in': ['developer', 'manager']},
    'is_active': True
})

# Convert to list
user_list = list(users.find({'role': 'developer'}))
print(f"Found {len(user_list)} developers")
```

#### Count Documents

```python
# Count all
total = users.count_documents({})
print(f"Total users: {total}")

# Count with filter
active_devs = users.count_documents({
    'role': 'developer',
    'is_active': True
})

# Estimated count (faster, less accurate)
estimated = users.estimated_document_count()
```

**⚠️ Deprecated:** Old `count()` is deprecated! Use `count_documents({})`.

### Update: Modifying Documents

#### Update One

```python
# Update single field
result = users.update_one(
    {'name': 'Alice Johnson'},
    {'$set': {'age': 29}}
)
print(f"Modified: {result.modified_count}")

# Update multiple fields
users.update_one(
    {'email': 'bob@example.com'},
    {
        '$set': {
            'age': 36,
            'status': 'active',
            'updated_at': datetime.now()
        }
    }
)

# Array operations
users.update_one(
    {'name': 'Alice'},
    {'$push': {'skills': 'Docker'}}  # Add to array
)

users.update_one(
    {'name': 'Alice'},
    {'$pull': {'skills': 'Old Skill'}}  # Remove from array
)

# Increment value
users.update_one(
    {'name': 'Alice'},
    {'$inc': {'age': 1, 'login_count': 1}}
)
```

#### Update Many

```python
# Update all matching
result = users.update_many(
    {'role': 'developer'},
    {
        '$set': {'department': 'Engineering'},
        '$currentDate': {'updated_at': True}
    }
)
print(f"✅ Updated {result.modified_count} documents")

# Update all (empty filter)
users.update_many(
    {},
    {'$set': {'company': 'Awesome Inc'}}
)
```

#### Upsert: Update or Insert

```python
# Insert if not exists, update if exists
result = users.update_one(
    {'email': 'new@example.com'},
    {
        '$set': {
            'name': 'New User',
            'role': 'trainee',
            'created_at': datetime.now()
        }
    },
    upsert=True  # Magic! ✨
)

if result.upserted_id:
    print(f"✅ Inserted: {result.upserted_id}")
else:
    print("✅ Updated existing document")
```

**Common Update Operators:**

| Operator | Description | Example |
|----------|-------------|---------|
| `$set` | Set field value | `{'$set': {'age': 30}}` |
| `$unset` | Remove field | `{'$unset': {'old_field': ''}}` |
| `$inc` | Increment | `{'$inc': {'views': 1}}` |
| `$push` | Add to array | `{'$push': {'tags': 'new'}}` |
| `$pull` | Remove from array | `{'$pull': {'tags': 'old'}}` |
| `$addToSet` | Add if not exists | `{'$addToSet': {'tags': 'unique'}}` |
| `$currentDate` | Set current date | `{'$currentDate': {'updated': True}}` |

### Delete: Removing Documents

#### Delete One

```python
# Delete first match
result = users.delete_one({'name': 'Bob'})
print(f"Deleted: {result.deleted_count}")

# Delete by _id
users.delete_one({'_id': ObjectId('...')})

# Returns 0 if not found
result = users.delete_one({'name': 'Nobody'})
print(result.deleted_count)  # 0
```

#### Delete Many

```python
# Delete all matching
result = users.delete_many({'role': 'trainee'})
print(f"✅ Deleted {result.deleted_count} trainees")

# Delete with filter
users.delete_many({
    'age': {'$gt': 65},
    'is_active': False
})

# Delete ALL (DANGEROUS!)
result = users.delete_many({})
```

#### Find and Delete

```python
# Atomic find and delete
deleted_user = users.find_one_and_delete(
    {'email': 'charlie@example.com'},
    projection={'name': 1, 'email': 1}
)

if deleted_user:
    print(f"✅ Deleted: {deleted_user['name']}")
    # Can use deleted data
    send_goodbye_email(deleted_user['email'])
```

**🎯 Soft Delete Pattern:**

```python
def soft_delete(user_id):
    """Mark as deleted instead of removing."""
    result = users.update_one(
        {'_id': user_id},
        {
            '$set': {
                'is_deleted': True,
                'deleted_at': datetime.now()
            }
        }
    )
    return result.modified_count > 0

# Query active users
active_users = users.find({'is_deleted': {'$ne': True}})
```


## 🔍 Advanced Querying

### Query Operators

#### Comparison

```python
# Equal
users.find({'age': 30})

# Not equal
users.find({'role': {'$ne': 'admin'}})

# Greater than/Less than
users.find({'age': {'$gt': 30}})   # >
users.find({'age': {'$gte': 30}})  # >=
users.find({'age': {'$lt': 30}})   # <
users.find({'age': {'$lte': 30}})  # <=

# In array
users.find({'role': {'$in': ['developer', 'designer']}})

# Not in array
users.find({'role': {'$nin': ['admin', 'moderator']}})

# Range
users.find({'age': {'$gte': 25, '$lte': 35}})
```

#### Logical

```python
# AND (implicit)
users.find({
    'role': 'developer',
    'age': {'$gte': 25}
})

# OR
users.find({
    '$or': [
        {'role': 'developer'},
        {'role': 'designer'}
    ]
})

# NOT
users.find({'age': {'$not': {'$lt': 18}}})

# NOR
users.find({
    '$nor': [
        {'role': 'admin'},
        {'status': 'inactive'}
    ]
})

# Complex
users.find({
    '$or': [
        {'role': 'developer', 'age': {'$gt': 25}},
        {'role': 'manager'}
    ],
    'status': 'active'
})
```

#### Element

```python
# Field exists
users.find({'phone': {'$exists': True}})
users.find({'deleted_at': {'$exists': False}})

# Type check
users.find({'age': {'$type': 'int'}})
users.find({'name': {'$type': 'string'}})
```

#### Array

```python
# Contains element
users.find({'skills': 'Python'})

# Contains any
users.find({'skills': {'$in': ['Python', 'JavaScript']}})

# Contains all
users.find({'skills': {'$all': ['Python', 'MongoDB']}})

# Array size
users.find({'skills': {'$size': 3}})

# Element match
users.find({
    'orders': {
        '$elemMatch': {
            'status': 'shipped',
            'total': {'$gt': 100}
        }
    }
})
```

#### Regular Expressions

```python
# Case-insensitive
users.find({
    'name': {'$regex': 'alice', '$options': 'i'}
})

# Starts with
users.find({'email': {'$regex': '^alice'}})

# Ends with
users.find({'email': {'$regex': '@gmail.com$'}})

# Contains
users.find({'name': {'$regex': 'john'}})

# Python re module
import re
pattern = re.compile('john', re.IGNORECASE)
users.find({'name': pattern})
```

### Projection & Field Selection

```python
# Include specific fields
users.find(
    {'role': 'developer'},
    {'name': 1, 'email': 1}  # _id included by default
)

# Exclude _id
users.find(
    {'role': 'developer'},
    {'name': 1, 'email': 1, '_id': 0}
)

# Exclude fields
users.find(
    {},
    {'password': 0, 'ssn': 0}
)

# Array slicing
users.find(
    {},
    {'skills': {'$slice': 3}}  # First 3
)

users.find(
    {},
    {'skills': {'$slice': -3}}  # Last 3
)

users.find(
    {},
    {'skills': {'$slice': [5, 10]}}  # Skip 5, take 10
)
```

### Sorting, Limiting, Skipping

```python
# Sort
users.find().sort('age', 1)   # Ascending
users.find().sort('age', -1)  # Descending

# Multiple fields
users.find().sort([
    ('role', 1),
    ('age', -1),
    ('name', 1)
])

# Limit
users.find().limit(10)

# Skip
users.find().skip(20).limit(10)

# Chaining
results = (users
    .find({'role': 'developer'})
    .sort('age', -1)
    .skip(0)
    .limit(10)
)
```

**Pagination Helper:**

```python
def paginate(collection, page=1, size=20, filter=None):
    """Paginate collection results."""
    filter = filter or {}
    skip = (page - 1) * size
    
    total = collection.count_documents(filter)
    results = collection.find(filter).skip(skip).limit(size)
    
    return {
        'data': list(results),
        'page': page,
        'size': size,
        'total': total,
        'pages': (total + size - 1) // size
    }

# Usage
page_1 = paginate(users, page=1, size=20, filter={'role': 'developer'})
```


## 📊 Aggregation Pipeline

The aggregation pipeline is MongoDB's superpower for data processing!

### Pipeline Basics

```python
# Basic structure
pipeline = [
    {'$match': {...}},     # Filter
    {'$group': {...}},     # Group & aggregate
    {'$sort': {...}},      # Sort
    {'$project': {...}}    # Shape output
]

results = collection.aggregate(pipeline)
```

### Common Stages

#### $match - Filter Documents

```python
pipeline = [
    {
        '$match': {
            'role': 'developer',
            'age': {'$gte': 25},
            'is_active': True
        }
    }
]

results = users.aggregate(pipeline)
```

#### $project - Select/Transform Fields

```python
pipeline = [
    {
        '$project': {
            'name': 1,
            'email': 1,
            'full_info': {
                '$concat': ['$name', ' (', '$role', ')']
            },
            'skills_count': {'$size': '$skills'},
            '_id': 0
        }
    }
]
```

#### $group - Aggregate Data

```python
pipeline = [
    {
        '$group': {
            '_id': '$role',  # Group by field
            'count': {'$sum': 1},
            'avg_age': {'$avg': '$age'},
            'users': {'$push': '$name'}
        }
    }
]

# Group by multiple fields
pipeline = [
    {
        '$group': {
            '_id': {
                'role': '$role',
                'department': '$department'
            },
            'count': {'$sum': 1}
        }
    }
]
```

**Accumulator Operators:**
- `$sum` - Sum values
- `$avg` - Average
- `$min` - Minimum
- `$max` - Maximum
- `$first` - First value
- `$last` - Last value
- `$push` - Array of all values
- `$addToSet` - Array of unique values

#### $sort - Sort Results

```python
pipeline = [
    {
        '$sort': {
            'age': -1,  # Descending
            'name': 1   # Ascending
        }
    }
]
```

#### $limit & $skip

```python
pipeline = [
    {'$match': {'role': 'developer'}},
    {'$sort': {'age': -1}},
    {'$skip': 10},
    {'$limit': 5}
]
```

#### $unwind - Deconstruct Arrays

```python
# Before: {'name': 'Alice', 'skills': ['Python', 'MongoDB']}
# After:  Two docs - one per skill

pipeline = [
    {'$unwind': '$skills'},
    {
        '$group': {
            '_id': '$skills',
            'count': {'$sum': 1}
        }
    },
    {'$sort': {'count': -1}}
]

# Most popular skills!
```

#### $lookup - Join Collections

```python
# Like SQL JOIN
pipeline = [
    {
        '$lookup': {
            'from': 'orders',           # Collection to join
            'localField': '_id',        # Field in users
            'foreignField': 'user_id',  # Field in orders
            'as': 'user_orders'         # Output array
        }
    },
    {'$unwind': '$user_orders'}  # Flatten
]

# Optimized lookup with pipeline
pipeline = [
    {
        '$lookup': {
            'from': 'orders',
            'let': {'user_id': '$_id'},
            'pipeline': [
                {
                    '$match': {
                        '$expr': {'$eq': ['$user_id', '$$user_id']},
                        'status': 'completed'  # Filter in lookup!
                    }
                },
                {'$project': {'total': 1, 'date': 1}}
            ],
            'as': 'completed_orders'
        }
    }
]
```

#### $addFields - Add New Fields

```python
pipeline = [
    {
        '$addFields': {
            'full_name': {
                '$concat': ['$first_name', ' ', '$last_name']
            },
            'is_senior': {
                '$cond': {
                    'if': {'$gte': ['$age', 30]},
                    'then': True,
                    'else': False
                }
            }
        }
    }
]
```

#### $bucket - Categorize

```python
pipeline = [
    {
        '$bucket': {
            'groupBy': '$age',
            'boundaries': [0, 20, 30, 40, 50, 100],
            'default': 'Other',
            'output': {
                'count': {'$sum': 1},
                'users': {'$push': '$name'}
            }
        }
    }
]
```

### Complete Example

```python
# Department statistics
pipeline = [
    # Filter active users from 2024+
    {
        '$match': {
            'is_active': True,
            'created_at': {'$gte': datetime(2024, 1, 1)}
        }
    },
    
    # Add computed field
    {
        '$addFields': {
            'skills_count': {'$size': '$skills'}
        }
    },
    
    # Group by department
    {
        '$group': {
            '_id': '$department',
            'employee_count': {'$sum': 1},
            'avg_age': {'$avg': '$age'},
            'total_skills': {'$sum': '$skills_count'},
            'roles': {'$addToSet': '$role'}
        }
    },
    
    # Sort by count
    {
        '$sort': {'employee_count': -1}
    },
    
    # Shape output
    {
        '$project': {
            'department': '$_id',
            'employee_count': 1,
            'avg_age': {'$round': ['$avg_age', 2]},
            'total_skills': 1,
            'roles': 1,
            '_id': 0
        }
    }
]

for dept in db.employees.aggregate(pipeline):
    print(f"{dept['department']}: {dept['employee_count']} employees")
```

### Performance Tips

**1. Filter Early with $match**
```python
# ✅ Good - Filter first
[
    {'$match': {'status': 'active'}},  # Reduces docs early!
    {'$group': {...}},
    {'$sort': {...}}
]

# ❌ Bad - Processes all
[
    {'$group': {...}},
    {'$sort': {...}},
    {'$match': {'status': 'active'}}  # Too late!
]
```

**2. Use Indexes**
```python
# Create index
db.users.create_index('status')

# $match at start can use it!
[
    {'$match': {'status': 'active'}},  # Uses index!
    ...
]
```

**3. Project Early**
```python
[
    {'$match': {...}},
    {
        '$project': {  # Reduce document size
            'name': 1,
            'age': 1,
            'dept': 1
        }
    },
    {'$group': {...}}
]
```

**4. Allow Disk Use**
```python
# For large datasets (>100MB memory)
results = db.users.aggregate(
    pipeline,
    allowDiskUse=True  # Use disk for sorting
)
```

**5. Optimize $lookup**
```python
# Use pipeline form with filters
{
    '$lookup': {
        'from': 'orders',
        'let': {'uid': '$_id'},
        'pipeline': [
            {
                '$match': {
                    '$expr': {'$eq': ['$user_id', '$$uid']},
                    'status': 'completed'  # Filter early!
                }
            },
            {'$project': {'total': 1}}  # Project early!
        ],
        'as': 'orders'
    }
}
```

---

*This completes the main sections of the tutorial! The file contains comprehensive coverage of PyMongo from basics to advanced topics with modern 2026 best practices.*

---

## 🚄 Indexing & Performance

Indexes are crucial for query performance!

### Creating Indexes

```python
# Single field
db.users.create_index('email')

# Specify direction
db.users.create_index([('age', -1)])  # Descending

# Compound index
db.users.create_index([
    ('role', 1),
    ('age', -1),
    ('department', 1)
])

# With options
db.users.create_index(
    'email',
    unique=True,
    name='email_unique_idx',
    background=True
)
```

### Index Types

**Single Field:**
```python
db.users.create_index('email')
db.users.find({'email': 'alice@example.com'})  # Fast!
```

**Compound:**
```python
db.users.create_index([('role', 1), ('dept', 1), ('age', -1)])

# Uses index for:
db.users.find({'role': 'developer'})  # ✅
db.users.find({'role': 'developer', 'dept': 'Eng'})  # ✅
# Not for:
db.users.find({'dept': 'Eng'})  # ❌ Skips first field
```

**Unique:**
```python
db.users.create_index('email', unique=True)
# Prevents duplicates!
```

**Text (Full-Text Search):**
```python
db.articles.create_index([('title', 'text'), ('content', 'text')])
db.articles.find({'$text': {'$search': 'mongodb python'}})
```

**Geospatial:**
```python
db.places.create_index([('location', '2dsphere')])
db.places.find({
    'location': {
        '$near': {
            '$geometry': {'type': 'Point', 'coordinates': [-73.9667, 40.78]},
            '$maxDistance': 5000
        }
    }
})
```

**TTL (Auto-Delete):**
```python
db.sessions.create_index('created_at', expireAfterSeconds=3600)
# Docs automatically deleted after 1 hour!
```

### Performance Tips

**1. Use explain()**
```python
explain = db.users.find({'role': 'developer'}).explain()
print(explain['executionStats'])

# Check if index used
if 'IXSCAN' in str(explain):
    print("✅ Using index!")
```

**2. Profile Slow Queries**
```python
db.set_profiling_level(1, slow_ms=100)
for op in db.system.profile.find().sort('ts', -1).limit(5):
    print(f"Duration: {op['millis']}ms")
```

**3. Optimize Queries**
```python
# ✅ Good - Uses index
db.users.find({'status': 'active', 'role': 'developer'})

# ❌ Bad - Full scan
db.users.find({'name': {'$regex': '.*smith.*'}})

# ✅ Better - Anchored regex
db.users.find({'name': {'$regex': '^Smith'}})
```

**4. Monitor Index Usage**
```python
stats = db.users.aggregate([{'$indexStats': {}}])
for idx in stats:
    print(f"{idx['name']}: {idx['accesses']['ops']} uses")
```


## 🔌 Connection Management

### Connection Pooling

```python
client = MongoClient(
    'mongodb://localhost:27017/',
    maxPoolSize=50,
    minPoolSize=10,
    maxIdleTimeMS=45000,
    waitQueueTimeoutMS=5000
)

# Check pool
pool_opts = client.options.pool_options
print(f"Max pool: {pool_opts.max_pool_size}")
```

**Best Practices:**
1. ✅ One MongoClient per application
2. ✅ MongoClient is thread-safe
3. ✅ Configure for your workload

### Context Managers

```python
# ✅ Best Practice
with MongoClient(uri) as client:
    db = client.my_database
    result = db.users.find_one({'name': 'Alice'})
# Automatic cleanup!
```

### Error Handling

```python
from pymongo.errors import (
    ConnectionFailure,
    DuplicateKeyError,
    OperationFailure
)

try:
    result = db.users.insert_one({'email': 'user@example.com'})
except ConnectionFailure:
    logger.error("Connection failed")
except DuplicateKeyError:
    logger.warning("Duplicate key")
except OperationFailure as e:
    if e.code == 13:  # Unauthorized
        logger.error("Auth failed")
```

**Retry Pattern:**
```python
from time import sleep

def insert_with_retry(collection, doc, max_retries=3):
    for attempt in range(max_retries):
        try:
            return collection.insert_one(doc)
        except ConnectionFailure as e:
            if attempt < max_retries - 1:
                wait = 2 ** attempt  # Exponential backoff
                sleep(wait)
            else:
                raise
```


## 🏭 Production Patterns

### Schema Design

**Embed vs Reference:**

```python
# ✅ Embed for 1-to-1 or 1-to-few
{
    '_id': 1,
    'name': 'Alice',
    'address': {
        'street': '123 Main',
        'city': 'NY'
    }
}

# ✅ Reference for 1-to-many or many-to-many
# users collection
{'_id': 1, 'name': 'Alice', 'order_ids': [101, 102]}
# orders collection
{'_id': 101, 'user_id': 1, 'total': 99.99}
```

**Denormalization:**
```python
# ✅ Denormalize for reads
{
    '_id': 101,
    'user_id': 1,
    'user_name': 'Alice',  # Denormalized!
    'user_email': 'alice@example.com',
    'items': [...]
}
```

### Bulk Operations

```python
from pymongo import InsertOne, UpdateOne, DeleteOne

# Bulk write
operations = [
    InsertOne({'name': 'New User'}),
    UpdateOne({'name': 'Alice'}, {'$set': {'age': 30}}),
    DeleteOne({'name': 'Bob'})
]

result = db.users.bulk_write(operations, ordered=False)
print(f"Inserted: {result.inserted_count}")
print(f"Modified: {result.modified_count}")
print(f"Deleted: {result.deleted_count}")
```

### Transactions

```python
# Multi-document ACID transactions (requires replica set)
with client.start_session() as session:
    with session.start_transaction():
        try:
            # Debit
            db.accounts.update_one(
                {'_id': 'account1', 'balance': {'$gte': 100}},
                {'$inc': {'balance': -100}},
                session=session
            )
            
            # Credit
            db.accounts.update_one(
                {'_id': 'account2'},
                {'$inc': {'balance': 100}},
                session=session
            )
            
            print("✅ Transaction successful!")
        except Exception as e:
            print(f"❌ Transaction failed: {e}")
            # Auto-rollback
```

### Change Streams

```python
# Watch for real-time changes
with db.users.watch() as stream:
    for change in stream:
        if change['operationType'] == 'insert':
            print(f"New user: {change['fullDocument']}")
        elif change['operationType'] == 'update':
            print(f"Updated: {change['updateDescription']}")
        elif change['operationType'] == 'delete':
            print(f"Deleted: {change['documentKey']}")

# With filter
pipeline = [
    {
        '$match': {
            'operationType': 'insert',
            'fullDocument.status': 'new'
        }
    }
]

with db.orders.watch(pipeline) as stream:
    for change in stream:
        process_new_order(change['fullDocument'])
```


## 📚 Additional Resources

### Official Documentation
- **PyMongo Docs**: https://pymongo.readthedocs.io/
- **MongoDB Docs**: https://docs.mongodb.com/
- **MongoDB University**: https://university.mongodb.com/ (Free!)

### Best Practices Checklist

✅ **Connection Management**
- One MongoClient per app
- Configure connection pool
- Use context managers
- Handle errors with retries

✅ **Querying**
- Create indexes on queried fields
- Use explain() to analyze
- Filter early in pipelines
- Project only needed fields

✅ **Schema Design**
- Embed related data
- Denormalize for reads
- Reference for large/independent data
- Keep docs under 16MB

✅ **Performance**
- Monitor slow queries
- Use bulk operations
- Implement proper error handling
- Use aggregation for complex queries

✅ **Production**
- Use replica sets
- Enable authentication
- Regular backups
- Monitor database metrics
- Keep software updated


## 🎉 Conclusion

Congratulations! You've completed the comprehensive PyMongo 2026 tutorial. You now know how to:

- ✅ Connect to MongoDB efficiently
- ✅ Perform all CRUD operations
- ✅ Write complex queries and aggregations
- ✅ Optimize performance with indexes
- ✅ Build production-ready applications

**Remember:** The best way to learn is by building! Start a project, experiment with different patterns, and don't hesitate to check the documentation when stuck.

**Happy Coding! 🚀**

---

## 💝 Contributing

Found a typo? Have a suggestion? Contributions are welcome!

1. Fork the repository
2. Create your feature branch
3. Commit your changes
4. Create a Pull Request


## Quick Reference

```python
# Connection
client = MongoClient('mongodb://localhost:27017/')
db = client.database_name
collection = db.collection_name

# Insert
collection.insert_one({...})
collection.insert_many([{...}, {...}])

# Find
collection.find_one({'key': 'value'})
collection.find({'key': 'value'})

# Update
collection.update_one({'_id': id}, {'$set': {...}})
collection.update_many({...}, {'$set': {...}})

# Delete
collection.delete_one({'_id': id})
collection.delete_many({...})

# Aggregation
collection.aggregate([
    {'$match': {...}},
    {'$group': {...}},
    {'$sort': {...}}
])

# Index
collection.create_index('field_name')
collection.create_index([('field1', 1), ('field2', -1)])
```

---

<div align="center">
  
**Made with ❤️ for the Python Community**

</div>
