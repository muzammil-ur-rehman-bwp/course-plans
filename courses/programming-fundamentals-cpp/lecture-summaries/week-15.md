# Week 15 Summary — Debugging, Testing, and Program Design/Style

**Key takeaways:**
- Compile with `-Wall -Wextra` and treat every warning as a likely real bug, not noise to
  suppress.
- A debugger (breakpoints, stepping, watching variables) finds bugs faster than scattered `print`
  statements by letting you inspect actual program state.
- `const` on reference/pointer parameters is a compiler-enforced promise, not decoration — it
  catches accidental modification at compile time.
- Validate inputs defensively, and write concrete input/expected-output test cases (including
  edge cases) by hand before trusting a function works.

**You should now be able to:** use compiler warnings and a debugger to isolate a bug; apply
const-correctness to function parameters; write basic manual test cases, including edge cases.

**Next week:** capstone project presentations and a review of the full course.
