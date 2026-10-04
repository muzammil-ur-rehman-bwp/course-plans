# Week 12 Lecture Plan — Artificial Intelligence (Graduate)
## Topic: Computational Complexity of AI Problems

**Duration:** 2 hours lecture + 3 hour lab/seminar

### Learning Objectives (Bloom's Level)
1. State precisely what NP-complete and PSPACE-complete mean, and the relationship NP ⊆ PSPACE.
   (*Understand*)
2. Classify SAT, CSP, and classical planning by complexity class and explain why each
   classification matters for algorithm design. (*Analyze*)
3. Empirically demonstrate runtime scaling of a complete solver on hard vs. easy instances.
   (*Analyze, Evaluate*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:30 | NP-completeness revisited | Cook-Levin theorem (stated); SAT and CSP as NP-complete |
| 0:30–0:55 | PSPACE-completeness revisited | Planning's PSPACE-completeness (Week 7 recap); NP ⊆ PSPACE |
| 0:55–1:25 | Why AI isn't hopeless | Heuristics, approximation, structure exploitation, average-case behavior |
| 1:25–1:50 | Empirical demonstration | Live-coded runtime-vs-size experiment on hard vs. easy SAT/CSP instances |
| 1:50–2:00 | Synthesis | Bridge to Week 13: why these results matter for how we evaluate AI systems |

### Materials/Equipment
- Slides: "Computational Complexity of AI: A Unified View"
- Live-coding environment (Jupyter) for the runtime-scaling demonstration

### Formative Check (in-class)
Given three problem descriptions, correctly classify each as NP-complete, PSPACE-complete, or
tractable (polynomial), with a one-sentence justification.

### Link to Lab/Assessment
Lab 12: run and plot a runtime-vs-problem-size experiment for a complete SAT or CSP solver
(see `lab-manuals/lab-12.md`).
