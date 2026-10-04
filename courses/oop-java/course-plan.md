# Course Plan: Object Oriented Programming (Java)

## 1. Course Information

| Field | Detail |
|---|---|
| Course Title | Object Oriented Programming |
| Level | Undergraduate (2nd year, BS Computer Science / Software Engineering) |
| Credit Hours | 3 (2 hrs lecture + 1 lab session of 3 hrs/week) |
| Prerequisites | Programming Fundamentals (Java) — students must already be comfortable with variables, control flow, methods, arrays, `ArrayList`, and a first look at classes/objects (fields, a constructor, public methods, with inheritance and polymorphism explicitly scoped out). |
| Programming Language | Java (JDK 17 or later, LTS) |
| Core Tools | JDK 17+, IntelliJ IDEA (Community Edition) or VS Code with the Java Extension Pack |
| Duration | 16 teaching weeks (1 semester) + exam week |
| Delivery Mode | Lecture + Lab (hands-on, compiled console programs) |

## 2. Course Description

This course is the direct continuation of Programming Fundamentals (Java). That course's Week 13
gave students private fields, a constructor, and encapsulation via `static` recap — on purpose,
stopping short of inheritance and polymorphism, which it explicitly named as belonging to "the
follow-on OOP course." This course picks up exactly there and builds the full object-oriented
toolkit in Java: constructors in depth and the `equals`/`hashCode`/`toString` contract, composition,
static vs. instance members, inheritance, polymorphism, abstract classes, interfaces, generics,
exception handling, the Collections Framework, and basic software-design vocabulary (UML, SOLID).
The course is explicit throughout about where Java's semantics differ from other OOP languages
students may encounter later: Java has no destructors (the garbage collector reclaims memory, so
there is no Rule of Three to track); Java has no multiple class inheritance (a class `extends`
at most one superclass, with multiple interface implementation as the mechanism for sharing
behavior across unrelated types, avoiding the diamond problem by construction); and Java object
references do not slice the way C++ objects can when passed by value, because Java always passes
and assigns object references, never raw object copies. The course keeps the same lab-intensive,
compiled-program discipline as its prerequisite.

## 3. Goals

- Deepen students' understanding of class construction: overloaded constructors, constructor
  chaining with `this(...)`, and the `equals`/`hashCode`/`toString` contract that every
  well-behaved Java class should honor.
- Teach composition and inheritance as two distinct ways to relate classes ("has-a" vs. "is-a"),
  and when each is the right tool — with a bias toward composition absent a genuine is-a
  relationship.
- Build a correct, precise understanding of polymorphism in Java — dynamic dispatch (the default
  for instance methods unless `final`/`private`/`static`), upcasting/downcasting, `instanceof`,
  and abstract classes — contrasted explicitly with the virtual-function model of C++.
- Teach interfaces as Java's mechanism for multiple behavioral inheritance, including default
  methods, and when an interface is the better design choice over an abstract class.
- Introduce generic programming (generic classes and methods, bounded types, wildcards) as the
  foundation beneath the Collections Framework students will use immediately afterward.
- Teach robust error handling with Java's checked/unchecked exception model, custom exception
  classes, and try-with-resources.
- Teach the Collections Framework (`List`/`ArrayList`, `Map`/`HashMap`) together with just enough
  lambda expressions and `Comparator` usage to sort a `List` of custom objects.
- Introduce basic software design vocabulary (UML class diagrams, a couple of SOLID principles)
  so students can reason about and communicate class designs, not just write them.
- Produce a portfolio-ready capstone project that integrates inheritance, interfaces/abstract
  classes, generics, collections, and exception handling in one program.

## 4. Course Learning Outcomes (CLOs) — Mapped to Bloom's Taxonomy

| CLO | Statement | Bloom's Level(s) |
|---|---|---|
| CLO1 | Recall the mechanics of encapsulation and class construction in Java, including the `equals`/`hashCode`/`toString` contract. | Remember, Understand |
| CLO2 | Apply composition and static vs. instance members to build well-encapsulated, correctly structured classes. | Apply |
| CLO3 | Analyze a problem domain to choose between composition and inheritance, and design a correct Java class hierarchy using `extends`. | Analyze |
| CLO4 | Implement polymorphic behavior correctly using method overriding, dynamic dispatch, and abstract classes, avoiding common pitfalls (missing `@Override`, incorrect `equals` signatures). | Apply, Analyze |
| CLO5 | Apply interfaces and generics to write flexible, reusable, type-safe code, and explain when each is the right design choice. | Apply |
| CLO6 | Evaluate and handle runtime error conditions using Java's checked/unchecked exception model and try-with-resources. | Apply, Evaluate |
| CLO7 | Apply the Collections Framework and functional-interface basics (lambdas, `Comparator`) to store, iterate, and sort collections of custom objects. | Apply |
| CLO8 | Evaluate a class design against basic software-design principles (UML, SOLID), and design, build, and present an original Java program that integrates inheritance, interfaces/abstract classes, generics, collections, and exception handling. | Analyze, Evaluate, Create |

### Bloom's Taxonomy progression across the semester

The course is deliberately sequenced to move students up Bloom's cognitive levels:

