# Lab Notes 7 — 2D Arrays and Strings

**Concept recap:** 2D arrays are indexed `[row][column]` and processed with nested loops;
`std::string` manages its own size and supports `+`, `==`, `substr`, and `find`; `getline` reads a
full line including spaces, while `>>` stops at whitespace.

**Common pitfalls:**
- Swapping row and column indices (`matrix[col][row]` instead of `matrix[row][col]`) — easy to
  do and produces a transposed, wrong result that still "looks like" a matrix.
- Using `std::cin >> name` for a full name with spaces — it only reads the first word; use
  `std::getline(std::cin, name)` instead.
- Comparing strings with `strcmp`-style thinking (`name == "Alice"` is actually correct for
  `std::string` — the pitfall is assuming it *isn't*, from C-string habits, and reaching for a
  function that isn't needed).
- Off-by-one row/column loop bounds, same as with 1D arrays, but now in two dimensions at once.

**Debugging tip:** print the matrix after filling it, before computing any sums — confirming the
input step worked correctly isolates whether a bug is in reading data or in processing it.

**Instructor tip:** show `std::cin >> fullName` failing to capture a two-word name live, then fix
it with `getline`, so the "`>>` stops at whitespace" rule is memorable rather than abstract.
