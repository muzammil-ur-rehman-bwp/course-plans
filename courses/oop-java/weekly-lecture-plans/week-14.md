# Week 14 Lecture Plan — Object Oriented Programming (Java)
## Topic: Software Design — UML, Composition vs. Inheritance, SOLID

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall composition and inheritance as two distinct relationships. (*Remember*)
2. Analyze a class diagram and a class design against SOLID principles. (*Analyze*)
3. Evaluate and refactor a class that violates Single Responsibility or Open/Closed. (*Evaluate*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:10 | Recap | Week 13's Collections Framework |
| 0:10–0:35 | UML class diagrams | Reading/drawing classes, attributes, methods, composition/inheritance arrows |
| 0:35–1:00 | "Is-a" vs. "has-a" revisited | Deciding between inheritance and composition for a given design, favoring composition absent a true is-a relationship |
| 1:00–1:25 | Single Responsibility Principle | Before/after refactor of a class doing "too much" |
| 1:25–1:50 | Open/Closed Principle | Replacing a chain of `instanceof` checks with polymorphism/interfaces |
| 1:50–2:00 | Looking ahead | Debugging & testing OOP code next week |

### Materials/Equipment
- Slides: "Software Design: UML & SOLID"
- Diagramming tool or pen and paper

### Formative Check (in-class)
Draw a UML class diagram for a hierarchy designed earlier in the course, and identify one SOLID
violation (if any) in a provided class, with a one-sentence fix.

### Link to Lab/Assessment
Lab 14: UML & design critique (see `lab-manuals/lab-14.md`).
