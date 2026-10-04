# Week 15 Lecture Plan — Object Oriented Programming (Java)
## Topic: Debugging & Testing OOP Code

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall common OOP-specific Java bugs as a checklist. (*Remember*)
2. Analyze a multi-frame stack trace spanning a class hierarchy to find the root cause. (*Analyze*)
3. Evaluate a class's correctness using hand-rolled, JUnit-style test cases. (*Evaluate*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:10 | Recap | Week 14's design principles |
| 0:10–0:35 | Reading stack traces | Tracing a `NullPointerException`/`ClassCastException` through two or three overridden methods |
| 0:35–1:00 | OOP bug checklist | Missing `@Override`, wrong `equals` signature, `equals` without `hashCode`, raw-type warnings |
| 1:00–1:30 | Testing concepts | What a JUnit-style test method and assertion check, conceptually, without a JUnit project setup |
| 1:30–1:50 | Hand-rolled test harness | Live-coded small harness exercising a class's constructors and polymorphic behavior |
| 1:50–2:00 | Looking ahead | Capstone presentations next week |

### Materials/Equipment
- Slides: "Debugging & Testing OOP Code"
- IDE debugger

### Formative Check (in-class)
Given a stack trace from a three-level class hierarchy, identify the exact line and overridden
method where the exception originated, as opposed to where it was printed.

### Link to Lab/Assessment
Lab 15: Debugging & testing (see `lab-manuals/lab-15.md`).
**Capstone implementation due for final review before Week 16 presentations.**
