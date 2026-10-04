# Lab Manual 15 — Smart Pointers & Debugging

**Duration:** 3 hours | **Prerequisite:** Week 15 lecture

## Objectives
Practice replacing raw owning pointers with smart pointers, and writing basic test cases for OOP
code.

## Setup
Create `lab15.cpp` in your working folder.

## Procedure
1. **Task A — `unique_ptr`:** take the `IntBuffer` class from Lab 2 (raw owning `int*`, manual
   destructor/copy constructor) and rewrite it to use `std::unique_ptr<int[]>` via
   `std::make_unique`, deleting the now-unnecessary destructor and copy constructor.
2. **Task B — `shared_ptr`:** write a small `class Texture` and a `class Sprite` that holds a
   `std::shared_ptr<Texture>`; create two `Sprite`s sharing the same `Texture`, print
   `use_count()` after each is created and after each is destroyed (in its own scope), and
   confirm the count behaves as expected.
3. **Task C — Choosing correctly:** for each of the following, state (in a comment) whether
   `unique_ptr`, `shared_ptr`, or a non-owning raw pointer is the right choice, and why: (1) a
   `Car`'s own `Engine`; (2) a `Scene`'s list of `Texture`s also referenced by multiple `Sprite`s;
   (3) a function parameter that only inspects an object it does not own.
4. **Task D — Testing:** write three short `assert`-based test functions exercising a class from
   any earlier week (e.g. `Stack<T>` from Lab 11): one normal-use case, one edge case, and one
   that specifically exercises polymorphic/virtual behavior through a base-class pointer if
   applicable to your chosen class.

## Expected Output
A single source file with four clearly labeled sections (A–D), compiling cleanly with `-Wall`,
with no raw `new`/`delete` remaining in Tasks A–B, and all test assertions passing.

## Submission
Submit `lab15.cpp` via the course submission system by the end of the lab session.
