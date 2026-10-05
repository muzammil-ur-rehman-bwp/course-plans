# Week 3: NumPy for Numerical Computing

## Learning Objectives

By the end of this lecture, you should be able to:

1. Explain why vectorized code is faster than a Python loop, and measure the difference.
2. Create arrays, inspect their shape and dtype, and use indexing and slicing.
3. Apply broadcasting rules to combine arrays of different shapes.
4. Use NumPy for dot products, matrix multiplication, inverses and norms.
5. Rewrite a loop-based calculation, such as Euclidean distance, as a vectorized expression.

## 1. Why Vectorization Matters

Machine learning data usually arrives as large arrays. An image is a grid of numbers, a dataset is a matrix with one row per sample, and a neural network layer is a matrix of weights. If you process these with a Python `for` loop, every single step passes through the interpreter. Python must check types, look up methods and create objects at each iteration, and this overhead dominates when the arithmetic itself is trivial.

NumPy avoids this in two ways.

1. It stores numbers in one contiguous block of memory with a single fixed type, instead of a list of separate Python objects.
2. It runs the loop in compiled C code, often using processor features that handle several numbers at once.

So the idea is to describe the operation on the whole array, and let NumPy loop in a place where loops are cheap. This style is called vectorization. It is the single most important habit in numerical Python.

## 2. Array Basics

```python
import numpy as np

a = np.array([1, 2, 3])
b = np.zeros((3, 4))         # 3x4 array of zeros
c = np.arange(0, 10, 2)      # [0 2 4 6 8]
d = np.linspace(0, 1, 5)     # 5 evenly spaced numbers from 0 to 1

print(a.shape, a.dtype)
print(b[1, :])               # row 1, all columns
print(a[::-1])               # reversed
print(c, d)
```

Three attributes are worth checking every time you meet a new array.

1. `shape`: the size along each dimension, as a tuple. A matrix with 100 rows and 3 columns has shape `(100, 3)`.
2. `dtype`: the element type, such as `int64` or `float64`.
3. `ndim`: the number of dimensions.

Many bugs in machine learning code are shape bugs, so print the shape early and often.

### 2.1 More ways to create arrays

```python
ones = np.ones((2, 3))
eye = np.eye(3)                          # identity matrix
rng = np.random.default_rng(seed=0)      # reproducible random numbers
r = rng.random((2, 3))                   # uniform in [0, 1)
n = rng.normal(loc=0, scale=1, size=5)   # standard normal samples

print(ones)
print(eye)
print(r)
print(n)
```

Always create a random generator with a seed when you want results that others can reproduce. This is a basic requirement of honest experimental work.

### 2.2 Indexing and slicing

```python
M = np.arange(12).reshape(3, 4)
print(M)
print(M[0, 1])        # single element
print(M[:, 2])        # third column
print(M[1:, :2])      # rows 1 onward, first two columns

mask = M % 2 == 0
print(mask)
print(M[mask])        # boolean indexing, returns a 1D array of even values

M[M > 8] = -1         # assignment through a mask
print(M)
```

Boolean masks are used constantly, for example to select all samples of one class or to clip values above a threshold.

One important point: a basic slice is a view, not a copy.

```python
x = np.arange(5)
y = x[1:4]
y[0] = 99
print(x)              # x changed too: [ 0 99  2  3  4]

z = x[1:4].copy()
z[0] = -5
print(x)              # unchanged by z
```

If you want independent data, call `.copy()` explicitly.

### 2.3 Data types

```python
ints = np.array([1, 2, 3])
floats = np.array([1, 2, 3], dtype=np.float32)
print(ints.dtype, floats.dtype)
print((ints / 2).dtype)        # true division gives float64
print(np.array([1, 2.5]).dtype)  # mixed input is promoted to float
```

Deep learning frameworks often use `float32` to save memory and speed up computation. NumPy defaults to `float64`, which is more precise. You will occasionally convert between them with `astype`.

## 3. Broadcasting and Vectorization

### 3.1 Element-wise operations

```python
x = np.array([1, 2, 3])
y = np.array([10, 20, 30])

z = x + y                                   # vectorized
z_loop = [x[i] + y[i] for i in range(len(x))]  # loop, for comparison

print(z, z_loop)
print(x * y, x ** 2, np.sqrt(y))
```

Arithmetic operators work element by element. Note that `x * y` is not a dot product. It multiplies matching positions.

### 3.2 Broadcasting rules

Broadcasting lets NumPy combine arrays whose shapes differ, by stretching the smaller array without actually copying data. The rule, applied from the last dimension backwards, is that two dimensions are compatible if they are equal, or if one of them is 1.

