# Lab Notes 13 — Fuzzy Sets, Membership Functions, and Rule Evaluation

**Concept recap:** a triangular membership function rises linearly from 0 at its left foot to 1
at its peak, then falls linearly to 0 at its right foot; fuzzy AND/OR/NOT generalize Boolean
operations via `min`/`max`/`1 - x`; a rule's firing strength is typically the fuzzy AND of its
antecedents' membership degrees.

**Common pitfalls:**
- Off-by-boundary errors in `triangular_membership` — at `x == a` or `x == c` the function must
  return exactly `0.0`, and at `x == b` exactly `1.0`; a common bug uses strict `<`/`>` throughout
  and ends up with a `ZeroDivisionError` or an incorrect value exactly at these boundary points.
- Implementing `trapezoidal_membership` as if it were triangular with an extra parameter ignored
  — the flat top between `b` and `c` must return `1.0` for the *entire* range, not just at a
  single peak point.
- Confusing fuzzy OR (`max`) with probabilistic OR (`a + b - a*b`) — both are valid ways to
  combine degrees in different formal systems, but this course's convention is the standard
  Zadeh min/max fuzzy operations; using the probabilistic formula will not match expected test
  values.
- Treating a membership degree as if it were a probability that must sum to 1 across a variable's
  fuzzy sets — fuzzy memberships are not required to sum to 1 (an input can be, e.g., 0.3 Cold and
  0.3 Hot and 0.4 nothing-in-particular all at once); do not normalize them.

**Debugging tip:** plot (even as a simple printed table of `x` vs. `μ(x)` across a range) each
membership function before using it in a rule — a visibly wrong shape (e.g., a triangle that
never reaches 1.0, or extends past its stated feet) is much easier to catch this way than by
testing isolated points.

**Instructor tip:** require Task A's boundary-case tests (`x == a`, `x == c`) explicitly in the
rubric discussion before grading Task C — a membership function that is subtly wrong only at its
boundaries will otherwise produce plausible-looking but silently incorrect rule-firing strengths
throughout Task C.
