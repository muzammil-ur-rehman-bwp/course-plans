# Lab Notes 4 — Static vs. Instance Members

**Concept recap:** an instance field exists once per object; a static field exists once per
class, shared by every instance. Static methods have no `this` and can only directly touch other
static members. A utility class holds only static methods and blocks instantiation with a
private constructor.

**Common pitfalls:**
- Trying to access an instance field from a static method — a compile error, not a runtime bug;
  read the error message, it names the exact field.
- Forgetting to increment the static counter in *every* constructor (e.g. adding a new overloaded
  constructor later and forgetting the increment there too).
- Making a utility class's constructor `public` (or leaving the default public constructor in
  place) — defeats the "cannot be instantiated" intent.
- Confusing a static initializer block's one-time, class-load-time execution with code that runs
  per-object.

**Debugging tip:** if a static counter reports the wrong count, check that every constructor
(including every overload from Week 2's chaining) increments it exactly once.

**Instructor tip:** show `Student.getInstanceCount()` called before any `Student` is constructed
(returns `0`, since the class was already loaded by the time `main` started) to make static
initialization timing concrete.
