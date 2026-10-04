# Assignment 3 — Abstract Classes, Function & Class Templates (Weeks 9–11)

**Weight:** 5% of course grade (one of 4 assignments, 20% total) | **Assigned:** Week 11 | **Due:** Start of Week 13

## Instructions
Submit a single source file `assignment03.cpp`. Code must compile cleanly with
`g++ -std=c++17 -Wall`. This assignment directly extends the `Employee`/`Manager`/`Executive`
hierarchy from Assignment 2 — reuse that code (adjusted as needed) rather than rebuilding it from
scratch.

## Questions
1. **(Abstract classes, 20 pts)** Convert `Employee` from Assignment 2 into an abstract base
   class by making `computeSalary()` pure virtual (`= 0`), removing any default implementation.
   Confirm, in a comment, that `Employee e;` now fails to compile, and that `Manager` and
   `Executive` remain valid, concrete, instantiable classes.
2. **(Virtual destructors through smart pointers, 15 pts)** Store the hierarchy using
   `std::vector<std::unique_ptr<Employee>>` instead of raw pointers, confirming the base
   destructor is still `virtual` (required since objects are now owned and destroyed
   polymorphically via the `unique_ptr`s' own destructors).
3. **(Function templates, 20 pts)** Write `template <typename T> T findMax(const std::vector<T>&
   values)` that returns the maximum element of any non-empty vector of a comparable type, and
   test it on `std::vector<double>` (e.g. a vector of computed salaries) and `std::vector<int>`.
4. **(Class templates, 25 pts)** Write `template <typename T> class Stack` with `push`, `pop`,
   `top() const`, and `empty() const`, backed by `std::vector<T>`. Use it to implement an
   "undo history" of the last several salary-adjustment amounts applied to an `Employee`-derived
   object (`Stack<double>`), supporting pushing a new adjustment and popping (undoing) the most
   recent one.
5. **(Integration, 20 pts)** In `main`, build the polymorphic `Employee` hierarchy via
   `unique_ptr`s, use `findMax` to report the highest `computeSalary()` result, and demonstrate
   the `Stack<double>` undo history with at least 4 pushes and 2 pops, printing the stack's state
   (via `top()`/`empty()`) after each operation.

## Submission
Upload `assignment03.cpp` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
