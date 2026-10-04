# Week 12 Summary — Recursion

**Key takeaways:**
- Every recursive function needs a base case (solved directly) and a recursive case (reduces
  toward the base case).
- Recursive calls stack up, each waiting for the one below it to return, then unwind in reverse
  order to produce the final result.
- A missing or incorrect base case causes infinite recursion and a stack overflow crash.
- Recursion and iteration are interchangeable in principle; recursion often reads more naturally
  for self-similar problems, at the cost of call-stack overhead.

**You should now be able to:** identify the base and recursive cases of a function; write and
trace simple recursive functions; explain the cause of a stack-overflow bug.

**Next week:** an introduction to classes — encapsulating data and behavior together, previewing
the dedicated Object-Oriented Programming course.