```python
A = np.array([[1, 2, 3],
              [4, 5, 6]])         # shape (2, 3)

print(A + 10)                     # scalar added to every element

row = np.array([100, 200, 300])   # shape (3,)
print(A + row)                    # added to every row

col = np.array([[1], [2]])        # shape (2, 1)
print(A + col)                    # added to every column
```

A realistic use is standardizing a feature matrix, so that each column has mean 0 and standard deviation 1.

```python
rng = np.random.default_rng(1)
X = rng.normal(loc=[50, 3, 1000], scale=[10, 0.5, 200], size=(200, 3))

mean = X.mean(axis=0)     # shape (3,)
std = X.std(axis=0)       # shape (3,)
X_std = (X - mean) / std  # (200,3) - (3,) broadcasts across rows

print("before:", X.mean(axis=0).round(2), X.std(axis=0).round(2))
print("after: ", X_std.mean(axis=0).round(2), X_std.std(axis=0).round(2))
```

The argument `axis=0` means "collapse the rows", giving one result per column. `axis=1` would give one per row. Remembering this by saying "the axis that disappears" helps.

### 3.3 When broadcasting goes wrong

```python
p = np.ones((3, 2))
q = np.ones((3,))
try:
    print(p + q)
except ValueError as e:
    print("ValueError:", e)

print(p + q.reshape(3, 1))   # fixed by making q a column
```

The shapes `(3, 2)` and `(3,)` align on the last dimension, which is 2 versus 3, so they fail. Reshaping to `(3, 1)` makes the intention explicit. Whenever you see a shape error, write the shapes down and align them from the right.

### 3.4 Timing comparison

This is the demonstration we run live in class.

```python
import time

n = 1_000_000
arr = np.arange(n)

start = time.perf_counter()
result = arr * 2
vec_time = time.perf_counter() - start

start = time.perf_counter()
result_loop = [v * 2 for v in arr]
loop_time = time.perf_counter() - start

print(f"vectorized: {vec_time:.5f} s")
print(f"loop:       {loop_time:.5f} s")
print(f"speedup:    about {loop_time / vec_time:.0f}x")
```

Your numbers will depend on your machine, but the vectorized version is usually between ten and a few hundred times faster. A fairer comparison also uses a plain Python list in the loop, because iterating over a NumPy array element by element is itself slow.

```python
plain = list(range(n))
start = time.perf_counter()
result_plain = [v * 2 for v in plain]
plain_time = time.perf_counter() - start
print(f"plain list loop: {plain_time:.5f} s")
```

Try running the cell three times. The first run is sometimes slower because of caching, and a single measurement can be noisy. For serious timing use `timeit`.

```python
import timeit

t = timeit.timeit("arr * 2", globals={"arr": arr}, number=20) / 20
print(f"average vectorized time over 20 runs: {t:.5f} s")
```

## 4. Aggregations and Useful Functions

```python
data = np.array([[3, 7, 1],
                 [9, 2, 6]])

print(data.sum(), data.sum(axis=0), data.sum(axis=1))
print(data.mean(), data.max(axis=1), data.argmax(axis=1))
print(np.sort(data, axis=1))
print(np.where(data > 5, 1, 0))
print(np.cumsum([1, 2, 3, 4]))
```

`argmax` returns the position of the largest value, not the value. A classifier that outputs probabilities usually ends with `argmax` to choose the predicted class.

## 5. Linear Algebra Operations

```python
A = np.array([[1, 2], [3, 4]])
B = np.array([[5, 6], [7, 8]])

print(np.dot(A, B))        # matrix multiplication
print(A @ B)               # same result, preferred syntax
print(A * B)               # element-wise, NOT matrix multiplication
print(A.T)                 # transpose
print(np.linalg.inv(A))    # inverse
print(np.linalg.det(A))    # determinant (about -2.0)
print(A @ np.linalg.inv(A))   # approximately identity
```

The product `A @ np.linalg.inv(A)` will show values like `1.00000000e+00` and `8.88e-16`. That tiny number is floating point rounding, not a bug. Use `np.allclose` to compare results.

```python
print(np.allclose(A @ np.linalg.inv(A), np.eye(2)))   # True
```

### 5.1 Where these operations appear

1. Linear regression through the normal equation: `w = (X^T X)^(-1) X^T y`.
2. Principal component analysis, which relies on eigenvectors of a covariance matrix.
3. The forward pass of a neural network layer: `W @ x + b`.

Here is the first of these, as a preview of Week 6. We fit a line to noisy data.

```python
rng = np.random.default_rng(42)
x = rng.uniform(0, 10, size=50)
y = 3.0 * x + 5.0 + rng.normal(0, 1.0, size=50)

X = np.column_stack([np.ones_like(x), x])   # add a column of ones for the intercept
w = np.linalg.solve(X.T @ X, X.T @ y)       # solves (X^T X) w = X^T y
print("intercept, slope:", w.round(3))      # close to 5 and 3
```

