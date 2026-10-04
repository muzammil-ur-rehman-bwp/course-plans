# Lab Manual 3 — Control Flow: `if`/`else`, `switch`

**Duration:** 3 hours | **Prerequisite:** Week 3 lecture

## Objectives
Practice translating decision logic into `if`/`else` chains and `switch` statements.

## Setup
Create `lab03.cpp` in your working folder.

## Procedure
1. **Task A — Grading scale:** read a numeric score and print the letter grade using an
   `if`/`else if`/`else` chain (A ≥ 85, B ≥ 70, C ≥ 55, F otherwise).
2. **Task B — Switch menu:** read an integer `1`–`4` representing a simple calculator operation
   choice (add/subtract/multiply/divide) and two numbers, then use a `switch` statement to print
   the result (guard against division by zero).
3. **Task C — Leap year:** read a year and determine whether it is a leap year (divisible by 4,
   but not by 100 unless also by 400), printing the result; use nested or combined conditionals.
4. **Task D — Mini-challenge:** read a month number (1–12) and print its number of days, using a
   `switch` with grouped fall-through cases for months sharing a day count (treat February as 28).

## Expected Output
A single source file with four clearly labeled sections (A–D), each compiling cleanly with
`-Wall` and producing correct output for at least two test inputs per task, including an edge
case (e.g., a boundary score, an invalid menu choice).

## Submission
Submit `lab03.cpp` via the course submission system by the end of the lab session.
