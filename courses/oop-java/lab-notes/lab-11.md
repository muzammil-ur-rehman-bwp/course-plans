# Lab Notes 11 — Generic Methods

**Concept recap:** a generic method declares its own type parameter (written before the return
type), independent of any class-level type parameter, inferred fresh at each call from the
arguments passed. `<? extends T>` accepts an unknown subtype of `T`; `<? super T>` accepts an
unknown supertype.

**Common pitfalls:**
- Omitting `<T extends Comparable<T>>` on a method that calls `compareTo` — without the bound,
  the compiler has no guarantee `T` supports `compareTo` at all.
- Writing the type parameter after the method name instead of before the return type — `<T>` must
  appear right before the return type, e.g. `static <T> T identity(T x)`.
- Assuming a wildcard-bounded parameter can be *written into* as freely as read from — a
  `List<? extends Number>` is safe to read `Number`s from, but unsafe to add arbitrary `Number`
  subtypes into (a subtlety this course only introduces at the reading level).
- Comparing elements with `==` instead of `.equals()` in Task D, which checks reference identity
  rather than logical equality (Week 2).

**Debugging tip:** if the compiler reports "incompatible types" on a generic method call,
re-check whether the inferred `T` is consistent across every argument the method takes.

**Instructor tip:** have students locate a real `List`/`Collections` method in the Java SE API
docs that uses a wildcard, and explain in their own words what it permits — practicing reading
library signatures, not just writing them.
