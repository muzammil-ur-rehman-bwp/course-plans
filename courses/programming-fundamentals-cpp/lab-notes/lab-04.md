# Lab Notes 4 — Loops: `for`, `while`, `do-while`

**Concept recap:** `for` suits known iteration counts; `while` checks its condition before the
body (may run zero times); `do-while` checks after the body (always runs at least once);
`break` exits a loop, `continue` skips to the next iteration.

**Common pitfalls:**
- Off-by-one loop bounds (`<=` vs `<`) — always state the intended first and last value before
  writing the loop header.
- Forgetting to update the loop variable (or the sentinel-read statement) inside a `while` body,
  producing an infinite loop.
- Using `do-while` when the body should *not* run on bad initial input (e.g., a menu loop that
  should skip entirely if some precondition already fails) — `do-while` always runs once.
- Confusing `break` (exits the loop) with `continue` (skips to the next iteration) — trace by
  hand if the output looks off by one iteration.

**Debugging tip:** if a loop seems to run forever, interrupt it (Ctrl+C) and re-read the loop
header specifically for the update step — missing or misplaced updates are the most common cause.

**Instructor tip:** live-demo an infinite loop on purpose (with a visible counter printed each
iteration) and have students spot the missing update before you fix it.
