# Week 3 Lecture Plan — Advanced Machine Learning (Post Graduate)
## Topic: High-Dimensional Statistics I — Concentration Beyond Hoeffding

**Duration:** 2 hours lecture + 3 hour research seminar/lab

### Learning Objectives (Bloom's Level)
1. Define sub-Gaussian and sub-exponential random variables and explain why they generalize
   bounded and Gaussian cases. (*Understand*)
2. State Bernstein's inequality and explain its two-regime structure. (*Understand, Apply*)
3. Simulate sums of sub-Gaussian/sub-exponential variables and empirically confirm the tail
   bounds. (*Apply, Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Motivation | Why Hoeffding (bounded variables only) is too narrow for modern high-dimensional estimators |
| 0:15–0:45 | Sub-Gaussian variables | MGF definition; equivalence to Gaussian-like tails; bounded ⟹ sub-Gaussian (Hoeffding's lemma) |
| 0:45–1:15 | Sub-exponential variables | Definition; why $\chi^2$-type variables need heavier tails; the two-regime tail shape |
| 1:15–1:50 | Bernstein's inequality | Statement; Chernoff-bound derivation sketch |
| 1:50–2:00 | Synthesis | Recap table: bounded ⊂ sub-Gaussian; sub-Gaussian ⊂ sub-exponential; which bound to reach for |

### Materials/Equipment
- Slides: "Concentration Beyond Hoeffding: Sub-Gaussian and Sub-Exponential Variables"
- Whiteboard for the MGF/Chernoff derivations
- Live-coding environment (Jupyter)

### Formative Check (in-class)
Given $X = Z^2$ for $Z\sim\mathcal N(0,1)$, explain why $X$ is sub-exponential but not
sub-Gaussian, and identify which inequality (Hoeffding, sub-Gaussian, or Bernstein) is the valid
tool for bounding $\mathbb{P}(\frac1n\sum_i X_i - 1 \geq t)$.

### Link to Lab/Assessment
Lab 3: simulate sub-Gaussian and sub-exponential concentration and verify Bernstein's inequality
empirically (see `lab-manuals/lab-03.md`).
