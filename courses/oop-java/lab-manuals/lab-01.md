# Lab Manual 1 — Encapsulation

**Duration:** 3 hours | **Prerequisite:** Week 1 lecture

## Objectives
Practice auditing and designing classes with correct field access, validated getters/setters,
`this`, and immutability.

## Setup
Create `Lab01.java` (and any supporting classes) in your working folder.

## Procedure
1. **Task A — Refactor:** given a provided `class RawTemperature { public double celsius; }` with
   a public field, rewrite it as `class Temperature` with a private `celsius`, a constructor, a
   getter `getCelsius()`, and a setter `setCelsius()` that clamps any value below `-273.15` to
   `-273.15`.
2. **Task B — A second class:** design `class Rectangle` with private `width`, `height`; a
   constructor that clamps any non-positive dimension to `1`; methods `area()` and `perimeter()`;
   and setters that re-validate the same way as the constructor.
3. **Task C — Immutability:** design an immutable `class Point2D` with `final` fields `x`, `y`
   set only in the constructor, getters, and no setters at all. Explain in a comment why no setter
   exists.
4. **Task D — Mini-challenge:** write a method `static boolean isSquare(Rectangle r)` that uses
   only `Rectangle`'s public interface (no direct field access) to determine whether a rectangle
   is a square.

## Expected Output
A single project with four clearly labeled classes/sections (A–D), compiling cleanly with
`javac`, demonstrating validated construction/mutation and correct use of `this`.

## Submission
Submit your `.java` files via the course submission system by the end of the lab session.
