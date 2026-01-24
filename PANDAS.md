<div align="center">

# Awesome Pandas [![Awesome](https://cdn.rawgit.com/sindresorhus/awesome/d7305f38d29fed78fa85652e3a63e154dd8e8829/media/badge.svg)](https://github.com/sindresorhus/awesome)

🐼 **Your complete, 2026 production-ready guide to mastering Pandas - from first import to enterprise-scale data pipelines**

<br>
</div>


## 📚 Table of Contents

- [Why Pandas?](#-why-pandas)
- [Installation](#-installation--setup)
- [Core Concepts](#-core-concepts)
- [Data Structures](#-data-structures-deep-dive)
- [Reading & Writing](#-reading--writing-data-like-a-pro)
- [Selection & Indexing](#-selection--indexing-mastery)
- [Data Cleaning](#-data-cleaning--preparation)
- [Transformation](#-data-transformation)
- [GroupBy Operations](#-groupby--aggregation)
- [Merging & Joining](#-merging-joining--concatenation)
- [Time Series](#-time-series-analysis)
- [Advanced Techniques](#-advanced-techniques)
- [Performance](#%EF%B8%8F-performance-optimization)
- [Production Practices](#-production-best-practices)
- [Real-World Projects](#-real-world-projects)
- [Common Pitfalls](#%EF%B8%8F-common-pitfalls--solutions)

---

## 🎯 Why Pandas?

Pandas is the **gold standard** for data manipulation in Python. Period.

### What Makes It Special?

- **🚀 Blazing Fast**: Built on NumPy with optimized C/Cython code
- **🎨 Intuitive API**: Feels like SQL + Excel + Python combined
- **💪 Industrial Strength**: Powers data pipelines at Netflix, Airbnb, JPMorgan
- **🔗 Perfect Integration**: Works seamlessly with NumPy, Scikit-learn, Matplotlib
- **📊 Battle-Tested**: 15+ years of development, billions of installs


## 🛠 Installation & Setup

### Basic Installation

```bash
# Virtual environment (always!)
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate

# Standard installation
pip install pandas

# With all extras (recommended for production)
pip install 'pandas[all]'

# Performance boosters
pip install pandas pyarrow bottleneck numexpr fastparquet

```


### Verify Installation

```python
import pandas as pd
import numpy as np

print(f"Pandas: {pd.__version__}")  
print(f"NumPy: {np.__version__}")

# Check optional dependencies
try:
    import pyarrow
    print(f"PyArrow: {pyarrow.__version__} ✓")
except ImportError:
    print("PyArrow: Not installed")
```

### Recommended Setup

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# Display options for better output
pd.set_option('display.max_columns', None)     # Show all columns
pd.set_option('display.max_rows', 100)         # Show up to 100 rows
pd.set_option('display.width', None)           # Auto-detect width
pd.set_option('display.precision', 2)          # 2 decimal places
pd.set_option('display.float_format', '{:.2f}'.format)

# For large datasets
pd.set_option('display.max_colwidth', 50)      # Truncate long strings
pd.set_option('mode.chunksize', 100000)        # Chunk size for iteration
```


## 🧱 Core Concepts

### Understanding Pandas Architecture

```
Pandas
├── Series (1D)
│   ├── Values (NumPy array)
│   └── Index
└── DataFrame (2D)
    ├── Columns (each is a Series)
    └── Index
```

### Series: The Building Block

```python
# Creating Series
s = pd.Series([10, 20, 30, 40, 50])
s = pd.Series([10, 20, 30], index=['a', 'b', 'c'])
s = pd.Series({'a': 10, 'b': 20, 'c': 30})
s = pd.Series(100, index=['x', 'y', 'z'])  # Scalar broadcast

# Key attributes
s.values        # Underlying NumPy array
s.index         # Index object
s.dtype         # Data type
s.name          # Series name
s.shape         # Dimensions
s.size          # Number of elements

# Essential operations
s.head(3)       # First 3 elements
s.tail(3)       # Last 3 elements
s.describe()    # Statistics
s.unique()      # Unique values
s.value_counts()  # Frequency count

# Vectorized operations (FAST!)
s * 2           # Multiply all by 2
s + 100         # Add 100 to all
np.log(s)       # Apply NumPy function
s.apply(lambda x: x ** 2)  # Custom function
```

### DataFrame: The Powerhouse

```python
# Creating DataFrames - Many Ways

# 1. From dictionary (most common)
df = pd.DataFrame({
    'product': ['A', 'B', 'C'],
    'price': [10.99, 15.99, 8.99],
    'quantity': [100, 50, 200]
})

# 2. From list of dicts (great for API responses)
data = [
    {'user': 'alice', 'score': 95, 'grade': 'A'},
    {'user': 'bob', 'score': 87, 'grade': 'B'}
]
df = pd.DataFrame(data)

# 3. From NumPy array
data = np.random.randn(1000, 4)
df = pd.DataFrame(data, columns=['A', 'B', 'C', 'D'])

# 4. From CSV (most common in practice)
df = pd.read_csv('data.csv')

# DataFrame anatomy
df.shape         # (rows, columns)
df.columns       # Column names
df.index         # Row index
df.dtypes        # Data types
df.info()        # Comprehensive overview
df.describe()    # Statistical summary
df.memory_usage(deep=True)  # Memory footprint

# Quick views
df.head(10)      # First 10 rows
df.tail(10)      # Last 10 rows  
df.sample(5)     # Random 5 rows
```

### Index Objects: The Secret Weapon

```python
# RangeIndex (default, memory-efficient)
df = pd.DataFrame({'A': range(1000000)})
print(df.index)  # RangeIndex(start=0, stop=1000000, step=1)

# DatetimeIndex (for time series)
dates = pd.date_range('2025-01-01', periods=365, freq='D')
ts = pd.Series(np.random.randn(365), index=dates)
ts['2025-01']    # All January data (magic!)

# MultiIndex (hierarchical)
idx = pd.MultiIndex.from_tuples([
    ('USA', 'NY'), ('USA', 'CA'), ('UK', 'London')
], names=['country', 'city'])
df = pd.DataFrame({'population': [8.3, 39.5, 9.0]}, index=idx)

# Index operations
df.set_index('column_name')      # Make column the index
df.reset_index()                 # Move index to column
df.sort_index()                  # Sort by index
df.reindex([0, 2, 4])           # Reindex with new labels
```

### Data Types: Get Them Right!

```python
# Pandas dtypes vs Python types
# int64, float64, object, bool, datetime64, category, string

# Best practice: Specify types explicitly
df = pd.DataFrame({
    'id': pd.Series([1, 2, 3], dtype='int64'),
    'name': pd.Series(['Alice', 'Bob', 'Charlie'], dtype='string'),  # Pandas 1.0+
    'date': pd.to_datetime(['2025-01-01', '2025-01-02', '2025-01-03']),
    'active': pd.Series([True, False, True], dtype='bool'),
    'price': pd.Series([10.99, 20.99, 15.99], dtype='float64'),
    'category': pd.Series(['A', 'B', 'A'], dtype='category')  # Huge memory savings!
})

# Check and convert types
df.dtypes
df.info()
df['id'] = df['id'].astype('int32')  # Downcast to save memory
df['category'] = df['category'].astype('category')

# Memory comparison
df.memory_usage(deep=True)
```


## 🗂 Data Structures Deep Dive

### Series Advanced Operations

```python
# Statistical methods
s = pd.Series([10, 20, 30, 40, 50, None])
s.count()         # Non-null count
s.sum()           # Sum
s.mean()          # Mean
s.median()        # Median
s.std()           # Standard deviation
s.var()           # Variance
s.min(), s.max()  # Min/max
s.quantile([0.25, 0.5, 0.75])  # Quartiles
s.describe()      # All statistics

# Boolean operations
s > 25            # Element-wise comparison
s.isnull()        # Check for nulls
s.notnull()       # Check for non-nulls
s.isin([10, 30])  # Check membership

# String operations (if dtype is string/object)
s = pd.Series(['apple', 'banana', 'cherry'])
s.str.upper()     # APPLE, BANANA, CHERRY
s.str.contains('an')  # Boolean mask
s.str.len()       # Length of each string
```

### DataFrame Advanced Operations

```python
# Column operations
df['new_col'] = df['col1'] + df['col2']  # Add column
df.drop('col_name', axis=1)              # Drop column
df.drop(columns=['col1', 'col2'])        # Drop multiple
df.rename(columns={'old': 'new'})        # Rename

# Row operations
df.drop([0, 2, 4])                       # Drop rows by index
df.drop_duplicates()                     # Remove duplicates
df.drop_duplicates(subset=['col1'])      # Based on column

# Sorting
df.sort_values('column')                 # Sort by column
df.sort_values(['col1', 'col2'], ascending=[True, False])
df.sort_index()                          # Sort by index

# Transposing
df.T                                     # Transpose rows/columns

# Iteration (avoid when possible - use vectorization!)
for index, row in df.iterrows():         # Iterate rows (slow)
    print(row['column'])
    
for index, value in df['column'].items():  # Iterate Series (better)
    print(value)
```

### MultiIndex Magic

```python
# Create MultiIndex DataFrame
arrays = [
    ['A', 'A', 'B', 'B', 'B'],
    ['one', 'two', 'one', 'two', 'three']
]
index = pd.MultiIndex.from_arrays(arrays, names=['first', 'second'])
df = pd.DataFrame(np.random.randn(5, 2), index=index, columns=['X', 'Y'])

# Selection with MultiIndex
df.loc['A']                    # All rows where first level = 'A'
df.loc[('A', 'one')]          # Specific tuple
df.xs('one', level='second')  # Cross-section

# Stack/Unstack
df.stack()                     # Pivot innermost column to row
df.unstack()                   # Pivot innermost row to column
df.swaplevel()                 # Swap MultiIndex levels

# Sorting MultiIndex
df.sort_index(level=0)
df.sort_index(level='second')
```


## 📥 Reading & Writing Data Like a Pro

### CSV: The Universal Format

```python
# Reading CSV (with all the power)
df = pd.read_csv(
    'data.csv',
    sep=',',                     # Delimiter
    header=0,                    # Row for column names
    names=['col1', 'col2'],      # Custom column names  
    index_col='id',              # Column to use as index
    usecols=['id', 'name', 'age'],  # Read specific columns
    dtype={'id': 'int64', 'age': 'int32'},  # Specify types
    parse_dates=['created_at'],  # Parse as datetime
    date_format='%Y-%m-%d',      # Date format (Pandas 2.0+)
    na_values=['NA', 'null', ''],  # Additional null values
    encoding='utf-8',            # File encoding
    skiprows=10,                 # Skip first 10 rows
    nrows=100000,                # Read only first 100k rows
    low_memory=False,            # Type inference on full dataset
    compression='gzip',          # Handle compressed files
    chunksize=50000              # Read in chunks (for large files)
)

# Writing CSV
df.to_csv(
    'output.csv',
    sep=',',
    index=False,                 # Don't write index
    header=True,                 # Write column names
    columns=['col1', 'col2'],    # Select columns to write
    encoding='utf-8',
    compression='gzip',          # Compress output
    date_format='%Y-%m-%d',
    float_format='%.2f',         # Float precision
    mode='w'                     # 'a' for append
)

# Reading large CSV in chunks
chunks = []
for chunk in pd.read_csv('huge_file.csv', chunksize=100000):
    # Process each chunk
    processed = chunk[chunk['amount'] > 0]
    chunks.append(processed)
    
df = pd.concat(chunks, ignore_index=True)
```

### Excel: Business-Friendly

```python
# Reading Excel
df = pd.read_excel(
    'data.xlsx',
    sheet_name='Sheet1',         # Or index: 0, or None for all
    header=0,
    usecols='A:D',               # Excel-style column range
    dtype={'id': 'int64'},
    engine='openpyxl'            # 'openpyxl', 'xlrd', 'pyxlsb'
)

# Read multiple sheets
sheets = pd.read_excel('data.xlsx', sheet_name=None)  # Returns dict
for sheet_name, df in sheets.items():
    print(f"{sheet_name}: {len(df)} rows")

# Writing Excel (with formatting!)
with pd.ExcelWriter('output.xlsx', engine='xlsxwriter') as writer:
    df.to_excel(writer, sheet_name='Data', index=False)
    
    # Get workbook and worksheet
    workbook = writer.book
    worksheet = writer.sheets['Data']
    
    # Add formatting
    header_format = workbook.add_format({
        'bold': True,
        'bg_color': '#D7E4BD',
        'border': 1
    })
    
    # Apply to header row
    for col_num, value in enumerate(df.columns.values):
        worksheet.write(0, col_num, value, header_format)
```

### JSON: API Standard

```python
# Reading JSON
df = pd.read_json(
    'data.json',
    orient='records',            # JSON structure
    lines=True,                  # JSON Lines format
    dtype={'id': 'int64'}
)

# From API response
import requests
response = requests.get('https://api.example.com/data')
df = pd.DataFrame(response.json()['results'])

# Nested JSON (the real challenge!)
data = {
    'user': {
        'id': 1,
        'name': 'Alice',
        'orders': [
            {'product': 'A', 'qty': 2, 'price': 10.99},
            {'product': 'B', 'qty': 1, 'price': 20.99}
        ]
    }
}

# Flatten nested structure
df = pd.json_normalize(
    data,
    record_path='user.orders',
    meta=[
        ['user', 'id'],
        ['user', 'name']
    ],
    meta_prefix='user_'
)

# Writing JSON
df.to_json(
    'output.json',
    orient='records',
    lines=True,                  # One JSON object per line
    indent=2,                    # Pretty print
    date_format='iso'            # ISO 8601 dates
)
```

### Parquet: Production Performance King 👑

```python
# WHY PARQUET?
# ✓ Columnar storage (10x faster reads for analytics)
# ✓ Excellent compression (3-10x smaller than CSV)
# ✓ Preserves data types perfectly
# ✓ Schema evolution support
# ✓ Industry standard for data lakes

# Writing Parquet
df.to_parquet(
    'data.parquet',
    engine='pyarrow',            # 'pyarrow' or 'fastparquet'
    compression='snappy',        # 'snappy', 'gzip', 'brotli', 'lz4', 'zstd'
    index=False
)

# Reading Parquet
df = pd.read_parquet(
    'data.parquet',
    engine='pyarrow',
    columns=['id', 'name', 'amount']  # Read only needed columns!
)

# Partitioned Parquet (for huge datasets)
df.to_parquet(
    'data_partitioned/',
    partition_cols=['year', 'month'],  # Creates directory structure
    engine='pyarrow'
)
# Creates: data_partitioned/year=2025/month=01/file.parquet

# Read with filters (blazing fast!)
df = pd.read_parquet(
    'data_partitioned/',
    filters=[('year', '=', 2025), ('month', '=', 1)]
)
```

### SQL Databases: Enterprise Standard

```python
from sqlalchemy import create_engine

# Connect to database
engine = create_engine('postgresql://user:pass@localhost:5432/db')
# Also supports: mysql, sqlite, oracle, mssql, etc.

# Reading from SQL
df = pd.read_sql(
    'SELECT * FROM users WHERE created_at > %s',
    engine,
    params=['2025-01-01']        # Parameterized (SQL injection safe!)
)

# Or use table name
df = pd.read_sql_table('users', engine)

# Or complex query
query = """
    SELECT 
        user_id,
        COUNT(*) as order_count,
        SUM(amount) as total_spent
    FROM orders
    WHERE created_at >= '2025-01-01'
    GROUP BY user_id
    HAVING COUNT(*) > 5
"""
df = pd.read_sql(query, engine)

# Writing to SQL
df.to_sql(
    'users',
    engine,
    if_exists='append',          # 'fail', 'replace', 'append'
    index=False,
    method='multi',              # Faster bulk inserts
    chunksize=1000               # Insert in batches
)

# Transaction support
with engine.begin() as conn:
    df.to_sql('temp_table', conn, if_exists='replace')
    # Do more work
    # Commits on exit, rolls back on error
```

### Other Useful Formats

```python
# HTML tables
tables = pd.read_html('https://example.com/table')  # Returns list
df = tables[0]  # First table

# Clipboard (quick data transfer!)
df = pd.read_clipboard()         # Read from clipboard
df.to_clipboard(index=False)     # Write to clipboard

# Feather (fast binary format)
df.to_feather('data.feather')
df = pd.read_feather('data.feather')

# HDF5 (hierarchical data)
df.to_hdf('data.h5', key='df', mode='w', format='table')
df = pd.read_hdf('data.h5', key='df')

# Pickle (Python-specific, preserves everything)
df.to_pickle('data.pkl')
df = pd.read_pickle('data.pkl')
# ⚠️ WARNING: Only use pickle with trusted sources!

# Stata, SAS, SPSS
df = pd.read_stata('data.dta')
df = pd.read_sas('data.sas7bdat')
df = pd.read_spss('data.sav')
```


## 🎯 Selection & Indexing Mastery

### The Four Horsemen: [], .loc, .iloc, .at

```python
df = pd.DataFrame({
    'name': ['Alice', 'Bob', 'Charlie', 'David'],
    'age': [25, 30, 35, 40],
    'city': ['NYC', 'LA', 'Chicago', 'Houston'],
    'salary': [70000, 80000, 90000, 100000]
}, index=['a', 'b', 'c', 'd'])

# 1. [] - Column and basic row selection
df['name']                       # Single column → Series
df[['name', 'age']]             # Multiple columns → DataFrame
df[df['age'] > 30]              # Boolean indexing
df[1:3]                         # Slice rows (positional)

# 2. .loc - Label-based selection
df.loc['a']                     # Row by label
df.loc['a', 'name']             # Specific cell
df.loc['a':'c']                 # Slice (INCLUSIVE!)
df.loc['a':'c', 'name':'city']  # Rows and columns
df.loc[df['age'] > 30, ['name', 'salary']]  # Boolean + columns

# 3. .iloc - Position-based selection (like NumPy)
df.iloc[0]                      # First row
df.iloc[0, 1]                   # First row, second column
df.iloc[0:2]                    # First two rows (EXCLUSIVE!)
df.iloc[:, 0:2]                 # All rows, first two columns
df.iloc[[0, 2], [0, 3]]         # Specific rows and columns

# 4. .at and .iat - Fast scalar access
df.at['a', 'name']              # Fastest single value (label)
df.iat[0, 0]                    # Fastest single value (position)

# 🎯 GOLDEN RULE: Always use .loc or .iloc explicitly!
# ❌ WRONG: df[df.age > 30]['salary'] = 50000  
# ✅ CORRECT: df.loc[df.age > 30, 'salary'] = 50000
```

### Boolean Indexing: Power Tool

```python
# Simple conditions
young = df[df['age'] < 30]
high_earners = df[df['salary'] > 85000]

# Multiple conditions (use & | ~, NOT and/or/not!)
young_high = df[(df['age'] < 30) & (df['salary'] > 75000)]
nyc_or_la = df[(df['city'] == 'NYC') | (df['city'] == 'LA')]
not_nyc = df[~(df['city'] == 'NYC')]

# .isin() for multiple values
major_cities = df[df['city'].isin(['NYC', 'LA', 'Chicago'])]

# .between() for ranges
mid_age = df[df['age'].between(30, 40, inclusive='both')]

# String operations
starts_a = df[df['name'].str.startswith('A')]
contains_li = df[df['name'].str.contains('li', case=False)]

# .query() - SQL-like syntax (readable!)
result = df.query('age > 30 and salary > 85000')
result = df.query('city in ["NYC", "LA"]')

# Using variables in .query()
min_age = 30
result = df.query('age > @min_age')  # @ for variables
```

### Advanced Indexing

```python
# Fancy indexing
df.loc[['a', 'c'], ['name', 'salary']]

# Boolean array indexing
mask = df['age'] > 30
df.loc[mask, 'bonus'] = df.loc[mask, 'salary'] * 0.1

# .where() and .mask()
df['age_category'] = df['age'].where(df['age'] < 35, 'Senior')
df['salary_masked'] = df['salary'].mask(df['salary'] < 80000, np.nan)

# .filter() - Select by label
df.filter(items=['name', 'age'])         # Specific columns
df.filter(like='sal')                     # Columns containing 'sal'
df.filter(regex='^s')                     # Columns starting with 's'

# .select_dtypes() - Select by type
df.select_dtypes(include=[np.number])    # All numeric columns
df.select_dtypes(include=['object'])     # All object columns
df.select_dtypes(exclude=['int64'])      # Exclude int64

# MultiIndex slicing
idx = pd.IndexSlice
df_multi.loc[idx['A', :], :]             # All with first level 'A'
df_multi.loc[idx[:, 'x'], :]             # All with second level 'x'
```


## 🧹 Data Cleaning & Preparation

### Missing Data: The Reality

```python
# Detect missing data
df.isnull()                      # Element-wise boolean
df.isnull().sum()                # Count per column
df.isnull().sum().sum()          # Total nulls
df.isna().any()                  # Any nulls per column

# Visualize missing data
import matplotlib.pyplot as plt
df.isnull().sum().plot(kind='bar')

# Drop missing data
df.dropna()                      # Drop rows with ANY null
df.dropna(how='all')             # Drop if ALL values null
df.dropna(subset=['critical_col'])  # Drop if null in specific columns
df.dropna(thresh=3)              # Keep rows with ≥3 non-nulls
df.dropna(axis=1)                # Drop columns with nulls

# Fill missing data
df.fillna(0)                     # Fill with constant
df.fillna(df.mean())             # Fill with column mean
df.fillna(df.median())           # Fill with median
df.fillna(method='ffill')        # Forward fill (deprecated in 2.0+)
df.fillna(method='bfill')        # Backward fill (deprecated in 2.0+)

# Pandas 2.0+ way (preferred!)
df.ffill()                       # Forward fill
df.bfill()                       # Backward fill
df.ffill(limit=2)                # Fill max 2 consecutive

# Different strategies per column
df.fillna({
    'age': df['age'].median(),
    'city': 'Unknown',
    'salary': df['salary'].mean()
})

# Interpolate (great for time series)
df['temperature'].interpolate(method='linear')
df['stock_price'].interpolate(method='time')

# Replace values
df.replace(0, np.nan)            # Replace 0 with NaN
df.replace({'A': 'X', 'B': 'Y'}) # Multiple replacements
df.replace([0, -1], np.nan)      # Replace multiple values

# 🎯 Production Pattern: Track imputation
df['age_was_null'] = df['age'].isnull()
df['age'] = df['age'].fillna(df['age'].median())
```

### Data Type Conversions

```python
# Convert types
df['age'] = df['age'].astype('int64')
df['salary'] = df['salary'].astype('float64')
df['name'] = df['name'].astype('string')  # Pandas 1.0+ StringDtype
df['category'] = df['category'].astype('category')  # Huge memory savings!

# Convert to datetime
df['date'] = pd.to_datetime(df['date_string'])
df['date'] = pd.to_datetime(df['date_string'], format='%Y-%m-%d')
df['date'] = pd.to_datetime(df['date_string'], errors='coerce')  # Invalid → NaT

# Convert to numeric
df['value'] = pd.to_numeric(df['value'], errors='coerce')  # Invalid → NaN
df['value'] = pd.to_numeric(df['value'], errors='ignore')  # Keep invalid as-is

# Categorical with ordering
df['size'] = pd.Categorical(
    df['size'],
    categories=['Small', 'Medium', 'Large', 'XL'],
    ordered=True
)
df[df['size'] > 'Medium']  # Now this works!

# Check memory savings
print(df.memory_usage(deep=True))
df['country'] = df['country'].astype('category')
print(df.memory_usage(deep=True))  # Much smaller!
```

### Handling Duplicates

```python
# Detect duplicates
df.duplicated()                  # Boolean Series
df.duplicated(keep='first')      # Mark all but first
df.duplicated(keep='last')       # Mark all but last
df.duplicated(keep=False)        # Mark ALL duplicates

# Find duplicates
duplicates = df[df.duplicated(keep=False)]
print(f"Found {len(duplicates)} duplicate rows")

# Check specific columns
df.duplicated(subset=['user_id', 'date'])

# Remove duplicates
df.drop_duplicates()             # Keep first occurrence
df.drop_duplicates(keep='last')  # Keep last occurrence
df.drop_duplicates(subset=['user_id'], keep='first')

# 🎯 Production Pattern: Investigate first!
dup_count = df.duplicated().sum()
if dup_count > 0:
    print(f"⚠️  Found {dup_count} duplicates")
    print(df[df.duplicated(keep=False)].sort_values('user_id'))
    # Then decide: keep first, last, or manual review
```

### String Cleaning

```python
# Basic string operations
df['name'].str.lower()           # Lowercase
df['name'].str.upper()           # Uppercase
df['name'].str.title()           # Title Case
df['name'].str.strip()           # Remove whitespace
df['name'].str.lstrip()          # Left strip
df['name'].str.rstrip()          # Right strip

# Replace and remove
df['text'].str.replace('old', 'new')
df['text'].str.replace(r'\d+', '', regex=True)  # Remove digits
df['email'].str.split('@')       # Split
df['email'].str.split('@', expand=True)  # Split into columns

# Pattern matching
df[df['text'].str.contains('keyword')]
df[df['text'].str.contains('keyword', case=False, na=False)]
df[df['email'].str.match(r'^[\w\.-]+@[\w\.-]+\.\w+$')]

# String operations
df['name'].str.len()             # Length
df['name'].str.startswith('A')   # Boolean
df['name'].str.endswith('son')   # Boolean
df['text'].str.count('word')     # Count occurrences

# Extract with regex
df['phone'].str.extract(r'(\d{3})-(\d{3})-(\d{4})')
df['code'].str.extractall(r'[A-Z]{2}\d{4}')

# 🎯 Production Pattern: Chain operations
df['clean_name'] = (
    df['name']
    .str.strip()
    .str.lower()
    .str.replace(r'[^a-z\s]', '', regex=True)
    .str.replace(r'\s+', ' ', regex=True)
    .str.title()
)
```

### Outlier Detection

```python
# Z-score method
from scipy import stats

z_scores = np.abs(stats.zscore(df['salary'].dropna()))
df['is_outlier_z'] = z_scores > 3

# IQR method (more robust)
Q1 = df['salary'].quantile(0.25)
Q3 = df['salary'].quantile(0.75)
IQR = Q3 - Q1

lower_bound = Q1 - 1.5 * IQR
upper_bound = Q3 + 1.5 * IQR

df['is_outlier_iqr'] = (
    (df['salary'] < lower_bound) | 
    (df['salary'] > upper_bound)
)

# Handle outliers
# Option 1: Remove
df_clean = df[~df['is_outlier_iqr']]

# Option 2: Cap (winsorize)
df['salary_capped'] = df['salary'].clip(lower=lower_bound, upper=upper_bound)

# Option 3: Transform
df['salary_log'] = np.log1p(df['salary'])  # log(1 + x)

# 🎯 Production: Always investigate!
outliers = df[df['is_outlier_iqr']]
print(f"Outliers: {len(outliers)}")
print(outliers[['name', 'salary']].sort_values('salary'))
```


## 🔄 Data Transformation

### Adding & Modifying Columns

```python
# Simple addition
df['total'] = df['price'] * df['quantity']

# Conditional logic
df['category'] = df['age'].apply(lambda x: 'Young' if x < 30 else 'Old')

# Better: np.where() (vectorized!)
df['category'] = np.where(df['age'] < 30, 'Young', 'Old')

# Multiple conditions: np.select()
conditions = [
    df['age'] < 25,
    df['age'].between(25, 35),
    df['age'] > 35
]
choices = ['Young', 'Middle', 'Senior']
df['age_group'] = np.select(conditions, choices, default='Unknown')

# From another DataFrame (aligns on index)
df['bonus'] = other_df['bonus']

# Insert at specific position
df.insert(1, 'new_col', values, allow_duplicates=False)

# Chain operations
df['normalized'] = (df['price'] - df['price'].mean()) / df['price'].std()
```

### Apply, Map, Transform

```python
# .apply() - Most flexible, row or column-wise
df['squared'] = df['value'].apply(lambda x: x ** 2)
df['total'] = df.apply(lambda row: row['price'] * row['quantity'], axis=1)

# Custom function
def categorize_age(age):
    if age < 25:
        return 'Young'
    elif age < 40:
        return 'Middle'
    else:
        return 'Senior'

df['age_category'] = df['age'].apply(categorize_age)

# .map() - Series only, element-wise
mapping = {'A': 1, 'B': 2, 'C': 3}
df['mapped'] = df['letter'].map(mapping)

# .transform() - Returns same shape, great with groupby
df['normalized'] = df.groupby('category')['value'].transform(
    lambda x: (x - x.mean()) / x.std()
)

# 🎯 Performance Tips
# Vectorized operations >> .apply() >> loops
# BEST: df['x'] * df['y']
# OK: df.apply(func)
# AVOID: for loops

# Use NumPy for math
df['result'] = np.sqrt(df['value'])  # Fast
df['result'] = df['value'].apply(np.sqrt)  # Slower

# .pipe() - Chain custom functions
def add_tax(df, rate):
    df['total_with_tax'] = df['total'] * (1 + rate)
    return df

def add_discount(df, amount):
    df['final'] = df['total_with_tax'] - amount
    return df

df_final = (
    df
    .pipe(add_tax, rate=0.08)
    .pipe(add_discount, amount=5)
)
```

### Binning & Discretization

```python
# pd.cut() - Equal-width bins
df['age_bin'] = pd.cut(df['age'], bins=5)
df['age_bin'] = pd.cut(df['age'], bins=[0, 25, 40, 60, 100])
df['age_bin'] = pd.cut(
    df['age'],
    bins=[0, 18, 35, 50, 65, 100],
    labels=['Child', 'Young Adult', 'Adult', 'Middle Age', 'Senior'],
    include_lowest=True
)

# pd.qcut() - Equal-frequency bins (quantiles)
df['salary_quartile'] = pd.qcut(df['salary'], q=4)
df['salary_quartile'] = pd.qcut(
    df['salary'],
    q=4,
    labels=['Q1', 'Q2', 'Q3', 'Q4']
)
df['salary_decile'] = pd.qcut(df['salary'], q=10)

# Custom binning
bins = [0, 50000, 100000, 150000, 200000]
labels = ['Low', 'Medium', 'High', 'Very High']
df['salary_tier'] = pd.cut(df['salary'], bins=bins, labels=labels)
```

### Sorting & Ranking

```python
# Sort by values
df.sort_values('age')                    # Ascending
df.sort_values('age', ascending=False)   # Descending
df.sort_values(['city', 'age'])          # Multiple columns
df.sort_values(['city', 'age'], ascending=[True, False])
df.sort_values('age', na_position='first')  # NaN handling

# Sort by index
df.sort_index()
df.sort_index(ascending=False)

# Ranking
df['age_rank'] = df['age'].rank()
df['age_rank'] = df['age'].rank(method='dense')  # No gaps in ranking
df['age_rank'] = df['age'].rank(ascending=False)  # Descending rank
df['age_rank'] = df['age'].rank(pct=True)  # Percentile ranks

# Rank within groups
df['rank_in_city'] = df.groupby('city')['salary'].rank(ascending=False)

# Top N (faster than sort + head)
top_10 = df.nlargest(10, 'salary')
bottom_10 = df.nsmallest(10, 'salary')
top_per_group = df.groupby('city').apply(lambda x: x.nlargest(3, 'salary'))
```

### Reshaping

```python
# pivot() - Reshape long to wide
sales = pd.DataFrame({
    'date': ['2025-01-01', '2025-01-01', '2025-01-02', '2025-01-02'],
    'product': ['A', 'B', 'A', 'B'],
    'revenue': [100, 150, 120, 180]
})

pivot = sales.pivot(index='date', columns='product', values='revenue')
# Result:
# product    A    B
# date            
# 2025-01-01 100  150
# 2025-01-02 120  180

# pivot_table() - With aggregation
pivot_table = sales.pivot_table(
    index='date',
    columns='product',
    values='revenue',
    aggfunc='sum',
    fill_value=0,
    margins=True,           # Add row/column totals
    margins_name='Total'
)

# melt() - Reshape wide to long (unpivot)
melted = pivot.reset_index().melt(
    id_vars=['date'],
    value_vars=['A', 'B'],
    var_name='product',
    value_name='revenue'
)

# stack() / unstack() - Convert MultiIndex levels
stacked = df.stack()      # Columns → rows
unstacked = df.unstack()  # Rows → columns

# crosstab() - Frequency table
pd.crosstab(df['city'], df['age_group'])
pd.crosstab(df['city'], df['age_group'], normalize='index')  # Row %
```


## 🎰 GroupBy & Aggregation

### GroupBy Fundamentals

```python
# Simple groupby
grouped = df.groupby('city')

# Multiple grouping columns
grouped = df.groupby(['city', 'category'])

# Single aggregation
df.groupby('city')['salary'].mean()
df.groupby('city')['salary'].sum()

# Multiple aggregations
df.groupby('city')['salary'].agg(['mean', 'sum', 'count', 'std'])

# Different aggregations per column
df.groupby('city').agg({
    'salary': ['mean', 'sum', 'std'],
    'age': ['min', 'max', 'mean'],
    'id': 'count'
})

# Named aggregations (Pandas 0.25+, cleaner!)
df.groupby('city').agg(
    avg_salary=('salary', 'mean'),
    total_revenue=('revenue', 'sum'),
    employee_count=('id', 'count'),
    max_age=('age', 'max'),
    min_age=('age', 'min')
).reset_index()

# Custom aggregation
df.groupby('city')['salary'].agg(lambda x: x.max() - x.min())

# Multiple functions including custom
def salary_range(x):
    return x.max() - x.min()

df.groupby('city')['salary'].agg(['mean', 'std', salary_range])
```

### Advanced GroupBy

```python
# Iterate through groups
for city, group_df in df.groupby('city'):
    print(f"{city}: {len(group_df)} employees")
    print(group_df[['name', 'salary']].head())

# Get specific group
nyc_data = df.groupby('city').get_group('NYC')

# Filter groups
# Keep only cities with >10 employees
df.groupby('city').filter(lambda x: len(x) > 10)

# Transform (returns same shape as original)
df['salary_vs_city_avg'] = df.groupby('city')['salary'].transform(
    lambda x: x - x.mean()
)

df['percentile_in_city'] = df.groupby('city')['salary'].transform(
    lambda x: x.rank(pct=True)
)

# Apply (most flexible, can return any shape)
def top_earners(group):
    return group.nlargest(3, 'salary')

top_3_per_city = df.groupby('city').apply(top_earners)

# 🎯 Production Pattern
result = (
    df
    .groupby(['city', 'department'])
    .agg(
        avg_salary=('salary', 'mean'),
        total_employees=('id', 'count'),
        total_payroll=('salary', 'sum')
    )
    .reset_index()
    .sort_values('avg_salary', ascending=False)
)
```

### Window Functions

```python
# Cumulative operations
df['cumsum'] = df.groupby('user')['amount'].cumsum()
df['cumcount'] = df.groupby('user').cumcount()
df['cummax'] = df.groupby('user')['score'].cummax()
df['cummin'] = df.groupby('user')['score'].cummin()

# Shifting
df['prev_value'] = df.groupby('user')['amount'].shift(1)
df['next_value'] = df.groupby('user')['amount'].shift(-1)

# Percentage change
df['pct_change'] = df.groupby('user')['amount'].pct_change()

# Difference
df['diff'] = df.groupby('user')['amount'].diff()

# Ranking within groups
df['rank'] = df.groupby('category')['value'].rank(ascending=False)

# Rolling windows
df['rolling_mean'] = df['value'].rolling(window=7).mean()
df['rolling_std'] = df['value'].rolling(window=7).std()
df['rolling_sum'] = df['value'].rolling(window=30).sum()

# Rolling with groups
df['rolling_avg'] = (
    df.sort_values(['user', 'date'])
    .groupby('user')['value']
    .rolling(window=7, min_periods=1)
    .mean()
    .reset_index(0, drop=True)
)

# Expanding (cumulative)
df['expanding_mean'] = df['value'].expanding().mean()

# Exponentially weighted
df['ewm'] = df['value'].ewm(span=7).mean()
```

### Pivot Tables Deep Dive

```python
# Complex pivot table
pivot = pd.pivot_table(
    df,
    values='revenue',
    index=['city', 'year'],
    columns=['product'],
    aggfunc='sum',
    fill_value=0,
    margins=True,
    margins_name='Total'
)

# Multiple value columns
pivot = pd.pivot_table(
    df,
    values=['revenue', 'quantity'],
    index='city',
    columns='product',
    aggfunc={'revenue': 'sum', 'quantity': 'mean'}
)

# Custom aggregations
pivot = pd.pivot_table(
    df,
    values='revenue',
    index='city',
    aggfunc=[np.sum, np.mean, lambda x: x.max() - x.min()]
)

# Crosstab for frequency tables
pd.crosstab(df['city'], df['product'])
pd.crosstab(df['city'], df['product'], normalize='index')  # Row %
pd.crosstab(df['city'], df['product'], normalize='columns')  # Column %
pd.crosstab(df['city'], df['product'], margins=True)
```


## 🔗 Merging, Joining & Concatenation

### Merge (SQL-style Joins)

```python
# Inner join (default - intersection)
result = pd.merge(df1, df2, on='user_id')
result = pd.merge(df1, df2, on=['user_id', 'date'])

# Left join (all from left)
left = pd.merge(df1, df2, on='key', how='left')

# Right join (all from right)
right = pd.merge(df1, df2, on='key', how='right')

# Outer join (union)
outer = pd.merge(df1, df2, on='key', how='outer')

# Cross join (Cartesian product)
cross = pd.merge(df1, df2, how='cross')

# Different column names
result = pd.merge(
    df1, df2,
    left_on='user_id',
    right_on='id',
    how='left'
)

# Merge on index
result = pd.merge(
    df1, df2,
    left_index=True,
    right_index=True,
    how='inner'
)

# Suffixes for overlapping columns
result = pd.merge(
    df1, df2,
    on='key',
    suffixes=('_left', '_right')
)

# Validate merge (catch data issues!)
result = pd.merge(
    df1, df2,
    on='key',
    validate='one_to_one'  # or 'one_to_many', 'many_to_one', 'many_to_many'
)

# Indicator column (track source)
result = pd.merge(
    df1, df2,
    on='key',
    how='outer',
    indicator=True
)
# Adds '_merge' column: 'left_only', 'right_only', 'both'

# 🎯 Production Pattern: Always validate!
print(f"Left: {df1.shape}, Right: {df2.shape}")
result = pd.merge(df1, df2, on='key', how='left', indicator=True, validate='many_to_one')
print(f"Result: {result.shape}")
print(result['_merge'].value_counts())
```

### Join (Index-based)

```python
# Join on index (simpler for index joins)
result = df1.join(df2)
result = df1.join(df2, how='left')
result = df1.join(df2, on='key')  # df1 column to df2 index

# Join multiple DataFrames
result = df1.join([df2, df3, df4])

# When to use join vs merge?
# - join: Index-based, simpler syntax
# - merge: More flexible, explicit, production preferred
```

### Concatenate (Stacking)

```python
# Vertical concatenation (stack rows)
result = pd.concat([df1, df2])
result = pd.concat([df1, df2], ignore_index=True)

# Horizontal concatenation (stack columns)
result = pd.concat([df1, df2], axis=1)

# With keys (MultiIndex)
result = pd.concat(
    [df1, df2, df3],
    keys=['group1', 'group2', 'group3']
)

# Only common columns (inner join)
result = pd.concat([df1, df2], join='inner')

# .append() is DEPRECATED in Pandas 2.0+
# OLD: df1.append(df2)
# NEW: pd.concat([df1, df2])

# 🎯 Production Pattern: Build lists, concat once
dfs = []
for file in file_list:
    df = pd.read_csv(file)
    df = process(df)
    dfs.append(df)

result = pd.concat(dfs, ignore_index=True)
# Much faster than repeated concatenation!
```

### Advanced Merges

```python
# Merge with tolerance (time series)
result = pd.merge_asof(
    df1, df2,
    on='timestamp',
    direction='nearest',     # 'backward', 'forward', 'nearest'
    tolerance=pd.Timedelta('1 hour')
)

# Nearest key merge
trades = pd.DataFrame({
    'time': pd.to_datetime(['2025-01-01 09:00', '2025-01-01 10:00']),
    'ticker': ['AAPL', 'AAPL'],
    'price': [150, 152]
})

quotes = pd.DataFrame({
    'time': pd.to_datetime(['2025-01-01 09:01', '2025-01-01 10:02']),
    'ticker': ['AAPL', 'AAPL'],
    'bid': [149, 151]
})

result = pd.merge_asof(
    trades, quotes,
    on='time',
    by='ticker',
    direction='backward'
)

# Self join
employees = pd.DataFrame({
    'employee_id': [1, 2, 3],
    'name': ['Alice', 'Bob', 'Charlie'],
    'manager_id': [np.nan, 1, 1]
})

result = pd.merge(
    employees,
    employees,
    left_on='manager_id',
    right_on='employee_id',
    suffixes=('', '_manager')
)
```


## 📅 Time Series Analysis

### DatetimeIndex Basics

```python
# Create date ranges
dates = pd.date_range('2025-01-01', periods=365, freq='D')
dates = pd.date_range('2025-01-01', '2025-12-31', freq='D')
business_days = pd.bdate_range('2025-01-01', periods=252)

# Frequency aliases
# D: Day, B: Business day, W: Week, M: Month end, MS: Month start
# Q: Quarter end, A/Y: Year end, H: Hour, T/min: Minute, S: Second

# Create time series
ts = pd.Series(np.random.randn(365), index=dates)

# Parse dates
df['date'] = pd.to_datetime(df['date_string'])
df['date'] = pd.to_datetime(df['date_string'], format='%Y-%m-%d')
df['date'] = pd.to_datetime(df['date_string'], errors='coerce')

# Extract components
df['year'] = df['date'].dt.year
df['month'] = df['date'].dt.month
df['day'] = df['date'].dt.day
df['dayofweek'] = df['date'].dt.dayofweek  # Monday=0
df['quarter'] = df['date'].dt.quarter
df['is_weekend'] = df['date'].dt.dayofweek >= 5
df['is_month_end'] = df['date'].dt.is_month_end

# Format dates
df['formatted'] = df['date'].dt.strftime('%Y-%m-%d')

# 🎯 Production: Always handle timezones!
df['date'] = pd.to_datetime(df['date'], utc=True)
df['date'] = df['date'].dt.tz_localize('UTC')
df['date'] = df['date'].dt.tz_convert('America/New_York')
```

### Time-based Selection

```python
# Set datetime index
df = df.set_index('date')

# Partial string indexing (magic!)
ts['2025']           # All of 2025
ts['2025-01']        # January 2025
ts['2025-01-15']     # Specific day

# Range selection
ts['2025-01-01':'2025-01-31']

# Select by time
ts.at_time('10:00')              # All 10:00 AM
ts.between_time('09:00', '17:00')  # Business hours

# Truncate
ts.truncate(before='2025-06-01', after='2025-12-31')

# 🎯 Production: Explicit date filtering
start, end = '2025-01-01', '2025-12-31'
filtered = df[(df.index >= start) & (df.index <= end)]
```

### Resampling

```python
# Downsampling (high → low frequency)
daily = pd.Series(range(365), index=pd.date_range('2025-01-01', periods=365))

monthly = daily.resample('M').mean()   # Month-end average
monthly = daily.resample('M').sum()    # Month-end sum
monthly = daily.resample('M').last()   # Last value of month
weekly = daily.resample('W').first()   # First value of week

# Upsampling (low → high frequency)
daily_filled = monthly.resample('D').ffill()  # Forward fill
daily_interp = monthly.resample('D').interpolate()  # Interpolate

# Custom aggregation
result = daily.resample('W').agg(['mean', 'std', 'min', 'max'])

# OHLC (financial data)
ohlc = daily.resample('M').ohlc()

# Multiple columns
df.resample('D').agg({
    'revenue': 'sum',
    'users': 'max',
    'sessions': 'mean'
})
```

### Shifting & Lagging

```python
# Shift forward/backward
df['prev_day'] = df['value'].shift(1)    # Previous day
df['next_day'] = df['value'].shift(-1)   # Next day

# Percentage change
df['pct_change'] = df['value'].pct_change()
df['pct_change_7d'] = df['value'].pct_change(periods=7)

# Difference
df['diff'] = df['value'].diff()
df['diff_7d'] = df['value'].diff(periods=7)

# Shift by time period (DatetimeIndex)
df['last_week'] = df['value'].shift(freq='7D')

# Financial calculations
df['daily_return'] = df['price'].pct_change()
df['log_return'] = np.log(df['price'] / df['price'].shift(1))

# Moving average crossover
df['ma_50'] = df['price'].rolling(50).mean()
df['ma_200'] = df['price'].rolling(200).mean()
df['signal'] = np.where(df['ma_50'] > df['ma_200'], 1, 0)
```

### Time Zones

```python
# Create timezone-aware
df['date'] = pd.to_datetime(df['date'], utc=True)

# Localize (naive → aware)
df['date'] = df['date'].dt.tz_localize('UTC')

# Convert timezone
df['ny_time'] = df['date'].dt.tz_convert('America/New_York')
df['tokyo_time'] = df['date'].dt.tz_convert('Asia/Tokyo')

# Remove timezone
df['naive'] = df['date'].dt.tz_localize(None)

# 🎯 Production: Store UTC, display local
df['timestamp_utc'] = pd.to_datetime(df['timestamp'], utc=True)
df['timestamp_local'] = df['timestamp_utc'].dt.tz_convert('America/New_York')
```


## 🚀 Advanced Techniques

### MultiIndex Mastery

```python
# Create MultiIndex
idx = pd.MultiIndex.from_tuples([
    ('USA', 'NY'), ('USA', 'CA'), ('UK', 'London')
], names=['country', 'city'])

df = pd.DataFrame({'pop': [8.3, 39.5, 9.0]}, index=idx)

# Selection
df.loc['USA']                    # All USA
df.loc[('USA', 'NY')]           # Specific tuple
df.xs('NY', level='city')       # Cross-section

# Stack/Unstack
stacked = df.stack()
unstacked = stacked.unstack()

# Swap levels
df.swaplevel('country', 'city')

# Sort by level
df.sort_index(level='city')
```

### Categorical Data

```python
# Create categorical
df['size'] = pd.Categorical(
    ['S', 'M', 'L', 'M'],
    categories=['S', 'M', 'L', 'XL'],
    ordered=True
)

# Convert
df['color'] = df['color'].astype('category')

# Memory savings (huge for repeated strings!)
print(df.memory_usage(deep=True))
df['country'] = df['country'].astype('category')
print(df.memory_usage(deep=True))  # 90%+ reduction!

# Operations on ordered categories
df[df['size'] > 'M']  # Works because ordered=True

# Category attributes
df['size'].cat.categories
df['size'].cat.codes
df['size'].cat.add_categories(['XXL'])
```

### Method Chaining

```python
# Build readable data pipelines
result = (
    df
    .query('age > 25')
    .assign(
        age_group=lambda x: pd.cut(x['age'], bins=[0, 30, 50, 100]),
        income_level=lambda x: pd.qcut(x['income'], q=4)
    )
    .groupby(['city', 'age_group'])
    .agg(
        avg_income=('income', 'mean'),
        count=('id', 'count')
    )
    .reset_index()
    .sort_values('avg_income', ascending=False)
    .head(10)
)

# .pipe() for custom functions
def add_features(df):
    df['total'] = df['price'] * df['qty']
    return df

def filter_valid(df):
    return df[df['total'] > 100]

result = (
    df
    .pipe(add_features)
    .pipe(filter_valid)
)
```


## ⚡️ Performance Optimization

### Memory Optimization

```python
# Check memory
df.memory_usage(deep=True)
df.memory_usage(deep=True).sum() / 1024**2  # MB

# Downcast numeric types
df['int_col'] = pd.to_numeric(df['int_col'], downcast='integer')
df['float_col'] = pd.to_numeric(df['float_col'], downcast='float')

# Use category for repeated strings
df['category'] = df['category'].astype('category')

# Read only needed columns
df = pd.read_csv('data.csv', usecols=['col1', 'col2'])

# Read in chunks
for chunk in pd.read_csv('huge.csv', chunksize=100000):
    process(chunk)

# 🎯 Production: Optimize on read
dtype_dict = {
    'user_id': 'int32',
    'category': 'category',
    'score': 'float32'
}
df = pd.read_csv('data.csv', dtype=dtype_dict)
```

### Vectorization

```python
# ❌ BAD: Loop
result = []
for idx, row in df.iterrows():
    result.append(row['a'] * row['b'])

# ✅ GOOD: Vectorized (100x faster!)
df['result'] = df['a'] * df['b']

# Speed hierarchy (fastest → slowest):
# 1. Vectorized ops (df['a'] * df['b'])
# 2. Built-in methods (.sum(), .mean())
# 3. Cython methods (.groupby().agg())
# 4. .apply() with NumPy
# 5. .apply() with lambda
# 6. .iterrows()
# 7. Python loops

# Use NumPy
df['sqrt'] = np.sqrt(df['value'])  # Fast

# .query() for large DataFrames
df.query('age > 30 and city == "NYC"')  # Faster than boolean indexing
```

### Index Optimization

```python
# Set index for frequent lookups
df = df.set_index('user_id')
user = df.loc[12345]  # O(1) lookup!

# Sort for faster range queries
df = df.sort_index()
result = df.loc[1000:2000]

# MultiIndex for hierarchical queries
df = df.set_index(['country', 'city'])
usa = df.loc['USA']
```


## 🏭 Production Best Practices

### Validation

```python
def validate_df(df):
    """Validate DataFrame meets requirements."""
    assert len(df) > 0, "Empty DataFrame"
    assert not df.duplicated().any(), "Duplicates found"
    assert 'id' in df.columns, "Missing 'id' column"
    assert df['id'].dtype == 'int64', "Wrong dtype for 'id'"
    assert df['id'].notnull().all(), "Nulls in 'id'"
    return True
```

### Error Handling

```python
import logging

logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

def load_data(filepath):
    try:
        if not os.path.exists(filepath):
            raise FileNotFoundError(f"{filepath} not found")
        
        df = pd.read_csv(filepath)
        
        if len(df) == 0:
            raise ValueError("Empty DataFrame")
        
        logger.info(f"Loaded {len(df)} rows")
        return df
        
    except Exception as e:
        logger.error(f"Error loading data: {e}")
        raise
```

### Configuration

```python
# config.yaml
"""
data:
  input: data/raw/input.csv
  output: data/processed/output.csv
processing:
  remove_duplicates: true
  fill_nulls: median
"""

import yaml

with open('config.yaml') as f:
    config = yaml.safe_load(f)

df = pd.read_csv(config['data']['input'])
```

### Testing

```python
import unittest
import pandas.testing as tm

class TestDataProcessing(unittest.TestCase):
    def test_column_addition(self):
        df = pd.DataFrame({'a': [1, 2], 'b': [3, 4]})
        result = df.copy()
        result['c'] = result['a'] + result['b']
        
        expected = pd.DataFrame({
            'a': [1, 2],
            'b': [3, 4],
            'c': [4, 6]
        })
        
        tm.assert_frame_equal(result, expected)
```


## 💼 Real-World Projects

### Project 1: E-commerce Analytics

```python
# Load orders
orders = pd.read_csv('orders.csv', parse_dates=['order_date'])

# Feature engineering
orders['month'] = orders['order_date'].dt.to_period('M')
orders['revenue'] = orders['quantity'] * orders['unit_price']

# Customer cohorts
cohorts = (
    orders
    .groupby('customer_id')
    .agg(
        first_purchase=('order_date', 'min'),
        total_orders=('order_id', 'count'),
        total_revenue=('revenue', 'sum')
    )
)

cohorts['cohort_month'] = cohorts['first_purchase'].dt.to_period('M')

# Monthly trends
monthly = (
    orders
    .groupby('month')
    .agg(
        revenue=('revenue', 'sum'),
        orders=('order_id', 'count'),
        customers=('customer_id', 'nunique')
    )
)

# RFM Analysis
current_date = orders['order_date'].max()

rfm = (
    orders
    .groupby('customer_id')
    .agg(
        recency=('order_date', lambda x: (current_date - x.max()).days),
        frequency=('order_id', 'count'),
        monetary=('revenue', 'sum')
    )
)

# Score customers
rfm['r_score'] = pd.qcut(rfm['recency'], 4, labels=[4, 3, 2, 1])
rfm['f_score'] = pd.qcut(rfm['frequency'].rank(method='first'), 4, labels=[1, 2, 3, 4])
rfm['m_score'] = pd.qcut(rfm['monetary'], 4, labels=[1, 2, 3, 4])

# Export
monthly.to_csv('monthly_revenue.csv')
rfm.to_parquet('customer_rfm.parquet')
```

### Project 2: Time Series Forecasting Prep

```python
# Load & resample
df = pd.read_csv('sales.csv', parse_dates=['date'], index_col='date')
df = df.resample('D').sum().fillna(method='ffill')

# Create features
df['dayofweek'] = df.index.dayofweek
df['month'] = df.index.month
df['is_weekend'] = df.index.dayofweek >= 5

# Lag features
for lag in [1, 7, 14, 30]:
    df[f'lag_{lag}'] = df['sales'].shift(lag)

# Rolling features
for window in [7, 14, 30]:
    df[f'roll_mean_{window}'] = df['sales'].rolling(window).mean()
    df[f'roll_std_{window}'] = df['sales'].rolling(window).std()

# Decomposition
from statsmodels.tsa.seasonal import seasonal_decompose
decomp = seasonal_decompose(df['sales'], model='additive', period=7)
df['trend'] = decomp.trend
df['seasonal'] = decomp.seasonal

# Train/test split
train = df[:'2024-12-31']
test = df['2025-01-01':]
```


## ⚠️ Common Pitfalls & Solutions

### 1. SettingWithCopyWarning

```python
# ❌ WRONG
df[df['age'] > 30]['salary'] = 50000

# ✅ CORRECT
df.loc[df['age'] > 30, 'salary'] = 50000
```

### 2. Type Inconsistencies

```python
# Always check types
df.dtypes
df.info()

# Explicit conversion
df['id'] = pd.to_numeric(df['id'], errors='coerce')
```

### 3. Index Alignment

```python
# Pandas aligns on index automatically
s1 = pd.Series([1, 2, 3], index=['a', 'b', 'c'])
s2 = pd.Series([4, 5, 6], index=['b', 'c', 'd'])
s1 + s2  # Aligns on index, NaN for missing

# Explicit control
s1.add(s2, fill_value=0)
```

### 4. Memory Issues

```python
# ❌ Load entire file
df = pd.read_csv('huge.csv')

# ✅ Read in chunks
chunks = []
for chunk in pd.read_csv('huge.csv', chunksize=100000):
    chunks.append(chunk)
df = pd.concat(chunks)
```


## 🎉 Congratulations!

You've completed the Pandas Zero to Production Hero guide! 

**What's Next?**
1. Build real projects
2. Explore NumPy, Matplotlib, Scikit-learn
3. Try alternatives: Polars, Dask
4. Contribute to open source
5. Share your knowledge!

---

<div align="center">
  
**Made with ❤️ for the Python Community by @RajeshTechForge**

</div>
