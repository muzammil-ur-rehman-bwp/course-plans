# Week 15 Summary — Debugging & Testing OOP Code

**Key takeaways:**
- A Java stack trace reads top to bottom: the top frame is where the exception was actually
  thrown, which across a class hierarchy tells you exactly which override ran and where it failed.
- A short checklist of recurring OOP bugs — a missing `@Override`, an `equals` override with the
  wrong parameter type, `equals` without a matching `hashCode`, and raw generic types — all
  compile cleanly and only misbehave at runtime, so they are worth checking for deliberately.
- A unit test calls a small piece of code and asserts the result matches what's expected; a real
  JUnit project wraps this idea in `@Test` annotations and a runner, but the underlying idea
  (call, assert) is what matters for writing testable classes.
- A hand-rolled test harness with no framework can still exercise a class's constructors,
  overridden methods, and polymorphic behavior usefully.

**You should now be able to:** trace a multi-frame stack trace to its root cause, recognize the
four recurring OOP bugs from code alone, and write simple assertion-based test cases.

**Next week:** capstone project presentations and course review.
