# Lab Notes 6 — Inheritance II

**Concept recap:** overriding redefines an inherited method with the exact same signature;
overloading declares a different method that only shares a name. `@Override` tells the compiler
an intended override must actually match something in a superclass, catching a signature mistake
immediately. Every class implicitly extends `Object` and already has `toString`/`equals`/
`hashCode` before any code is written.

**Common pitfalls:**
- Assuming an overload "replaces" the original method — it does not; both versions coexist, and
  which one runs depends entirely on the arguments at the call site.
- Forgetting `@Override`, which lets a signature typo compile as a silent new method instead of
  failing immediately.
- Misreading the compiler error from a deliberately broken `@Override` as a bug in `@Override`
  itself, rather than as `@Override` correctly doing its job.
- Assuming a brand-new class has no behavior at all before any method is written — it already
  has `Object`'s defaults.

**Debugging tip:** if `@Override` fails to compile, re-check the exact method name, parameter
types (not just count), and return type against the superclass method you intend to override.

**Instructor tip:** connect Task C directly back to Week 2's `equals(Point)` bug — the same
compiler mechanism that catches Task C's typo would have caught that bug immediately too.
