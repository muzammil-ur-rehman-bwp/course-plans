# Course Plan: Programming Fundamentals (Java)

## 1. Course Information

| Field | Detail |
|---|---|
| Course Title | Programming Fundamentals |
| Level | Undergraduate (1st year, BS Computer Science / Software Engineering) |
| Credit Hours | 3 (2 hrs lecture + 1 lab session of 3 hrs/week) |
| Prerequisites | None (high-school level mathematics assumed). This is the first programming course (CS1). |
| Programming Language | Java (JDK 17 or later, LTS) |
| Core Tools | JDK 17+, IntelliJ IDEA (Community Edition) or VS Code with the Java extension pack |
| Duration | 16 teaching weeks (1 semester) + exam week |
| Delivery Mode | Lecture + Lab (hands-on, compiled console programs) |

## 2. Course Description

This course is the student's first formal introduction to programming, taught using Java. It
builds a solid foundation in imperative programming — variables, control flow, methods, arrays,
and basic program structure — before introducing just enough of Java's class mechanism to preview
the follow-on Object-Oriented Programming course. Because every Java program lives inside a
class and runs on the Java Virtual Machine, the course places explicit emphasis on how a program
is actually built and run (`javac`, `java`, the JVM), and on the discipline (correct indexing,
careful use of `==` vs. `.equals()`, checked exceptions, testing) needed to avoid the runtime
errors Java is designed to surface loudly rather than silently. The course is lab-intensive:
every lecture topic is paired with a hands-on lab that produces a working, compiled program.

## 3. Goals

- Build fluency in core Java syntax and the edit–compile–run–debug cycle.
- Develop the ability to decompose a problem into variables, control flow, and methods.
- Teach correct use of arrays and `String`s, and Java's reference-type semantics (how object
  references, `null`, and automatic memory management behave) — and the discipline to avoid the
  bugs (off-by-one indexing, `NullPointerException`, `==` vs. `.equals()` confusion) that careless
  use of these features causes.
- Introduce `ArrayList`, wrapper classes, and a first look at designing simple classes so students
  arrive at the Object-Oriented Programming course already comfortable with user-defined types.
- Produce a portfolio-ready capstone console application that integrates the semester's concepts.

## 4. Course Learning Outcomes (CLOs) — Mapped to Bloom's Taxonomy

| CLO | Statement | Bloom's Level(s) |
|---|---|---|
| CLO1 | Recall Java syntax, primitive types, and the compile-run model on the JVM. | Remember, Understand |
| CLO2 | Use operators, control flow, and loops to implement algorithmic logic. | Apply |
| CLO3 | Analyze a problem to choose appropriate control structures, methods, and data layout (arrays/`ArrayList`). | Analyze |
| CLO4 | Implement methods, arrays, and `String`/`StringBuilder` processing correctly, avoiding common Java pitfalls (off-by-one indexing, `NullPointerException`, `==` vs. `.equals()`). | Apply, Analyze |
| CLO5 | Evaluate and debug program behavior using compiler/runtime errors, stack traces, a debugger, and systematic testing. | Analyze, Evaluate |
| CLO6 | Design simple user-defined classes (fields, constructors, methods, encapsulation) and file-based I/O with exception handling for a program's data. | Apply, Analyze |
| CLO7 | Design, build, and present an original console application that integrates arrays/collections, classes, and file I/O. | Create, Evaluate |

### Bloom's Taxonomy progression across the semester

The course is deliberately sequenced to move students up Bloom's cognitive levels:

| Phase | Weeks | Dominant Bloom's Levels | Focus |
|---|---|---|---|
| Foundation | 1–4 | Remember, Understand, Apply | Java basics, operators, control flow, loops |
| Core Mechanics | 5–8 | Apply, Analyze | Methods, arrays, strings, objects/references |
| Structure & Data | 9–12 | Apply, Analyze | Classes, collections, file I/O, exceptions, recursion |
| Synthesis | 13–16 | Apply, Analyze, Evaluate, Create | Encapsulation, algorithms, software craftsmanship, capstone |

