# Week 6 Summary — Arrays (1D)

**Key takeaways:**
- Arrays are objects on the heap, created with `new`; numeric elements default to `0`.
- Valid indices run from `0` to `length - 1`; Java always checks bounds, throwing
  `ArrayIndexOutOfBoundsException` rather than reading adjacent memory.
- Both classic indexed loops and `for-each` iterate an array; only the indexed form can assign
  into elements.
- Because arrays are reference types, a method can mutate an array's contents in place without
  needing to return a new array.

**You should now be able to:** declare, create, index, and iterate 1D arrays; compute aggregates
(sum, max, min, average); explain and avoid `ArrayIndexOutOfBoundsException`.

**This week:** Quiz 3 (methods & 1D arrays) — see `quizzes/quiz-03.md`.

**Next week:** 2D arrays and `String`s — grids, text, and immutability.
