# Week 16 — Lecture Content: Capstone Presentations & Course Review

## 1. Capstone Presentations
Each student or pair presents their capstone console application (5–7 minutes + Q&A), following
the structure in `presentations/capstone-presentation-template.md`: problem statement,
data/design (arrays/`ArrayList`/classes used), approach (which methods and techniques), a live or
recorded demo, and at least one honest limitation or next step. Grading follows
`assignments/capstone-rubric.md`.

## 2. Course Review — The Full Map
This semester moved through Java in a deliberate order, each week building directly on the last:

1. **Basics** (Wk 1): the JVM, `javac`/`java`, variables, primitive types — how a program
   compiles and runs.
2. **Operators & type conversion** (Wk 2): combining values into expressions correctly.
3. **Control flow** (Wk 3): `if`/`else`, `switch` — branching on a condition.
4. **Loops** (Wk 4): `for`/`while`/`do-while` — repeating logic.
5. **Methods** (Wk 5): decomposing programs; pass-by-value for primitives vs. references.
6. **1D arrays** (Wk 6): storing and processing collections.
7. **2D arrays & strings** (Wk 7): grids, immutable `String`s, `StringBuilder`.
8. **Objects & references** (Wk 8): `null`, the stack/heap model; midterm review.
9. **Midterm + classes** (Wk 9): fields, constructors, methods — a first user-defined type.
10. **Collections** (Wk 10): `ArrayList`, wrapper classes, autoboxing.
11. **File I/O & exceptions** (Wk 11): persisting data between runs, handling failures gracefully.
12. **Recursion** (Wk 12): solving problems in terms of themselves.
13. **Encapsulation** (Wk 13): private fields, `static` vs. instance members — a second, deeper
    look at classes.
14. **Sorting & searching** (Wk 14): bubble sort, selection sort, binary search on arrays.
15. **Debugging & style** (Wk 15): stack traces, debuggers, defensive programming, testing mindset.
16. **Capstone** (Wk 16): integrating arrays/collections, classes, and file I/O into one program.

Every later topic reused earlier ones directly: methods (Wk 5) parameterize array processing (Wk
6); classes (Wk 9, 13) combine with collections and file I/O (Wk 10–11); the capstone (Wk 16) is,
in essence, "all of the above" applied to one real problem.

## 3. Looking Ahead
- **Object-Oriented Programming**: the class basics from Weeks 9 and 13 — encapsulation,
  constructors — extend into inheritance, polymorphism, interfaces, and `abstract` classes.
- **Data Structures & Algorithms**: the arrays and informal efficiency intuition from Weeks 6–7
  and 14 extend into linked lists, trees, stacks/queues, and formal Big-O analysis.
- **Memory model**: the stack/heap/reference model from Week 8 is the same model those later
  courses build on when discussing how linked data structures (nodes connected by references) are
  actually laid out in memory.

## 4. Closing Exercise
As a class, reconstruct the dependency chain above from memory (without looking at the list) —
for each week, name one concept from an earlier week that it directly relied on.
