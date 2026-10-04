# Lab Manual 9 — Abstract Classes

**Duration:** 3 hours (lighter session given the midterm earlier in the week) | **Prerequisite:** Week 9 lecture

## Objectives
Practice designing an abstract base class with pure virtual functions, and deriving concrete
classes from it.

## Setup
Create `lab09.cpp` in your working folder.

## Procedure
1. **Task A — Abstract base:** write `class Shape` with a pure virtual `double area() const = 0;`
   and a `virtual ~Shape() = default;`. Confirm (in a comment) that `Shape s;` fails to compile.
2. **Task B — Concrete classes:** derive `class Circle` and `class Rectangle` from `Shape`,
   each overriding `area()` correctly, each with its own constructor.
3. **Task C — Polymorphic use:** build a `std::vector<Shape*>` containing a `Circle` and a
   `Rectangle` (allocated with `new`, for now — Week 15 will revisit this with smart pointers),
   and print each one's `area()` through the loop; remember to `delete` each pointer afterward.
4. **Task D — Mini-challenge:** add a non-pure `virtual void describe() const` to `Shape` with a
   default implementation that calls `area()` (demonstrating that a default method can still use
   a pure virtual one polymorphically); optionally override it in one derived class.

## Expected Output
A single source file with four clearly labeled sections (A–D), compiling cleanly with `-Wall`,
correctly dispatching `area()` (and `describe()`) through `Shape*` for each concrete type.

## Submission
Submit `lab09.cpp` via the course submission system by the end of the lab session.
