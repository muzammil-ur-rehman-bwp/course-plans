# Week 9 Lecture Plan — Advanced Knowledge Representation and Reasoning (Post Graduate)
## Topic: Midterm Exam; Formal Verification for Knowledge-Based Systems

**Duration:** 2 hours lecture + 3 hour research seminar/lab (first hour is the midterm exam)

### Learning Objectives (Bloom's Level)
1. Complete the midterm exam (Weeks 1–8). (*Remember–Analyze*)
2. State the model-checking problem M ⊨ φ precisely for a Kripke structure and LTL formula.
   (*Understand, Apply*)
3. Implement a small explicit-state LTL model checker via cycle detection. (*Apply, Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–1:00 | **Midterm Exam** | Closed-book/open-notes per instructor policy, covers Weeks 1–8 |
| 1:00–1:20 | Model checking motivation | Kripke structures; recap of LTL (graduate course) |
| 1:20–1:50 | Automata-theoretic approach | Büchi automaton for ¬φ, product construction, accepting cycles |
| 1:50–2:00 | Complexity | PSPACE-completeness in formula size; state-space bottleneck in practice |

### Materials/Equipment
- Midterm exam papers
- Slides: "Model Checking: The Automata-Theoretic Approach"

### Formative Check (in-class)
For a 4-state example system and a safety property, identify by inspection whether a reachable
accepting cycle (a property-violating execution) exists.

### Link to Lab/Assessment
Lab 9: implement an explicit-state LTL model checker and check a safety/liveness property (see
`lab-manuals/lab-09.md`).
