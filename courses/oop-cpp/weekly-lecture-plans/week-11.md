# Week 11 Lecture Plan — Object Oriented Programming (C++)
## Topic: Templates II — Class Templates

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall the `template <typename T> class` syntax and how to define member functions outside the class body. (*Remember*)
2. Explain how a class template differs from a function template in terms of instantiation. (*Understand*)
3. Apply class templates to implement a generic container (`Stack<T>`) usable with multiple element types. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | From function to class templates | Motivation: a `Stack` that works for any element type |
| 0:20–0:55 | `template <typename T> class` | Live-coded `Stack<T>` backed by `std::vector<T>` |
| 0:55–1:20 | Out-of-class member definitions | Syntax for defining a template class's methods outside the class body |
| 1:20–1:45 | Multiple type parameters | `template <typename T, typename U> class Pair` |
| 1:45–1:55 | Instantiating for multiple types | Using `Stack<int>` and `Stack<std::string>` in the same program |
| 1:55–2:00 | Formative check | — |

### Materials/Equipment
- Slides: "Templates II — Class Templates"
- Live-coding environment (VS Code + terminal)

### Formative Check (in-class)
Why does writing `Stack<int> s1; Stack<std::string> s2;` in the same program not cause any
conflict, even though both come from the same `Stack<T>` template?

### Link to Lab/Assessment
Lab 11: Class template exercises (see `lab-manuals/lab-11.md`).
**Quiz 5** this week (Weeks 9–10) — see `quizzes/quiz-05.md`.
**Assignment 3 assigned this week** (see `assignments/assignment-03.md`), due start of Week 13.
