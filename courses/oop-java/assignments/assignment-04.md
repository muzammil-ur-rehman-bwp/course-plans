# Assignment 4 — Generics II, Exception Handling, Collections Framework (Weeks 11–13)

**Weight:** 5% of course grade (one of 4 assignments, 20% total) | **Assigned:** Week 13 | **Due:** Start of Week 15

## Instructions
Submit a single project containing all classes described below. Code must compile cleanly with
`javac` with no warnings.

## Questions
1. **(Generic methods, 15 pts)** Write `static <T extends Comparable<T>> T max(List<T> items)`
   and `static <T> boolean containsDuplicate(List<T> items)` (using `.equals()` for comparison).
2. **(Custom exceptions, 20 pts)** Write a checked `class InsufficientFundsException extends
   Exception` and an unchecked `class InvalidAmountException extends RuntimeException`. Write
   `class BankAccount` whose `withdraw(double)` throws `InsufficientFundsException` (declared
   with `throws`) for an over-large withdrawal, and whose `deposit(double)` throws
   `InvalidAmountException` for a non-positive amount.
3. **(try-with-resources, 15 pts)** Write a method that reads all lines of a provided text file
   using a `BufferedReader` inside a `try (...)` header, with no explicit `close()` call, and
   returns them as a `List<String>`.
4. **(Collections + sorting, 25 pts)** Build a `List<BankAccount>` (add a `String ownerName`
   field to `BankAccount` for this purpose) and sort it by balance descending using a lambda-based
   `Comparator`, then by balance descending with `ownerName` ascending as a tie-break using
   `Comparator.comparing(...).thenComparing(...)`.
5. **(Integration, 25 pts)** In `main`, construct several `BankAccount` objects, trigger and catch
   both exception types with appropriately ordered `catch` clauses, build and sort the
   `List<BankAccount>` as in Question 4, and print the two sorted orders.

## Submission
Upload your project via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
