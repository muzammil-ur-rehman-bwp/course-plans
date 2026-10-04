# Lab Notes 3 — Control Flow

**Concept recap:** `if`/`else if`/`else` chains test conditions top to bottom; `switch` compares
one value against constant cases and needs `break` to avoid fall-through (or use the arrow-form
`switch` expression, which never falls through).

**Common pitfalls:**
- Ordering grading bands incorrectly (e.g., testing `score >= 40` before `score >= 85`) — the
  first matching condition wins, so looser bands must come last.
- Forgetting `break` in a traditional `switch`, causing execution to fall through into the next
  case unintentionally.
- Omitting `default` in a `switch`, silently doing nothing for unexpected input instead of
  reporting an error.
- Writing `if (grade = 'A')` instead of `if (grade == 'A')` — again, Java's type system usually
  catches this at compile time since the assignment's result type won't be `boolean`, but it's
  worth flagging explicitly as the single-equals/double-equals confusion.

**Debugging tip:** when a `switch` produces unexpected behavior, check every case for a missing
`break` first — add it back one case at a time if unsure which one is missing.

**Instructor tip:** have students trace a decision table on paper first, then write the code — it
separates "did I get the logic right" from "did I get the Java syntax right" as two distinct
skills to debug.
