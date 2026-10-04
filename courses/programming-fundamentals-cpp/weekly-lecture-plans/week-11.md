# Week 11 Lecture Plan — Programming Fundamentals (C++)
## Topic: File I/O

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall `ifstream`/`ofstream` and how to open, check, and close a file. (*Remember*)
2. Explain the difference between `getline` and `>>` for reading input. (*Understand*)
3. Write a program that persists an array of structs to a file and reloads it. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:25 | Why file I/O? | Motivation: data must survive after the program ends |
| 0:25–0:55 | Writing files | Live-coded demo: `ofstream`, formatted output |
| 0:55–1:30 | Reading files | Live-coded demo: `ifstream`, `getline` vs. `>>`, checking `is_open()` |
| 1:30–1:55 | Structured data | Reading lines into a struct/array |
| 1:55–2:00 | Wrap-up | Error handling checklist for file operations |

### Materials/Equipment
- Slides: "Persisting Data: File I/O in C++"
- Sample data file for in-class demo

### Formative Check (in-class)
Write the code that opens a file for writing, checks it opened successfully, and explains what to
do if it did not.

### Link to Lab/Assessment
Lab 11: File I/O exercises (see `lab-manuals/lab-11.md`).
**Quiz 5** this week (structs & file I/O) — see `quizzes/quiz-05.md`.
**Capstone proposal due this week** — see `assignments/capstone-proposal-guidelines.md`.
