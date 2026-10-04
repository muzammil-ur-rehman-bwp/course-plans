# Week 3 Lecture Plan — Object Oriented Programming (Java)
## Topic: Composition ("Has-A" Relationships)

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall the distinction between a class's own fields and primitive/object fields. (*Remember*)
2. Explain "has-a" composition and how it differs from "is-a" inheritance. (*Understand*)
3. Apply composition to design a class built from one or more other classes. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:10 | Recap | Week 2's `equals`/`hashCode`/`toString` trio |
| 0:10–0:35 | Objects as fields | A class whose fields are themselves objects of another class |
| 0:35–1:05 | Constructing composed objects | Initializing a composed field in the constructor; passing an already-constructed object in |
| 1:05–1:30 | Delegation | Calling a composed object's methods from the outer class instead of duplicating logic |
| 1:30–1:50 | "Has-a" vs. "is-a" preview | Why `Car` has-a `Engine` but is not an `Engine`; teaser for inheritance (Week 5) |
| 1:50–2:00 | Looking ahead | Static vs. instance members next week |

### Materials/Equipment
- Slides: "Composition: Building Classes from Classes"
- Live-coding environment

### Formative Check (in-class)
Design a `Library` class composed of a `List<Book>` and a `Book` class with its own fields;
identify which class is responsible for which behavior.

### Link to Lab/Assessment
Lab 3: Composition exercises (see `lab-manuals/lab-03.md`).
**Quiz 2** this week — Week 2 material (see `quizzes/quiz-02.md`).
**Assignment 1 assigned** this week (Weeks 1–3), due start of Week 6.
