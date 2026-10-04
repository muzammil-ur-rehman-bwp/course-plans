# Week 7 Summary — Polymorphism I: Dynamic Dispatch, Upcasting/Downcasting, `instanceof`

**Key takeaways:**
- Every Java instance method is virtual by default unless marked `final`, `private`, or
  `static` — no separate keyword is needed to get dynamic dispatch, unlike languages that require
  an explicit `virtual` declaration.
- Upcasting (subclass object through a superclass reference) is always safe; downcasting requires
  an explicit cast and can throw `ClassCastException` at runtime if the object's actual type
  doesn't match.
- Java references never slice an object the way a C++ by-value parameter can — a Java variable
  always holds a reference to the same object, so passing it anywhere preserves every override.
- `instanceof` (including the Java 16+ pattern-matching form) safely checks an object's actual
  runtime type before a downcast.

**You should now be able to:** call an overridden method polymorphically through a superclass
reference, and safely downcast using an `instanceof` guard.

**Next week:** polymorphism II — abstract classes and abstract methods, plus midterm review.
