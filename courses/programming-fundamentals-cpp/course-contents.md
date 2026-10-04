# Course Contents: Programming Fundamentals (C++)

Detailed per-week breakdown of topics, subtopics, and resources. Companion to `course-plan.md`.
Each week lists: **Topics**, **Subtopics/Skills**, **Readings**, **Software/Libraries used**.

---

## Week 1 — Intro to Programming & C++ Basics
- **Topics:** What a program is; the compile-link-run model (`g++`); your first program
  (`#include`, `main`, `return 0`); `std::cout`/`std::cin`; variables and identifiers; primitive
  types (`int`, `double`, `char`, `bool`); basic I/O.
- **Subtopics/Skills:** compiling from the command line (`g++ -std=c++17 -Wall file.cpp -o prog`);
  reading compiler errors; using an IDE (VS Code) to edit and run a C++ file.
- **Readings:** Introductory textbook ch. 1–2 (getting started, variables/types).
- **Software:** g++/clang++, VS Code.

## Week 2 — Operators, Expressions, and Type Conversion
- **Topics:** Arithmetic, relational, logical, and assignment operators; operator precedence;
  increment/decrement; implicit vs. explicit type conversion (casts); integer vs. floating-point
  arithmetic and overflow/precision pitfalls.
- **Subtopics/Skills:** writing expressions that compute correctly without relying on undefined
  evaluation order; using `static_cast<T>` for explicit conversions.
- **Readings:** Introductory textbook ch. 2–3 (operators, expressions).
- **Software:** g++/clang++.

## Week 3 — Control Flow: `if`/`else`, `switch`
- **Topics:** Boolean expressions; `if`/`else if`/`else`; nested conditionals; the `switch`
  statement and `break`/fall-through; the conditional (ternary) operator.
- **Subtopics/Skills:** translating a decision table into branching code; avoiding the dangling-
  `else` and missing-`break` pitfalls.
- **Readings:** Introductory textbook ch. 4 (control structures).
- **Software:** g++/clang++.

## Week 4 — Loops: `for`, `while`, `do-while`
- **Topics:** `for`, `while`, `do-while`; loop control (`break`, `continue`); nested loops;
  sentinel-controlled vs. counter-controlled loops; common loop patterns (accumulation, counting,
  searching).
- **Subtopics/Skills:** choosing the right loop construct for a task; avoiding off-by-one and
  infinite-loop errors.
- **Readings:** Introductory textbook ch. 5 (loops/repetition).
- **Software:** g++/clang++.
- **Assignment 1 assigned** (basics, operators, control flow, loops).

## Week 5 — Functions
- **Topics:** Function declaration vs. definition; parameters and return types; pass-by-value vs.
  pass-by-reference (`&`); default arguments; function overloading; scope and lifetime of local
  variables.
- **Subtopics/Skills:** decomposing a program into small, testable functions; writing a function
  that modifies a caller's variable via a reference parameter.
- **Readings:** Introductory textbook ch. 6 (functions).
- **Software:** g++/clang++.

## Week 6 — Arrays (1D)
- **Topics:** Array declaration, initialization, indexing; passing arrays to functions; the
  array-decays-to-pointer behavior; fixed-size array limitations; `std::array` as a safer
  alternative (brief mention).
- **Subtopics/Skills:** iterating over an array with a loop; computing aggregates (sum, max, min,
  average) over an array; passing an array and its size together to a function.
- **Readings:** Introductory textbook ch. 7 (arrays).
- **Software:** g++/clang++.
- **Quiz 3 — functions & 1D arrays.**

## Week 7 — 2D Arrays and Strings
- **Topics:** 2D array declaration/indexing (row-major layout); nested loops over a 2D array;
  C-strings (`char[]`, null terminator, `<cstring>` functions) vs. `std::string`; common
  `std::string` operations (concatenation, indexing, `substr`, `find`, comparison).
- **Subtopics/Skills:** processing a matrix (e.g., row/column sums); converting between C-strings
  and `std::string`; why `std::string` is preferred in modern C++ code.
- **Readings:** Introductory textbook ch. 7–8 (multidimensional arrays, strings).
- **Software:** g++/clang++, `<string>`, `<cstring>`.

## Week 8 — Pointers & References; Midterm Review
- **Topics:** Memory addresses and the `&`/`*` operators; pointer declaration, initialization,
  `nullptr`; pointer arithmetic basics; pointers vs. references (similarities/differences); review
  session for Weeks 1–7.
- **Subtopics/Skills:** writing a function that takes a pointer parameter; using a pointer to
  traverse an array; practice problems for the midterm.
- **Readings:** Introductory textbook ch. 9 (pointers).
- **Software:** g++/clang++.
- **Quiz 4 — 2D arrays, strings, pointers.**

