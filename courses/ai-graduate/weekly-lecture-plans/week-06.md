# Week 6 Lecture Plan — Artificial Intelligence (Graduate)
## Topic: Automated Reasoning — SAT and SMT

**Duration:** 2 hours lecture + 3 hour lab/seminar

### Learning Objectives (Bloom's Level)
1. Implement the DPLL algorithm with unit propagation from scratch in Python. (*Apply*)
2. Trace DPLL by hand on a small CNF formula, including a backtracking step. (*Analyze*)
3. Explain, conceptually, how clause learning and SMT solving extend/generalize basic DPLL.
   (*Understand*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | CNF and propositional logic review |
| 0:15–0:55 | DPLL algorithm | Board-worked algorithm: unit propagation, pure-literal elimination, branching, backtracking |
| 0:55–1:20 | Hand trace | Worked example: DPLL solving a small unsatisfiable and a small satisfiable formula |
| 1:20–1:45 | Clause learning (conceptual) & SMT | How CDCL learns from conflicts; SAT + theories = SMT |
| 1:45–2:00 | Synthesis | Where SAT/SMT solving is used in real systems (verification, scheduling) |

### Materials/Equipment
- Slides: "DPLL, Clause Learning, and SMT"
- Whiteboard for the hand-traced DPLL examples
- Live-coding environment (Jupyter)

### Formative Check (in-class)
Given a 4-clause CNF formula, identify which unit clauses trigger propagation first and predict
whether DPLL will find it satisfiable or not before running the algorithm.

### Link to Lab/Assessment
Lab 6: implement DPLL with unit propagation for small CNF instances (see `lab-manuals/lab-06.md`).
