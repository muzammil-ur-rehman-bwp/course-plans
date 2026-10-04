# Week 1 Lecture Plan — Object Oriented Programming (Java)
## Topic: Classes Recap & Encapsulation

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall `class`, field, and method syntax from the prerequisite course. (*Remember*)
2. Explain why `public` fields are a design smell and how encapsulation fixes it. (*Understand*)
3. Apply `private` fields, getters/setters, and `this` to redesign a poorly-encapsulated class. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Welcome back & course roadmap | From CS1's "preview of classes" to the full OOP toolkit this course builds |
| 0:15–0:40 | Encapsulation recap | `private` fields vs. `public` interface; why direct field access is a liability |
| 0:40–1:05 | Getters/setters & `this` | Writing accessor/mutator methods; using `this` to disambiguate a field from a same-named parameter |
| 1:05–1:30 | Immutability basics | Live-coded demo: a class with only `final` fields and no setters, set once in the constructor |
| 1:30–1:50 | Refactoring exercise | Live-coding: take a class with public fields and rebuild it with private fields + validated accessors |
| 1:50–2:00 | Looking ahead | One-slide roadmap of the semester (constructors → `equals`/`hashCode` → composition → static members → inheritance → polymorphism → interfaces → generics → exceptions → collections → design → testing) |

### Materials/Equipment
- Slides: "Encapsulation Revisited"
- Live-coding environment (IntelliJ IDEA or VS Code + terminal)

### Formative Check (in-class)
Given a class with all-public fields, identify which should become `private`, write a getter and a
validated setter for each, and justify in one sentence which (if any) field should be immutable
(no setter at all).

### Link to Lab/Assessment
Lab 1: Encapsulation exercises (see `lab-manuals/lab-01.md`).
**Quiz 1** next week (Week 1 material) — see `quizzes/quiz-01.md`.
