# Week 12 Lecture Plan — Object Oriented Programming (C++)
## Topic: Exception Handling

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall the syntax of `try`/`catch`/`throw` and the standard exception hierarchy. (*Remember*)
2. Explain stack unwinding and why exceptions should be caught by reference, not by value. (*Understand*)
3. Apply exception handling to validate input and write a custom exception class. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | Motivation | Why return codes don't scale for error handling; exceptions as an alternative |
| 0:20–0:45 | `throw`/`try`/`catch` | Live-coded minimal example; stack unwinding explained |
| 0:45–1:10 | Standard exception hierarchy | `std::exception`, `std::runtime_error`, `std::out_of_range`, `std::invalid_argument` |
| 1:10–1:35 | Custom exception classes | Deriving from `std::exception`, overriding `what()` |
| 1:35–1:55 | Catching correctly | Catch by reference (ties to slicing, Week 8); ordering `catch` clauses specific-to-general; `catch (...)` |
| 1:55–2:00 | Formative check | — |

### Materials/Equipment
- Slides: "Exception Handling"
- Live-coding environment (VS Code + terminal)

### Formative Check (in-class)
Why should a `catch` clause take its exception parameter by reference (`catch (const
std::exception& e)`) rather than by value?

### Link to Lab/Assessment
Lab 12: Exception handling exercises (see `lab-manuals/lab-12.md`).
