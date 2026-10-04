# Week 3 Summary — Control Flow: `if`/`else`, `switch`

**Key takeaways:**
- `if`/`else if`/`else` chains are evaluated top to bottom; the first true condition wins —
  ordering decision bands correctly (loosest last) matters.
- An `else` binds to the nearest unmatched `if`; use braces consistently to avoid ambiguity when
  nesting conditionals.
- `switch` compares one value against constant cases; forgetting `break` causes fall-through. The
  modern arrow-form `switch` expression (Java 14+) avoids fall-through entirely.
- The ternary operator `condition ? a : b` compactly selects between two values.

**You should now be able to:** translate a decision table into correct `if`/`else` or `switch`
code; explain and avoid the dangling-`else` and missing-`break` pitfalls.

**Next week:** loops — repeating logic with `for`, `while`, and `do-while`.
