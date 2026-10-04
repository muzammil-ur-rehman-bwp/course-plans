# Lab Manual 1 — Encapsulation

**Duration:** 3 hours | **Prerequisite:** Week 1 lecture

## Objectives
Practice auditing and designing classes with correct access specifiers, validated getters/
setters, and `const`-correct member functions.

## Setup
Create `lab01.cpp` in your working folder.

## Procedure
1. **Task A — Refactor:** given a provided `struct RawTemperature { double celsius; };` with all-
   public data, rewrite it as `class Temperature` with a private `celsius_`, a constructor, a
   `const` getter `getCelsius()`, and a setter `setCelsius()` that clamps any value below
   `-273.15` to `-273.15`.
2. **Task B — A second class:** design `class Rectangle` with private `width_`, `height_`; a
   constructor that clamps any non-positive dimension to `1`; `const` methods `area()` and
   `perimeter()`; and setters that re-validate the same way as the constructor.
3. **Task C — `protected` preview:** add a comment block (no code) explaining, for a hypothetical
   future `Square` derived from `Rectangle`, which of `Rectangle`'s members you would make
   `protected` instead of `private`, and why.
4. **Task D — Mini-challenge:** write a free function `bool isSquare(const Rectangle& r)` that
   uses only `Rectangle`'s public interface (no `friend`, no private access) to determine whether
   a rectangle is a square.

## Expected Output
A single source file with four clearly labeled sections (A–D), compiling cleanly with
`g++ -std=c++17 -Wall`, demonstrating validated construction/mutation and `const`-correct
accessors.

## Submission
Submit `lab01.cpp` via the course submission system by the end of the lab session.
