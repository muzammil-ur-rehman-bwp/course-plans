# Week 5 Lecture Plan — Object Oriented Programming (C++)
## Topic: Composition ("Has-A" Relationships)

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall that a class can hold another class type as a data member. (*Remember*)
2. Explain member construction/destruction order when a class is composed of other objects. (*Understand*)
3. Apply composition to design a class whose constructor correctly initializes its member objects. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | "Has-a" vs. "is-a" | Framing composition against the inheritance that's coming next week |
| 0:20–0:50 | Objects as members | Live-coded `Car` composed of an `Engine` |
| 0:50–1:20 | Construction/destruction order | Member objects constructed in declaration order before the outer constructor body runs; destroyed in reverse |
| 1:20–1:45 | Initializing member objects | Using the member initializer list for a member that is itself a class type |
| 1:45–1:55 | Composition as the default choice | Design discussion: prefer composition unless there's a genuine is-a relationship |
| 1:55–2:00 | Formative check | — |

### Materials/Equipment
- Slides: "Composition"
- Live-coding environment (VS Code + terminal)

### Formative Check (in-class)
If class `Car` has a member `Engine engine_;`, in what order do `Car`'s and `Engine`'s
constructors run when a `Car` is created — and in what order do their destructors run when it is
destroyed?

### Link to Lab/Assessment
Lab 5: Composition exercises (see `lab-manuals/lab-05.md`).
**Quiz 3** this week (Weeks 4–5) — see `quizzes/quiz-03.md`.
