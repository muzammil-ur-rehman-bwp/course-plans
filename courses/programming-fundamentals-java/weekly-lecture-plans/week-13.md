# Week 13 Lecture Plan — Programming Fundamentals (Java)
## Topic: More on Classes — Encapsulation, Static vs. Instance

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall `private`/`public` access modifiers and the purpose of encapsulation. (*Remember*)
2. Explain the difference between a `static` member and an instance member. (*Understand*)
3. Apply encapsulation by designing a class with private fields and public accessor/mutator
   methods. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:25 | Encapsulation | Why public mutable fields are risky; private fields + getters/setters |
| 0:25–1:00 | Designing a class | Live-coded demo: `BankAccount` with validated deposit/withdraw |
| 1:00–1:30 | `static` vs. instance | Live-coded demo: a `static` counter shared across instances |
| 1:30–1:50 | Constructors revisited | Validating constructor arguments |
| 1:50–2:00 | Scope note | Explicitly: no inheritance/polymorphism yet — that's the OOP course |

### Materials/Equipment
- Slides: "Encapsulation and Static Members"
- Live-coding environment

### Formative Check (in-class)
Quick exercise: add a `private static int accountCount` field to a `BankAccount` class that
increments in the constructor, and print the total number of accounts created.

### Link to Lab/Assessment
Lab 13: Encapsulation practice (see `lab-manuals/lab-13.md`). **Assignment 3 assigned this week**
(see `assignments/assignment-03.md`). **Quiz 6 this week** (see `quizzes/quiz-06.md`).
