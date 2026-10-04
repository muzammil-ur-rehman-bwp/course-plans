# Lab Notes 10 — Structs

**Concept recap:** `struct` groups related fields into one type, accessed with `.`; aggregate
initialization sets fields in declaration order; pass structs by `const` reference to read-only
functions and by non-const reference to functions that must modify the caller's struct.

**Common pitfalls:**
- Passing a struct by value to a function meant to modify it — the function only gets a copy,
  so changes never reach the caller's original struct (same pass-by-value trap as Week 5, now on
  a bigger object).
- Mixing up the order of fields in aggregate initialization (`{"Alice", 20, 3.8}`) — the compiler
  will not catch a swapped `age`/`gpa` if both happen to be numeric-compatible in position.
- Forgetting `const` on a read-only struct parameter, missing a chance for the compiler to catch
  an accidental modification.
- Returning a large struct by value from a function (as in `findTopStudent`) and worrying it's
  "inefficient" — for this course's struct sizes this is fine and idiomatic; premature
  optimization here obscures the simpler, correct code.

**Debugging tip:** print one full struct's fields with a dedicated small print function/loop
right after filling it — isolate "did I read the data correctly" from "did I process it
correctly" as two separate debugging steps.

**Instructor tip:** have students predict, before running, whether a by-value `raiseGpa` would
visibly change the roster array afterward — this reinforces pass-by-value vs. reference with
struct-sized data specifically.
