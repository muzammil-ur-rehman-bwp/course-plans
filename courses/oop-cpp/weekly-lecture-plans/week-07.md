# Week 7 Lecture Plan — Object Oriented Programming (C++)
## Topic: Inheritance II — Overriding & Multiple Inheritance

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall the `override` keyword and the syntax for overriding a base-class member function. (*Remember*)
2. Explain the difference between overriding and name hiding, and the diamond problem in multiple inheritance. (*Understand*)
3. Analyze a small multiple-inheritance diagram to identify where ambiguity would arise. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:25 | Overriding | Live-coded: redefining a base method in a derived class, without `virtual` yet |
| 0:25–0:50 | `override` keyword | Catching signature mismatches at compile time; common typo bugs it prevents |
| 0:50–1:10 | Name hiding | What happens when a derived class defines a method with the *same name* but different signature |
| 1:10–1:40 | Multiple inheritance | `class C : public A, public B`; when it's used |
| 1:40–1:55 | The diamond problem | Diagram: two bases sharing a common ancestor; ambiguity this creates |
| 1:55–2:00 | Formative check | — |

### Materials/Equipment
- Slides: "Inheritance II — Overriding & Multiple Inheritance"
- Live-coding environment (VS Code + terminal)
- Diamond-inheritance diagram handout

### Formative Check (in-class)
Draw (or describe) a diamond-inheritance diagram and explain, in one or two sentences, what
becomes ambiguous about a most-derived object's data.

### Link to Lab/Assessment
Lab 7: Overriding exercises (see `lab-manuals/lab-07.md`).
**Quiz 4** this week (Weeks 6–7) — see `quizzes/quiz-04.md`.
