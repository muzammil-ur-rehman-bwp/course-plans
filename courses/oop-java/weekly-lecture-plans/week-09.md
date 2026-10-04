# Week 9 Lecture Plan — Object Oriented Programming (Java)
## Topic: Midterm Exam; Interfaces

**Duration:** 2 hours lecture (midterm + lecture) + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall Weeks 1–8 material under exam conditions. (*Remember, Understand, Apply*)
2. Explain how interfaces let a class gain behavior from more than one source without multiple class inheritance. (*Understand*)
3. Apply the `interface` keyword and `implements` to design a simple contract. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–1:00 | **Midterm Exam** | Covers Weeks 1–8 |
| 1:00–1:20 | `interface` keyword | Declaring an interface's method signatures; a class `implements` it |
| 1:20–1:40 | Multiple interface implementation | Why Java permits implementing several interfaces but extending only one class; sidestepping the diamond problem |
| 1:40–1:55 | `default` methods | Brief look at a default method's body and why it exists |
| 1:55–2:00 | Interfaces vs. abstract classes | One-slide decision guide |

### Materials/Equipment
- Midterm exam paper/online test
- Slides: "Interfaces: Contracts Without Inheritance"

### Formative Check (in-class)
Design an interface `Payable` with a `pay(double amount)` method, and have two unrelated classes
(e.g. `Employee`, `Invoice`) each `implements` it independently.

### Link to Lab/Assessment
Lab 9: Interfaces (see `lab-manuals/lab-09.md`).
**Capstone project introduced** this week — proposal due Week 11 (see
`assignments/capstone-proposal-guidelines.md`).
