# Week 9 Lecture Plan — Knowledge Representation and Reasoning (Graduate)
## Topic: Midterm Exam; Markov Logic Networks in Depth

**Duration:** 2 hours lecture + 3 hour lab (midterm exam occupies part of the lecture slot)

### Learning Objectives (Bloom's Level)
1. Complete the Midterm Exam (Weeks 1–8). (*Remember–Apply*)
2. State the log-linear distribution an MLN induces over possible worlds, given weighted
   formulas and a finite domain. (*Understand*)
3. Ground a small MLN by hand and compute relative world probabilities on a toy domain.
   (*Apply*, *Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–1:00 | Midterm Exam | Closed-book/as-specified, covers Weeks 1–8 |
| 1:00–1:10 | Break | — |
| 1:10–1:40 | MLN formulation | Weighted formulas, grounding, the log-linear distribution P(x) = (1/Z)exp(Σw_i n_i(x)) |
| 1:40–2:00 | Worked grounding | A 2-constant, 2-formula toy MLN grounded and scored by hand |

### Materials/Equipment
- Midterm exam materials
- Slides: "Markov Logic Networks: Grounding and the Log-Linear Model"
- Live-coding environment (Jupyter)

### Formative Check (in-class)
Given a 2-formula MLN and a 2-constant domain, list every ground atom and every formula
grounding, then compute one world's unnormalized score by hand.

### Link to Lab/Assessment
Lab 9: implement a brute-force MLN evaluator over a small domain and inspect how weight changes
shift relative world probabilities (see `lab-manuals/lab-09.md`).