We used `solve` instead of explicitly inverting the matrix. It is faster and numerically safer, and it is the recommended habit.

### 5.2 A neural network layer in three lines

```python
rng = np.random.default_rng(0)
W = rng.normal(size=(4, 3))     # 3 inputs, 4 outputs
b = np.zeros(4)
x = np.array([0.5, -1.0, 2.0])

out = np.maximum(0, W @ x + b)  # linear layer followed by ReLU
print(out)
```

This is genuinely what a layer does. A deep network is many of these stacked, and a framework such as PyTorch mainly adds automatic gradient computation and GPU support.

## 6. Worked Example: Distances Between Many Points

K-nearest neighbors and clustering both depend on distances. Suppose we have 1000 points in 5 dimensions and one query point, and we want the distance from the query to every point.

```python
import numpy as np
import time

rng = np.random.default_rng(7)
points = rng.normal(size=(1000, 5))
query = rng.normal(size=5)

# loop version
t0 = time.perf_counter()
d_loop = []
for p in points:
    s = 0.0
    for i in range(5):
        s += (p[i] - query[i]) ** 2
    d_loop.append(s ** 0.5)
t_loop = time.perf_counter() - t0

# vectorized version
t0 = time.perf_counter()
d_vec = np.linalg.norm(points - query, axis=1)
t_vec = time.perf_counter() - t0

print(np.allclose(d_loop, d_vec))
print(f"loop {t_loop:.5f}s, vectorized {t_vec:.6f}s")
print("nearest point index:", d_vec.argmin(), "distance:", d_vec.min().round(3))
```

The key line is `points - query`. The array `points` has shape `(1000, 5)` and `query` has shape `(5,)`, so broadcasting subtracts the query from every row. `np.linalg.norm(..., axis=1)` then computes one length per row. No loop is written, and the code is shorter and also far faster.

## 7. In-Class Exercise

You are given a nested-loop implementation of Euclidean distance between two vectors. Rewrite it as one vectorized NumPy expression using `np.linalg.norm`, and time both versions.

```python
import numpy as np
import timeit

def euclid_loop(u, v):
    total = 0.0
    for i in range(len(u)):
        total += (u[i] - v[i]) ** 2
    return total ** 0.5

def euclid_vec(u, v):
    return np.linalg.norm(u - v)

rng = np.random.default_rng(3)
u = rng.normal(size=10_000)
v = rng.normal(size=10_000)

print(np.isclose(euclid_loop(u, v), euclid_vec(u, v)))

t_loop = timeit.timeit(lambda: euclid_loop(u, v), number=20) / 20
t_vec = timeit.timeit(lambda: euclid_vec(u, v), number=20) / 20
print(f"loop: {t_loop:.6f}s  vectorized: {t_vec:.8f}s  ratio: {t_loop / t_vec:.0f}x")
```

Discussion:

1. Why do we use `np.isclose` and not `==` to compare the two results?
2. Does the speed gap grow or shrink as the vectors get longer? Try 100 and 1,000,000 elements.
3. Write the distance without `norm`, using `np.sqrt(np.sum((u - v) ** 2))`. Is it equivalent?

## 8. Common Mistakes

1. Using `*` when matrix multiplication is intended. Use `@`.
2. Forgetting that slices are views and modifying the original data by accident.
3. Mixing up `axis=0` and `axis=1`.
4. Creating a 1D array of shape `(n,)` when a column of shape `(n, 1)` is needed, which leads to surprising broadcasting.
5. Comparing floating point results with `==`.
6. Looping over rows when a single array expression would do.

## 9. Summary

NumPy gives Python the speed it needs for numerical work by storing data compactly and looping in compiled code. Shapes and dtypes should be checked constantly. Broadcasting removes most explicit loops, but it follows strict rules, and shape errors usually mean those rules were broken. The linear algebra functions covered today are the working parts of regression, PCA and neural networks, which we study in the coming weeks.

## 10. Practice Problems

1. Create a 5 by 5 array where each element equals the sum of its row and column index, without using loops.
2. Given a matrix of exam scores with one row per student, compute each student's average and subtract it from their row.
3. Write a function that computes the cosine similarity between one vector and every row of a matrix.
4. Generate 10,000 samples from a normal distribution, compute the fraction within one standard deviation of the mean, and compare it with the theoretical value of about 0.683.

## 11. Suggested Reading

1. The NumPy quickstart tutorial and the page on broadcasting in the official documentation.
2. The NumPy user guide section on indexing.
