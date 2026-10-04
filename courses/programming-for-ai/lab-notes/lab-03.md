# Lab Notes 3 — NumPy

**Concept recap:** NumPy arrays store homogeneous numeric data in contiguous memory, enabling
vectorized (loop-free) operations that run in optimized C. Broadcasting extends operations
across arrays of compatible shapes without explicit loops.

**Common pitfalls:**
- Shape mismatches in broadcasting (e.g., subtracting a (3,) array from a (100,3) matrix works;
  subtracting a (100,) array from it does not — check shapes with `.shape` when confused).
- Confusing `*` (element-wise multiply) with `@`/`np.dot` (matrix multiply).
- Forgetting that `np.linalg.inv` can fail/be numerically unstable for singular or
  near-singular matrices — always sanity-check with `A @ A_inv ≈ I`.

**Debugging tip:** when broadcasting fails, print `.shape` of every array involved — shape
mismatches are the #1 source of NumPy errors for beginners.

**Instructor tip:** make sure every student actually observes the timing difference in Task C on
their own machine — the "vectorization matters" lesson lands much better experienced firsthand
than read about.
