# Assignment 3 — Polymorphism II, Interfaces, Generics I (Weeks 8–10)

**Weight:** 5% of course grade (one of 4 assignments, 20% total) | **Assigned:** Week 11 | **Due:** Start of Week 13

## Instructions
Submit a single project containing all classes described below. Code must compile cleanly with
`javac` with no warnings (including no unchecked-type warnings).

## Questions
1. **(Abstract classes, 20 pts)** Write `abstract class Shape` with an abstract `double area()`
   and a concrete `String describe()`. Implement at least two concrete subclasses (e.g. `Circle`,
   `Rectangle`), each correctly overriding `area()`.
2. **(Interfaces, 20 pts)** Write `interface Payable` with `double computeAmountDue()`.
   Implement it in two unrelated classes (neither extending the other), and write a method
   `static double totalDue(List<Payable> items)` that sums the amounts polymorphically.
3. **(Interfaces vs. abstract classes, 10 pts)** In a comment, explain which of `Shape` and
   `Payable` would be the wrong design choice for the other's purpose, and why (one sentence
   each).
4. **(Generics I, 25 pts)** Write a generic `class Pair<T, U>` with a constructor,
   `getFirst()`/`getSecond()`, and a method `Pair<U, T> swapped()` returning a new pair with the
   two values and type parameters swapped.
5. **(Integration, 25 pts)** In `main`, drive a `List<Shape>` holding a mix of your concrete
   subclasses to print `describe()` for each and the total area; compute `totalDue` for a
   `List<Payable>` holding instances of both your `Payable` implementers; instantiate `Pair<T,
   U>` with two different type combinations and print both the original and swapped pairs.

## Submission
Upload your project via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
