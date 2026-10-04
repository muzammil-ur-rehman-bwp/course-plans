# Week 12 Lecture Plan — Programming Fundamentals (Java)
## Topic: Recursion

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall the two required parts of a correct recursive method: base case and recursive case. (*Remember*)
2. Explain how the call stack grows and shrinks during recursive calls. (*Understand*)
3. Apply recursion to classic problems (factorial, Fibonacci, array sum). (*Apply*)
4. Analyze the trade-offs between a recursive and an iterative solution to the same problem. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:25 | Recursive structure | Base case + recursive case; simplest example (factorial) |
| 0:25–1:00 | Tracing the call stack | Live trace diagram for `factorial(4)` |
| 1:00–1:30 | More examples | Fibonacci, sum of an array, recursively |
| 1:30–1:50 | Recursion vs. iteration | When each is clearer or more efficient |
| 1:50–2:00 | `StackOverflowError` | Demo of a missing/incorrect base case |

### Materials/Equipment
- Slides: "Recursion in Java"
- Call-stack trace diagram handout

### Formative Check (in-class)
Quick exercise: trace by hand what `factorial(3)` returns and how many stack frames are active
at the deepest point.

### Link to Lab/Assessment
Lab 12: Recursion practice (see `lab-manuals/lab-12.md`). No graded assignment this week.
