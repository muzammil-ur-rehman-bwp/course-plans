# Lab Notes 3 — Control Flow: `if`/`else`, `switch`

**Concept recap:** `if`/`else if`/`else` chains are evaluated top to bottom, first match wins;
`switch` compares an integer/char value against constant case labels and falls through without
`break`; the ternary operator is a compact alternative for simple either/or expressions.

**Common pitfalls:**
- Ordering `if`/`else if` conditions incorrectly (e.g., checking `>= 70` before `>= 85`), so a
  high score matches the wrong, earlier branch.
- Omitting `break` in a `switch` case unintentionally, causing fall-through into the next case's
  code.
- Not bracing single-statement `if`/`else` bodies, risking a silently-wrong addition later.
- Forgetting `switch` cannot test ranges (`score >= 70`) — only exact constant matches.

**Debugging tip:** when a `switch`/`if` chain gives a surprising result, add a `std::cout` right
before the branch printing the exact value being tested — many "wrong branch" bugs are actually
"wrong value going in" bugs.

**Instructor tip:** deliberately show a `switch` with a missing `break` running live, so students
see fall-through happen rather than just reading about it.
