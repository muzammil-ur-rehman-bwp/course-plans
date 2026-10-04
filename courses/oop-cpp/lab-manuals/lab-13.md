# Lab Manual 13 — Introduction to the STL

**Duration:** 3 hours | **Prerequisite:** Week 13 lecture

## Objectives
Practice using `std::vector`, `std::map`, and iterators (explicit and range-`for`).

## Setup
Create `lab13.cpp` in your working folder.

## Procedure
1. **Task A — `std::vector`:** read a list of integers into a `std::vector<int>` (using
   `push_back` in a loop, no fixed-size array), then compute and print their sum and average.
2. **Task B — `std::map`:** build a `std::map<std::string, int>` tallying word frequency from a
   small hard-coded list of words (e.g. from a sentence split into tokens), using `map["word"]++`.
3. **Task C — Iterators:** print the `std::map` from Task B twice: once with an explicit
   `std::map<std::string,int>::iterator` loop, once with a range-`for` loop, and confirm both
   print in the same (sorted-by-key) order.
4. **Task D — Mini-challenge:** write a function template `template <typename T> T sumAll(const
   std::vector<T>& values)` that sums any numeric `std::vector<T>` using an iterator-based loop
   (`begin()`/`end()`, not `[]`), and test it on `std::vector<int>` and `std::vector<double>`.

## Expected Output
A single source file with four clearly labeled sections (A–D), compiling cleanly with `-Wall`,
Task C's two loops producing identical output.

## Submission
Submit `lab13.cpp` via the course submission system by the end of the lab session.
