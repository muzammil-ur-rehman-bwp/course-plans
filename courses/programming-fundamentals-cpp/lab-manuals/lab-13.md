# Lab Manual 13 — Intro to Classes

**Duration:** 3 hours | **Prerequisite:** Week 13 lecture

## Objectives
Practice designing a simple class with private data, a constructor, and public member functions.

## Setup
Create `lab13.cpp` in your working folder.

## Procedure
1. **Task A — Define a class:** define `class Rectangle` with private `width`, `height`; a
   constructor that clamps non-positive dimensions to `1`; and public `area()`/`perimeter()`
   methods.
2. **Task B — Use the class:** create 3 `Rectangle` objects (including one constructed with an
   invalid, e.g. negative, dimension) and print each one's area and perimeter.
3. **Task C — A second class:** define `class BankAccount` with a private `balance`; a
   constructor rejecting a negative starting balance; and public `deposit`, `withdraw` (returning
   `bool` for success/failure), and `getBalance() const` methods.
4. **Task D — Mini-challenge:** create an array of 3 `BankAccount` objects, perform a mix of
   valid and invalid deposits/withdrawals on each, and print the final balances plus which
   operations were rejected.

## Expected Output
A single source file with four clearly labeled sections (A–D), each compiling cleanly with
`-Wall`, demonstrating that invalid inputs are rejected/clamped rather than corrupting state.

## Submission
Submit `lab13.cpp` via the course submission system by the end of the lab session.
