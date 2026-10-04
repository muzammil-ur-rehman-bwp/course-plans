# Lab Notes 1 — Python Essentials

**Concept recap:** variables hold references to values; `if/elif/else` branches on a condition;
`for`/`while` repeat a block; `def` defines a reusable function with parameters and a return value.

**Common pitfalls:**
- Forgetting the colon `:` at the end of `if`/`for`/`def` lines.
- Mixing tabs and spaces for indentation (use spaces consistently — most editors default to 4).
- Using `=` (assignment) where `==` (comparison) is intended inside an `if`.
- Off-by-one errors in `range(n)` — it produces `0..n-1`, not `1..n`.

**Debugging tip:** insert `print()` statements to inspect intermediate values when a function
doesn't behave as expected — this is the simplest and most effective first debugging step.

**Instructor tip:** walk around during Task B/C and check that students can explain *why* their
code works, not just that it runs — this supports the Bloom's "Understand" objective, not just
"Remember."
