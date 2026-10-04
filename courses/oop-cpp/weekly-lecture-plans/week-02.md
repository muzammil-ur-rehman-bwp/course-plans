# Week 2 Lecture Plan — Object Oriented Programming (C++)
## Topic: Constructors in Depth; Destructors

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall the syntax of default, parameterized, and copy constructors, and of a destructor. (*Remember*)
2. Explain why a member initializer list is not equivalent to assignment inside the constructor body. (*Understand*)
3. Apply the Rule of Three checklist to decide whether a resource-owning class needs a user-defined destructor, copy constructor, and copy-assignment operator. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | Constructors recap+extension | Default vs. parameterized constructors; constructor overloading |
| 0:20–0:45 | Member initializer lists | Syntax, member construction order, why `const`/reference members *require* one |
| 0:45–1:10 | Copy constructors | Signature `T(const T&)`; when the compiler invokes one implicitly (pass-by-value, return-by-value) |
| 1:10–1:35 | Destructors | Syntax `~T()`; when they run; releasing an owned resource |
| 1:35–1:50 | Rule of Three | Live demo: a class that owns a raw pointer, copied with the compiler-generated (shallow) copy constructor — the double-free/dangling-pointer bug this causes |
| 1:50–2:00 | Formative check | — |

### Materials/Equipment
- Slides: "Construction and Destruction"
- Live-coding environment (VS Code + terminal)

### Formative Check (in-class)
In one or two sentences, explain why writing a destructor that `delete`s an owned pointer, without
also writing a copy constructor, is dangerous — what goes wrong when such an object is copied?

### Link to Lab/Assessment
Lab 2: Constructors/destructors exercises (see `lab-manuals/lab-02.md`).
**Quiz 1** this week (Week 1 material) — see `quizzes/quiz-01.md`.
