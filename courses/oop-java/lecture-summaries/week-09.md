# Week 9 Summary — Midterm Exam; Interfaces

**Key takeaways:**
- An `interface` declares a contract of method signatures with no instance state; a class
  declares it fulfills that contract with `implements`.
- A class may `implements` many interfaces while `extends`-ing at most one superclass — Java's
  deliberate alternative to multiple class inheritance, which avoids the diamond problem by
  construction since an interface carries no state of its own.
- `default` methods let an interface supply a method with a body, so existing implementers keep
  compiling when a new method is added later.
- An abstract class is the right tool when subclasses share real state or substantial
  implementation and form a genuine "is-a" hierarchy; an interface is the right tool for a
  capability shared across otherwise-unrelated classes.

**You should now be able to:** design and implement an interface, implement multiple interfaces
in one class, and choose between an interface and an abstract class for a given design.

**Next week:** generics I — generic classes, type parameters, and bounded types. The capstone
project is now underway (proposal due Week 11).
