# Course Contents: Programming Fundamentals (Java)

Detailed per-week breakdown of topics, subtopics, and resources. Companion to `course-plan.md`.
Each week lists: **Topics**, **Subtopics/Skills**, **Readings**, **Software/Libraries used**.

---

## Week 1 — Intro to Programming & Java Basics
- **Topics:** What a program is; the JVM and the compile-run model (`javac`/`java`); your first
  program (`class`, `public static void main(String[] args)`); `System.out.println`/`print`;
  `Scanner` for console input; variables and identifiers; primitive types (`int`, `double`,
  `char`, `boolean`); basic I/O.
- **Subtopics/Skills:** compiling and running from the command line (`javac Hello.java`,
  `java Hello`); reading compiler errors; using an IDE (IntelliJ IDEA or VS Code) to edit and run
  a Java file.
- **Readings:** Introductory textbook ch. 1–2 (getting started, variables/types).
- **Software:** JDK 17+ (`javac`/`java`), IntelliJ IDEA or VS Code.

## Week 2 — Operators, Expressions, and Type Conversion
- **Topics:** Arithmetic, relational, logical, and assignment operators; operator precedence;
  increment/decrement; implicit widening vs. explicit casting; integer vs. floating-point
  arithmetic and overflow/precision pitfalls; the primitive-type vs. reference-type distinction.
- **Subtopics/Skills:** writing expressions that compute correctly; using an explicit cast
  (e.g., `(double) a / b`) to avoid integer-division bugs; recognizing which Java types are
  primitives and which are object references.
- **Readings:** Introductory textbook ch. 2–3 (operators, expressions).
- **Software:** JDK 17+.

## Week 3 — Control Flow: `if`/`else`, `switch`
- **Topics:** Boolean expressions; `if`/`else if`/`else`; nested conditionals; the `switch`
  statement and `break`/fall-through; the traditional `switch` vs. the Java 14+ switch expression
  (arrow form); the conditional (ternary) operator.
- **Subtopics/Skills:** translating a decision table into branching code; avoiding the
  dangling-`else` and missing-`break` pitfalls.
- **Readings:** Introductory textbook ch. 4 (control structures).
- **Software:** JDK 17+.

## Week 4 — Loops: `for`, `while`, `do-while`
- **Topics:** `for`, `while`, `do-while`; loop control (`break`, `continue`); nested loops;
  sentinel-controlled vs. counter-controlled loops; common loop patterns (accumulation, counting,
  searching).
- **Subtopics/Skills:** choosing the right loop construct for a task; avoiding off-by-one and
  infinite-loop errors.
- **Readings:** Introductory textbook ch. 5 (loops/repetition).
- **Software:** JDK 17+.
- **Assignment 1 assigned** (basics, operators, control flow, loops).

