# Week 6 Summary — Inheritance II: Overriding vs. Overloading, `@Override`, `Object`

**Key takeaways:**
- Overriding redefines an inherited method with the exact same signature; overloading declares a
  different method that only shares a name — the two have very different effects and are easy to
  confuse.
- `@Override` tells the compiler an intended override must actually match a superclass method,
  turning a signature typo (or Week 2's `equals(MyClass)` bug) into a compile error instead of a
  silent new overload.
- Every class implicitly extends `java.lang.Object`, which is where the default `toString()`,
  `equals(Object)`, and `hashCode()` every class already has come from.

**You should now be able to:** distinguish overriding from overloading in a given pair of
methods, use `@Override` correctly, and explain what `Object` supplies by default.

**Next week:** polymorphism I — dynamic dispatch, upcasting/downcasting, and `instanceof`.
