# Week 15 Lecture Plan — Object Oriented Programming (C++)
## Topic: RAII & Smart Pointers; Debugging/Testing OOP Code

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall RAII and the syntax for `std::unique_ptr`/`std::shared_ptr`. (*Remember*)
2. Explain the ownership model of `unique_ptr` vs. `shared_ptr` and why each resolves the Rule-of-Three liabilities of a raw owning pointer. (*Understand*)
3. Apply smart pointers to replace raw `new`/`delete` in an existing class, and write basic test cases for OOP code. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | RAII, revisited | Destructors (Week 2) as the general pattern; exceptions (Week 12) as another case RAII already handles |
| 0:20–0:35 | The problem with raw owning pointers | Recap: leaks, double frees, exception-unsafety |
| 0:35–1:05 | `std::unique_ptr<T>` | Live-coded: exclusive ownership, `make_unique`, move-only semantics |
| 1:05–1:30 | `std::shared_ptr<T>` | Live-coded: reference counting (conceptual), `make_shared`, when shared ownership is genuinely needed |
| 1:30–1:50 | Debugging/testing OOP code | Stepping through a constructor/destructor pair in a debugger; writing hand-written test cases for a class's public interface |
| 1:50–2:00 | Formative check | — |

### Materials/Equipment
- Slides: "RAII & Smart Pointers"
- Live-coding environment (VS Code + terminal + debugger)

### Formative Check (in-class)
Why does replacing a class's raw owning `int*` member with a `std::unique_ptr<int>` make the
compiler-generated copy constructor safe to delete (or require explicit opt-in), rather than
dangerous as it was with the raw pointer in Week 2?

### Link to Lab/Assessment
Lab 15: Smart pointer & debugging exercises (see `lab-manuals/lab-15.md`).
**Assignment 4 due this week** (see `assignments/assignment-04.md`).
