# Week 2 Lecture Plan — Machine Learning (Graduate)
## Topic: PAC Learning in Depth

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. State the PAC-learnability definition precisely, including the roles of $\epsilon$ and $\delta$. (*Understand*)
2. Derive the finite-hypothesis-class sample-complexity bound from Hoeffding's inequality and a union bound. (*Apply, Analyze*)
3. Apply the derived bound to compute sample complexity for a given $|H|,\epsilon,\delta$. (*Apply*)
4. Explain why PAC learnability requires polynomial (not exponential) sample complexity. (*Understand*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | PAC definition | Formal statement; "probably" vs. "approximately correct" |
| 0:20–0:40 | Setup | Bad hypotheses; realizability; why ERM fails only if a bad $h$ looks perfect |
| 0:40–1:10 | Hoeffding step | Bounding one bad hypothesis's probability of zero empirical risk |
| 1:10–1:20 | Break | — |
| 1:20–1:45 | Union bound step | Combining over all of $H$; solving for $m$ |
| 1:45–2:00 | Agnostic case | Two-sided bound; uniform convergence, stated as the general template |

### Materials/Equipment
- Whiteboard/slides for the step-by-step derivation
- Jupyter notebook for the empirical verification demo

### Formative Check (in-class)
Students re-derive, on paper, the sample-complexity formula for $|H|=100,\epsilon=0.1,\delta=0.1$
and check their numeric answer against a projected solution.

### Link to Lab/Assessment
Lab 2: empirically verify the finite-class PAC bound by simulation; **Quiz 1** next week covers
this content. **Assignment 1** assigned this week.
