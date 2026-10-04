# Week 6 Lecture Plan — Advanced Artificial Intelligence (Post Graduate)
## Topic: Algorithmic Game Theory I — Equilibrium Computation

**Duration:** 2 hours lecture + 3 hour research seminar/lab

### Learning Objectives (Bloom's Level)
1. Explain why zero-sum two-player Nash equilibria are polynomial-time computable via linear
   programming. (*Understand*)
2. State the PPAD-completeness result for general Nash-equilibrium computation and explain what
   kind of hardness claim it is. (*Analyze*)
3. Implement support enumeration for small bimatrix games and compute a correlated equilibrium.
   (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap + motivation | From "what is a Nash equilibrium" (graduate course) to "how hard is it to compute one" |
| 0:15–0:45 | Zero-sum games | Minimax theorem; linear-programming formulation; polynomial-time solvability |
| 0:45–1:25 | PPAD-completeness | What PPAD is, why Nash's existence proof (Brouwer fixed point) mirrors PPAD's defining structure, and what "complete" means here |
| 1:25–1:50 | Correlated equilibria | Definition, linear-programming computability, contrast with Nash |
| 1:50–2:00 | Synthesis | Recap table: solution concept vs. computational complexity |

### Materials/Equipment
- Slides: "Algorithmic Game Theory I: Computing Equilibria"
- Whiteboard for the PPAD discussion
- Live-coding environment (Jupyter)

### Formative Check (in-class)
Explain why "PPAD-complete" is a meaningfully different kind of claim from "NP-complete," and why
neither is currently known to be solvable in polynomial time.

### Link to Lab/Assessment
Lab 6: implement support enumeration and compute a correlated equilibrium (see
`lab-manuals/lab-06.md`).
