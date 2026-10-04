# Week 16 — Lecture Content: Capstone Presentations; Course Review

## 1. Presentation Logistics
Each team presents for 5–7 minutes using `presentations/capstone-presentation-template.md`,
followed by audience Q&A. The instructor grades live using `assignments/capstone-rubric.md`.
Audience members are expected to ask at least one substantive question per project (e.g. "which
class is your abstract base class, and what does it force every subclass to implement?" or "walk
me through the exception your program throws and where it's caught").

## 2. Course Map: What Each Week Built On the Last
```
Week 1  Encapsulation (private fields, getters/setters, this, immutability)
Week 2  Constructors, this(...), equals/hashCode/toString, GC (no destructors)
Week 3  Composition ("has-a")
Week 4  Static vs. instance members, utility classes
Week 5  Inheritance I: extends, access control, super(...)
Week 6  Inheritance II: overriding vs. overloading, @Override, Object
Week 7  Polymorphism I: dynamic dispatch, upcasting/downcasting, instanceof
Week 8  Polymorphism II: abstract classes
Week 9  Interfaces: implements, multiple interfaces, default methods
Week 10 Generics I: generic classes (Box<T>, Pair<T,U>)
Week 11 Generics II: generic methods, wildcards, bridge to Collections
Week 12 Exception handling: checked/unchecked, custom exceptions, try-with-resources
Week 13 Collections Framework: List/Map, lambdas, Comparator
Week 14 Software design: UML, composition vs. inheritance, SOLID (SRP, OCP)
Week 15 Debugging & testing OOP code: stack traces, OOP bug checklist, test concepts
Week 16 Capstone presentations; course review
```
Every later week assumed the ones before it: polymorphism (Week 7) only works because overriding
(Week 6) is defined on top of inheritance (Week 5); generics (Weeks 10–11) are the machinery
behind the Collections Framework (Week 13); and the capstone (introduced Week 9) was designed to
require inheritance/interfaces, generics or collections, and exception handling together — the
three pillars the course spent the most weeks building.

## 3. Java vs. Other OOP Languages: What Made This Course Distinctly Java
- **No destructors.** Garbage collection (Week 2) replaces the C++ Rule of Three entirely — there
  is no destructor to forget, and no dangling-pointer bug class to defend against.
- **No multiple class inheritance.** `extends` takes exactly one superclass (Week 5); multiple
  *interface* implementation (Week 9) is Java's answer, and it sidesteps the diamond problem by
  construction rather than requiring careful disambiguation rules.
- **Virtual by default.** Every instance method dispatches dynamically unless explicitly
  `final`/`private`/`static` (Week 7) — no separate keyword is needed to opt in, unlike languages
  where a method must be marked virtual to get this behavior.
- **No object slicing.** A Java variable always holds a reference, never a by-value object copy,
  so passing a subclass object as a superclass-typed parameter never truncates it (Week 7).
- **Checked exceptions.** The compiler enforces that certain exceptions be declared or handled
  (Week 12) — a discipline some languages leave entirely to documentation and convention.

## 4. Looking Ahead
Data Structures & Algorithms builds directly on the Collections Framework (Week 13) and generics
(Weeks 10–11) covered here, now analyzing the time/space trade-offs behind `ArrayList`, `HashMap`,
and the data structures you will implement from scratch. Software Engineering and Design Patterns
courses build directly on Week 14's SOLID introduction and the interfaces-vs-abstract-classes
distinction from Week 9.

## 5. In-Class Exercise
During Q&A, each student writes down, for one project other than their own, which of the three
required pillars (inheritance/interfaces, generics/collections, exception handling) they found
most convincingly demonstrated, and one specific line or method that proved it.
