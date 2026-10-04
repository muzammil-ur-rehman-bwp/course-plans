# Course Plan: Object Oriented Programming (C++)

## 1. Course Information

| Field | Detail |
|---|---|
| Course Title | Object Oriented Programming |
| Level | Undergraduate (2nd year, BS Computer Science / Software Engineering) |
| Credit Hours | 3 (2 hrs lecture + 1 lab session of 3 hrs/week) |
| Prerequisites | Programming Fundamentals (C++) — students must already be comfortable with variables, control flow, functions, arrays, pointers, and a first look at classes/structs. |
| Programming Language | C++ (ISO C++17) |
| Core Tools | g++ or clang++, VS Code (or any equivalent editor/IDE with a C++ debugger) |
| Duration | 16 teaching weeks (1 semester) + exam week |
| Delivery Mode | Lecture + Lab (hands-on, compiled console programs) |

## 2. Course Description

This course is the direct continuation of Programming Fundamentals (C++). Where that course gave
students a first, deliberately limited look at `class` — private data, a constructor, and public
accessor/mutator methods, with inheritance and polymorphism explicitly scoped out — this course
picks up exactly there and builds the full object-oriented toolkit in C++: constructors and
destructors in depth, operator overloading, composition, inheritance, polymorphism, templates,
exception handling, a first look at the Standard Template Library, basic software design, and
RAII/smart pointers as the modern C++ answer to manual `new`/`delete`. The course keeps the same
lab-intensive, compiled-program discipline as its prerequisite, and is equally explicit about the
subtle semantics C++ makes a programmer's responsibility — when a destructor must be `virtual`,
what object slicing is and how to avoid it, the Rule of Three/Five, and why raw owning pointers
are a liability once smart pointers are available.

## 3. Goals

- Deepen students' understanding of class construction and destruction: constructors, copy
  construction, member initializer lists, destructors, and the Rule of Three/Five.
- Teach operator overloading so students can give user-defined types natural, readable syntax.
- Teach composition and inheritance as two distinct ways to relate classes ("has-a" vs. "is-a"),
  and when each is the right tool.
- Build a correct, precise understanding of polymorphism — virtual functions, dynamic dispatch,
  abstract base classes — including the pitfalls (missing `virtual` destructors, object slicing)
  that make C++ polymorphism different from garbage-collected OOP languages.
- Introduce generic programming (function and class templates) and the STL containers/iterators
  built on the same idea.
- Teach robust error handling with C++ exceptions, and resource safety with RAII and smart
  pointers, replacing the manual `new`/`delete` discipline of the prerequisite course.
- Introduce basic software design vocabulary (UML class diagrams, a couple of SOLID principles)
  so students can reason about and communicate class designs, not just write them.
- Produce a portfolio-ready capstone project that integrates inheritance, polymorphism,
  templates/STL, and exception handling in one program.

## 4. Course Learning Outcomes (CLOs) — Mapped to Bloom's Taxonomy

| CLO | Statement | Bloom's Level(s) |
|---|---|---|
| CLO1 | Recall the mechanics of constructors, destructors, and access control in C++ classes. | Remember, Understand |
| CLO2 | Apply operator overloading and composition to build well-encapsulated user-defined types. | Apply |
| CLO3 | Analyze a problem domain to choose between composition and inheritance, and design a correct class hierarchy. | Analyze |
| CLO4 | Implement polymorphic behavior correctly using virtual functions and abstract base classes, avoiding slicing and missing-virtual-destructor bugs. | Apply, Analyze |
| CLO5 | Apply generic programming (templates) and STL containers/iterators to write reusable, type-independent code. | Apply |
| CLO6 | Evaluate and handle runtime error conditions using exceptions, and manage resources safely using RAII/smart pointers. | Apply, Evaluate |
| CLO7 | Evaluate a class design against basic software-design principles (UML, SOLID) and justify design choices. | Analyze, Evaluate |
| CLO8 | Design, build, and present an original C++ program that integrates inheritance, polymorphism, templates/STL, and exception handling. | Create, Evaluate |

### Bloom's Taxonomy progression across the semester

