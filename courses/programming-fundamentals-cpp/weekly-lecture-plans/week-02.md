# Week 2 Lecture Plan — Programming Fundamentals (C++)
## Topic: Operators, Expressions, and Type Conversion

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall C++'s arithmetic, relational, logical, and assignment operators. (*Remember*)
2. Explain operator precedence and implicit type conversion between numeric types. (*Understand*)
3. Write expressions that correctly combine mixed-type operands, using explicit casts where needed. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:25 | Arithmetic & assignment operators | Live-coded demo: `+ - * / %`, compound assignment (`+=`, etc.) |
| 0:25–0:50 | Relational & logical operators | Live-coded demo: `== != < > && \|\| !` |
| 0:50–1:15 | Operator precedence | Worked examples; parenthesization for clarity |
| 1:15–1:40 | Type conversion | Implicit conversion pitfalls (int division); `static_cast<T>` |
| 1:40–2:00 | Practice problems | Paired exercise predicting expression results |

### Materials/Equipment
- Slides: "Operators & Type Conversion in C++"
- Live-coding environment (VS Code + terminal)

### Formative Check (in-class)
Predict the output of `std::cout << 7 / 2 << " " << 7.0 / 2 << " " << 7 % 2;` before running it.

### Link to Lab/Assessment
Lab 2: Operators and expressions exercises (see `lab-manuals/lab-02.md`).
