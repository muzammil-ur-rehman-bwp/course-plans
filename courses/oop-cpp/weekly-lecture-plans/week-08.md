# Week 8 Lecture Plan — Object Oriented Programming (C++)
## Topic: Polymorphism I — Virtual Functions; Midterm Review

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall the `virtual` keyword and the syntax for a virtual destructor. (*Remember*)
2. Explain dynamic dispatch and why a base class used polymorphically needs a virtual destructor. (*Understand*)
3. Analyze a by-value vs. by-reference/pointer example to identify where object slicing occurs and how to prevent it. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:25 | Static vs. dynamic binding | Motivating example: a `Dog` called through an `Animal*` without `virtual` |
| 0:25–0:50 | `virtual` and dynamic dispatch | Live-coded: adding `virtual` fixes the previous example |
| 0:50–1:15 | Virtual destructors | Why a polymorphic base class needs one; demo of the leak/UB when it's missing |
| 1:15–1:40 | Object slicing | Live-coded: passing a `Dog` by value as `Animal` loses `Dog`'s data and behavior |
| 1:40–2:00 | Midterm review | Weeks 1–8 recap; practice problems |

### Materials/Equipment
- Slides: "Polymorphism I — Virtual Functions"
- Live-coding environment (VS Code + terminal)
- Midterm practice problem set

### Formative Check (in-class)
Explain, with reference to the vtable concept, why calling `delete basePtr;` on a base class
without a `virtual` destructor — when `basePtr` actually points to a derived object with its own
resources — is undefined behavior.

### Link to Lab/Assessment
Lab 8: Virtual functions & slicing exercises (see `lab-manuals/lab-08.md`).
**Midterm Exam next week** (Week 9), covering Weeks 1–8.
