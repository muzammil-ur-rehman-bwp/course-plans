# Week 5 Lecture Plan — Object Oriented Programming (Java)
## Topic: Inheritance I — `extends`, Access Control, Constructor Chaining

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall `protected` as a middle ground between `private` and `public`. (*Remember*)
2. Explain how a subclass constructor must invoke a superclass constructor. (*Understand*)
3. Apply `extends` and `super(...)` to design a correct two-level class hierarchy. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:10 | Recap | Week 4's static members; quiz debrief |
| 0:10–0:35 | `extends` and what is inherited | Fields and methods a subclass receives from its superclass |
| 0:35–1:00 | Access control across inheritance | `public`/`protected`/`private` revisited from the subclass's point of view |
| 1:00–1:30 | Constructor chaining with `super(...)` | Live-coded `Animal` → `Dog` hierarchy; explicit vs. implicit superclass constructor calls |
| 1:30–1:50 | Single inheritance in Java | Why `extends` takes exactly one superclass; teaser for interfaces (Week 9) as Java's answer to needing more than one |
| 1:50–2:00 | Looking ahead | Overriding vs. overloading, `@Override`, and `Object` next week |

### Materials/Equipment
- Slides: "Inheritance I: Extends and Super"
- Live-coding environment

### Formative Check (in-class)
Given a `Vehicle` class with a non-default constructor, write a `Car` subclass whose constructor
correctly chains to it via `super(args)`, and explain what error occurs if `super(args)` is
omitted and `Vehicle` has no no-argument constructor.

### Link to Lab/Assessment
Lab 5: Inheritance I (see `lab-manuals/lab-05.md`).
**Quiz 3** this week — Week 4 material (see `quizzes/quiz-03.md`).
