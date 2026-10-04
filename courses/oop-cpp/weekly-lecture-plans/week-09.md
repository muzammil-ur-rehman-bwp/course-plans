# Week 9 Lecture Plan — Object Oriented Programming (C++)
## Topic: Midterm Exam; Polymorphism II — Abstract Classes

**Duration:** 2 hours lecture (shortened by exam) + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall the syntax for a pure virtual function (`= 0`) and what makes a class abstract. (*Remember*)
2. Explain why an abstract base class cannot be instantiated, and how C++ expresses an "interface" without a dedicated keyword. (*Understand*)
3. Apply abstract base classes to design a small polymorphic hierarchy (e.g. `Shape`) driven entirely through base-class pointers. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–1:00 | **Midterm Exam** | Covers Weeks 1–8 |
| 1:00–1:20 | Pure virtual functions | `virtual void draw() const = 0;` syntax and meaning |
| 1:20–1:45 | Abstract base classes | Live-coded `Shape` hierarchy; why `Shape s;` fails to compile |
| 1:45–2:00 | Interfaces-by-convention | How C++ expresses "interface" (an all-pure-virtual abstract class) without an `interface` keyword |

### Materials/Equipment
- Midterm exam papers/online exam system
- Slides: "Polymorphism II — Abstract Classes"

### Formative Check (in-class)
Why does declaring even one pure virtual function in a class make that class impossible to
instantiate directly — what is the compiler protecting you from?

### Link to Lab/Assessment
Lab 9: Abstract classes exercises (see `lab-manuals/lab-09.md`) — lighter session given the exam.
**Capstone project introduced this week** — see `assignments/capstone-proposal-guidelines.md`
(proposal due Week 11).