## Week 9 — Midterm Exam; Dynamic Memory
- **Topics:** Midterm Exam (covers Weeks 1–8). Afterward: the stack vs. the heap; `new`/`delete`
  for single objects and arrays; memory leaks and dangling pointers; why every `new` needs a
  matching `delete`.
- **Readings:** Introductory textbook ch. 9–10 (dynamic memory).
- **Software:** g++/clang++.
- **Capstone project introduced** (proposal due Week 11).

## Week 10 — Structs
- **Topics:** Defining a `struct`; member access (`.`); arrays of structs; passing structs to
  functions (by value vs. by reference); structs vs. arrays for organizing related data.
- **Subtopics/Skills:** modeling a real-world record (e.g., a student or product) as a struct;
  building and processing an array of structs.
- **Readings:** Introductory textbook ch. 11 (structs/records).
- **Software:** g++/clang++.
- **Assignment 2 assigned** (pointers, dynamic memory, structs).

## Week 11 — File I/O
- **Topics:** `<fstream>`; `ifstream`/`ofstream`; opening/closing files; checking for open
  failures; reading line-by-line (`getline`) vs. token-by-token (`>>`); writing formatted output
  to a file; reading structured data (e.g., CSV-like lines) into structs/arrays.
- **Subtopics/Skills:** persisting a program's data to a file and reloading it on the next run.
- **Readings:** Introductory textbook ch. 12 (file I/O).
- **Software:** g++/clang++, `<fstream>`.
- **Quiz 5 — structs & file I/O.**
- **Capstone proposal due.**

## Week 12 — Recursion
- **Topics:** Recursive function structure (base case + recursive case); tracing recursion with a
  call stack/trace diagram; classic examples (factorial, Fibonacci, sum of an array); recursion
  vs. iteration trade-offs; stack overflow from missing/incorrect base cases.
- **Subtopics/Skills:** writing and tracing a recursive function by hand; converting a simple
  iterative function to a recursive one and back.
- **Readings:** Introductory textbook ch. 13 (recursion).
- **Software:** g++/clang++.

## Week 13 — Intro to Classes
- **Topics:** `class` vs. `struct`; access specifiers (`private`/`public`); encapsulation;
  constructors; member functions; a first simple class (e.g., `Rectangle`, `BankAccount`) — just
  enough mechanics to preview the dedicated Object-Oriented Programming course, not full OOP
  (no inheritance/polymorphism here).
- **Subtopics/Skills:** designing a class with private data and public accessor/mutator methods;
  writing and using a constructor.
- **Readings:** Introductory textbook ch. 14 (intro to classes).
- **Software:** g++/clang++.
- **Assignment 3 assigned** (file I/O, recursion).
- **Quiz 6 — recursion & intro to classes.**

## Week 14 — Sorting & Searching Algorithms
- **Topics:** Bubble sort and selection sort (on arrays); linear search; binary search (on sorted
  arrays); informal comparison of algorithm efficiency (number of comparisons/swaps, intuition
  for Big-O without a formal proof).
- **Subtopics/Skills:** implementing bubble sort, selection sort, and binary search from scratch;
  tracing algorithm execution on a small example array.
- **Readings:** Introductory textbook ch. 15 (intro to algorithms/sorting-searching).
- **Software:** g++/clang++.
- **Assignment 4 assigned** (intro classes, sorting/searching).

## Week 15 — Debugging, Testing, and Program Design/Style
- **Topics:** Reading and acting on compiler warnings (`-Wall -Wextra`); using a debugger
  (breakpoints, stepping, watching variables); const-correctness (`const` parameters/variables);
  defensive programming (input validation, assertions); a basic unit-testing mindset (writing
  small test cases/expected outputs by hand, before formal frameworks).
- **Subtopics/Skills:** debugging a provided buggy program using the debugger and compiler
  warnings; refactoring a working program for const-correctness and clearer style.
- **Readings:** Introductory textbook ch. 16 (program style/debugging), or supplementary notes.
- **Software:** g++/clang++ (`-Wall -Wextra`), gdb or IDE debugger.

## Week 16 — Capstone Presentations, Course Review
- **Topics:** Student capstone project presentations; recap of the course map (basics → control
  flow → functions → arrays/strings → pointers → dynamic memory → structs → file I/O → recursion →
  classes → algorithms); brief look ahead to the Object-Oriented Programming and Data Structures
  courses.
- **Deliverable:** Capstone project final submission + presentation.

## Week 17 — Final Exam Week
- Comprehensive final exam, weighted toward Weeks 9–16 content (per Assessment Plan).

---

## Capstone Project (introduced Week 9, proposal Week 11, final Week 16)
Students (individually or in pairs) design and build a C++ console application that integrates
several course concepts end-to-end: arrays and/or structs for data storage, functions for
organization, file I/O for persistence, and (optionally) recursion or simple sorting/searching.
Example topics: a student grade-book manager, a simple library catalog, a personal inventory
tracker, or a to-do list manager with save/load — each reading and writing its data to a file so
the program's state survives between runs.
