# Assignment 1 — Encapsulation, Constructors, `Object` Methods, Composition (Weeks 1–3)

**Weight:** 5% of course grade (one of 4 assignments, 20% total) | **Assigned:** Week 4 | **Due:** Start of Week 6

## Instructions
Submit a single project containing all classes described below, named clearly by class. Code must
compile cleanly with `javac` with no warnings.

## Questions
1. **(Encapsulation, 15 pts)** Write `class BankAccount` with a private `double balance`; a
   constructor that rejects a negative starting balance (clamp to `0`); public `deposit(double)`,
   `withdraw(double)` (returning `boolean` for success/failure), and a `getBalance()`.
2. **(Constructors & chaining, 20 pts)** Write `class Student` with private `String name`, `int
   id`, and a `List<Double> grades`. Provide a parameterized constructor (name, id; empty
   grades), and a one-argument constructor `Student(String name)` that chains to it via
   `this(name, 0)`.
3. **(`equals`/`hashCode`/`toString`, 20 pts)** Override `equals(Object obj)` (with `@Override`,
   exact `Object` parameter type), a matching `hashCode()`, and `toString()` on `Student`, based
   on `name` and `id`. In a comment, explain why Java needs no destructor for this class, unlike
   a C++ equivalent that might own a raw resource.
4. **(Composition, 20 pts)** Write `class Course` composed of a `List<Student>` (enrolled
   students) and a method `averageGradeOf(Student s)` that looks up `s`'s own average from its
   `grades` list via `Student`'s public interface only.
5. **(Integration, 25 pts)** In `main`, create at least 3 `BankAccount` objects and 2 `Student`
   objects demonstrating valid and invalid inputs; verify `equals`/`hashCode` consistency for two
   `Student`s with identical `name`/`id`; create a `Course`, enroll both students, and print each
   student's average grade via `Course`.

## Submission
Upload your project via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
