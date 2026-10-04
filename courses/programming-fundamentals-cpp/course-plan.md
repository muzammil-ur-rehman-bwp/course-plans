# Course Plan: Programming Fundamentals (C++)

## 1. Course Information

| Field | Detail |
|---|---|
| Course Title | Programming Fundamentals |
| Level | Undergraduate (1st year, BS Computer Science / Software Engineering) |
| Credit Hours | 3 (2 hrs lecture + 1 lab session of 3 hrs/week) |
| Prerequisites | None (high-school level mathematics assumed). This is the first programming course (CS1). |
| Programming Language | C++ (ISO C++17) |
| Core Tools | g++ or clang++, VS Code (or any editor/IDE with a C++ debugger) |
| Duration | 16 teaching weeks (1 semester) + exam week |
| Delivery Mode | Lecture + Lab (hands-on, compiled console programs) |

## 2. Course Description

This course is the student's first formal introduction to programming, taught using C++. It
builds a solid foundation in imperative programming — variables, control flow, functions, arrays,
pointers, and basic program structure — before introducing just enough of C++'s class mechanism to
preview the follow-on Object-Oriented Programming course. Because C++ is a compiled, statically
typed language with manual memory management, the course places explicit emphasis on how a
program is actually built and run (compiling, linking, the stack and the heap), and on the
discipline (careful indexing, initialization, const-correctness, testing) needed to avoid the
undefined behavior that C++ permits. The course is lab-intensive: every lecture topic is paired
with a hands-on lab that produces a working, compiled program.

## 3. Goals

- Build fluency in core C++ syntax and the edit–compile–run–debug cycle.
- Develop the ability to decompose a problem into variables, control flow, and functions.
- Teach safe, correct use of arrays, pointers, and dynamically allocated memory — and the
  discipline to avoid the bugs (out-of-bounds access, memory leaks, dangling pointers) that
  careless use of these features causes.
- Introduce structs and a first look at classes so students arrive at the Object-Oriented
  Programming course already comfortable with user-defined types.
- Produce a portfolio-ready capstone console application that integrates the semester's concepts.

## 4. Course Learning Outcomes (CLOs) — Mapped to Bloom's Taxonomy

| CLO | Statement | Bloom's Level(s) |
|---|---|---|
| CLO1 | Recall C++ syntax, primitive types, and the compile-link-run model. | Remember, Understand |
| CLO2 | Use operators, control flow, and loops to implement algorithmic logic. | Apply |
| CLO3 | Analyze a problem to choose appropriate control structures, functions, and data layout (arrays/structs). | Analyze |
| CLO4 | Implement functions, arrays, pointers, and dynamic memory correctly, avoiding common C++ pitfalls (out-of-bounds access, leaks, dangling pointers). | Apply, Analyze |
| CLO5 | Evaluate and debug program behavior using compiler warnings, a debugger, and systematic testing. | Analyze, Evaluate |
| CLO6 | Design simple user-defined types (structs, introductory classes) and file-based I/O for a program's data. | Apply, Analyze |
| CLO7 | Design, build, and present an original console application that integrates arrays, structs, functions, and file I/O. | Create, Evaluate |

### Bloom's Taxonomy progression across the semester

The course is deliberately sequenced to move students up Bloom's cognitive levels:

| Phase | Weeks | Dominant Bloom's Levels | Focus |
|---|---|---|---|
| Foundation | 1–4 | Remember, Understand, Apply | C++ basics, operators, control flow, loops |
| Core Mechanics | 5–8 | Apply, Analyze | Functions, arrays, strings, pointers/references |
| Structure & Data | 9–12 | Apply, Analyze | Dynamic memory, structs, file I/O, recursion |
| Synthesis | 13–16 | Apply, Analyze, Evaluate, Create | Intro to classes, algorithms, software craftsmanship, capstone |

## 5. Weekly Topic Overview (16 Weeks)

| Week | Topic | Bloom's Focus |
|---|---|---|
| 1 | Intro to programming & C++ basics: compiling a program, `cin`/`cout`, variables, primitive types | Remember, Understand |
| 2 | Operators, expressions, and type conversion | Understand, Apply |
| 3 | Control flow: `if`/`else`, `switch` | Apply |
| 4 | Loops: `for`, `while`, `do-while` | Apply |
| 5 | Functions: declaration/definition, parameters (by value/by reference), overloading | Apply, Analyze |
| 6 | Arrays (1D) | Apply, Analyze |
| 7 | Arrays (2D) and strings: C-strings vs. `std::string` | Apply, Analyze |
| 8 | Pointers & references; midterm review | Understand, Apply, Analyze |
| 9 | **Midterm Exam** + dynamic memory (`new`/`delete`) | Remember–Apply |
| 10 | Structs | Apply, Analyze |
| 11 | File I/O: `ifstream`/`ofstream` | Apply |
| 12 | Recursion | Apply, Analyze |
| 13 | Intro to classes: encapsulation basics (preview of the OOP course) | Understand, Apply |
| 14 | Sorting & searching: bubble sort, selection sort, binary search on arrays | Apply, Analyze |
| 15 | Debugging, testing, and program design/style (const-correctness, compiler warnings, basic unit testing) | Analyze, Evaluate |
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

- A C++17-capable compiler: `g++` (GCC) or `clang++`
- An editor/IDE with C++ support, e.g. VS Code (with the C/C++ extension) or any equivalent IDE
- A command-line/terminal for compiling and running programs
- A debugger (`gdb` or the IDE's integrated debugger) — introduced from Week 15 onward
- Git/GitHub (optional, instructor's discretion) for lab and project submission

This course uses C++ exclusively; no other programming language is required or used.

## 9. Reference Textbooks

- A standard introductory C++ textbook such as *C++ Primer* (Lippman, Lajoie, Moo) or
  *Starting Out with C++: From Control Structures through Objects* (Gaddis) — either edition
  available through the institution's library is suitable; chapter numbers below refer to topics,
  not a specific edition.
- Stroustrup, B. — *Programming: Principles and Practice Using C++* (for a from-scratch,
  author-of-the-language treatment).
- The official C++ reference documentation (cppreference.com) for standard library details
  (`std::string`, `std::vector`, streams, etc.).

## 10. Academic Integrity

Labs and assignments are individual unless stated otherwise. The capstone project may be done
individually or in pairs with clearly attributed contributions. Code plagiarism (including
uncredited AI-generated code submitted as original work) is handled per institutional academic
integrity policy. Students may discuss concepts with peers but must write and understand their own
code.
