<div align="center">

# Awesome Pandas [![Awesome](https://cdn.rawgit.com/sindresorhus/awesome/d7305f38d29fed78fa85652e3a63e154dd8e8829/media/badge.svg)](https://github.com/sindresorhus/awesome)

🐼 **Master data wrangling for ML/DL in 2026 - the fun way!**

<br>
<br>
</div>


## 🎯 Why This Guide?

You want to prepare data for Machine Learning. This guide teaches you **exactly** what you need to:
- Load and explore datasets
- Clean messy data
- Engineer features
- Prepare data for models
- Handle real-world ML workflows

No fluff. Just the essentials. Let's dive in! 🚀


## 📚 Quick Navigation

1. [Setup & Basics](#-setup--basics)
2. [Loading Data](#-loading-data-like-a-pro)
3. [Exploring Your Data](#-exploring-your-data-eda)
4. [Selecting & Filtering](#-selecting--filtering-data)
5. [Cleaning Data](#-cleaning-data)
6. [Feature Engineering](#-feature-engineering)
7. [Handling Dates](#-handling-dates--time)
8. [Grouping & Aggregating](#-grouping--aggregating)
9. [Merging Datasets](#-merging-datasets)
10. [Preparing for ML](#-preparing-data-for-ml)
11. [Quick Reference](#-quick-reference-cheat-sheet)

---

## 🛠 Setup & Basics

### Installation

```bash
# Basic installation
pip install pandas numpy

# With ML essentials
pip install pandas numpy scikit-learn

# For data visualization (optional but recommended)
pip install pandas matplotlib seaborn
```

### First Steps

```python
import pandas as pd
import numpy as np

# Display settings (makes output prettier)
pd.set_option('display.max_columns', None)
pd.set_option('display.max_rows', 100)
pd.set_option('display.precision', 2)

# Check version (should be 2.0+)
print(pd.__version__)
```

### The Two Core Structures

```python
# Series - 1D array (like a column)
ages = pd.Series([25, 30, 35, 40], name='age')
print(ages)

# DataFrame - 2D table (your main tool!)
df = pd.DataFrame({
    'name': ['Alice', 'Bob', 'Charlie', 'David'],
    'age': [25, 30, 35, 40],
    'salary': [70000, 80000, 90000, 100000]
})
print(df)
```

> [!NOTE]
>**Think of it**:  
>Series = Excel column, 
>DataFrame = Excel spreadsheet


## 📥 Loading Data Like a Pro

### CSV (Most Common)

```python
# Basic loading
df = pd.read_csv('data.csv')

# Power loading (for real datasets)
df = pd.read_csv(
    'data.csv',
    sep=',',                    # Delimiter
    header=0,                   # First row is header
    index_col='id',             # Use 'id' column as index
    usecols=['id', 'name', 'age', 'salary'],  # Load only these columns
    dtype={'id': 'int32', 'age': 'int32'},    # Specify data types
    parse_dates=['created_at'],               # Parse as datetime
    na_values=['NA', 'null', ''],            # Treat these as NaN
    nrows=10000                              # Load only first 10k rows
)

# For huge files (read in chunks)
chunks = []
for chunk in pd.read_csv('huge_file.csv', chunksize=10000):
    chunks.append(chunk)
df = pd.concat(chunks, ignore_index=True)
```

### Other Formats

```python
# Excel
df = pd.read_excel('data.xlsx', sheet_name='Sheet1')

# JSON (from APIs)
df = pd.read_json('data.json')
df = pd.read_json('data.json', lines=True)  # JSON Lines format

# SQL
from sqlalchemy import create_engine
engine = create_engine('sqlite:///database.db')
df = pd.read_sql('SELECT * FROM users', engine)

# Parquet (fast and efficient!)
df = pd.read_parquet('data.parquet')  # Recommended for large datasets
```

### Quick Data Creation

```python
# From dictionary
df = pd.DataFrame({
    'feature1': [1, 2, 3],
    'feature2': [4, 5, 6],
    'target': [0, 1, 0]
})

# From NumPy array
import numpy as np
data = np.random.randn(100, 5)
df = pd.DataFrame(data, columns=['A', 'B', 'C', 'D', 'E'])

# From list of dictionaries (API responses)
data = [
    {'name': 'Alice', 'score': 95},
    {'name': 'Bob', 'score': 87}
]
df = pd.DataFrame(data)
```


## 🔍 Exploring Your Data (EDA)

### First Look

```python
# Shape and basic info
print(df.shape)           # (rows, columns)
print(df.info())          # Column types and memory
print(df.dtypes)          # Data types
print(df.columns)         # Column names

# Quick peek
print(df.head(10))        # First 10 rows
print(df.tail(10))        # Last 10 rows
print(df.sample(5))       # Random 5 rows

# Statistical summary
print(df.describe())      # Numeric columns only
print(df.describe(include='all'))  # All columns
```

### Understanding Your Data

```python
# Missing values
print(df.isnull().sum())  # Count nulls per column
print(df.isnull().sum() / len(df) * 100)  # Percentage

# Unique values
print(df['category'].unique())      # All unique values
print(df['category'].nunique())     # Count of unique values
print(df['category'].value_counts())  # Frequency count

# Memory usage
print(df.memory_usage(deep=True))
print(f"Total: {df.memory_usage(deep=True).sum() / 1024**2:.2f} MB")

# Correlations (important for ML!)
print(df.corr())  # Numeric columns only

# Quick visualization
df.hist(figsize=(12, 10))
df['age'].plot(kind='hist', bins=30)
df.boxplot(column='salary', by='department')
```


## 🎯 Selecting & Filtering Data

### Column Selection

```python
# Single column (returns Series)
ages = df['age']

# Multiple columns (returns DataFrame)
subset = df[['name', 'age', 'salary']]

# All numeric columns
numeric_df = df.select_dtypes(include=[np.number])

# All categorical columns
categorical_df = df.select_dtypes(include=['object', 'category'])
```

### Row Selection

```python
# By position (like array indexing)
first_row = df.iloc[0]           # First row
first_10 = df.iloc[:10]          # First 10 rows
last_row = df.iloc[-1]           # Last row

# By label (if you have custom index)
df = df.set_index('user_id')
user_data = df.loc[12345]        # User with id 12345

# Boolean indexing (most useful for ML!)
young = df[df['age'] < 30]
high_earners = df[df['salary'] > 80000]

# Multiple conditions (use & and | operators)
young_high = df[(df['age'] < 30) & (df['salary'] > 70000)]
nyc_or_la = df[(df['city'] == 'NYC') | (df['city'] == 'LA')]

# Using .loc for powerful selection
df.loc[df['age'] > 30, ['name', 'salary']]  # Rows and columns together

# Query method (cleaner for complex conditions)
df.query('age > 30 and salary > 80000')
df.query('city in ["NYC", "LA"]')
```

### Useful Filters

```python
# Filter by list
cities_of_interest = ['NYC', 'LA', 'Chicago']
df[df['city'].isin(cities_of_interest)]

# Filter by range
df[df['age'].between(25, 40)]

# String operations
df[df['name'].str.startswith('A')]
df[df['email'].str.contains('@gmail.com')]

# Remove nulls
df_clean = df[df['important_column'].notnull()]

# Top/Bottom N
top_10 = df.nlargest(10, 'salary')
bottom_10 = df.nsmallest(10, 'salary')
```


## 🧹 Cleaning Data

### Handling Missing Values

```python
# Check for missing
print(df.isnull().sum())

# Drop rows with any missing values
df_clean = df.dropna()

# Drop rows where specific columns are missing
df_clean = df.dropna(subset=['important_col1', 'important_col2'])

# Drop columns with too many missing values
df_clean = df.dropna(axis=1, thresh=len(df) * 0.7)  # Keep cols with 70%+ data

# Fill missing values
df['age'] = df['age'].fillna(df['age'].median())      # With median
df['category'] = df['category'].fillna('Unknown')     # With constant
df['price'] = df['price'].fillna(df['price'].mean())  # With mean

# Forward/backward fill (for time series)
df['value'] = df['value'].ffill()   # Forward fill
df['value'] = df['value'].bfill()   # Backward fill

# Different strategies per column
df = df.fillna({
    'age': df['age'].median(),
    'city': 'Unknown',
    'salary': df['salary'].mean()
})

# Interpolate (smooth filling)
df['temperature'] = df['temperature'].interpolate()
```

### Handling Duplicates

```python
# Find duplicates
print(df.duplicated().sum())  # Count duplicates

# See duplicate rows
duplicates = df[df.duplicated(keep=False)]

# Remove duplicates
df_clean = df.drop_duplicates()

# Remove based on specific columns
df_clean = df.drop_duplicates(subset=['user_id', 'date'])

# Keep last occurrence instead of first
df_clean = df.drop_duplicates(keep='last')
```

### Data Type Conversions

```python
# Convert to numeric
df['age'] = pd.to_numeric(df['age'], errors='coerce')  # Invalid → NaN

# Convert to datetime
df['date'] = pd.to_datetime(df['date'])
df['date'] = pd.to_datetime(df['date'], format='%Y-%m-%d')

# Convert to category (saves memory!)
df['country'] = df['country'].astype('category')
df['status'] = df['status'].astype('category')

# Downcast to save memory
df['age'] = df['age'].astype('int32')  # Instead of int64
df['price'] = df['price'].astype('float32')  # Instead of float64
```

### String Cleaning

```python
# Basic operations
df['name'] = df['name'].str.lower()        # Lowercase
df['name'] = df['name'].str.upper()        # Uppercase
df['name'] = df['name'].str.strip()        # Remove whitespace
df['name'] = df['name'].str.title()        # Title Case

# Replace
df['text'] = df['text'].str.replace('old', 'new')
df['text'] = df['text'].str.replace(r'\d+', '', regex=True)  # Remove digits

# Split and extract
df[['first', 'last']] = df['name'].str.split(' ', expand=True)
df['domain'] = df['email'].str.split('@').str[1]

# Check for patterns
df[df['text'].str.contains('keyword', case=False, na=False)]
```

### Outlier Detection

```python
# IQR method (commonly used)
Q1 = df['salary'].quantile(0.25)
Q3 = df['salary'].quantile(0.75)
IQR = Q3 - Q1

lower_bound = Q1 - 1.5 * IQR
upper_bound = Q3 + 1.5 * IQR

# Remove outliers
df_clean = df[(df['salary'] >= lower_bound) & (df['salary'] <= upper_bound)]

# Or cap outliers (winsorize)
df['salary_capped'] = df['salary'].clip(lower=lower_bound, upper=upper_bound)

# Z-score method
from scipy import stats
z_scores = np.abs(stats.zscore(df['salary'].dropna()))
df_clean = df[z_scores < 3]  # Keep within 3 standard deviations
```


## ⚙️ Feature Engineering

### Creating New Features

```python
# Simple math
df['total'] = df['price'] * df['quantity']
df['bmi'] = df['weight'] / (df['height'] ** 2)

# Conditional features
df['is_adult'] = df['age'] >= 18
df['is_premium'] = df['plan'] == 'premium'

# Binning (discretization)
df['age_group'] = pd.cut(
    df['age'],
    bins=[0, 18, 35, 50, 100],
    labels=['Child', 'Young Adult', 'Adult', 'Senior']
)

# Quantile-based binning
df['income_quartile'] = pd.qcut(df['income'], q=4, labels=['Q1', 'Q2', 'Q3', 'Q4'])

# Polynomial features
df['age_squared'] = df['age'] ** 2
df['age_cubed'] = df['age'] ** 3

# Interaction features
df['age_income'] = df['age'] * df['income']
```

### Encoding Categorical Variables

```python
# One-hot encoding (for nominal categories)
df_encoded = pd.get_dummies(df, columns=['category'], prefix='cat')

# Or with drop_first to avoid multicollinearity
df_encoded = pd.get_dummies(df, columns=['category'], drop_first=True)

# Label encoding (for ordinal categories or trees)
from sklearn.preprocessing import LabelEncoder
le = LabelEncoder()
df['category_encoded'] = le.fit_transform(df['category'])

# Pandas categorical (memory efficient)
df['size'] = pd.Categorical(
    df['size'],
    categories=['S', 'M', 'L', 'XL'],
    ordered=True
)
df['size_encoded'] = df['size'].cat.codes
```

### Text Features

```python
# Length features
df['text_length'] = df['text'].str.len()
df['word_count'] = df['text'].str.split().str.len()

# Contains patterns
df['has_email'] = df['text'].str.contains('@', na=False)
df['has_url'] = df['text'].str.contains('http', na=False)

# TF-IDF (with scikit-learn)
from sklearn.feature_extraction.text import TfidfVectorizer
tfidf = TfidfVectorizer(max_features=100)
tfidf_features = tfidf.fit_transform(df['text'])
```

### Aggregated Features

```python
# Group statistics
df['user_avg_purchase'] = df.groupby('user_id')['amount'].transform('mean')
df['user_total_spent'] = df.groupby('user_id')['amount'].transform('sum')
df['user_purchase_count'] = df.groupby('user_id')['amount'].transform('count')

# Rank within group
df['rank_in_category'] = df.groupby('category')['score'].rank(ascending=False)

# Difference from group mean
df['diff_from_avg'] = df['score'] - df.groupby('category')['score'].transform('mean')
```

### Scaling (Important for ML!)

```python
# Standardization (mean=0, std=1)
from sklearn.preprocessing import StandardScaler
scaler = StandardScaler()
df[['age', 'income']] = scaler.fit_transform(df[['age', 'income']])

# Min-Max scaling (0 to 1)
from sklearn.preprocessing import MinMaxScaler
scaler = MinMaxScaler()
df[['age', 'income']] = scaler.fit_transform(df[['age', 'income']])

# Manual normalization
df['age_normalized'] = (df['age'] - df['age'].min()) / (df['age'].max() - df['age'].min())

# Log transformation (for skewed data)
df['log_income'] = np.log1p(df['income'])  # log(1 + x) to handle zeros
```


## 📅 Handling Dates & Time

### Parsing Dates

```python
# Convert to datetime
df['date'] = pd.to_datetime(df['date_string'])
df['date'] = pd.to_datetime(df['date_string'], format='%Y-%m-%d')

# Handle errors
df['date'] = pd.to_datetime(df['date_string'], errors='coerce')  # Invalid → NaT
```

### Extracting Date Components

```python
# Date parts
df['year'] = df['date'].dt.year
df['month'] = df['date'].dt.month
df['day'] = df['date'].dt.day
df['dayofweek'] = df['date'].dt.dayofweek  # Monday=0
df['quarter'] = df['date'].dt.quarter

# Time parts
df['hour'] = df['timestamp'].dt.hour
df['minute'] = df['timestamp'].dt.minute

# Boolean features
df['is_weekend'] = df['date'].dt.dayofweek >= 5
df['is_month_start'] = df['date'].dt.is_month_start
df['is_month_end'] = df['date'].dt.is_month_end
df['is_quarter_start'] = df['date'].dt.is_quarter_start

# Day name
df['day_name'] = df['date'].dt.day_name()
```

### Time-based Features

```python
# Time since event
reference_date = pd.to_datetime('2025-01-01')
df['days_since'] = (reference_date - df['date']).dt.days

# Time differences
df['duration'] = df['end_time'] - df['start_time']
df['hours'] = df['duration'].dt.total_seconds() / 3600

# Lag features (for time series)
df = df.sort_values('date')
df['prev_day_value'] = df['value'].shift(1)
df['next_day_value'] = df['value'].shift(-1)

# Rolling statistics
df['rolling_mean_7d'] = df['value'].rolling(window=7).mean()
df['rolling_std_7d'] = df['value'].rolling(window=7).std()
```

### Time Zones

```python
# Set timezone
df['date'] = pd.to_datetime(df['date'], utc=True)

# Convert timezone
df['date_ny'] = df['date'].dt.tz_convert('America/New_York')

# Remove timezone
df['date_naive'] = df['date'].dt.tz_localize(None)
```


## 🎰 Grouping & Aggregating

### Basic GroupBy

```python
# Single aggregation
df.groupby('category')['price'].mean()
df.groupby('category')['price'].sum()

# Multiple aggregations
df.groupby('category')['price'].agg(['mean', 'sum', 'count', 'std'])

# Different aggregations per column
df.groupby('category').agg({
    'price': ['mean', 'sum'],
    'quantity': ['sum', 'count'],
    'rating': ['mean', 'min', 'max']
})

# Named aggregations (cleaner!)
df.groupby('category').agg(
    avg_price=('price', 'mean'),
    total_sales=('price', 'sum'),
    num_products=('product_id', 'count')
).reset_index()

# Multiple grouping columns
df.groupby(['category', 'region'])['sales'].sum()
```

### Advanced Aggregations

```python
# Custom functions
def price_range(x):
    return x.max() - x.min()

df.groupby('category')['price'].agg(['mean', price_range])

# Percentiles
df.groupby('category')['price'].quantile([0.25, 0.5, 0.75])

# Multiple outputs
df.groupby('user_id').agg({
    'order_id': 'count',
    'amount': ['sum', 'mean', 'max'],
    'date': ['min', 'max']
})
```

### Transform (Keep Original Shape)

```python
# Add group statistics as features
df['category_avg_price'] = df.groupby('category')['price'].transform('mean')
df['category_std_price'] = df.groupby('category')['price'].transform('std')

# Normalize within groups
df['price_normalized'] = df.groupby('category')['price'].transform(
    lambda x: (x - x.mean()) / x.std()
)

# Cumulative operations
df['cumulative_sales'] = df.groupby('user_id')['amount'].cumsum()
df['purchase_number'] = df.groupby('user_id').cumcount() + 1
```

### Pivot Tables

```python
# Basic pivot
pivot = df.pivot_table(
    values='sales',
    index='category',
    columns='region',
    aggfunc='sum',
    fill_value=0
)

# Multiple aggregations
pivot = df.pivot_table(
    values='sales',
    index='category',
    columns='region',
    aggfunc=['sum', 'mean'],
    fill_value=0,
    margins=True  # Add totals
)

# Crosstab (for frequency tables)
pd.crosstab(df['category'], df['region'])
pd.crosstab(df['category'], df['region'], normalize='index')  # Row percentages
```


## 🔗 Merging Datasets

### Concatenating DataFrames

```python
# Stack rows (vertical)
df_combined = pd.concat([df1, df2], ignore_index=True)

# Stack columns (horizontal)
df_combined = pd.concat([df1, df2], axis=1)

# Build list then concat (faster for many dfs)
dfs = []
for file in file_list:
    df = pd.read_csv(file)
    dfs.append(df)
df_all = pd.concat(dfs, ignore_index=True)
```

### Merging (Joins)

```python
# Inner join (only matching rows)
merged = pd.merge(df1, df2, on='user_id', how='inner')

# Left join (all from left, matching from right)
merged = pd.merge(df1, df2, on='user_id', how='left')

# Multiple keys
merged = pd.merge(df1, df2, on=['user_id', 'date'])

# Different column names
merged = pd.merge(df1, df2, left_on='user_id', right_on='id')

# Add suffix for overlapping columns
merged = pd.merge(df1, df2, on='id', suffixes=('_left', '_right'))

# Indicator to see merge source
merged = pd.merge(df1, df2, on='id', how='outer', indicator=True)
# Shows: 'left_only', 'right_only', 'both'
```

### Typical ML Workflows

```python
# 1. Merge features with target
features = pd.read_csv('features.csv')
targets = pd.read_csv('targets.csv')
df = pd.merge(features, targets, on='id')

# 2. Merge with lookup tables
df = pd.merge(df, categories_lookup, on='category_id', how='left')

# 3. Merge time series data
df = pd.merge_asof(
    transactions,
    prices,
    on='timestamp',
    by='product_id',
    direction='backward'  # Use most recent price
)
```


## 🤖 Preparing Data for ML

### Train-Test Split

```python
from sklearn.model_selection import train_test_split

# Separate features and target
X = df.drop('target', axis=1)
y = df['target']

# Split
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y  # Stratify for classification
)

# Time series split (no shuffling!)
split_date = '2024-01-01'
train = df[df['date'] < split_date]
test = df[df['date'] >= split_date]

X_train = train.drop('target', axis=1)
y_train = train['target']
X_test = test.drop('target', axis=1)
y_test = test['target']
```

### Handling Categorical Variables

```python
# Select categorical columns
categorical_cols = X_train.select_dtypes(include=['object', 'category']).columns

# One-hot encode
X_train_encoded = pd.get_dummies(X_train, columns=categorical_cols, drop_first=True)
X_test_encoded = pd.get_dummies(X_test, columns=categorical_cols, drop_first=True)

# Ensure same columns in train and test
X_train_encoded, X_test_encoded = X_train_encoded.align(
    X_test_encoded, join='left', axis=1, fill_value=0
)
```

### Feature Selection

```python
# Select numeric columns only
numeric_features = X.select_dtypes(include=[np.number]).columns
X_numeric = X[numeric_features]

# Drop low variance features
from sklearn.feature_selection import VarianceThreshold
selector = VarianceThreshold(threshold=0.01)
X_selected = selector.fit_transform(X_numeric)

# Correlation-based selection
corr_matrix = df.corr().abs()
upper = corr_matrix.where(np.triu(np.ones(corr_matrix.shape), k=1).astype(bool))
to_drop = [column for column in upper.columns if any(upper[column] > 0.95)]
X_reduced = X.drop(to_drop, axis=1)
```

### Handling Imbalanced Data

```python
# Check class distribution
print(y.value_counts())
print(y.value_counts(normalize=True))

# Undersample majority class
from sklearn.utils import resample

df_majority = df[df['target'] == 0]
df_minority = df[df['target'] == 1]

df_majority_downsampled = resample(
    df_majority,
    replace=False,
    n_samples=len(df_minority),
    random_state=42
)

df_balanced = pd.concat([df_majority_downsampled, df_minority])

# Oversample minority class
df_minority_upsampled = resample(
    df_minority,
    replace=True,
    n_samples=len(df_majority),
    random_state=42
)

df_balanced = pd.concat([df_majority, df_minority_upsampled])
```

### Complete Preprocessing Pipeline

```python
import pandas as pd
import numpy as np
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler

# 1. Load data
df = pd.read_csv('data.csv')

# 2. Handle missing values
df = df.dropna(subset=['important_col'])
df['age'] = df['age'].fillna(df['age'].median())
df['category'] = df['category'].fillna('Unknown')

# 3. Remove duplicates
df = df.drop_duplicates()

# 4. Handle outliers
Q1 = df['price'].quantile(0.25)
Q3 = df['price'].quantile(0.75)
IQR = Q3 - Q1
df = df[(df['price'] >= Q1 - 1.5*IQR) & (df['price'] <= Q3 + 1.5*IQR)]

# 5. Feature engineering
df['total'] = df['price'] * df['quantity']
df['is_weekend'] = df['date'].dt.dayofweek >= 5

# 6. Encode categorical
df = pd.get_dummies(df, columns=['category'], drop_first=True)

# 7. Split features and target
X = df.drop('target', axis=1)
y = df['target']

# 8. Train-test split
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y
)

# 9. Scale features
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)

# 10. Convert back to DataFrame (optional, for interpretability)
X_train_scaled = pd.DataFrame(
    X_train_scaled,
    columns=X_train.columns,
    index=X_train.index
)
X_test_scaled = pd.DataFrame(
    X_test_scaled,
    columns=X_test.columns,
    index=X_test.index
)

print(f"Training set: {X_train_scaled.shape}")
print(f"Test set: {X_test_scaled.shape}")
print("Ready for modeling! 🚀")
```


## 📊 Common ML Data Patterns

### Classification Dataset Prep

```python
# Load and explore
df = pd.read_csv('classification_data.csv')
print(df['target'].value_counts())

# Handle imbalance if needed
# (see Handling Imbalanced Data section above)

# Prepare features
X = df.drop('target', axis=1)
y = df['target']

# Encode categorical
X = pd.get_dummies(X, drop_first=True)

# Split
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y
)

# Scale
from sklearn.preprocessing import StandardScaler
scaler = StandardScaler()
X_train = scaler.fit_transform(X_train)
X_test = scaler.transform(X_test)
```

### Regression Dataset Prep

```python
# Load data
df = pd.read_csv('regression_data.csv')

# Remove outliers
from scipy import stats
z_scores = np.abs(stats.zscore(df.select_dtypes(include=[np.number])))
df = df[(z_scores < 3).all(axis=1)]

# Log transform skewed target
df['target_log'] = np.log1p(df['target'])

# Feature engineering
df['feature_squared'] = df['feature'] ** 2

# Prepare
X = df.drop(['target', 'target_log'], axis=1)
y = df['target_log']

# Split
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)

# Scale
scaler = StandardScaler()
X_train = scaler.fit_transform(X_train)
X_test = scaler.transform(X_test)
```

### Time Series Dataset Prep

```python
# Load and sort
df = pd.read_csv('timeseries_data.csv', parse_dates=['date'])
df = df.sort_values('date')

# Create time features
df['year'] = df['date'].dt.year
df['month'] = df['date'].dt.month
df['dayofweek'] = df['date'].dt.dayofweek
df['is_weekend'] = df['date'].dt.dayofweek >= 5

# Create lag features
for lag in [1, 7, 30]:
    df[f'lag_{lag}'] = df['value'].shift(lag)

# Create rolling features
df['rolling_mean_7'] = df['value'].rolling(7).mean()
df['rolling_std_7'] = df['value'].rolling(7).std()

# Drop NaN from lag/rolling
df = df.dropna()

# Split by time (no shuffle!)
split_idx = int(len(df) * 0.8)
train = df.iloc[:split_idx]
test = df.iloc[split_idx:]

X_train = train.drop('target', axis=1)
y_train = train['target']
X_test = test.drop('target', axis=1)
y_test = test['target']
```

### NLP Dataset Prep

```python
# Load text data
df = pd.read_csv('text_data.csv')

# Basic text cleaning
df['text_clean'] = df['text'].str.lower()
df['text_clean'] = df['text_clean'].str.replace(r'[^\w\s]', '', regex=True)

# Text features
df['text_length'] = df['text'].str.len()
df['word_count'] = df['text'].str.split().str.len()

# TF-IDF
from sklearn.feature_extraction.text import TfidfVectorizer

tfidf = TfidfVectorizer(max_features=1000, stop_words='english')
X_tfidf = tfidf.fit_transform(df['text_clean'])

# Convert to DataFrame
feature_names = tfidf.get_feature_names_out()
X = pd.DataFrame(X_tfidf.toarray(), columns=feature_names)

# Combine with other features
X['text_length'] = df['text_length'].values
X['word_count'] = df['word_count'].values

y = df['target']

# Split
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)
```


## 🚀 Quick Reference Cheat Sheet

### Loading Data
```python
df = pd.read_csv('file.csv')
df = pd.read_excel('file.xlsx')
df = pd.read_json('file.json')
df = pd.read_parquet('file.parquet')
```

### Exploration
```python
df.head()          # First 5 rows
df.info()          # Data types and info
df.describe()      # Statistics
df.shape           # (rows, cols)
df.isnull().sum()  # Missing values
```

### Selection
```python
df['column']              # Single column
df[['col1', 'col2']]     # Multiple columns
df[df['age'] > 30]       # Filter rows
df.loc[0:5, 'col']       # By label
df.iloc[0:5, 0]          # By position
```

### Cleaning
```python
df.dropna()              # Drop missing
df.fillna(value)         # Fill missing
df.drop_duplicates()     # Remove duplicates
df.drop('col', axis=1)   # Drop column
```

### Transformation
```python
df['new'] = df['a'] + df['b']        # New column
df.groupby('cat')['val'].mean()      # Aggregate
pd.get_dummies(df, columns=['cat'])  # One-hot encode
df.merge(df2, on='id')               # Merge
pd.concat([df1, df2])                # Concatenate
```

### Feature Engineering
```python
df['age_group'] = pd.cut(df['age'], bins=[0, 18, 65, 100])
df['year'] = df['date'].dt.year
df['log_price'] = np.log1p(df['price'])
df.groupby('user')['amount'].transform('mean')
```

### ML Prep
```python
X = df.drop('target', axis=1)
y = df['target']
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)

from sklearn.preprocessing import StandardScaler
scaler = StandardScaler()
X_train = scaler.fit_transform(X_train)
X_test = scaler.transform(X_test)
```


## 💡 Pro Tips for ML Practitioners

### 1. Memory Optimization

```python
# Use categorical for repeated strings (huge memory savings!)
df['country'] = df['country'].astype('category')

# Downcast numeric types
df['age'] = df['age'].astype('int32')  # Instead of int64
df['price'] = df['price'].astype('float32')  # Instead of float64

# Load only needed columns
df = pd.read_csv('data.csv', usecols=['id', 'name', 'target'])
```

### 2. Avoid Data Leakage

```python
# ❌ WRONG: Fit on entire dataset
scaler = StandardScaler()
X = scaler.fit_transform(X)  # Leakage!

# ✅ CORRECT: Fit only on training data
X_train = scaler.fit_transform(X_train)
X_test = scaler.transform(X_test)  # Use fitted scaler
```

### 3. Handle Rare Categories

```python
# Replace rare categories with 'Other'
value_counts = df['category'].value_counts()
rare_categories = value_counts[value_counts < 100].index
df['category'] = df['category'].replace(rare_categories, 'Other')
```

### 4. Feature Correlation

```python
# Check correlation with target
correlations = df.corr()['target'].sort_values(ascending=False)
print(correlations)

# Remove highly correlated features
corr_matrix = df.corr().abs()
upper = corr_matrix.where(np.triu(np.ones(corr_matrix.shape), k=1).astype(bool))
to_drop = [col for col in upper.columns if any(upper[col] > 0.95)]
df = df.drop(to_drop, axis=1)
```

### 5. Quick EDA Function

```python
def quick_eda(df, target_col=None):
    """Quick exploratory data analysis."""
    print("Shape:", df.shape)
    print("\nData Types:")
    print(df.dtypes.value_counts())
    print("\nMissing Values:")
    print(df.isnull().sum()[df.isnull().sum() > 0])
    print("\nNumerical Summary:")
    print(df.describe())
    
    if target_col:
        print(f"\nTarget Distribution:")
        print(df[target_col].value_counts())
        print(f"\nCorrelation with {target_col}:")
        print(df.corr()[target_col].sort_values(ascending=False))

quick_eda(df, target_col='target')
```

### 6. Pipeline Pattern

```python
def preprocess_data(df, is_training=True, scaler=None):
    """Reusable preprocessing pipeline."""
    # 1. Handle missing
    df = df.copy()
    df['age'] = df['age'].fillna(df['age'].median())
    
    # 2. Feature engineering
    df['age_squared'] = df['age'] ** 2
    
    # 3. Encode categorical
    df = pd.get_dummies(df, columns=['category'], drop_first=True)
    
    # 4. Scale
    if is_training:
        scaler = StandardScaler()
        df_scaled = scaler.fit_transform(df)
        return df_scaled, scaler
    else:
        df_scaled = scaler.transform(df)
        return df_scaled

# Use it
X_train_processed, scaler = preprocess_data(X_train, is_training=True)
X_test_processed = preprocess_data(X_test, is_training=False, scaler=scaler)
```


## 🎉 You're Ready!

You now have all the Pandas skills needed for Machine Learning and Deep Learning! 🚀

**Key Takeaways**:
- Load data efficiently with `read_csv()` and friends
- Explore with `.head()`, `.info()`, `.describe()`
- Clean with `.dropna()`, `.fillna()`, `.drop_duplicates()`
- Engineer features with groupby, binning, encoding
- Split properly to avoid data leakage
- Always scale after splitting!

**Next Steps**:
- Master data visualization (Matplotlib)

**Remember**: Data preparation is 80% of ML work. Master Pandas, and you're already ahead! 💪

---

<div align="center">
  
**Made with ❤️ for the Python Community by @RajeshTechForge**

</div>