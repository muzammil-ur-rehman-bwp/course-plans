# Lab Manual 12 — Exception Handling

**Duration:** 3 hours | **Prerequisite:** Week 12 lecture

## Objectives
Practice `try`/`catch`/`throw`, the standard exception hierarchy, and writing a custom exception
class.

## Setup
Create `lab12.cpp` in your working folder.

## Procedure
1. **Task A — Throwing standard exceptions:** write `void setAge(int age)` that throws
   `std::invalid_argument` for `age < 0 || age > 150`, and `int getElement(const
   std::vector<int>& v, std::size_t i)` that throws `std::out_of_range` for an invalid index.
2. **Task B — `try`/`catch`:** call both functions from `main` inside `try` blocks with both
   valid and invalid arguments, catching by reference and printing `e.what()`.
3. **Task C — Custom exception:** write `class InsufficientFundsError : public
   std::runtime_error` with a `getShortfall() const` method, and a `class Account` whose
   `withdraw` throws it when the requested amount exceeds the balance.
4. **Task D — Mini-challenge:** write a `try` block with **three** ordered `catch` clauses (most
   specific to least specific: `InsufficientFundsError`, then `std::exception`, then `catch
   (...)`), and trigger each one with a different call in turn.

## Expected Output
A single source file with four clearly labeled sections (A–D), compiling cleanly with `-Wall`,
correctly catching and reporting each exception type by reference.

## Submission
Submit `lab12.cpp` via the course submission system by the end of the lab session.
