# Week 3 — Lecture Content: NumPy for Numerical Computing

## 1. Why Vectorization Matters
AI/ML workloads operate on large arrays (images, feature matrices, weight matrices). Pure-Python
loops over these are slow because each iteration carries Python's interpreter overhead. NumPy
pushes the loop down into optimized C code, operating on whole arrays at once.

## 2. Array Basics
```python
import numpy as np

a = np.array([1, 2, 3])
b = np.zeros((3, 4))        # 3x4 array of zeros
c = np.arange(0, 10, 2)     # [0, 2, 4, 6, 8]
print(a.shape, a.dtype)
print(b[1, :])               # row 1, all columns
print(a[::-1])                # reversed
```

## 3. Broadcasting & Vectorization
```python
x = np.array([1, 2, 3])
y = np.array([10, 20, 30])

# Vectorized (fast)
z = x + y

# Equivalent loop (slow) — for comparison only
z_loop = [x[i] + y[i] for i in range(len(x))]
```
Broadcasting lets NumPy apply an operation between arrays of different (but compatible) shapes,
e.g., adding a scalar to every element of a matrix, or adding a 1D array to every row of a 2D
array, without explicit loops.

### Timing comparison (illustrative pattern used in the live demo)
```python
import time
n = 1_000_000
arr = np.arange(n)

start = time.time()
result = arr * 2                 # vectorized
vec_time = time.time() - start

start = time.time()
result_loop = [v * 2 for v in arr]  # loop
loop_time = time.time() - start
```
Students run this and observe the vectorized version is typically one to two orders of
magnitude faster — the exact ratio depends on hardware, but the direction of the result is
consistent and is the pedagogical point of the exercise.

## 4. Linear Algebra Operations
```python
A = np.array([[1, 2], [3, 4]])
B = np.array([[5, 6], [7, 8]])

np.dot(A, B)      # matrix multiplication
A @ B             # same, using the @ operator
np.linalg.inv(A)  # matrix inverse
np.linalg.det(A)  # determinant
```
These operations are the backbone of linear regression (normal equation), PCA, and neural
network forward passes (`W @ x + b`), all covered later in the course.

## 5. In-Class Exercise
Given a nested-loop implementation of Euclidean distance between two vectors, rewrite it as a
single vectorized NumPy expression using `np.linalg.norm`, and time both versions.
