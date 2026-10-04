# Week 8 Summary — Polymorphism II: Abstract Classes; Midterm Review

**Key takeaways:**
- An `abstract` class cannot be instantiated and may mix abstract methods (no body, must be
  implemented by a concrete subclass) with ordinary concrete methods shared by every subclass.
- The compiler enforces that every concrete subclass implements every abstract method it
  inherits, so there is no way to end up with a subclass silently missing one.
- Code written entirely against an abstract superclass type can drive a mix of concrete subclass
  objects correctly through dynamic dispatch, with no `instanceof` chain needed.
- Weeks 1–8 together cover encapsulation, constructors/`Object` methods, composition, static
  members, inheritance, and polymorphism — the midterm's full scope.

**You should now be able to:** design and use an abstract class with both abstract and concrete
methods, and recall Weeks 1–8 material for the midterm.

**Next week:** Midterm Exam, then interfaces — the `interface` keyword, multiple interface
implementation, and default methods.
