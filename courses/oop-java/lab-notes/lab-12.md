# Lab Notes 12 — Exception Handling

**Concept recap:** a checked exception (extends `Exception`, not `RuntimeException`) must be
declared with `throws` or caught; an unchecked exception (extends `RuntimeException`) requires
neither. try-with-resources automatically closes any `AutoCloseable` resource when the block
exits, normally or via an exception.

**Common pitfalls:**
- Forgetting `throws InvalidAgeException` on a method that throws a checked exception without
  catching it internally — a compile error, not a runtime surprise.
- Ordering `catch` clauses general-to-specific instead of specific-to-general — the compiler
  rejects an unreachable specific `catch` placed after a more general one.
- Manually calling `close()` inside a try-with-resources block in addition to the automatic
  close — redundant, and a sign the pattern wasn't fully understood.
- Catching `Exception` immediately and silently discarding it (an empty `catch` block) — hides
  real bugs; always do something meaningful with a caught exception, even if just logging it.

**Debugging tip:** if a checked exception "won't compile" at a call site, the method either needs
a `try`/`catch` around the call, or needs its own `throws` clause to pass the obligation up to
*its* caller.

**Instructor tip:** deliberately comment out a resource's close logic in a non-try-with-resources
version first, to make the resource-leak risk concrete before introducing the safer pattern.
