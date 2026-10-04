# Week 6 Summary — Arrays (1D)

**Key takeaways:**
- An array is a fixed-size, contiguous block of same-typed elements, indexed from `0` to `n - 1`.
- C++ performs no automatic bounds checking on raw arrays — out-of-bounds access is undefined
  behavior, not a safe error.
- An array passed to a function decays to a pointer; always pass the size alongside it, and mark
  the parameter `const` if the function only reads the data.
- `std::array` exists as a safer, size-aware alternative, but this course works with raw arrays to
  build the fundamentals.

**You should now be able to:** declare, initialize, and iterate over a 1D array; write functions
that take an array and its size as parameters; explain why out-of-bounds access is dangerous.

**Next week:** 2D arrays and strings — extending indexing to a grid, and comparing C-strings to
`std::string`.
