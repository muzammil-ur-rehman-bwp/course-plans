# Week 14 Lecture Plan — Object Oriented Programming (C++)
## Topic: Software Design — UML, Composition vs. Inheritance, SOLID

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall the basic notation of a UML class diagram (classes, attributes, methods, relationship arrows). (*Remember*)
2. Analyze a class design to decide whether composition or inheritance better fits the relationship. (*Analyze*)
3. Evaluate a class against the Single Responsibility and Open/Closed principles and propose a refactor. (*Evaluate*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:25 | Reading a UML class diagram | Classes, attributes, methods, composition/aggregation vs. inheritance arrows |
| 0:25–0:45 | Drawing one | Live exercise: diagram the `Shape`/`Animal` hierarchies from earlier in the course |
| 0:45–1:15 | Composition vs. inheritance, revisited | "Is-a" vs. "has-a" as a design decision, not just syntax; preferring composition absent a true is-a |
| 1:15–1:40 | Single Responsibility Principle | Critiquing a class that does "too much"; refactor demo |
| 1:40–1:55 | Open/Closed Principle | A `switch` on type vs. polymorphism (ties back to Weeks 8–9) |
| 1:55–2:00 | Formative check | — |

### Materials/Equipment
- Slides: "Software Design — UML & SOLID"
- Diagramming tool or pen-and-paper handout

### Formative Check (in-class)
Given a class that reads a file, computes a statistic, and formats a report, which SOLID
principle does it violate, and how would you split its responsibilities?

### Link to Lab/Assessment
Lab 14: UML diagramming & refactoring exercises (see `lab-manuals/lab-14.md`).
