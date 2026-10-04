# Week 3 Summary — NumPy for Numerical Computing

**Key takeaways:**
- NumPy arrays (`ndarray`) are the foundation of the entire Python AI/ML stack.
- Vectorized operations replace explicit loops and run substantially faster because the loop
  executes in optimized C rather than the Python interpreter.
- Broadcasting lets you combine arrays of compatible (not necessarily identical) shapes without
  manual loops.
- Core linear algebra ops (`dot`/`@`, `linalg.inv`, `linalg.det`) underlie linear regression, PCA,
  and neural network forward passes covered later in the course.

**You should now be able to:** create/index/slice NumPy arrays; vectorize a loop-based
computation; perform basic matrix operations.

**Next week:** pandas — applying these numerical skills to real, messy, tabular data.
