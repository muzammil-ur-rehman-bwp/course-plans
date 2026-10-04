# Lab Notes 7 — 2D Arrays and Strings

**Concept recap:** a 2D array is an array of arrays; `String` is immutable, so every "modifying"
method returns a new `String`; `==` compares object identity, `.equals()` compares content —
always use `.equals()` for `String` content comparison; `StringBuilder` is for efficient
incremental text building.

**Common pitfalls:**
- Using `==` to compare `String` content — this is the single most common Java beginner bug
  related to strings, and it frequently "accidentally works" with literals due to string
  interning, masking the bug until a `new String(...)` or user input breaks it.
- Forgetting that `substring`, `toUpperCase`, `trim`, etc. all return a *new* `String` — writing
  `s.toUpperCase();` alone (discarding the result) does nothing to `s` itself.
- Mixing up `grid.length` (row count) with `grid[0].length` (column count of row 0) — especially
  risky for jagged arrays where rows may have different lengths.
- Building a long string with repeated `+=` in a loop and being surprised it's slow for large
  inputs — `StringBuilder` is the intended fix, not a workaround for "broken" code.

**Debugging tip:** if a `String` comparison is behaving unpredictably (works in one test, fails
in another), suspect `==` immediately and switch to `.equals()`.

**Instructor tip:** show the `new String("test") == new String("test")` vs. literal comparison
side by side — the fact that literals sometimes return `true` by coincidence is exactly why
relying on `==` is dangerous, not a sign that it's "usually fine."
