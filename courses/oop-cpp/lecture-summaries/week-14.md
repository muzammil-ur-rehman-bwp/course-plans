# Week 14 Summary — Software Design: UML, Composition vs. Inheritance, SOLID

**Key takeaways:**
- A UML class diagram shows a class's name, attributes, and methods; a hollow-triangle arrow
  means inheritance, a filled-diamond line means composition.
- Default to composition; use inheritance only when the derived type must be genuinely
  substitutable for the base type everywhere — not merely to reuse code.
- The Single Responsibility Principle: a class should have one reason to change; split a class
  that mixes unrelated responsibilities (I/O, computation, presentation).
- The Open/Closed Principle: prefer designs where new behavior is added via a new class (e.g. a
  new `Shape` subclass) rather than by editing an existing `if`/`switch` on type — exactly what
  Weeks 8–9's polymorphism enables.

**You should now be able to:** read and sketch a basic UML class diagram, justify a composition-
vs.-inheritance choice, and critique a class design against SRP and OCP.

**Next week:** RAII and smart pointers (`unique_ptr`, `shared_ptr`) as the modern C++ answer to
manual `new`/`delete`, plus basic debugging/testing of OOP code.
