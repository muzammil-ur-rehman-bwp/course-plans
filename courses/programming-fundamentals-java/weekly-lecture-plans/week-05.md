# Week 5 Lecture Plan — Programming Fundamentals (Java)
## Topic: Methods

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall method declaration syntax: return type, name, parameters. (*Remember*)
2. Explain pass-by-value semantics for primitives vs. object/array references. (*Understand*)
3. Apply method decomposition and overloading to organize a program into small, testable pieces. (*Apply*)
4. Analyze why a method can mutate an array's contents but cannot reseat the caller's reference. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:25 | Why methods? | Decomposition motivation; declaration vs. definition |
| 0:25–1:00 | Parameters & pass-by-value | Live-coded demo: primitive parameter unaffected by method; array parameter's contents mutated |
| 1:00–1:30 | Overloading | Live-coded demo: same name, different parameter lists |
| 1:30–1:50 | Scope & lifetime | Local variable scope; shadowing pitfalls |
| 1:50–2:00 | Recap | Checklist: when to extract a method |

### Materials/Equipment
- Slides: "Methods and Pass-by-Value in Java"
- Live-coding environment; reference-diagram handout

### Formative Check (in-class)
Quick exercise: predict whether a method that reassigns a primitive parameter, vs. one that
sets `arr[0] = 99` on an array parameter, changes the caller's variable — then verify by running
the code.

### Link to Lab/Assessment
Lab 5: Methods practice (see `lab-manuals/lab-05.md`). No graded assignment this week.
