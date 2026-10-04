# Week 16 — Lecture Content: Capstone Presentations & Course Review

## 1. Capstone Presentations
Each student or pair presents their capstone console application (5–7 minutes + Q&A), following
the structure in `presentations/capstone-presentation-template.md`: problem statement, data/
design (arrays/structs used), approach (which functions and techniques), a live or recorded demo,
and at least one honest limitation or next step. Grading follows
`assignments/capstone-rubric.md`.

## 2. Course Review — The Full Map
This semester moved through C++ in a deliberate order, each week building directly on the last:

1. **Basics** (Wk 1): compiling, variables, primitive types — how a program becomes an executable.
2. **Operators & type conversion** (Wk 2): combining values into expressions correctly.
3. **Control flow** (Wk 3): `if`/`else`, `switch` — branching on a condition.
4. **Loops** (Wk 4): `for`/`while`/`do-while` — repeating logic.
5. **Functions** (Wk 5): decomposing programs; value vs. reference parameters.
6. **1D arrays** (Wk 6): storing and processing collections.
7. **2D arrays & strings** (Wk 7): grids and text.
8. **Pointers & references** (Wk 8): naming memory directly; midterm review.
9. **Midterm + dynamic memory** (Wk 9): `new`/`delete`, the stack vs. the heap.
10. **Structs** (Wk 10): grouping related fields into one record.
11. **File I/O** (Wk 11): persisting data between runs.
12. **Recursion** (Wk 12): solving problems in terms of themselves.
13. **Intro to classes** (Wk 13): encapsulation, constructors — a first step toward OOP.
14. **Sorting & searching** (Wk 14): bubble sort, selection sort, binary search on arrays.
15. **Debugging & style** (Wk 15): warnings, debuggers, const-correctness, testing mindset.
16. **Capstone** (Wk 16): integrating arrays, structs, functions, and file I/O into one program.

Every later topic reused earlier ones directly: functions (Wk 5) parameterize array processing
(Wk 6); structs (Wk 10) combine with arrays and file I/O (Wk 11); the capstone (Wk 16) is, in
essence, "all of the above" applied to one real problem.

## 3. Looking Ahead
- **Object-Oriented Programming**: the `class` basics from Week 13 — encapsulation, constructors
  — extend into inheritance, polymorphism, and operator overloading.
- **Data Structures & Algorithms**: the arrays and informal efficiency intuition from Weeks 6–7
  and 14 extend into linked lists, trees, stacks/queues, and formal Big-O analysis.
- **Memory discipline**: the manual `new`/`delete` habits from Week 9 motivate why later courses
  introduce RAII and smart pointers (`std::unique_ptr`, `std::shared_ptr`) as safer alternatives.

## 4. Closing Exercise
As a class, reconstruct the dependency chain above from memory (without looking at the list) —
for each week, name one concept from an earlier week that it directly relied on.
