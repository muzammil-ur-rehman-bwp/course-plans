# Week 15 Summary — Debugging, Testing, and Program Design/Style

**Key takeaways:**
- A stack trace should be read from the top: the exception type and message describe the problem,
  and the top-most `at ...` line in your own code is almost always where the bug is.
- `NullPointerException`, `ArrayIndexOutOfBoundsException`, `NumberFormatException`, and
  `ArithmeticException` (integer division by zero) are the most common runtime exceptions, each
  with a recognizable typical cause.
- A debugger's breakpoints, stepping, and variable watches are far faster than
  print-statement debugging for understanding *why* a value is wrong.
- Validating method arguments and failing loudly with a clear exception catches misuse at the
  source; writing expected-output test cases by hand builds the same discipline formal unit
  testing frameworks later automate.

**You should now be able to:** read and act on a Java stack trace; use an IDE debugger to isolate
a bug; validate inputs defensively; write and check simple hand-built test cases.

**Next week:** capstone project presentations and course review.
