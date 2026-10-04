# Lab Manual 7 — Inheritance II (Overriding & Multiple Inheritance)

**Duration:** 3 hours | **Prerequisite:** Week 7 lecture

## Objectives
Practice overriding member functions with `override`, recognizing name hiding, and exploring
multiple inheritance without a diamond.

## Setup
Create `lab07.cpp` in your working folder (reuse `Animal`/`Dog` from Lab 6).

## Procedure
1. **Task A — Overriding:** add a `virtual`-free `speak() const` to `Animal` and override it in
   `Dog`, marking the override with `override`; call it directly on a `Dog` object.
2. **Task B — Name hiding:** add an overload `greet(int times) const` to `Animal` alongside
   `greet() const`; in `Dog`, define only `greet() const` (same name, no `override` needed since
   not virtual yet) and show, in a comment, which call to `greet` becomes inaccessible on a `Dog`
   object as a result, and how to reach it with explicit qualification.
3. **Task C — Multiple inheritance:** define two unrelated classes `Flyable` and `Swimmable`
   (each with one method, no shared base), and `class Duck : public Flyable, public Swimmable`;
   call both inherited methods on a `Duck` object.
4. **Task D — Mini-challenge:** in a comment (no code required), sketch what would go wrong if
   both `Flyable` and `Swimmable` instead inherited from a shared `Animal` base, and `Duck`
   inherited from both — name the problem and one way C++ addresses it.

## Expected Output
A single source file with four clearly labeled sections (A–D), compiling cleanly with `-Wall`,
Task B's and Task D's comments correctly describing the behavior/problem.

## Submission
Submit `lab07.cpp` via the course submission system by the end of the lab session.
