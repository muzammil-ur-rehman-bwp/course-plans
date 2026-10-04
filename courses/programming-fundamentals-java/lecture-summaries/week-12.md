# Week 12 Summary — Recursion

**Key takeaways:**
- Every correct recursive method needs a base case (stops the recursion) and a recursive case
  (progresses toward it).
- Each active call gets its own stack frame with its own local variables; tracing the call stack
  by hand clarifies how a chain of recursive calls resolves.
- Recursion and iteration can solve the same problems; recursion often reads more naturally for
  self-similar problems, while a loop is usually more efficient for simple linear accumulation.
- A missing or incorrect base case causes unbounded recursion and a `StackOverflowError` — the
  recursive analog of an infinite loop.

**You should now be able to:** write and trace simple recursive methods (factorial, Fibonacci,
array sum); identify a missing base case as the cause of a `StackOverflowError`.

**Next week:** more on classes — encapsulation and `static` vs. instance members.
