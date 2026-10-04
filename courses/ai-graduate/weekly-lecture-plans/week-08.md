# Week 8 Lecture Plan — Artificial Intelligence (Graduate)
## Topic: Markov Decision Processes I; Midterm Review

**Duration:** 2 hours lecture + 3 hour lab/seminar

### Learning Objectives (Bloom's Level)
1. Define the MDP formalism precisely (states, actions, transition model, reward, discount
   factor). (*Understand*)
2. Derive and apply the Bellman equation, and implement value iteration. (*Apply, Analyze*)
3. Explain why value iteration converges (the Bellman backup as a contraction) and diagnose
   non-convergence from a misconfigured discount factor. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | MDP formalism | States, actions, P(s'\|s,a), R(s,a,s'), discount factor γ |
| 0:20–0:50 | Bellman equation | Derivation of the optimal value function's recursive definition |
| 0:50–1:20 | Value iteration | Algorithm, convergence argument (contraction mapping, conceptual) |
| 1:20–1:50 | Midterm review | Structured review of Weeks 1–7 key results and proof sketches |
| 1:50–2:00 | Q&A | Open floor for midterm questions |

### Materials/Equipment
- Slides: "MDPs, the Bellman Equation, and Value Iteration" + "Midterm Review" deck
- Live-coding environment (Jupyter) for the grid-world value iteration demo

### Formative Check (in-class)
Trace one Bellman backup by hand for a single grid-world cell given its neighbors' current value
estimates, a reward function, and γ = 0.9.

### Link to Lab/Assessment
Lab 8: implement value iteration on a small grid-world MDP (see `lab-manuals/lab-08.md`).
**Assignment 2 assigned** (CSP/optimization, SAT/SMT, planning, Weeks 5–8; due Week 10).
