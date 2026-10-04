# Week 10 Lecture Plan — Object Oriented Programming (Java)
## Topic: Generics I — Generic Classes

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall why a pre-generics `Object`-based container needs unchecked casts. (*Remember*)
2. Explain type parameters and bounded types. (*Understand*)
3. Apply `class Box<T>`/`class Pair<T, U>` syntax to write a type-safe generic class. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:10 | Recap | Week 9's interfaces; midterm debrief |
| 0:10–0:35 | Motivation | An `Object`-based box requiring an unchecked downcast on retrieval |
| 0:35–1:05 | Generic class syntax | Live-coded `Box<T>` with a type-safe getter/setter |
| 1:05–1:30 | Two type parameters | `Pair<T, U>` with independent type parameters |
| 1:30–1:50 | Bounded types & raw types | `<T extends Number>` briefly; why a raw-type usage produces an unchecked warning |
| 1:50–2:00 | Looking ahead | Generic methods and wildcards next week |

### Materials/Equipment
- Slides: "Generics I: Generic Classes"
- Live-coding environment

### Formative Check (in-class)
Instantiate the same `Box<T>` class with `String` and with `Integer` in the same `main`, and
explain why no cast is needed on retrieval, unlike an `Object`-based version.

### Link to Lab/Assessment
Lab 10: Generic classes (see `lab-manuals/lab-10.md`).
**Quiz 6** this week — Week 9 material (see `quizzes/quiz-06.md`).
