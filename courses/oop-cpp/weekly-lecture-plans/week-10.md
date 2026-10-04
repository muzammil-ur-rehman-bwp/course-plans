# Week 10 Lecture Plan — Object Oriented Programming (C++)
## Topic: Templates I — Function Templates

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall the `template <typename T>` syntax for a function template. (*Remember*)
2. Explain template argument deduction and why a template function only compiles for types supporting the operations it uses. (*Understand*)
3. Apply function templates to write a single generic function that replaces several near-duplicate overloads. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | Motivation | Three near-identical `max` overloads for `int`/`double`/`std::string` |
| 0:20–0:50 | `template <typename T>` syntax | Live-coded generic `max`/`swap` |
| 0:50–1:20 | Argument deduction | Tracing which `T` the compiler picks for different calls |
| 1:20–1:45 | Explicit instantiation | When deduction is ambiguous; `maxOf<double>(3, 4.5)` |
| 1:45–1:55 | Implicit constraints | Why a template only compiles for types supporting `<`, `==`, etc. |
| 1:55–2:00 | Formative check | — |

### Materials/Equipment
- Slides: "Templates I — Function Templates"
- Live-coding environment (VS Code + terminal)

### Formative Check (in-class)
Given `template <typename T> T maxOf(T a, T b) { return a > b ? a : b; }`, what requirement does
this place on any type `T` used with it, and what happens if that requirement isn't met?

### Link to Lab/Assessment
Lab 10: Function template exercises (see `lab-manuals/lab-10.md`).
