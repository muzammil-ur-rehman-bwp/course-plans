# Week 15 Lecture Plan — Programming Fundamentals (C++)
## Topic: Debugging, Testing, and Program Design/Style

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall the meaning of common compiler warnings and what const-correctness means. (*Remember*)
2. Explain how a debugger (breakpoints, stepping, watching variables) helps isolate a bug faster than `print` statements alone. (*Understand*)
3. Debug a provided buggy program and refactor a working program for const-correctness and clearer style. (*Analyze, Evaluate*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:25 | Compiler warnings | Live demo: `-Wall -Wextra` catching real bugs |
| 0:25–0:55 | Using a debugger | Live demo: breakpoints, stepping, watching variables |
| 0:55–1:20 | Const-correctness | Why `const` parameters/variables catch bugs at compile time |
| 1:20–1:45 | Defensive programming & testing mindset | Input validation; writing small test cases by hand |
| 1:45–2:00 | Capstone check-in | Open lab time to debug/refine capstone progress |

### Materials/Equipment
- Slides: "Debugging and Program Style"
- A provided buggy program for the in-class/lab debugging exercise
- Debugger (gdb or IDE-integrated)

### Formative Check (in-class)
Given a compiler warning output, identify which line it refers to and what change fixes it.

### Link to Lab/Assessment
Lab 15: Debugging exercise + capstone work session (see `lab-manuals/lab-15.md`). This is the
final lab of the semester; Week 16 is capstone presentations with no new lab.
