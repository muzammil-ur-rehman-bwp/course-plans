# Lab Notes 6 — Propositional Logic: Truth Tables & Equivalence

**Concept recap:** a model assigns True/False to every symbol; a sentence's truth table lists
its value under every model; satisfiable means true in at least one model, valid means true in
every model, and two sentences are equivalent iff their truth tables match exactly.

**Common pitfalls:**
- Implementing `implies` as `a and b` instead of `(not a) or b` — a classic error that silently
  breaks every equivalence check involving `->`.
- Forgetting that `itertools.product([False, True], repeat=n)` order determines column order in
  the printed table — if the table "looks wrong," check whether you are reading columns in the
  order you intended.
- Treating `iff` as equivalent to `and` — remember `P <-> Q` is true when both are False too.

**Debugging tip:** for any equivalence check that returns `False` unexpectedly, print both
sentences' full truth tables side by side (same symbol order) and find the first row where they
disagree — that row is usually enough to spot the bug in one of the two expressions.

**Instructor tip:** ask students to predict the number of rows in a truth table before running
the code (`2^n` for `n` symbols) — a quick way to catch students who have not yet connected the
combinatorics to the code.
