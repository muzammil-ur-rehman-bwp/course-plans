# Lab Manual 4 — Operator Overloading II (Streams & `friend`)

**Duration:** 3 hours | **Prerequisite:** Week 4 lecture

## Objectives
Practice implementing `operator<<`/`operator>>` as `friend` free functions.

## Setup
Create `lab04.cpp` in your working folder (reuse `Fraction` from Lab 3).

## Procedure
1. **Task A — `operator<<`:** declare `friend std::ostream& operator<<(std::ostream&, const
   Fraction&);` and implement it to print as `"num/den"`, returning the stream for chaining.
2. **Task B — `operator>>`:** declare and implement `friend std::istream& operator>>(std::istream&,
   Fraction&);`, reading the format `"num/den"` and failing the stream (`setstate`) on malformed
   input or a zero denominator.
3. **Task C — Round trip:** in `main`, print several `Fraction`s with `operator<<`, then read a
   series of fractions from `std::cin` with `operator>>` and print each one back.
4. **Task D — Mini-challenge:** without using `friend`, write `operator<<` for `Fraction` using
   only its existing public getters (`getNum()`/`getDen()` from Lab 3), and add a comment
   explaining when this no-`friend` version would **not** be possible for a different class.

## Expected Output
A single source file with four clearly labeled sections (A–D), compiling cleanly with `-Wall`,
correctly chaining multiple `<<`/`>>` calls and rejecting malformed input in Task B.

## Submission
Submit `lab04.cpp` via the course submission system by the end of the lab session.
