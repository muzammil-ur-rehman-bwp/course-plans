# Lab Manual 3 — NumPy for Numerical Computing

**Duration:** 3 hours | **Prerequisite:** Week 3 lecture

## Objectives
Build fluency with NumPy array creation, indexing, broadcasting, and basic linear algebra.

## Setup
`pip install numpy`; create `lab03.ipynb`.

## Procedure
1. **Task A — Array basics:** create a 5x5 array of random integers (0–100); extract its middle
   row, middle column, and all values greater than 50.
2. **Task B — Broadcasting:** given a matrix of shape (100, 3) representing 100 points in 3D,
   subtract the column-wise mean from every row (mean-centering) without a loop.
3. **Task C — Vectorization timing:** implement Euclidean distance between two vectors both as a
   Python loop and as a vectorized `np.linalg.norm` call; time both for a vector of length
   1,000,000 and report the speedup ratio observed.
4. **Task D — Linear algebra:** given a 3x3 matrix, compute its inverse and determinant, and
   verify `A @ A_inv` is (approximately) the identity matrix.

## Expected Output
A notebook with Tasks A–D, including the measured timing numbers and speedup ratio for Task C
(exact numbers will vary by machine — the direction/magnitude of the speedup is what matters).

## Submission
Submit `lab03.ipynb` by the end of the lab session.
