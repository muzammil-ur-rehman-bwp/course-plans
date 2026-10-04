# Lab Notes 12 — Recursion

**Concept recap:** every correct recursive method needs a base case and a recursive case that
progresses toward it; each call gets its own stack frame; a missing/incorrect base case causes
unbounded recursion and a `StackOverflowError`.

**Common pitfalls:**
- Omitting the base case entirely, or writing one that is never actually reached (e.g., testing
  for exact equality to `0` when the recursive case can skip past `0`, as with a step of `2`).
- Forgetting that each recursive call's local variables are independent — confusing a variable's
  value in one call with its value in a different, nested call.
- Writing `fibonacci` (or similar) without realizing how many redundant calls it makes for larger
  `n` — this is a performance lesson, not a correctness bug, and worth flagging as such.
- Confusing `StackOverflowError` (too many nested calls) with `OutOfMemoryError` (heap
  exhaustion) — they have different causes and different fixes.

**Debugging tip:** when a recursive method misbehaves, trace it by hand for a small input first
(e.g., `n = 2` or `3`) before assuming the logic is correct for the full-size input.

**Instructor tip:** use a visual call-stack diagram (boxes stacking up and then resolving back
down) every time recursion is introduced or debugged — it is the single most effective aid for
students building intuition for recursive execution.
