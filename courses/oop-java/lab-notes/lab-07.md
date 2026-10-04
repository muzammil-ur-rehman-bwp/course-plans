# Lab Notes 7 — Polymorphism I

**Concept recap:** every Java instance method is virtual by default unless `final`/`private`/
`static`. Upcasting is always safe and implicit; downcasting requires an explicit cast and can
throw `ClassCastException` at runtime. `instanceof` checks the actual runtime type before a
downcast. Java references never slice an object — a variable always refers to the same full
object, regardless of its declared type.

**Common pitfalls:**
- Assuming a `static` method is polymorphic — it is resolved by the reference's *declared* type,
  not the object's actual type, unlike instance methods.
- Downcasting without an `instanceof` guard "because it usually works in testing" — the grader's
  test cases are specifically designed to include a mismatched type.
- Confusing "upcast" and "downcast" direction — upcast is always toward a more general
  (superclass) type and is implicit; downcast is toward a more specific (subclass) type and needs
  an explicit cast.
- Forgetting that a `ClassCastException` is a `RuntimeException` (Week 12 preview) — it compiles
  fine and only fails when actually run with a mismatched object.

**Debugging tip:** a `ClassCastException` message names both the actual class and the attempted
target class — read it before guessing at a fix.

**Instructor tip:** contrast Task A's output directly with what the *same* code would do if
`makeSound()` were marked `final` in `Animal` (it would not compile in `Dog` as an override at
all) to make "virtual by default, opt out explicitly" concrete.
