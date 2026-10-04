# Week 1 Lecture Plan — Object Oriented Programming (C++)
## Topic: Classes Recap & Encapsulation

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall access specifiers (`public`/`private`/`protected`) and `const` member function syntax from the prerequisite course. (*Remember*)
2. Explain why `protected` exists and how it differs from `private`. (*Understand*)
3. Apply getters/setters and `const`-correctness to redesign a poorly-encapsulated class. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Welcome back & course roadmap | From CS1's "preview of classes" to the full OOP toolkit this course builds |
| 0:15–0:40 | Access specifiers recap | `public`/`private`; introduce `protected` as "private to outsiders, visible to subclasses" (teaser for Week 6) |
| 0:40–1:05 | Getters/setters | Writing accessor/mutator methods; validating input inside a setter |
| 1:05–1:30 | `const` member functions | Live-coded demo: marking read-only methods `const`; what the compiler then enforces |
| 1:30–1:50 | Refactoring exercise | Live-coding: take a class with public data and rebuild it with private data + validated accessors |
| 1:50–2:00 | Looking ahead | One-slide roadmap of the semester (constructors → operators → composition → inheritance → polymorphism → templates → exceptions → STL → design → smart pointers) |

### Materials/Equipment
- Slides: "Encapsulation Revisited"
- Live-coding environment (VS Code + terminal)

### Formative Check (in-class)
Given a class with all-public data members, identify which should become `private`, which (if
any) should become `protected`, and justify each choice in one sentence.

### Link to Lab/Assessment
Lab 1: Encapsulation exercises (see `lab-manuals/lab-01.md`).
**Quiz 1** next week (Week 1 material) — see `quizzes/quiz-01.md`.
