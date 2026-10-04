# Week 4 Summary — Static vs. Instance Members

**Key takeaways:**
- An instance field exists once per object; a `static` field exists once per class, shared by
  every instance — the same relationship `main` has always had to its class.
- A `static` method has no `this` and can only directly access other static members, never an
  instance field.
- Static field initializers and static initializer blocks run exactly once, when the class is
  first loaded, before any instance is created.
- A utility class holds only `static` methods and conventionally has a private constructor to
  block instantiation.

**You should now be able to:** add a static counter to a class, write a static utility class, and
explain why a static method cannot read an instance field directly.

**Next week:** inheritance I — `extends`, access control across inheritance, and constructor
chaining with `super(...)`.
