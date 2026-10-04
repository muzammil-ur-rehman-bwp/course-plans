# Lab Notes 2 — Constructors & the `Object` Contract

**Concept recap:** `this(...)` chains to another constructor of the same class and must be the
first statement. `equals(Object obj)` must use exactly that signature to override `Object`'s
version; a matching `hashCode()` is required whenever `equals()` is overridden. Java has no
destructors — garbage collection reclaims unreachable objects automatically.

**Common pitfalls:**
- Writing `equals(Point other)` instead of `equals(Object obj)` — compiles, but silently
  overloads instead of overriding, and collection classes (`HashSet`, `HashMap`) will not use it.
- Overriding `equals()` but leaving `hashCode()` as `Object`'s default — two "equal" objects can
  then report different hash codes, breaking hash-based collections.
- Putting a statement before `this(...)` in a constructor — this is a compile error, not a
  warning.
- Forgetting `@Override` on `equals`/`hashCode`/`toString` — without it, a signature mistake
  compiles silently instead of failing loudly.

**Debugging tip:** always write `@Override` on an intended override. If it doesn't compile, the
method doesn't actually override anything — check the parameter type first.

**Instructor tip:** demonstrate `new Point(1,1).equals(new Point(1,1))` returning `false` with
the buggy `equals(Point)` overload called through an `Object` reference, to make the bug visible
rather than abstract.
