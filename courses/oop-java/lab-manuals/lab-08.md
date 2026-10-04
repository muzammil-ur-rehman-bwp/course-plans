# Lab Manual 8 — Abstract Classes

**Duration:** 3 hours | **Prerequisite:** Week 8 lecture

## Objectives
Practice designing an abstract class with abstract and concrete methods, and driving concrete
subclasses through it polymorphically.

## Setup
Create `Lab08.java` (and any supporting classes) in your working folder.

## Procedure
1. **Task A — Abstract class:** design `abstract class Shape` with an abstract method
   `double area()` and a concrete method `String describe()` that calls `area()`.
2. **Task B — Concrete subclasses:** implement `class Circle extends Shape` and
   `class Rectangle extends Shape`, each overriding `area()` correctly.
3. **Task C — Cannot instantiate:** attempt `new Shape()`, record the exact compiler error in a
   comment, then remove the attempt.
4. **Task D — Mini-challenge:** store a mix of `Circle` and `Rectangle` objects in a
   `List<Shape>` and print `describe()` for each, then compute and print the total area using a
   loop over the same list.

## Expected Output
A project with `Shape`, `Circle`, and `Rectangle` classes compiling cleanly with `javac`,
correctly computing and printing per-shape and total areas through the abstract `Shape` type.

## Submission
Submit your `.java` files via the course submission system by the end of the lab session.
