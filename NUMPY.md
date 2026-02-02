<div align="center">

# Awesome Numpy [![Awesome](https://cdn.rawgit.com/sindresorhus/awesome/d7305f38d29fed78fa85652e3a63e154dd8e8829/media/badge.svg)](https://github.com/sindresorhus/awesome)

🔢 *Master numerical computing for ML/DL - fast, efficient, and fun!*

<br>
<br>
</div>


## 📚 Quick Navigation

1. [Setup & Arrays](#-setup--arrays-101)
2. [Array Operations](#-array-operations)
3. [Indexing & Slicing](#-indexing--slicing)
4. [Reshaping Arrays](#-reshaping-arrays)
5. [Math Operations](#-math-operations)
6. [Broadcasting](#-broadcasting-magic)
7. [Linear Algebra](#-linear-algebra-for-ml)
8. [Random Numbers](#-random-numbers)
9. [Useful Functions](#-useful-functions-for-ml)
10. [Performance Tips](#%EF%B8%8F-performance-tips)
11. [Quick Reference](#-quick-reference-cheat-sheet)

---

## 🛠 Setup & Arrays 101

### Installation

```bash
# Basic installation
pip install numpy

# Check version (should be 2.0+)
python -c "import numpy as np; print(np.__version__)"
```

### First Array

```python
import numpy as np

# Create array from list
arr = np.array([1, 2, 3, 4, 5])
print(arr)          # [1 2 3 4 5]
print(type(arr))    # <class 'numpy.ndarray'>

# 2D array (matrix)
matrix = np.array([[1, 2, 3], 
                   [4, 5, 6]])
print(matrix)
# [[1 2 3]
#  [4 5 6]]
```

### Array Attributes

```python
arr = np.array([[1, 2, 3, 4],
                [5, 6, 7, 8]])

print(arr.shape)      # (2, 4) - rows, columns
print(arr.ndim)       # 2 - number of dimensions
print(arr.size)       # 8 - total elements
print(arr.dtype)      # dtype('int64') - data type
print(arr.itemsize)   # 8 - bytes per element
print(arr.nbytes)     # 64 - total bytes
```

### Creating Arrays

```python
# Zeros (common for initialization)
zeros = np.zeros((3, 4))        # 3x4 array of zeros
zeros_like = np.zeros_like(arr) # Same shape as arr

# Ones
ones = np.ones((2, 3))          # 2x3 array of ones
ones_like = np.ones_like(arr)   # Same shape as arr

# Empty (faster but uninitialized)
empty = np.empty((2, 2))        # 2x2 array (random values)

# Full (filled with specific value)
full = np.full((3, 3), 7)       # 3x3 array of 7s

# Range
range_arr = np.arange(10)       # [0, 1, 2, ..., 9]
range_arr = np.arange(2, 10, 2) # [2, 4, 6, 8] - start, stop, step

# Linspace (evenly spaced)
linear = np.linspace(0, 1, 5)   # 5 values from 0 to 1
# [0.   0.25 0.5  0.75 1.  ]

# Identity matrix (important for ML!)
identity = np.eye(3)            # 3x3 identity matrix
# [[1. 0. 0.]
#  [0. 1. 0.]
#  [0. 0. 1.]]

# Random arrays (very common in ML!)
random = np.random.rand(3, 3)   # 3x3 uniform [0, 1)
randn = np.random.randn(3, 3)   # 3x3 standard normal
randint = np.random.randint(0, 10, (3, 3))  # 3x3 integers [0, 10)
```

### Data Types

```python
# Explicit data types
int_arr = np.array([1, 2, 3], dtype=np.int32)
float_arr = np.array([1, 2, 3], dtype=np.float32)
bool_arr = np.array([True, False, True], dtype=np.bool_)

# Convert types
arr_float = arr.astype(np.float32)  # int to float
arr_int = arr_float.astype(np.int32)  # float to int

# Common dtypes for ML:
# int8, int16, int32, int64
# float16, float32, float64  (float32 is default for deep learning)
# bool_
```


## ⚡ Array Operations

### Basic Math

```python
a = np.array([1, 2, 3, 4])
b = np.array([5, 6, 7, 8])

# Element-wise operations (vectorized - FAST!)
print(a + b)      # [ 6  8 10 12]
print(a - b)      # [-4 -4 -4 -4]
print(a * b)      # [ 5 12 21 32]
print(a / b)      # [0.2 0.33 0.43 0.5]
print(a ** 2)     # [ 1  4  9 16]

# With scalars (broadcasting)
print(a + 10)     # [11 12 13 14]
print(a * 2)      # [2 4 6 8]
print(a / 2)      # [0.5 1.  1.5 2. ]

# Comparison (element-wise)
print(a > 2)      # [False False  True  True]
print(a == b)     # [False False False False]
```

### Universal Functions (ufuncs)

```python
arr = np.array([1, 4, 9, 16])

# Math functions
np.sqrt(arr)      # [1. 2. 3. 4.]
np.exp(arr)       # Exponential
np.log(arr)       # Natural log
np.log10(arr)     # Log base 10
np.sin(arr)       # Sine
np.cos(arr)       # Cosine
np.abs(arr)       # Absolute value

# Rounding
arr = np.array([1.234, 5.678, 9.012])
np.round(arr, 2)  # [1.23 5.68 9.01]
np.floor(arr)     # [1. 5. 9.]
np.ceil(arr)      # [2. 6. 10.]

# Clipping (important for ML!)
arr = np.array([-5, 2, 10, -3, 8])
np.clip(arr, 0, 5)  # [0 2 5 0 5] - clip to [0, 5]
```

### Aggregation Functions

```python
arr = np.array([[1, 2, 3],
                [4, 5, 6]])

# Whole array
print(arr.sum())      # 21
print(arr.mean())     # 3.5
print(arr.std())      # 1.707... - standard deviation
print(arr.var())      # 2.916... - variance
print(arr.min())      # 1
print(arr.max())      # 6

# Along axis
print(arr.sum(axis=0))   # [5 7 9] - sum each column
print(arr.sum(axis=1))   # [6 15] - sum each row
print(arr.mean(axis=0))  # [2.5 3.5 4.5] - mean of each column

# Other useful aggregations
print(arr.prod())        # Product of all elements
print(arr.cumsum())      # Cumulative sum
print(arr.argmin())      # Index of minimum
print(arr.argmax())      # Index of maximum
```


## 🎯 Indexing & Slicing

### Basic Indexing

```python
arr = np.array([10, 20, 30, 40, 50])

# Single element
print(arr[0])     # 10 - first element
print(arr[-1])    # 50 - last element
print(arr[2])     # 30

# Slicing (start:stop:step)
print(arr[1:4])   # [20 30 40] - elements 1, 2, 3
print(arr[:3])    # [10 20 30] - first 3
print(arr[2:])    # [30 40 50] - from index 2 to end
print(arr[::2])   # [10 30 50] - every 2nd element
print(arr[::-1])  # [50 40 30 20 10] - reverse
```

### 2D Indexing

```python
matrix = np.array([[1, 2, 3, 4],
                   [5, 6, 7, 8],
                   [9, 10, 11, 12]])

# Single element
print(matrix[0, 0])      # 1
print(matrix[1, 2])      # 7
print(matrix[-1, -1])    # 12

# Rows
print(matrix[0])         # [1 2 3 4] - first row
print(matrix[0:2])       # First 2 rows

# Columns
print(matrix[:, 0])      # [1 5 9] - first column
print(matrix[:, 1:3])    # Columns 1 and 2

# Sub-matrices
print(matrix[0:2, 1:3])  # 2x2 sub-matrix
# [[2 3]
#  [6 7]]
```

### Boolean Indexing (Powerful!)

```python
arr = np.array([1, 2, 3, 4, 5, 6, 7, 8, 9, 10])

# Boolean mask
mask = arr > 5
print(mask)              # [False False False False False True True True True True]
print(arr[mask])         # [6 7 8 9 10]

# Direct filtering
print(arr[arr > 5])      # [6 7 8 9 10]
print(arr[arr % 2 == 0]) # [2 4 6 8 10] - even numbers

# Multiple conditions
print(arr[(arr > 3) & (arr < 8)])  # [4 5 6 7]
print(arr[(arr < 3) | (arr > 8)])  # [1 2 9 10]

# Modify with boolean indexing
arr[arr > 5] = 5         # Clip values > 5
print(arr)               # [1 2 3 4 5 5 5 5 5 5]
```

### Fancy Indexing

```python
arr = np.array([10, 20, 30, 40, 50])

# Index with list
indices = [0, 2, 4]
print(arr[indices])      # [10 30 50]

# 2D fancy indexing
matrix = np.array([[1, 2, 3],
                   [4, 5, 6],
                   [7, 8, 9]])

rows = [0, 2]
cols = [1, 2]
print(matrix[rows, cols])  # [2 9] - elements at (0,1) and (2,2)
```


## 🔄 Reshaping Arrays

### Basic Reshaping

```python
arr = np.arange(12)  # [0 1 2 3 4 5 6 7 8 9 10 11]

# Reshape
matrix = arr.reshape(3, 4)
# [[ 0  1  2  3]
#  [ 4  5  6  7]
#  [ 8  9 10 11]]

# Reshape with -1 (auto-calculate dimension)
matrix = arr.reshape(4, -1)  # 4 rows, auto columns → (4, 3)
matrix = arr.reshape(-1, 2)  # auto rows, 2 columns → (6, 2)

# Flatten (2D → 1D)
flat = matrix.flatten()      # [0 1 2 ... 11]
flat = matrix.ravel()        # Same but faster (view when possible)

# Transpose
print(matrix.T)              # Transpose (rows ↔ columns)
print(np.transpose(matrix))  # Same thing
```

### Adding/Removing Dimensions

```python
arr = np.array([1, 2, 3, 4])  # Shape: (4,)

# Add dimension
arr_2d = arr.reshape(-1, 1)   # Shape: (4, 1) - column vector
arr_2d = arr.reshape(1, -1)   # Shape: (1, 4) - row vector

# Using newaxis (cleaner!)
arr_col = arr[:, np.newaxis]  # Shape: (4, 1)
arr_row = arr[np.newaxis, :]  # Shape: (1, 4)

# Expand dimensions (for batch processing)
arr_3d = np.expand_dims(arr, axis=0)  # Add dimension at position 0
arr_3d = np.expand_dims(arr, axis=-1)  # Add at end

# Remove single dimensions
arr_1d = arr_2d.squeeze()     # (4, 1) → (4,)
```

### Stacking Arrays

```python
a = np.array([1, 2, 3])
b = np.array([4, 5, 6])

# Vertical stack (rows)
np.vstack([a, b])
# [[1 2 3]
#  [4 5 6]]

# Horizontal stack (columns)
np.hstack([a, b])  # [1 2 3 4 5 6]

# Stack along new axis
np.stack([a, b])          # Stack as new dimension
np.stack([a, b], axis=1)  # Stack as columns

# Concatenate (more general)
np.concatenate([a, b])           # [1 2 3 4 5 6]
np.concatenate([a, b], axis=0)   # Same
```


## 🧮 Math Operations

### Matrix Multiplication

```python
# Dot product (1D arrays)
a = np.array([1, 2, 3])
b = np.array([4, 5, 6])
print(np.dot(a, b))  # 32 = 1*4 + 2*5 + 3*6

# Matrix multiplication (2D arrays)
A = np.array([[1, 2], [3, 4]])
B = np.array([[5, 6], [7, 8]])

print(np.dot(A, B))    # Matrix multiplication
# [[19 22]
#  [43 50]]

# @ operator (cleaner, NumPy 1.10+)
print(A @ B)           # Same as np.dot(A, B)

# Element-wise multiplication (different!)
print(A * B)
# [[ 5 12]
#  [21 32]]
```

### Common ML Operations

```python
# Softmax (for classification)
def softmax(x):
    exp_x = np.exp(x - np.max(x))  # Subtract max for stability
    return exp_x / exp_x.sum()

logits = np.array([2.0, 1.0, 0.1])
probs = softmax(logits)
print(probs)  # [0.659 0.242 0.099]

# ReLU activation
def relu(x):
    return np.maximum(0, x)

x = np.array([-2, -1, 0, 1, 2])
print(relu(x))  # [0 0 0 1 2]

# Sigmoid activation
def sigmoid(x):
    return 1 / (1 + np.exp(-x))

x = np.array([-2, -1, 0, 1, 2])
print(sigmoid(x))  # [0.12 0.27 0.5 0.73 0.88]

# Mean Squared Error
y_true = np.array([1, 2, 3, 4, 5])
y_pred = np.array([1.1, 2.2, 2.9, 4.1, 5.2])
mse = np.mean((y_true - y_pred) ** 2)
print(f"MSE: {mse:.4f}")
```

### Statistical Operations

```python
data = np.random.randn(100, 5)  # 100 samples, 5 features

# Mean and std (for normalization)
mean = data.mean(axis=0)      # Mean of each feature
std = data.std(axis=0)        # Std of each feature
normalized = (data - mean) / std  # Z-score normalization

# Min-max normalization
min_val = data.min(axis=0)
max_val = data.max(axis=0)
normalized = (data - min_val) / (max_val - min_val)

# Correlation
corr_matrix = np.corrcoef(data.T)  # Feature correlation

# Covariance
cov_matrix = np.cov(data.T)        # Feature covariance
```


## 📡 Broadcasting Magic

### What is Broadcasting?

Broadcasting allows operations on arrays of different shapes **without copying data**. It's what makes NumPy blazing fast!

```python
# Scalar + array
arr = np.array([1, 2, 3, 4])
print(arr + 10)  # [11 12 13 14]
# 10 is "broadcast" to [10, 10, 10, 10]

# 1D + 2D
matrix = np.array([[1, 2, 3],
                   [4, 5, 6],
                   [7, 8, 9]])
row = np.array([10, 20, 30])

print(matrix + row)
# [[11 22 33]
#  [14 25 36]
#  [17 28 39]]
# row is broadcast to each row of matrix

# Column broadcasting
col = np.array([[10], [20], [30]])  # Shape: (3, 1)
print(matrix + col)
# [[11 12 13]
#  [24 25 26]
#  [37 38 39]]
```

### Broadcasting Rules

1. If arrays have different dimensions, pad the smaller shape with 1s on the left
2. Arrays are compatible if dimensions are equal or one is 1
3. Arrays broadcast to the larger dimension

```python
# Shape (3,) and (3, 4) → (1, 3) and (3, 4) → Compatible!
a = np.array([1, 2, 3])        # Shape: (3,)
b = np.ones((3, 4))            # Shape: (3, 4)
result = a + b                 # Works! Shape: (3, 4)

# Shape (4, 1) and (3,) → (4, 1) and (1, 3) → (4, 3)
a = np.array([[1], [2], [3], [4]])  # Shape: (4, 1)
b = np.array([10, 20, 30])          # Shape: (3,)
result = a + b                      # Shape: (4, 3)
```

### Practical Examples

```python
# Normalize each feature (column-wise)
X = np.random.randn(100, 5)  # 100 samples, 5 features
mean = X.mean(axis=0)        # Shape: (5,)
std = X.std(axis=0)          # Shape: (5,)
X_normalized = (X - mean) / std  # Broadcasting!

# Distance matrix (vectorized!)
points = np.random.rand(10, 2)  # 10 points in 2D
distances = np.sqrt(((points[:, np.newaxis] - points) ** 2).sum(axis=2))
# Shape: (10, 10) - pairwise distances
```


## 🎲 Linear Algebra for ML

### Matrix Operations

```python
A = np.array([[1, 2], [3, 4]])
B = np.array([[5, 6], [7, 8]])

# Matrix multiplication
print(A @ B)
print(np.matmul(A, B))  # Same thing

# Transpose
print(A.T)

# Inverse
A_inv = np.linalg.inv(A)
print(A @ A_inv)  # Should be identity

# Determinant
det = np.linalg.det(A)
print(det)  # -2.0

# Rank
rank = np.linalg.matrix_rank(A)
```

### Eigenvalues & Eigenvectors

```python
# Important for PCA!
A = np.array([[1, 2], [2, 1]])

eigenvalues, eigenvectors = np.linalg.eig(A)
print(eigenvalues)    # [ 3. -1.]
print(eigenvectors)   # Corresponding eigenvectors

# Verify: A @ v = λ * v
v = eigenvectors[:, 0]
lambda_val = eigenvalues[0]
print(A @ v)
print(lambda_val * v)  # Should be the same
```

### Solving Linear Systems

```python
# Solve Ax = b
A = np.array([[3, 1], [1, 2]])
b = np.array([9, 8])

x = np.linalg.solve(A, b)
print(x)  # [2. 3.]

# Verify
print(A @ x)  # [9. 8.] ✓

# Least squares (for regression!)
# Find x that minimizes ||Ax - b||²
x, residuals, rank, s = np.linalg.lstsq(A, b, rcond=None)
```

### Singular Value Decomposition (SVD)

```python
# SVD: A = U @ S @ V^T
# Essential for PCA, recommendation systems, etc.

A = np.random.randn(10, 5)
U, s, Vt = np.linalg.svd(A, full_matrices=False)

print(U.shape)   # (10, 5)
print(s.shape)   # (5,)
print(Vt.shape)  # (5, 5)

# Reconstruct
S = np.diag(s)
A_reconstructed = U @ S @ Vt
print(np.allclose(A, A_reconstructed))  # True

# Low-rank approximation (dimensionality reduction)
k = 2  # Keep top 2 components
A_approx = U[:, :k] @ np.diag(s[:k]) @ Vt[:k, :]
```

### Norms

```python
x = np.array([3, 4])

# L2 norm (Euclidean)
print(np.linalg.norm(x))        # 5.0 = sqrt(3² + 4²)

# L1 norm (Manhattan)
print(np.linalg.norm(x, ord=1)) # 7.0 = |3| + |4|

# Max norm
print(np.linalg.norm(x, ord=np.inf))  # 4.0 = max(|3|, |4|)

# Matrix norms
A = np.array([[1, 2], [3, 4]])
print(np.linalg.norm(A, 'fro'))  # Frobenius norm
```


## 🎲 Random Numbers

### Random Number Generation

```python
# New in NumPy 2.0: Use Generator (better than legacy methods!)
from numpy.random import default_rng
rng = default_rng(seed=42)  # Reproducible random numbers

# Uniform [0, 1)
uniform = rng.random((3, 4))

# Standard normal (mean=0, std=1)
normal = rng.standard_normal((3, 4))

# Normal with custom mean and std
normal = rng.normal(loc=10, scale=2, size=(3, 4))  # mean=10, std=2

# Integers
integers = rng.integers(0, 10, size=(3, 4))  # [0, 10)

# Choice (sampling)
data = np.array([1, 2, 3, 4, 5])
sample = rng.choice(data, size=3, replace=False)  # Sample without replacement

# Shuffle
arr = np.arange(10)
rng.shuffle(arr)  # In-place shuffle

# Permutation (returns shuffled copy)
shuffled = rng.permutation(arr)
```

### Common Distributions

```python
rng = default_rng(seed=42)

# Binomial (coin flips)
# n trials, p probability of success
binomial = rng.binomial(n=10, p=0.5, size=1000)

# Poisson (event counts)
poisson = rng.poisson(lam=5, size=1000)

# Exponential
exponential = rng.exponential(scale=1.0, size=1000)

# Uniform in range [a, b)
uniform = rng.uniform(low=0, high=10, size=1000)

# Beta distribution
beta = rng.beta(a=2, b=5, size=1000)
```

### ML Use Cases

```python
rng = default_rng(seed=42)

# Initialize neural network weights
def initialize_weights(n_inputs, n_outputs):
    # Xavier/Glorot initialization
    scale = np.sqrt(2.0 / (n_inputs + n_outputs))
    return rng.normal(0, scale, size=(n_inputs, n_outputs))

weights = initialize_weights(784, 128)

# Train/test split with shuffling
n_samples = 1000
indices = rng.permutation(n_samples)
train_idx = indices[:800]
test_idx = indices[800:]

# Data augmentation (add noise)
image = np.random.rand(28, 28)
noise = rng.normal(0, 0.1, size=image.shape)
augmented = image + noise

# Bootstrap sampling
data = np.random.randn(100)
bootstrap_sample = rng.choice(data, size=len(data), replace=True)
```


## 🔧 Useful Functions for ML

### Array Manipulation

```python
# Repeat elements
arr = np.array([1, 2, 3])
print(np.repeat(arr, 3))  # [1 1 1 2 2 2 3 3 3]

# Tile (repeat entire array)
print(np.tile(arr, 3))    # [1 2 3 1 2 3 1 2 3]

# Unique values
arr = np.array([1, 2, 2, 3, 3, 3, 4])
unique, counts = np.unique(arr, return_counts=True)
print(unique)   # [1 2 3 4]
print(counts)   # [1 2 3 1]

# Where (conditional selection)
x = np.array([1, 2, 3, 4, 5])
result = np.where(x > 2, x, 0)  # If x > 2 keep x, else 0
print(result)  # [0 0 3 4 5]

# Select (multiple conditions)
conditions = [x < 2, x < 4, x >= 4]
choices = ['small', 'medium', 'large']
result = np.select(conditions, choices)
```

### Comparison & Logic

```python
a = np.array([1, 2, 3, 4, 5])
b = np.array([2, 2, 3, 5, 4])

# Element-wise comparison
print(a == b)       # [False  True  True False False]
print(a > b)        # [False False False False  True]

# Any/All
print(np.any(a > 3))    # True - at least one element > 3
print(np.all(a > 0))    # True - all elements > 0

# Logical operations
print(np.logical_and(a > 2, a < 5))  # [False False  True  True False]
print(np.logical_or(a < 2, a > 4))   # [ True False False False  True]
print(np.logical_not(a > 3))         # [ True  True  True False False]

# allclose (for float comparison)
print(np.allclose(a, b, atol=1))  # True - within tolerance of 1
```

### NaN Handling

```python
arr = np.array([1, 2, np.nan, 4, np.nan, 6])

# Check for NaN
print(np.isnan(arr))  # [False False  True False  True False]

# NaN-safe operations
print(np.nanmean(arr))  # 3.25 - ignore NaN
print(np.nansum(arr))   # 13.0
print(np.nanstd(arr))   # Standard deviation ignoring NaN

# Replace NaN
arr_clean = np.where(np.isnan(arr), 0, arr)  # Replace NaN with 0
arr_clean = np.nan_to_num(arr, nan=0)        # Same thing
```

### Sorting

```python
arr = np.array([3, 1, 4, 1, 5, 9, 2, 6])

# Sort (returns sorted copy)
sorted_arr = np.sort(arr)

# Argsort (indices that would sort the array)
indices = np.argsort(arr)
print(indices)  # [1 3 6 0 2 4 7 5]
print(arr[indices])  # Sorted array

# Sort 2D array
matrix = np.array([[3, 2, 1],
                   [6, 5, 4]])
print(np.sort(matrix, axis=1))  # Sort each row
print(np.sort(matrix, axis=0))  # Sort each column

# Partial sort (k smallest elements)
k_smallest = np.partition(arr, k=3)[:3]  # 3 smallest
```

### Set Operations

```python
a = np.array([1, 2, 3, 4, 5])
b = np.array([4, 5, 6, 7, 8])

# Intersection
print(np.intersect1d(a, b))  # [4 5]

# Union
print(np.union1d(a, b))      # [1 2 3 4 5 6 7 8]

# Difference
print(np.setdiff1d(a, b))    # [1 2 3]

# Unique elements
print(np.unique(np.concatenate([a, b])))  # [1 2 3 4 5 6 7 8]
```


## ⚡️ Performance Tips

### Vectorization (The Golden Rule!)

```python
import time

# ❌ BAD: Python loop (SLOW!)
arr = np.random.rand(1000000)
start = time.time()
result = []
for x in arr:
    result.append(x ** 2)
result = np.array(result)
print(f"Loop: {time.time() - start:.4f}s")

# ✅ GOOD: Vectorized (100x faster!)
start = time.time()
result = arr ** 2
print(f"Vectorized: {time.time() - start:.4f}s")

# Always use vectorized operations!
# NumPy handles the loops in optimized C code
```

### Memory Efficiency

```python
# Use appropriate data types
arr_float64 = np.random.rand(1000, 1000)  # 8MB
arr_float32 = arr_float64.astype(np.float32)  # 4MB - half the memory!

# Use views when possible (no copy)
view = arr[10:20]         # View - no copy
copy = arr[10:20].copy()  # Copy - uses more memory

# In-place operations
arr = np.random.rand(1000, 1000)
arr *= 2           # In-place (✓)
arr = arr * 2      # Creates new array (✗)

# Delete unused arrays
del large_array
```

### Avoid Copies

```python
# Slicing creates views (fast!)
arr = np.arange(10)
view = arr[2:7]
view[0] = 999
print(arr)  # [0 1 999 3 4 5 6 7 8 9] - original modified!

# Boolean indexing creates copies
copy = arr[arr > 5]
copy[0] = 0
print(arr)  # Original unchanged

# Use .copy() when you need a copy
true_copy = arr[2:7].copy()
true_copy[0] = 0  # Won't affect arr
```

### Axis Parameter

```python
# Using axis is faster than loops
arr = np.random.rand(1000, 100)

# ❌ Slow
result = []
for i in range(arr.shape[1]):
    result.append(arr[:, i].sum())

# ✅ Fast
result = arr.sum(axis=0)
```

### Pre-allocation

```python
# ❌ Slow: Growing arrays
result = np.array([])
for i in range(1000):
    result = np.append(result, i)  # Slow!

# ✅ Fast: Pre-allocate
result = np.empty(1000)
for i in range(1000):
    result[i] = i

# Even better: Use vectorization!
result = np.arange(1000)
```

---

## 🚀 Quick Reference Cheat Sheet

### Creating Arrays
```python
np.array([1, 2, 3])              # From list
np.zeros((3, 4))                 # 3x4 zeros
np.ones((2, 3))                  # 2x3 ones
np.empty((2, 2))                 # Empty (fast)
np.full((3, 3), 7)               # 3x3 filled with 7
np.arange(10)                    # [0, 1, ..., 9]
np.linspace(0, 1, 5)             # 5 values from 0 to 1
np.eye(3)                        # 3x3 identity
np.random.rand(3, 3)             # 3x3 uniform [0,1)
```

### Attributes
```python
arr.shape       # Dimensions
arr.ndim        # Number of dimensions
arr.size        # Total elements
arr.dtype       # Data type
```

### Indexing
```python
arr[0]          # First element
arr[-1]         # Last element
arr[1:4]        # Slice
arr[arr > 5]    # Boolean indexing
arr[[0, 2, 4]]  # Fancy indexing
```

### Reshaping
```python
arr.reshape(3, 4)        # Reshape
arr.flatten()            # Flatten to 1D
arr.T                    # Transpose
arr[:, np.newaxis]       # Add dimension
np.expand_dims(arr, 0)   # Add dimension
arr.squeeze()            # Remove single dims
```

### Math
```python
arr + 5         # Add scalar
arr * arr       # Element-wise multiply
arr @ arr2      # Matrix multiply
arr.sum()       # Sum all
arr.mean()      # Mean
arr.std()       # Standard deviation
np.sqrt(arr)    # Square root
np.exp(arr)     # Exponential
```

### Linear Algebra
```python
np.dot(a, b)              # Dot product
a @ b                     # Matrix multiply
np.linalg.inv(A)          # Inverse
np.linalg.det(A)          # Determinant
np.linalg.eig(A)          # Eigenvalues
np.linalg.svd(A)          # SVD
np.linalg.norm(v)         # Vector norm
```

### Random
```python
rng = np.random.default_rng(42)
rng.random((3, 4))               # Uniform [0,1)
rng.standard_normal((3, 4))      # Standard normal
rng.integers(0, 10, (3, 4))      # Random integers
rng.choice(arr, size=5)          # Sample
```

### Broadcasting
```python
arr + 10                 # Scalar broadcast
arr + row_vector         # Row broadcast
arr + col_vector         # Column broadcast
```


## 💡 Pro Tips for ML

### 1. Batch Processing Pattern

```python
# Process data in batches (memory efficient)
batch_size = 32
n_samples = 10000
n_batches = n_samples // batch_size

for i in range(n_batches):
    start = i * batch_size
    end = start + batch_size
    batch = data[start:end]
    # Process batch
    predictions = model.predict(batch)
```

### 2. Efficient Normalization

```python
# Normalize features (vectorized)
def normalize(X):
    mean = X.mean(axis=0, keepdims=True)
    std = X.std(axis=0, keepdims=True)
    return (X - mean) / (std + 1e-8)  # Add epsilon to avoid division by zero

X_normalized = normalize(X)
```

### 3. One-Hot Encoding

```python
# Manual one-hot encoding
def one_hot(y, num_classes):
    n = len(y)
    one_hot = np.zeros((n, num_classes))
    one_hot[np.arange(n), y] = 1
    return one_hot

labels = np.array([0, 2, 1, 0])
one_hot_labels = one_hot(labels, num_classes=3)
# [[1. 0. 0.]
#  [0. 0. 1.]
#  [0. 1. 0.]
#  [1. 0. 0.]]
```

### 4. Efficient Train-Test Split

```python
def train_test_split(X, y, test_size=0.2, seed=42):
    rng = default_rng(seed)
    n = len(X)
    indices = rng.permutation(n)
    split_idx = int(n * (1 - test_size))
    
    train_idx = indices[:split_idx]
    test_idx = indices[split_idx:]
    
    return X[train_idx], X[test_idx], y[train_idx], y[test_idx]
```

### 5. Mini-Batch Gradient Descent Pattern

```python
def create_batches(X, y, batch_size=32, shuffle=True):
    n_samples = len(X)
    indices = np.arange(n_samples)
    
    if shuffle:
        rng = default_rng()
        rng.shuffle(indices)
    
    for start_idx in range(0, n_samples, batch_size):
        end_idx = min(start_idx + batch_size, n_samples)
        batch_idx = indices[start_idx:end_idx]
        yield X[batch_idx], y[batch_idx]

# Use it
for epoch in range(num_epochs):
    for X_batch, y_batch in create_batches(X_train, y_train):
        # Compute gradients and update weights
        pass
```

### 6. Numerical Stability

```python
# Avoid overflow in softmax
def stable_softmax(x):
    # Subtract max for numerical stability
    exp_x = np.exp(x - np.max(x, axis=-1, keepdims=True))
    return exp_x / np.sum(exp_x, axis=-1, keepdims=True)

# Avoid log(0)
def safe_log(x, eps=1e-10):
    return np.log(np.clip(x, eps, None))

# Cross-entropy loss (stable)
def cross_entropy(y_true, y_pred, eps=1e-10):
    y_pred = np.clip(y_pred, eps, 1 - eps)
    return -np.sum(y_true * np.log(y_pred))
```

---

## 🎉 You're a NumPy Ninja!

You now have everything you need to handle numerical operations in ML/DL! 🚀


**NumPy in the ML Stack**:
```
NumPy → Pandas → Scikit-learn → Deep Learning Frameworks
  ↓       ↓           ↓                    ↓
Arrays  Tables   ML Models         Neural Networks
```

Now go build something amazing! 💪

---

<div align="center">
  
**Made with ❤️ for the Python Community by @RajeshTechForge**

</div>