## Week 5 — Methods
- **Topics:** Method declaration vs. definition; parameters and return types; pass-by-value
  semantics for primitives vs. object references (why a method can mutate a passed array's
  contents but cannot reseat the caller's reference); method overloading; scope and lifetime of
  local variables.
- **Subtopics/Skills:** decomposing a program into small, testable static methods; distinguishing
  "changing what a reference points to" from "changing the object a reference points to."
- **Readings:** Introductory textbook ch. 6 (methods).
- **Software:** JDK 17+.

## Week 6 — Arrays (1D)
- **Topics:** Array declaration, creation (`new`), initialization, indexing; arrays are objects
  on the heap; default element values; passing arrays to methods; fixed-size array limitations;
  `ArrayIndexOutOfBoundsException`.
- **Subtopics/Skills:** iterating over an array with a loop (including `for-each`); computing
  aggregates (sum, max, min, average) over an array; passing an array to a method and having the
  method's changes to its elements visible to the caller.
- **Readings:** Introductory textbook ch. 7 (arrays).
- **Software:** JDK 17+.
- **Quiz 3 — methods & 1D arrays.**

## Week 7 — 2D Arrays and Strings
- **Topics:** 2D array declaration/indexing (array-of-arrays model); nested loops over a 2D
  array; `String` as an immutable object; common `String` methods (`length`, `charAt`,
  `substring`, `indexOf`, `equals`, `equalsIgnoreCase`, `compareTo`); `==` vs. `.equals()` for
  `String`s; `StringBuilder` for efficient concatenation.
- **Subtopics/Skills:** processing a matrix (e.g., row/column sums); building a string
  incrementally with `StringBuilder` instead of repeated `+` concatenation; explaining why
  `String` immutability makes `==` comparison unreliable for content equality.
- **Readings:** Introductory textbook ch. 7–8 (multidimensional arrays, strings).
- **Software:** JDK 17+, `java.lang.String`, `java.lang.StringBuilder`.

## Week 8 — Intro to Objects & References; Midterm Review
- **Topics:** How Java references work (a variable holds a reference to an object, not the
  object itself); `null`; the stack/heap model for objects and arrays; reference equality
  (`==`) vs. logical equality (`.equals()`); review session for Weeks 1–7.
- **Subtopics/Skills:** tracing a reference diagram by hand (which variables point to which heap
  objects); writing code that correctly checks for `null` before use; practice problems for the
  midterm.
- **Readings:** Introductory textbook ch. 9 (objects and references, intro).
- **Software:** JDK 17+.
- **Quiz 4 — 2D arrays, strings, objects/references.**

## Week 9 — Midterm Exam; Classes and Objects
- **Topics:** Midterm Exam (covers Weeks 1–8). Afterward: defining a class (fields, a
  constructor, methods) — just enough to create and use simple objects (not full OOP; inheritance
  and polymorphism are deferred to the follow-on course).
- **Readings:** Introductory textbook ch. 9–10 (classes and objects, intro).
- **Software:** JDK 17+.
- **Capstone project introduced** (proposal due Week 11).

## Week 10 — Using Existing Classes/APIs
- **Topics:** `ArrayList<T>` as a resizable alternative to arrays (`add`, `get`, `set`, `remove`,
  `size`); autoboxing and wrapper classes (`Integer`, `Double`, `Character`, `Boolean`); brief
  survey of other useful `java.util` classes; when to prefer `ArrayList` over a raw array.
- **Subtopics/Skills:** modeling a growable collection of records with `ArrayList`; converting
  between a primitive and its wrapper type; iterating an `ArrayList` with `for-each`.
- **Readings:** Introductory textbook ch. 11 (collections/`ArrayList` intro).
- **Software:** JDK 17+, `java.util.ArrayList`.
- **Assignment 2 assigned** (methods, arrays, strings, objects, classes, `ArrayList`).

## Week 11 — File I/O and Exceptions
- **Topics:** `java.io`/`java.util.Scanner` for reading files; `BufferedReader` for
  line-by-line reading; `PrintWriter`/`FileWriter` for writing; checked exceptions
  (`IOException`); `try`/`catch`/`finally`; just enough exception handling to handle I/O errors
  gracefully.
- **Subtopics/Skills:** persisting a program's data to a text file and reloading it on the next
  run; wrapping file operations in `try`/`catch` and reporting a clear error instead of crashing.
- **Readings:** Introductory textbook ch. 12 (file I/O, intro to exceptions).
- **Software:** JDK 17+, `java.io.*`, `java.util.Scanner`.
- **Quiz 5 — classes, `ArrayList` & file I/O.**
- **Capstone proposal due.**

## Week 12 — Recursion
- **Topics:** Recursive method structure (base case + recursive case); tracing recursion with a
  call-stack/trace diagram; classic examples (factorial, Fibonacci, sum of an array); recursion
  vs. iteration trade-offs; `StackOverflowError` from missing/incorrect base cases.
- **Subtopics/Skills:** writing and tracing a recursive method by hand; converting a simple
  iterative method to a recursive one and back.
- **Readings:** Introductory textbook ch. 13 (recursion).
- **Software:** JDK 17+.

## Week 13 — More on Classes: Encapsulation, Static vs. Instance
- **Topics:** Access modifiers (`private`/`public`) and encapsulation; getter/setter methods;
  `static` vs. instance fields and methods; a first simple class with full encapsulation (e.g.,
  `Rectangle`, `BankAccount`) — just enough mechanics to preview the dedicated Object-Oriented
  Programming course, not full OOP (no inheritance/polymorphism here).
- **Subtopics/Skills:** designing a class with private fields and public accessor/mutator
  methods; distinguishing a `static` field (shared by all instances) from an instance field (one
  per object); writing and using a constructor.
- **Readings:** Introductory textbook ch. 14 (encapsulation, static members).
- **Software:** JDK 17+.
- **Assignment 3 assigned** (file I/O, exceptions, recursion).
- **Quiz 6 — recursion & encapsulation.**

## Week 14 — Sorting & Searching Algorithms
- **Topics:** Bubble sort and selection sort (on arrays); linear search; binary search (on sorted
  arrays); informal comparison of algorithm efficiency (number of comparisons/swaps, intuition
  for Big-O without a formal proof).
- **Subtopics/Skills:** implementing bubble sort, selection sort, and binary search from scratch;
  tracing algorithm execution on a small example array.
- **Readings:** Introductory textbook ch. 15 (intro to algorithms/sorting-searching).
- **Software:** JDK 17+.
- **Assignment 4 assigned** (encapsulation, sorting/searching).

## Week 15 — Debugging, Testing, and Program Design/Style
- **Topics:** Reading and acting on compiler errors and runtime stack traces
  (`NullPointerException`, `ArrayIndexOutOfBoundsException`); using a debugger (breakpoints,
  stepping, watching variables); defensive programming (input validation, `assert`); a basic
  unit-testing mindset (writing small test cases/expected outputs by hand, before formal
  frameworks like JUnit).
- **Subtopics/Skills:** debugging a provided buggy program using the IDE debugger and by reading
  its stack trace; refactoring a working program for clearer style and better encapsulation.
- **Readings:** Introductory textbook ch. 16 (program style/debugging), or supplementary notes.
- **Software:** JDK 17+, IDE debugger (IntelliJ IDEA or VS Code).

## Week 16 — Capstone Presentations, Course Review
- **Topics:** Student capstone project presentations; recap of the course map (basics → control
  flow → methods → arrays/strings → objects/references → classes → collections → file I/O →
  recursion → encapsulation → algorithms); brief look ahead to the Object-Oriented Programming
  and Data Structures courses.
- **Deliverable:** Capstone project final submission + presentation.

## Week 17 — Final Exam Week
- Comprehensive final exam, weighted toward Weeks 9–16 content (per Assessment Plan).

---

## Capstone Project (introduced Week 9, proposal Week 11, final Week 16)
Students (individually or in pairs) design and build a Java console application that integrates
several course concepts end-to-end: arrays and/or `ArrayList`s for data storage, simple classes
for organizing records, methods for structure, file I/O for persistence, and (optionally)
recursion or simple sorting/searching. Example topics: a student grade-book manager, a simple
library catalog, a personal inventory tracker, or a to-do list manager with save/load — each
reading and writing its data to a file so the program's state survives between runs.
