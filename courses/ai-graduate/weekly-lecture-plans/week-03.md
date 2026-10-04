# Week 3 Lecture Plan — Artificial Intelligence (Graduate)
## Topic: Game Theory & Adversarial Search I

**Duration:** 2 hours lecture + 3 hour lab/seminar

### Learning Objectives (Bloom's Level)
1. Restate minimax and alpha-beta pruning, and prove by induction that alpha-beta returns the
   same value as minimax. (*Apply, Analyze*)
2. Explain why minimax assumes perfect information and what changes in imperfect-information
   games (brief). (*Understand*)
3. Define a normal-form game and a Nash equilibrium precisely, and compute pure-strategy Nash
   equilibria of small 2x2 games by hand. (*Apply, Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | Minimax/alpha-beta refresher from the undergraduate course |
| 0:15–0:50 | Alpha-beta correctness | Board-worked induction proof that alpha-beta == minimax |
| 0:50–1:05 | Imperfect information | Brief discussion: why minimax's perfect-information assumption breaks, and a pointer to more advanced treatments |
| 1:05–1:45 | Normal-form games & Nash equilibrium | Payoff matrices, dominant strategies, formal Nash equilibrium definition, worked 2x2 examples |
| 1:45–2:00 | Synthesis | Connecting adversarial search (two-player zero-sum) to general game theory |

### Materials/Equipment
- Slides: "Game Theory Foundations"
- Whiteboard for the alpha-beta correctness proof and Nash-equilibrium worked examples
- Live-coding environment (Jupyter)

### Formative Check (in-class)
Given a 2x2 payoff matrix (e.g., a Prisoner's Dilemma matrix), identify any dominant strategies
and verify which strategy profile(s) are Nash equilibria.

### Link to Lab/Assessment
Lab 3: implement alpha-beta pruning with node-count instrumentation, and compute Nash equilibria
for small games (see `lab-manuals/lab-03.md`).
