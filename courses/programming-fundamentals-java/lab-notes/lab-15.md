# Lab Notes 15 — Debugging and Testing

**Concept recap:** read a stack trace from the top — the exception type and (in modern JDKs) the
named null expression tell you what went wrong, and the top-most `at ...` line in your own code
is almost always where to look; a debugger's breakpoints/stepping/watches are faster than
print-statement debugging; `assert` documents an invariant but is disabled by default at runtime.

**Common pitfalls:**
- Reading a stack trace bottom-up instead of top-down, or stopping at the first `at` line that
  mentions a JDK internal class rather than continuing to the first line that names the student's
  own class.
- Relying on `assert` statements for real input validation — they do nothing at all unless the
  JVM is run with `-ea`, so production-facing validation must use real `if`/exception checks.
- Adding `System.out.println` debugging statements and forgetting to remove them before
  submission, cluttering the real program output.
- Writing test cases only for the "happy path" and skipping boundary values (zero, negative,
  empty array/list) — exactly the inputs most likely to reveal a bug.

**Debugging tip:** before opening the debugger, always form a specific, falsifiable hypothesis
from the stack trace first ("I think line 12 is null because X was never assigned") — this turns
debugging into confirming or rejecting a guess rather than aimless stepping.

**Instructor tip:** grade the mini-challenge test harness partly on whether it includes at least
one boundary-value test, not just typical-case tests — this habit is worth reinforcing explicitly
before the capstone, where untested edge cases are the most common source of lost points.
