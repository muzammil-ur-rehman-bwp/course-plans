# Week 7 Summary — 2D Arrays and Strings

**Key takeaways:**
- A 2D array in Java is an array of arrays; `grid.length` is the row count, `grid[row].length`
  is that row's column count.
- `String` is immutable — every "modifying" method actually returns a new `String`; the original
  never changes.
- `==` on `String`s (and reference types generally) compares object identity, not content —
  always use `.equals()` to compare `String` content.
- `StringBuilder` builds text incrementally far more efficiently than repeated `+=` concatenation
  in a loop.

**You should now be able to:** process a matrix with nested loops; use core `String` methods
correctly; explain why `==` is unreliable for `String` comparison; use `StringBuilder` for
efficient string building.

**Next week:** intro to objects & references — how reference-type variables actually work, and
midterm review.
