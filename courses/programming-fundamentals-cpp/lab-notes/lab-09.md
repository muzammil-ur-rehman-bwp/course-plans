# Lab Notes 9 — Dynamic Memory Basics

**Concept recap:** `new` allocates on the heap and must be paired with `delete`; `new[]` allocates
a dynamic array and must be paired with `delete[]`; the heap's lifetime is independent of any
function's scope, unlike the stack.

**Common pitfalls:**
- Mismatching `new`/`delete[]` or `new[]`/`delete` — undefined behavior, not a compiler error in
  most implementations, so it can go unnoticed until it causes a crash elsewhere.
- Forgetting to `delete`/`delete[]` at all — a memory leak that may not be visible in a short-
  running lab program but matters in any long-running or repeatedly-called code.
- Using a pointer after `delete`-ing it (a dangling pointer) — set it to `nullptr` immediately
  after freeing as a habit.
- Allocating inside a loop without freeing the previous allocation first — leaks one block per
  iteration.

**Debugging tip:** for every `new`/`new[]` you write, immediately write its matching
`delete`/`delete[]` in the same editing pass, even before filling in the logic between them — this
habit prevents most leaks before they happen.

**Instructor tip:** this lab is intentionally short given the midterm; use any remaining time for
open midterm-results Q&A rather than rushing additional dynamic-memory content.