| Phase | Weeks | Dominant Bloom's Levels | Focus |
|---|---|---|---|
| Foundation | 1–4 | Remember, Understand, Apply | Encapsulation recap, constructors/destructors, operator overloading |
| Relationships | 5–8 | Apply, Analyze | Composition, inheritance, introduction to virtual functions/polymorphism |
| Abstraction & Genericity | 9–13 | Apply, Analyze, Evaluate | Abstract classes, templates, exceptions, STL |
| Design & Synthesis | 14–16 | Analyze, Evaluate, Create | Software design (UML/SOLID), RAII/smart pointers, capstone |

## 5. Weekly Topic Overview (16 Weeks)

| Week | Topic | Bloom's Focus |
|---|---|---|
| 1 | Classes recap & encapsulation: access specifiers, getters/setters, `const` member functions | Remember, Understand |
| 2 | Constructors in depth: default, parameterized, copy constructor, member initializer lists; destructors | Understand, Apply |
| 3 | Operator overloading I: arithmetic and comparison operators as member functions | Apply |
| 4 | Operator overloading II: stream operators (`<<`/`>>`) as `friend` functions | Apply |
| 5 | Composition ("has-a"): objects as members of other classes | Apply, Analyze |
| 6 | Inheritance I: base/derived classes, `protected` members, constructor chaining | Apply, Analyze |
| 7 | Inheritance II: overriding member functions, multiple inheritance (brief, with pitfalls) | Apply, Analyze |
| 8 | Polymorphism I: virtual functions, dynamic dispatch, virtual destructors; midterm review | Understand, Apply, Analyze |
| 9 | **Midterm Exam** + Polymorphism II: pure virtual functions, abstract base classes | Remember–Analyze |
| 10 | Templates I: function templates (generic programming basics) | Apply |
| 11 | Templates II: class templates (e.g. a generic `Stack<T>` or `Pair<T,U>`) | Apply, Analyze |
| 12 | Exception handling: `try`/`catch`/`throw`, exception hierarchies, custom exceptions | Apply, Analyze |
| 13 | Introduction to the STL: `std::vector`, `std::map`, iterators | Apply |
| 14 | Software design: UML class diagrams, composition vs. inheritance, intro to SOLID | Analyze, Evaluate |
| 15 | RAII and smart pointers (`unique_ptr`, `shared_ptr`); basic debugging/testing of OOP code | Apply, Analyze, Evaluate |
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
reserved for code that compiles but behaves incorrectly. Because this course depends directly on
C++ semantics (slicing, dangling pointers from a missing virtual destructor, etc.), code that
compiles but exhibits undefined behavior under the grader's test cases is graded as incorrect, not
merely "style."

## 8. Tools & Software

- A C++17-capable compiler: `g++` (GCC) or `clang++`
- An editor/IDE with C++ support, e.g. VS Code (with the C/C++ extension) or any equivalent IDE
- A command-line/terminal for compiling and running programs
- A debugger (`gdb` or the IDE's integrated debugger), used from Week 15 onward for RAII/leak
  diagnosis as well as general debugging
- Git/GitHub (optional, instructor's discretion) for lab and project submission

This course uses C++ exclusively; no other programming language is required or used.

## 9. Reference Textbooks

- Lippman, S. B., Lajoie, J., Moo, B. E. — *C++ Primer* — for in-depth treatment of constructors,
  operator overloading, inheritance, and templates; chapter numbers refer to topics, not a
  specific edition.
- Deitel, P. & Deitel, H. — *C++ How to Program* — for its structured treatment of classes,
  inheritance, polymorphism, and the STL, with worked examples close to this course's labs.
- Stroustrup, B. — *Programming: Principles and Practice Using C++* — for an author-of-the-
  language treatment of classes, inheritance, and generic programming.
- The official C++ reference documentation (cppreference.com) for standard library details
  (`std::vector`, `std::map`, `std::unique_ptr`, `std::shared_ptr`, exception classes, etc.).

## 10. Academic Integrity

Labs and assignments are individual unless stated otherwise. The capstone project may be done
individually or in pairs with clearly attributed contributions. Code plagiarism (including
uncredited AI-generated code submitted as original work) is handled per institutional academic
integrity policy. Students may discuss concepts with peers but must write and understand their own
code.
