# Week 8 Lecture Plan — Object Oriented Programming (Java)
## Topic: Polymorphism II — Abstract Classes; Midterm Review

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall the `abstract` keyword on a class and on a method. (*Remember*)
2. Explain why an abstract class cannot be instantiated. (*Understand*)
3. Apply an abstract class to design a polymorphic hierarchy with shared and subclass-specific behavior. (*Apply*)
4. Analyze Weeks 1–8 material in preparation for the midterm. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:10 | Recap | Week 7's dynamic dispatch |
| 0:10–0:35 | `abstract` classes & methods | Live-coded `Shape` abstract class with an abstract `area()` |
| 0:35–1:00 | Concrete subclasses | Implementing `Circle`/`Rectangle` extending `Shape` |
| 1:00–1:20 | Driving through the abstract type | Storing mixed subclasses in a `Shape[]`/`List<Shape>` and calling `area()` polymorphically |
| 1:20–1:55 | Midterm review | Practice problems spanning encapsulation → polymorphism |
| 1:55–2:00 | Logistics | Midterm exam format and coverage reminder |

### Materials/Equipment
- Slides: "Polymorphism II: Abstract Classes" + "Midterm Review"
- Practice problem set (handout)

### Formative Check (in-class)
Attempt `new Shape()` on an abstract `Shape` class and explain the resulting compile error; then
correctly instantiate a concrete subclass instead.

### Link to Lab/Assessment
Lab 8: Abstract classes (see `lab-manuals/lab-08.md`).
**Quiz 5** this week — Weeks 6–7 material (see `quizzes/quiz-05.md`).
**Midterm Exam next week** (Week 9), covering Weeks 1–8.
