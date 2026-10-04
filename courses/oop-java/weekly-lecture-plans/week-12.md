# Week 12 Lecture Plan — Object Oriented Programming (Java)
## Topic: Exception Handling

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall the `Throwable`/`Exception`/`Error` hierarchy. (*Remember*)
2. Explain checked vs. unchecked exceptions and try-with-resources. (*Understand*)
3. Apply custom exception classes and `try`/`catch`/`throw` to handle invalid input robustly. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:10 | Recap | Week 11's generics; capstone proposal shout-outs |
| 0:10–0:35 | Exception hierarchy | `Throwable` → `Exception`/`Error`; checked vs. unchecked |
| 0:35–1:00 | `throws` and checked exceptions | Declaring a checked exception; what the compiler then requires of callers |
| 1:00–1:25 | Custom exception classes | Extending `Exception` (checked) and `RuntimeException` (unchecked) |
| 1:25–1:50 | try-with-resources | Live-coded `AutoCloseable` resource cleanup, replacing manual `finally` |
| 1:50–2:00 | Looking ahead | The Collections Framework next week |

### Materials/Equipment
- Slides: "Exception Handling in Java"
- Live-coding environment

### Formative Check (in-class)
Write a method that validates a withdrawal amount and throws a custom checked
`InsufficientFundsException`, declared with `throws`, and a caller that catches it.

### Link to Lab/Assessment
Lab 12: Exception handling (see `lab-manuals/lab-12.md`).
