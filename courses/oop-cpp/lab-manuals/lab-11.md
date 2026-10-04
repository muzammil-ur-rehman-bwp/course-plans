# Lab Manual 11 — Class Templates

**Duration:** 3 hours | **Prerequisite:** Week 11 lecture

## Objectives
Practice designing and implementing a generic class template with one or more type parameters.

## Setup
Create `lab11.cpp` in your working folder.

## Procedure
1. **Task A — `Stack<T>`:** write `template <typename T> class Stack` backed internally by a
   `std::vector<T>`, with `push`, `pop`, `top() const`, `empty() const`, and `size() const`.
2. **Task B — Two instantiations:** instantiate `Stack<int>` and `Stack<std::string>` in the same
   `main`, exercising all five methods on each.
3. **Task C — `Pair<T, U>`:** write `template <typename T, typename U> class Pair` with a
   constructor, `getFirst() const`, `getSecond() const`, and an `operator==`.
4. **Task D — Mini-challenge:** write a function template `template <typename T> T peekAndPop(Stack<T>&
   s)` that returns the top element of a `Stack<T>` and removes it in one call, handling (e.g. via
   an exception — a preview of next week) the empty-stack case.

## Expected Output
A single source file with four clearly labeled sections (A–D), compiling cleanly with `-Wall`,
each template correctly instantiated for at least two different type arguments.

## Submission
Submit `lab11.cpp` via the course submission system by the end of the lab session.
