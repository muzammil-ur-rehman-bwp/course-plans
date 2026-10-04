# Week 13 Lecture Plan — Object Oriented Programming (C++)
## Topic: Introduction to the STL

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall the basic interfaces of `std::vector` and `std::map`. (*Remember*)
2. Explain what an iterator is and how it provides a uniform way to traverse any container. (*Understand*)
3. Apply `std::vector`/`std::map` and iterators to replace a hand-rolled data structure from earlier in the course. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | From `Stack<T>` to the STL | `std::vector`/`std::map` are themselves class templates, just like Week 11's `Stack<T>` |
| 0:20–0:50 | `std::vector<T>` | Live-coded: growable array operations (`push_back`, indexing, `size`) |
| 0:50–1:20 | `std::map<K, V>` | Live-coded: insertion, lookup, iteration over key-value pairs |
| 1:20–1:50 | Iterators | `begin()`/`end()`, explicit iterator loop vs. range-`for` |
| 1:50–2:00 | Formative check | — |

### Materials/Equipment
- Slides: "Introduction to the STL"
- Live-coding environment (VS Code + terminal)

### Formative Check (in-class)
What does an iterator let you do that a raw index (`v[i]`) does not, and why does `std::map` need
iterators rather than integer indexing?

### Link to Lab/Assessment
Lab 13: STL exercises (see `lab-manuals/lab-13.md`).
**Quiz 6** this week (Weeks 11–12) — see `quizzes/quiz-06.md`.
**Assignment 4 assigned this week** (see `assignments/assignment-04.md`), due start of Week 15.
