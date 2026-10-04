# Lab Manual 10 — Function Templates

**Duration:** 3 hours | **Prerequisite:** Week 10 lecture

## Objectives
Practice writing and using function templates, including tracing argument deduction and implicit
constraints.

## Setup
Create `lab10.cpp` in your working folder.

## Procedure
1. **Task A — Generic `maxOf`:** write `template <typename T> T maxOf(T a, T b)`, and test it
   with `int`, `double`, and `std::string` arguments.
2. **Task B — Generic `swapValues`:** write `template <typename T> void swapValues(T& a, T& b)`
   using a temporary variable (no `std::swap`), and test it with at least two different types.
3. **Task C — Explicit instantiation:** write a call to `maxOf` where the two arguments have
   different types (e.g. `3` and `4.5`), first show (in a comment) that plain deduction fails,
   then fix it with an explicit `maxOf<double>(3, 4.5)` call.
4. **Task D — Mini-challenge:** write `template <typename T> void printAll(const std::vector<T>&
   values)` that prints every element separated by spaces, and test it with a `std::vector<int>`
   and a `std::vector<std::string>`.

## Expected Output
A single source file with four clearly labeled sections (A–D), compiling cleanly with `-Wall`,
each template instantiated correctly for at least two different types.

## Submission
Submit `lab10.cpp` via the course submission system by the end of the lab session.
