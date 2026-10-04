# Week 11 Lecture Plan — Artificial Intelligence (Graduate)
## Topic: Probabilistic Graphical Models at Rigor

**Duration:** 2 hours lecture + 3 hour lab/seminar

### Learning Objectives (Bloom's Level)
1. Explain the computational complexity of exact inference in Bayesian networks (worst-case
   intractability, dependence on treewidth). (*Understand, Analyze*)
2. Implement rejection sampling and likelihood weighting for query inference. (*Apply*)
3. Compare the efficiency/variance of the two sampling methods empirically. (*Analyze, Evaluate*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | Recap | Bayesian networks as factored joint distributions |
| 0:20–0:50 | Complexity of exact inference | Why exact inference is worst-case intractable; treewidth intuition |
| 0:50–1:20 | Rejection sampling & likelihood weighting | Algorithms, worked trace |
| 1:20–1:45 | MCMC (brief) | Why Gibbs sampling avoids rejection sampling's inefficiency |
| 1:45–2:00 | Synthesis | Exact vs. approximate inference: when to use which |

### Materials/Equipment
- Slides: "Exact and Approximate Inference in Bayesian Networks"
- Live-coding environment (Jupyter)

### Formative Check (in-class)
Given a small Bayesian network and an evidence variable, estimate how many samples rejection
sampling would waste if the evidence is a rare event, and contrast with likelihood weighting.

### Link to Lab/Assessment
Lab 11: implement rejection sampling and likelihood weighting on a small Bayesian network
(see `lab-manuals/lab-11.md`). **Assignment 3 assigned** (MDPs, POMDPs, probabilistic inference,
Weeks 8–11; due Week 13).
