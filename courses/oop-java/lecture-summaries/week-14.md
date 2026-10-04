# Week 14 Summary — Software Design: UML, Composition vs. Inheritance, SOLID

**Key takeaways:**
- A UML class diagram shows a class's attributes and methods with visibility markers, and uses a
  hollow-triangle arrow for inheritance ("is-a") and a diamond for composition/aggregation
  ("has-a") — a clear way to communicate a design before or alongside writing code.
- Inheritance should model a genuine "is-a" relationship where every superclass invariant holds
  for the subclass; when that is doubtful, composition is usually the right tool even if
  inheritance would compile.
- The Single Responsibility Principle favors splitting a class that changes for multiple unrelated
  reasons into several smaller classes, each with one job.
- The Open/Closed Principle favors a design (typically via polymorphism) that lets new behavior
  be added without editing existing, already-tested code — the alternative being an
  `instanceof` chain that must be revisited for every new case.

**You should now be able to:** draw a basic UML class diagram, and recognize and fix an SRP or
OCP violation in a given class.

**Next week:** debugging and testing OOP code — reading stack traces across class hierarchies and
an introduction to unit-testing concepts.
