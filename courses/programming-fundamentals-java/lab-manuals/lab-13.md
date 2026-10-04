# Lab Manual 13 — Encapsulation

**Duration:** 3 hours | **Prerequisite:** Week 13 lecture

## Objectives
Practice designing a properly encapsulated class with `static` and instance members.

## Setup
Create `Lab13.java` in your working folder.

## Procedure
1. **Task A — Encapsulated class:** define a `BankAccount` class with a private `balance` field,
   a constructor validating a non-negative initial balance, and `deposit`/`withdraw` methods that
   validate their arguments (reject negative/invalid amounts, reject overdraws).
2. **Task B — Demonstrate protection:** in `main`, attempt an invalid withdrawal and a negative
   deposit, showing both are correctly rejected without changing the balance.
3. **Task C — `static` counter:** add a `private static int accountCount` field incremented in
   the constructor, and a `public static int getAccountCount()` method; create 3 accounts and
   print the total.
4. **Task D — Mini-challenge:** add a `Rectangle` class with private `width`/`height`, a
   constructor, and methods `area()` and `perimeter()`; demonstrate it with 2 test instances.

## Expected Output
A single source file with four clearly labeled sections (A–D), each compiling cleanly and
producing correct output.

## Submission
Submit `Lab13.java` via the course submission system by the end of the lab session.
