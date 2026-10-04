# Week 5 Lecture Plan — Programming Fundamentals (C++)
## Topic: Functions

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall the syntax for declaring and defining a function with parameters and a return type. (*Remember*)
2. Explain the difference between pass-by-value and pass-by-reference parameters. (*Understand*)
3. Decompose a small program into functions, including an overloaded function family. (*Apply, Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:25 | Why functions? | Decomposition motivation; function declaration vs. definition |
| 0:25–0:55 | Parameters & return values | Live-coded demo: pass-by-value |
| 0:55–1:25 | Pass-by-reference | Live-coded demo: a function that swaps two variables |
| 1:25–1:45 | Default arguments & overloading | Live-coded demo: overloaded `area()` for different shapes |
| 1:45–2:00 | Scope & lifetime | Local variables, shadowing |

### Materials/Equipment
- Slides: "Functions: Decomposing a Program"
- Live-coding environment (VS Code + terminal)

### Formative Check (in-class)
Write a `swap(int& a, int& b)` function by reference; explain why a pass-by-value version cannot
swap the caller's variables.

### Link to Lab/Assessment
Lab 5: Function design exercises (see `lab-manuals/lab-05.md`).
