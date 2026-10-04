# Course Contents: Object Oriented Programming (Java)

Detailed per-week breakdown of topics, subtopics, and resources. Companion to `course-plan.md`.
Each week lists: **Topics**, **Subtopics/Skills**, **Readings**, **Software/Libraries used**.

---

## Week 1 — Classes Recap & Encapsulation
- **Topics:** Recap of `class`, fields, and methods from the prerequisite course; `private` fields
  and why direct public field access is a design smell; getters and setters; the `this` keyword
  (disambiguating a field from a same-named parameter, and as a qualifier in general); immutability
  basics — a class with no setters and `final` fields, and why immutable objects are easier to
  reason about.
- **Subtopics/Skills:** auditing a class for which fields should be private vs. exposed through an
  accessor; writing a validated setter; writing a small immutable class with `final` fields set
  only in the constructor.
- **Readings:** *Core Java*/*Big Java* ch. on classes and encapsulation; *Java How to Program* ch.
  on classes and objects.
- **Software:** JDK 17+, IntelliJ IDEA or VS Code.

## Week 2 — Constructors in Depth; `equals`/`hashCode`/`toString`; No Destructors
- **Topics:** Overloaded constructors; constructor chaining with `this(...)` (and why it must be
  the first statement in a constructor); the default (no-argument) constructor and when Java
  stops supplying one; overriding `toString()` for readable output; overriding `equals(Object)`
  correctly (the common mistake of overloading with `equals(MyClass o)` instead of overriding
  `equals(Object o)`); the `equals`/`hashCode` contract (equal objects must have equal hash codes)
  and why overriding one without the other breaks hash-based collections; Java has no
  destructors — a recap of garbage collection from the prerequisite course and why there is no
  Rule of Three to track in Java.
- **Subtopics/Skills:** writing two or three overloaded constructors that chain via `this(...)`;
  writing a correct `equals`/`hashCode`/`toString` trio for a small value class; explaining, in
  one sentence, why Java's garbage collector removes the need for a destructor.
- **Readings:** *Core Java* ch. on constructors and the `Object` methods; *Effective Java* item on
  the `equals`/`hashCode` contract.
- **Software:** JDK 17+.
- **Quiz 1** this week — Week 1 (encapsulation, getters/setters, `this`, immutability).

## Week 3 — Composition ("Has-A" Relationships)
- **Topics:** Objects as fields of another class; construction order (a composed object is
  typically constructed in the enclosing class's constructor, or passed in already constructed);
  delegating behavior to a composed object rather than duplicating it; composition as the default,
  lower-coupling alternative to inheritance (previewed, inheritance arrives Week 5).
- **Subtopics/Skills:** designing a class (e.g. `Car` composed of an `Engine`) where the outer
  class's constructor correctly initializes its composed field; explaining "has-a" vs. the "is-a"
  relationship inheritance will introduce next.
- **Readings:** *Java How to Program* ch. on composition/aggregation; *Effective Java* item on
  favoring composition over inheritance.
- **Software:** JDK 17+.
- **Quiz 2** this week — Week 2 (constructors, `this(...)`, `equals`/`hashCode`/`toString`).

## Week 4 — Static vs. Instance Members
- **Topics:** Instance fields/methods (one copy per object) vs. `static` fields/methods (one copy
  per class); static initialization (static field initializers and static initializer blocks, and
  when each runs); static methods have no `this` and can only directly touch static state; utility
  classes built entirely from `static` methods (e.g. a `MathUtils`-style class) and the convention
  of a private constructor to prevent instantiation.
- **Subtopics/Skills:** adding a `static` counter field that tracks how many instances of a class
  have been created; writing a small static utility class with a private constructor; explaining
  why `main` has always been `static` since Week 1 of the prerequisite course.
- **Readings:** *Core Java* ch. on static members; *Java How to Program* ch. on static class
  members.
- **Software:** JDK 17+.
- **Assignment 1 assigned** (encapsulation, constructors, `equals`/`hashCode`/`toString`,
  composition — Weeks 1–3), due start of Week 6.

## Week 5 — Inheritance I: `extends`, Access Control, Constructor Chaining
- **Topics:** `class Derived extends Base`; what a subclass inherits; access control across
  inheritance (`public`/`protected`/`private` — `protected` accessible to subclasses, not to
  outside code); constructor chaining — a subclass constructor must invoke a superclass
  constructor, explicitly via `super(args)` as the first statement, or implicitly via the
  superclass's no-argument constructor; Java allows a class to `extends` only one superclass
  (no multiple class inheritance — contrast with C++, where this is permitted but creates the
  diamond problem; Java's answer, multiple interface implementation, arrives Week 9).
- **Subtopics/Skills:** designing a small two-level hierarchy (e.g. `Animal` → `Dog`); writing a
  subclass constructor that passes arguments to a non-default superclass constructor via
  `super(args)`.
- **Readings:** *Core Java* ch. on inheritance basics; *Java How to Program* ch. on inheritance.
- **Software:** JDK 17+.
- **Quiz 3** this week — Week 4 (static vs. instance members, utility classes).

## Week 6 — Inheritance II: Overriding vs. Overloading; `@Override`; `Object`
- **Topics:** Overriding (redefining) a superclass's method with the same signature vs. overloading
  (same name, different parameter list) — the two are easy to confuse and have very different
  effects; the `@Override` annotation and why it should be used on every intended override (the
  compiler then catches a signature mismatch — e.g. the classic `equals(MyClass o)` bug from Week
  2 — as an error instead of silently compiling a new overload); `Object` as the universal
  superclass every class implicitly extends, and the methods (`toString`, `equals`, `hashCode`)
  every class inherits from it and may override.
- **Subtopics/Skills:** overriding a method and marking it `@Override`; deliberately introducing a
  signature typo to see the compiler catch it; tracing which `Object` methods a class already has
  before writing any code of its own.
- **Readings:** *Core Java* ch. on inheritance (overriding); *Effective Java* items on `@Override`
  and the `Object` methods.
- **Software:** JDK 17+.
- **Quiz 4** this week — Week 5 (inheritance I: `extends`, access control, `super(...)`).
- **Assignment 2 assigned** (static vs. instance members, inheritance I & II — Weeks 4–7
  cumulative through this week's material), due start of Week 10.

## Week 7 — Polymorphism I: Dynamic Dispatch, Upcasting/Downcasting, `instanceof`
- **Topics:** Static vs. dynamic binding; in Java, every instance method is **virtual by default**
  unless marked `final`, `private`, or `static` — no separate `virtual` keyword is needed, unlike
  C++, where a method must be explicitly declared `virtual` to get dynamic dispatch; upcasting
  (treating a subclass object through a superclass reference, always safe) vs. downcasting
  (casting back down, which requires a runtime check); the `instanceof` operator (including the
  Java 16+ pattern-matching form) to check a reference's actual runtime type before downcasting;
  why Java references never "slice" an object the way a C++ by-value parameter can — a Java
  variable always holds a reference to the same object, never a truncated copy of it.
- **Subtopics/Skills:** calling an overridden method through a superclass-typed reference and
  observing the subclass's version run (dynamic dispatch); writing a safe downcast guarded by
  `instanceof`.
- **Readings:** *Core Java* ch. on polymorphism; *Java How to Program* ch. on polymorphism,
  `instanceof`.
- **Software:** JDK 17+.

## Week 8 — Polymorphism II: Abstract Classes; Midterm Review
- **Topics:** The `abstract` keyword on a class (cannot be instantiated) and on a method (no body,
  must be implemented by a concrete subclass); designing a small polymorphic hierarchy (e.g.
  `Shape` with concrete subclasses `Circle`, `Rectangle`) driven entirely through superclass-typed
  references; midterm review session (Weeks 1–8).
- **Subtopics/Skills:** writing an abstract base class with at least one abstract method and at
  least one concrete (shared) method; implementing two or more concrete subclasses; practice
  problems for the midterm.
- **Readings:** *Core Java* ch. on abstract classes; *Java How to Program* ch. on abstract classes.
- **Software:** JDK 17+.
- **Quiz 5** this week — Weeks 6–7 (overriding/`@Override`, polymorphism I).

## Week 9 — Midterm Exam; Interfaces
- **Topics:** Midterm Exam (covers Weeks 1–8). Afterward: the `interface` keyword; a class
  `implements` one or more interfaces — Java's mechanism for sharing behavior across unrelated
  classes without multiple class inheritance, sidestepping the diamond problem because an
  interface (pre-Java 8) supplies no state and any conflicting default methods must be resolved
  explicitly; `default` methods on an interface (brief) as a way to add a method with a body
  without breaking existing implementers; interfaces vs. abstract classes — when each is the
  right design choice (an abstract class can hold shared state and a partial implementation; an
  interface defines a contract a class can honor alongside its one `extends` relationship).
- **Readings:** *Core Java* ch. on interfaces; *Java How to Program* ch. on interfaces;
  *Effective Java* item on interfaces vs. abstract classes.
- **Software:** JDK 17+.
- **Capstone project introduced** (proposal due Week 11).

## Week 10 — Generics I: Generic Classes
- **Topics:** Motivation (avoiding near-duplicate classes for each type, and avoiding the
  unchecked casts a pre-generics `Object`-based container would need); `class Box<T>` / `class
  Pair<T, U>` syntax; type parameters as placeholders filled in at use; bounded type parameters
  (`<T extends Number>`, brief) to require a capability of `T`; raw types and why using a generic
  class without its type parameter produces an unchecked-call warning and defeats the point of
  generics.
- **Subtopics/Skills:** implementing a generic `Box<T>` or `Pair<T, U>` with type-safe
  getters/setters; instantiating the same generic class for at least two different types to show
  genuine type independence; recognizing and avoiding a raw-type usage.
- **Readings:** *Core Java* ch. on generic programming.
- **Software:** JDK 17+.
- **Quiz 6** this week — Week 9 (interfaces, interfaces vs. abstract classes).

## Week 11 — Generics II: Generic Methods, Wildcards, Bridge to Collections
- **Topics:** Generic methods (a type parameter scoped to one method rather than the whole class);
  wildcards (`<? extends T>`, `<? super T>`, brief) for flexible method parameters; how the
  Collections Framework (`List<T>`, `Map<K, V>`, arriving Week 13) is itself built entirely from
  the generic classes and bounded types just covered, so that framework will already look
  familiar.
- **Subtopics/Skills:** writing a generic method (e.g. a generic `max` or `printAll`) with its own
  type parameter; reading a wildcard-bounded method signature from the standard library and
  explaining what it permits.
- **Readings:** *Core Java* ch. on generics (wildcards, generic methods).
- **Software:** JDK 17+.
- **Assignment 3 assigned** (polymorphism II, interfaces, generics I — Weeks 8–10), due start of
  Week 13.

## Week 12 — Exception Handling
- **Topics:** `try`/`catch`/`throw`; the Java exception hierarchy (`Throwable` → `Exception` /
  `Error`; checked exceptions, which must be declared with `throws` or caught, vs. unchecked
  `RuntimeException`s, which need not be); writing a custom exception class extending `Exception`
  (checked) or `RuntimeException` (unchecked); `try`-with-resources for any `AutoCloseable`
  resource, replacing manual `finally`-block cleanup; multiple `catch` clauses ordered specific to
  general; the checked-exception requirement as Java's compiler-enforced version of the error
  handling discipline C++ leaves entirely to convention.
- **Subtopics/Skills:** writing a method that validates input and `throws` an appropriate checked
  or unchecked exception; writing a `try`-with-resources block; writing a custom checked exception
  class and a method that correctly declares it with `throws`.
- **Readings:** *Core Java* ch. on exceptions; *Java How to Program* ch. on exception handling.
- **Software:** JDK 17+.

## Week 13 — The Collections Framework
- **Topics:** `List<T>`/`ArrayList<T>` as a growable, type-safe array (bridging directly to Weeks
  10–11's generics); `Map<K, V>`/`HashMap<K, V>` as an associative container, and why a key type
  used in a `HashMap` must have a correct `equals`/`hashCode` pair (tying back to Week 2); iterating
  with the enhanced `for` loop; a first look at lambda expressions as a compact way to implement a
  single-method functional interface; using `Comparator` (written as a lambda) with
  `List.sort`/`Collections.sort` to sort a `List` of custom objects by a chosen field.
- **Subtopics/Skills:** replacing a hand-rolled array or lookup structure with `ArrayList`/
  `HashMap`; iterating a `List`/`Map` with an enhanced `for` loop; sorting a `List<CustomObject>`
  with a lambda-based `Comparator`.
- **Readings:** *Core Java* ch. on the Collections Framework; *Java How to Program* ch. on
  generic collections; Java SE API docs on `List`, `Map`, `Comparator`.
- **Software:** JDK 17+, `java.util`.
- **Assignment 4 assigned** (generics II, exception handling, Collections Framework — Weeks
  11–13), due start of Week 15.

## Week 14 — Software Design: UML, Composition vs. Inheritance, SOLID
- **Topics:** Reading and drawing a basic UML class diagram (classes, attributes, methods,
  composition/aggregation vs. inheritance arrows); revisiting "is-a" (inheritance) vs. "has-a"
  (composition) as a design decision, not just a syntax choice, with guidance to prefer composition
  when there is no true is-a relationship; an undergraduate-level introduction to two SOLID
  principles — the Single Responsibility Principle and the Open/Closed Principle — with small Java
  before/after examples.
- **Subtopics/Skills:** drawing a UML class diagram for a hierarchy designed earlier in the course;
  critiquing a class that does "too much" and refactoring it to follow single responsibility;
  recognizing an Open/Closed violation (a chain of `instanceof` checks that must be edited for
  every new subclass) and how polymorphism/interfaces (Weeks 7–9) resolve it.
- **Readings:** *Java How to Program* ch. on object-oriented design/UML; supplementary notes on
  SOLID (undergraduate-level treatment).
- **Software:** JDK 17+; any diagramming tool (or pen and paper) for UML.

## Week 15 — Debugging & Testing OOP Code
- **Topics:** Reading a Java stack trace that spans a class hierarchy (which override actually
  ran, where a `NullPointerException` or `ClassCastException` originated versus where it
  surfaced); common OOP-specific bugs revisited as a checklist (a missing `@Override` that
  silently overloads, an `equals` override with the wrong parameter type, overriding `equals`
  without `hashCode`, a raw generic type producing an unchecked warning); an introduction, at the
  conceptual level, to unit testing OOP code (what a JUnit-style test method and assertion check,
  without requiring an actual JUnit project setup) — writing a small hand-rolled test harness that
  exercises a class's constructors, overridden methods, and polymorphic behavior.
- **Subtopics/Skills:** tracing a multi-frame stack trace back to its root cause across two or
  three overridden methods; writing a few assertion-style test cases (hand-rolled `if`/print, or
  Java's `assert`) that exercise a class's public interface and its polymorphic behavior.
- **Readings:** *Core Java* ch. on exceptions/debugging; *Effective Java* items on testing and
  defensive design (survey level).
- **Software:** JDK 17+, IDE debugger.

## Week 16 — Capstone Presentations, Course Review
- **Topics:** Student capstone project presentations; recap of the course map (encapsulation →
  constructors/`equals`-`hashCode`-`toString` → composition → static members → inheritance →
  polymorphism → abstract classes → interfaces → generics → exceptions → collections → design →
  testing); brief look ahead to Data Structures & Algorithms and other follow-on courses.
- **Deliverable:** Capstone project final submission + presentation.

## Week 17 — Final Exam Week
- Comprehensive final exam, weighted toward Weeks 9–16 content (per Assessment Plan).

---

## Capstone Project (introduced Week 9, proposal Week 11, final Week 16)
Students (individually or in pairs) design and build a Java console application that models a
small domain using a polymorphic class hierarchy, and that integrates inheritance, interfaces
and/or an abstract class, at least one generic type or Collections Framework container, and
exception handling for invalid input or invalid operations end-to-end. Example topics: a shape
hierarchy with an area/perimeter calculator (`Shape` abstract class with `Circle`/`Rectangle`/
`Triangle` subclasses), a polymorphic inventory system (an `Item` hierarchy or an interface for
shared item behavior), or a mini banking system with an account-type hierarchy (e.g. checking vs.
savings, each with different interest/fee behavior) using an interface for transaction behavior —
each driven through superclass- or interface-typed references, each storing its objects in a
`List`/`Map`, and each handling at least one realistic error condition (invalid amount, missing
record) by throwing and catching an exception.
