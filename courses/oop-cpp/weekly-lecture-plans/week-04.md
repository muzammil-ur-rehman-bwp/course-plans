# Week 4 Lecture Plan — Object Oriented Programming (C++)
## Topic: Operator Overloading II — Stream Operators & `friend`

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall the `friend` keyword and the signature of `operator<<`/`operator>>` for a user-defined type. (*Remember*)
2. Explain why `operator<<` cannot be an ordinary member function of the class being printed. (*Understand*)
3. Apply `friend` functions to implement a correct, chainable `operator<<`/`operator>>` pair. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | The problem | Why `a << std::cout` (member-function order) is backwards from what we want |
| 0:20–0:45 | `friend` | What access `friend` grants; why it is used sparingly |
| 0:45–1:20 | `operator<<` | Live-coded `friend std::ostream& operator<<(std::ostream&, const T&)` |
| 1:20–1:45 | `operator>>` | Live-coded input-reading counterpart |
| 1:45–1:55 | When to use `friend` vs. public accessors | Design discussion |
| 1:55–2:00 | Formative check | — |

### Materials/Equipment
- Slides: "Operator Overloading II — friend & Streams"
- Live-coding environment (VS Code + terminal)

### Formative Check (in-class)
Why must `operator<<` for printing a `Fraction` take `std::ostream&` as its *first* parameter
rather than being a member function of `Fraction`?

### Link to Lab/Assessment
Lab 4: Stream operator overloading exercises (see `lab-manuals/lab-04.md`).
**Quiz 2** this week (Weeks 2–3) — see `quizzes/quiz-02.md`.
**Assignment 1 assigned this week** (see `assignments/assignment-01.md`), due start of Week 6.