## 5. Weekly Topic Overview (16 Weeks)

| Week | Topic | Bloom's Focus |
|---|---|---|
| 1 | Intro to programming & Java basics: the JVM, `javac`/`java`, `System.out`, `Scanner`, variables, primitive types | Remember, Understand |
| 2 | Operators, expressions, and type conversion; primitive vs. reference types | Understand, Apply |
| 3 | Control flow: `if`/`else`, `switch` | Apply |
| 4 | Loops: `for`, `while`, `do-while` | Apply |
| 5 | Methods: declaration/definition, parameters, overloading, pass-by-value semantics | Apply, Analyze |
| 6 | Arrays (1D) | Apply, Analyze |
| 7 | Arrays (2D) and `String`: immutability, common methods, `StringBuilder` | Apply, Analyze |
| 8 | Intro to objects & references; the stack/heap model; midterm review | Understand, Apply, Analyze |
| 9 | **Midterm Exam** + classes and objects (fields, constructors, methods) | Remember–Apply |
| 10 | Using existing classes/APIs: `ArrayList`, wrapper classes, `java.util` | Apply, Analyze |
| 11 | File I/O (`Scanner`/`BufferedReader`, `PrintWriter`/`FileWriter`) and exceptions basics | Apply |
| 12 | Recursion | Apply, Analyze |
| 13 | More on classes: encapsulation, static vs. instance members (preview of the OOP course) | Understand, Apply |
| 14 | Sorting & searching: bubble sort, selection sort, binary search on arrays | Apply, Analyze |
| 15 | Debugging, testing, and program design/style (stack traces, debugger, simple assertions) | Analyze, Evaluate |
| 16 | Capstone project presentations; course review | Evaluate, Create |
| 17 | Final Exam Week | — |

## 6. Assessment Plan

| Component | Weight | Notes |
|---|---|---|
| Lab Work (weekly) | 20% | Graded programs, submitted weekly (Labs 1–15) |
| Assignments (4) | 20% | Problem sets assigned Weeks 4, 10, 13, 14 |
| Quizzes (6, best 5 counted) | 10% | Short, in-class, 15 min each |
| Midterm Exam | 15% | Week 9, covers Weeks 1–8 |
| Capstone Project | 20% | Proposal (Wk 11) + implementation + presentation (Wk 16) |
| Final Exam | 15% | Comprehensive, emphasis on Weeks 9–16 |

## 7. Grading Policy

Standard letter grading per institutional policy (e.g., A ≥ 85, B ≥ 70, C ≥ 55, D ≥ 40, F < 40;
adjust to institution). Late submissions: −10% per day up to 3 days, then not accepted unless
documented emergency. Code that does not compile receives no functional credit; partial credit is
reserved for code that compiles but behaves incorrectly.

## 8. Tools & Software

- JDK 17 or later (LTS release)
- An IDE with Java support, e.g. IntelliJ IDEA (Community Edition) or VS Code with the Java
  Extension Pack
- A command-line/terminal for compiling (`javac`) and running (`java`) programs
- A debugger (the IDE's integrated debugger) — introduced from Week 15 onward
- Git/GitHub (optional, instructor's discretion) for lab and project submission

This course uses Java exclusively; no other programming language is required or used.

## 9. Reference Textbooks

- A standard introductory Java textbook such as *Java: An Introduction to Problem Solving and
  Programming* (Savitch) or *Big Java* (Horstmann) — either edition available through the
  institution's library is suitable; chapter numbers below refer to topics, not a specific
  edition.
- Any current edition of *Java: The Complete Reference* (Schildt) for standard library/language
  reference depth beyond what the lectures cover.
- The official Java documentation (the Java SE API documentation, docs.oracle.com) for standard
  library details (`String`, `ArrayList`, `java.io`, etc.).

## 10. Academic Integrity

Labs and assignments are individual unless stated otherwise. The capstone project may be done
individually or in pairs with clearly attributed contributions. Code plagiarism (including
uncredited AI-generated code submitted as original work) is handled per institutional academic
integrity policy. Students may discuss concepts with peers but must write and understand their own
code.
