# Lab Manual 12 — Exception Handling

**Duration:** 3 hours | **Prerequisite:** Week 12 lecture

## Objectives
Practice checked/unchecked exceptions, custom exception classes, and try-with-resources.

## Setup
Create `Lab12.java` (and any supporting classes) in your working folder.

## Procedure
1. **Task A — Custom checked exception:** design `class InvalidAgeException extends Exception`
   with a message-passing constructor, and a `Person` class whose constructor throws it
   (declared with `throws`) when given a negative age.
2. **Task B — Custom unchecked exception:** design `class InvalidAmountException extends
   RuntimeException`, and a method that throws it for a negative deposit amount, with no
   `throws` declaration required.
3. **Task C — Multiple `catch` clauses:** write a `try` block constructing several `Person`
   objects with a mix of valid and invalid ages, with `catch` clauses ordered from
   `InvalidAgeException` to a general `Exception` catch-all.
4. **Task D — try-with-resources:** write a method that reads the first line of a text file using
   a `BufferedReader` inside a `try (...)` header, with no explicit `close()` call anywhere.

## Expected Output
A project with `InvalidAgeException`, `InvalidAmountException`, `Person`, and the
try-with-resources method, compiling cleanly with `javac` and handling every exception without
crashing the program.

## Submission
Submit your `.java` files via the course submission system by the end of the lab session.
