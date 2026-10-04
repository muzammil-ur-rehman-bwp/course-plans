# Lab Manual 6 — Inheritance I

**Duration:** 3 hours | **Prerequisite:** Week 6 lecture

## Objectives
Practice designing a base/derived class pair with `protected` members and correct constructor
chaining.

## Setup
Create `lab06.cpp` in your working folder.

## Procedure
1. **Task A — Base class:** write `class Animal` with a `protected std::string name_` and a
   constructor taking it; add a `public eat() const` method using `name_`.
2. **Task B — Derived class:** write `class Dog : public Animal` adding its own private
   `breed_`; its constructor must chain to `Animal`'s constructor via the initializer list, and
   add a `bark() const` method using both `name_` (inherited, `protected`) and `breed_`.
3. **Task C — Destructor order:** add a destructor to both `Animal` and `Dog` that prints a
   message; construct and destroy a `Dog` and record the printed order in a comment.
4. **Task D — Mini-challenge:** write a second derived class `Cat : public Animal` with its own
   added member, and build an array (or `std::vector`) of 3 `Dog`/`Cat` objects constructed with
   different names, calling each one's own method (`bark()`/your `Cat`-specific method).

## Expected Output
A single source file with four clearly labeled sections (A–D), compiling cleanly with `-Wall`,
with Task C's recorded destructor order matching the actual output.

## Submission
Submit `lab06.cpp` via the course submission system by the end of the lab session.
