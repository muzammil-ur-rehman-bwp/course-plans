# Week 11 Lecture Plan — Programming Fundamentals (Java)
## Topic: File I/O and Exceptions

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall the classes used to read (`Scanner`, `BufferedReader`) and write (`PrintWriter`,
   `FileWriter`) text files. (*Remember*)
2. Explain checked exceptions and the purpose of `try`/`catch`/`finally`. (*Understand*)
3. Apply file I/O and exception handling to persist and reload a program's data. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:25 | Why exceptions? | Checked vs. unchecked; `try`/`catch`/`finally` syntax |
| 0:25–0:55 | Reading files | Live-coded demo: `Scanner` on a `File`, `BufferedReader.readLine()` |
| 0:55–1:30 | Writing files | Live-coded demo: `PrintWriter`/`FileWriter` |
| 1:30–2:00 | Round trip | Saving structured data and reloading it on next run |

### Materials/Equipment
- Slides: "File I/O and Exceptions in Java"
- Sample data file; live-coding environment

### Formative Check (in-class)
Quick exercise: write a `try`/`catch` block that attempts to open a nonexistent file and prints
a friendly error message instead of letting the program crash.

### Link to Lab/Assessment
Lab 11: File I/O practice (see `lab-manuals/lab-11.md`). **Quiz 5 this week** (see
`quizzes/quiz-05.md`). **Capstone proposal due** (see
`assignments/capstone-proposal-guidelines.md`).
