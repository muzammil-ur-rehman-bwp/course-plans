# Lab Notes 8 — Pointers & Midterm Practice

**Concept recap:** `&x` gets the address of `x`; `*p` dereferences pointer `p` to access/modify
the value it points to; an array name decays to a pointer to its first element; always initialize
pointers (`nullptr` if no valid target yet) and check before dereferencing.

**Common pitfalls:**
- Confusing `*` the declaration marker (`int* p`) with `*` the dereference operator (`*p`) —
  they look identical but mean different things depending on context.
- Dereferencing an uninitialized or `nullptr` pointer — undefined behavior/crash.
- Writing `*(values + i)` incorrectly as `*values + i` (missing parentheses) — this dereferences
  first, then adds `i` to the *value*, not the address; always parenthesize pointer arithmetic
  before dereferencing.
- For Task C: forgetting to dereference the output pointers (`*minOut = ...`, not
  `minOut = ...`) when writing results back through them.

**Debugging tip:** when a pointer-based function gives a wrong answer but the equivalent
array-indexed version works, print the pointer's address and the addresses of a couple of array
elements to confirm the arithmetic is landing where you expect.

**Instructor tip:** for midterm review, prioritize having students *explain* their answers aloud
to a partner before checking a key — verbalizing exposes gaps that silent review hides.