| Phase | Weeks | Dominant Bloom's Levels | Focus |
|---|---|---|---|
| Foundation | 1–4 | Remember, Understand, Apply | Encapsulation recap, constructors, `equals`/`hashCode`/`toString`, composition, static members |
| Relationships & Dynamic Behavior | 5–9 | Apply, Analyze | Inheritance, overriding, polymorphism, abstract classes, interfaces |
| Genericity & Robustness | 10–13 | Apply, Analyze | Generics, exception handling, the Collections Framework |
| Design & Synthesis | 14–16 | Analyze, Evaluate, Create | Software design (UML/SOLID), testing, capstone |

## 5. Weekly Topic Overview (16 Weeks)

| Week | Topic | Bloom's Focus |
|---|---|---|
| 1 | Classes recap & encapsulation: `private` fields, getters/setters, `this`, immutability basics | Remember, Understand |
| 2 | Constructors in depth: overloaded constructors, `this(...)` chaining; overriding `equals`/`hashCode`/`toString`; no destructors — garbage collection recap | Understand, Apply |
| 3 | Composition ("has-a"): objects as fields of other classes | Apply, Analyze |
| 4 | Static vs. instance members: static fields/methods, static initialization, utility classes | Understand, Apply |
| 5 | Inheritance I: `extends`, access control across inheritance, constructor chaining with `super(...)` | Apply, Analyze |
| 6 | Inheritance II: overriding vs. overloading, `@Override`, `Object` as the universal superclass | Apply, Analyze |
| 7 | Polymorphism I: dynamic dispatch, upcasting/downcasting, `instanceof` | Apply, Analyze |
| 8 | Polymorphism II: abstract classes, abstract methods; midterm review | Understand, Apply, Analyze |
| 9 | **Midterm Exam** + Interfaces: the `interface` keyword, multiple interface implementation, default methods | Remember–Analyze |
| 10 | Generics I: generic classes (`Box<T>`, `Pair<T,U>`), type parameters, bounded types | Apply |
| 11 | Generics II: generic methods, wildcards, bridging to the Collections Framework | Apply, Analyze |
| 12 | Exception handling: checked vs. unchecked exceptions, custom exception classes, try-with-resources | Apply, Analyze |
| 13 | The Collections Framework: `List`/`ArrayList`, `Map`/`HashMap`, enhanced `for`, lambdas and `Comparator` | Apply |
| 14 | Software design: UML class diagrams, composition vs. inheritance, intro to SOLID | Analyze, Evaluate |
| 15 | Debugging & testing OOP code: reading stack traces across hierarchies, intro to JUnit-style unit testing concepts | Analyze, Evaluate |
| 16 | Capstone project presentations; course review | Evaluate, Create |
| 17 | Final Exam Week | — |

## 6. Assessment Plan

| Component | Weight | Notes |
|---|---|---|
| Lab Work (weekly) | 20% | Graded programs, submitted weekly (Labs 1–15) |
| Assignments (4) | 20% | Problem sets assigned Weeks 4, 8, 11, 13 |
| Quizzes (6, best 5 counted) | 10% | Short, in-class, 15 min each |
| Midterm Exam | 15% | Week 9, covers Weeks 1–8 |
| Capstone Project | 20% | Introduced Wk 9, proposal Wk 11, implementation + presentation (Wk 16) |
| Final Exam | 15% | Comprehensive, emphasis on Weeks 9–16 |

## 7. Grading Policy

Standard letter grading per institutional policy (e.g., A ≥ 85, B ≥ 70, C ≥ 55, D ≥ 40, F < 40;
adjust to institution). Late submissions: −10% per day up to 3 days, then not accepted unless
documented emergency. Code that does not compile receives no functional credit; partial credit is
reserved for code that compiles but behaves incorrectly. Because this course depends on
correct object-oriented semantics (an `equals`/`hashCode` override that violates the contract, a
missing `@Override` that silently overloads instead of overriding, a raw generic type that
produces an unchecked-cast warning), code that compiles but violates these contracts under the
grader's test cases is graded as incorrect, not merely "style."

## 8. Tools & Software

- JDK 17 or later (LTS release)
- An IDE with Java support, e.g. IntelliJ IDEA (Community Edition) or VS Code with the Java
  Extension Pack
- A command-line/terminal for compiling (`javac`) and running (`java`) programs
- A debugger (the IDE's integrated debugger), used from Week 15 onward for stack-trace and
  test-driven debugging as well as general debugging
- Git/GitHub (optional, instructor's discretion) for lab and project submission

This course uses Java exclusively; no other programming language is required or used.

## 9. Reference Textbooks

- Horstmann, C. S. — *Core Java* (or *Big Java*) — for in-depth treatment of classes,
  inheritance, interfaces, generics, and the Collections Framework; chapter numbers refer to
  topics, not a specific edition.
- Deitel, P. & Deitel, H. — *Java How to Program* — for its structured treatment of classes,
  inheritance, polymorphism, interfaces, and generics, with worked examples close to this
  course's labs.
- Bloch, J. — *Effective Java* — for its chapters on the `equals`/`hashCode`/`toString` contract,
  favoring composition over inheritance, and generics best practices; used as supplementary
  best-practices reading rather than a primary text.
- The official Java SE API documentation (docs.oracle.com) for standard library details
  (`Object`, `List`, `Map`, `Comparator`, exception classes, etc.).

## 10. Academic Integrity

Labs and assignments are individual unless stated otherwise. The capstone project may be done
individually or in pairs with clearly attributed contributions. Code plagiarism (including
uncredited AI-generated code submitted as original work) is handled per institutional academic
integrity policy. Students may discuss concepts with peers but must write and understand their own
code.
