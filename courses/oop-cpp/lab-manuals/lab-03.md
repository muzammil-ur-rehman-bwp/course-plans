# Lab Manual 3 — Operator Overloading I

**Duration:** 3 hours | **Prerequisite:** Week 3 lecture

## Objectives
Practice overloading arithmetic and comparison operators as member functions.

## Setup
Create `lab03.cpp` in your working folder.

## Procedure
1. **Task A — Arithmetic:** write `class Fraction` with private `num_, den_`; overload
   `operator+`, `operator-` as `const` member functions returning a new `Fraction`.
2. **Task B — Compound assignment:** add `operator+=` (mutating `*this`, returning `Fraction&`),
   then reimplement `operator+` to copy `*this` and delegate to `operator+=`.
3. **Task C — Comparison:** add `operator==` and `operator<`, cross-multiplying to compare
   fractions without requiring reduced form; verify they are mutually consistent on a few test
   cases.
4. **Task D — Mini-challenge:** write a free function `Fraction maxFraction(const Fraction& a,
   const Fraction& b)` using only `operator<`, and test it against several fraction pairs,
   including equal fractions in different forms (e.g. `1/2` and `2/4`).

## Expected Output
A single source file with four clearly labeled sections (A–D), compiling cleanly with `-Wall`,
printing each operator's result against hand-computed expected values.

## Submission
Submit `lab03.cpp` via the course submission system by the end of the lab session.
