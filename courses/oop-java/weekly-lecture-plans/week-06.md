# Week 6 Lecture Plan — Object Oriented Programming (Java)
## Topic: Inheritance II — Overriding vs. Overloading, `@Override`, `Object`

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall that every class implicitly extends `Object`. (*Remember*)
2. Explain the difference between overriding and overloading a method. (*Understand*)
3. Apply the `@Override` annotation to catch signature mismatches at compile time. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:10 | Recap | Week 5's `extends`/`super(...)`; quiz debrief |
| 0:10–0:35 | Overriding vs. overloading | Side-by-side: same signature (override) vs. different parameter list (overload) |
| 0:35–1:05 | `@Override` | Live-coded demo: introduce a signature typo and watch `@Override` turn it into a compile error |
| 1:05–1:30 | `Object` as universal superclass | `toString`/`equals`/`hashCode` as inherited methods every class already has |
| 1:30–1:50 | Revisiting Week 2's bug | Reframing the `equals(MyClass)` overload bug in terms of `@Override` |
| 1:50–2:00 | Looking ahead | Polymorphism and dynamic dispatch next week |

### Materials/Equipment
- Slides: "Overriding, Overloading & Object"
- Live-coding environment

### Formative Check (in-class)
Classify five method-pair examples as "override," "overload," or "neither," and add `@Override`
to the ones that should compile as overrides.

### Link to Lab/Assessment
Lab 6: Inheritance II (see `lab-manuals/lab-06.md`).
**Quiz 4** this week — Week 5 material (see `quizzes/quiz-04.md`).
**Assignment 2 assigned** this week (Weeks 4–7), due start of Week 10.
