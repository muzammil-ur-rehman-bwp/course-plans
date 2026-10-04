# Week 3 Lecture Plan — Object Oriented Programming (C++)
## Topic: Operator Overloading I — Arithmetic & Comparison

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall the syntax for overloading an operator as a member function. (*Remember*)
2. Explain why overloaded arithmetic operators should return a new object rather than mutate an operand, and why `==`/`<` should be mutually consistent. (*Understand*)
3. Apply operator overloading to give a value type (e.g. `Fraction`, `Complex`) natural arithmetic and comparison syntax. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | Motivation | Why `a + b` reads better than `a.add(b)`; what C++ lets you overload |
| 0:20–0:50 | Arithmetic operators | Live-coded `operator+`, `operator-` as member functions returning a new object |
| 0:50–1:15 | Compound assignment | `operator+=` mutating `*this`, then `operator+` implemented in terms of it |
| 1:15–1:40 | Comparison operators | `operator==`, `operator<`; consistency between them |
| 1:40–1:55 | Member vs. free function | Preview: why some operators (stream insertion, Week 4) cannot be members |
| 1:55–2:00 | Formative check | — |

### Materials/Equipment
- Slides: "Operator Overloading I"
- Live-coding environment (VS Code + terminal)

### Formative Check (in-class)
Why should `operator+` typically return a *new* object by value instead of modifying and
returning `*this`?

### Link to Lab/Assessment
Lab 3: Arithmetic/comparison operator overloading exercises (see `lab-manuals/lab-03.md`).
