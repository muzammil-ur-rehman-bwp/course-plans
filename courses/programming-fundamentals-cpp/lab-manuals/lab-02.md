# Lab Manual 2 — Operators, Expressions, and Type Conversion

**Duration:** 3 hours | **Prerequisite:** Week 2 lecture

## Objectives
Practice arithmetic/relational/logical operators, operator precedence, and explicit type
conversion.

## Setup
Create `lab02.cpp` in your working folder.

## Procedure
1. **Task A — Arithmetic:** read two integers and print their sum, difference, product,
   quotient, and remainder, each labeled.
2. **Task B — Integer vs. float division:** given the two integers from Task A, print their
   quotient both as plain integer division and as a `static_cast<double>` division; explain in a
   one-line comment why they differ.
3. **Task C — Logical expressions:** read three integers `a, b, c`; print whether `a` is strictly
   between `b` and `c` (in either order) using a single boolean expression.
4. **Task D — Mini-challenge:** write a program that reads a temperature and a unit character
   (`'C'` or `'F'`) and converts to the other unit, using `static_cast` where needed for any
   mixed-type arithmetic.

## Expected Output
A single source file with four clearly labeled sections (A–D), each compiling cleanly with
`-Wall` and producing correct output for at least two different test inputs per task.

## Submission
Submit `lab02.cpp` via the course submission system by the end of the lab session.
