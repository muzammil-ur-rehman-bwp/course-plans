# Week 4 Lecture Plan — Machine Learning (Graduate)
## Topic: Rademacher Complexity

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Define empirical Rademacher complexity and the Rademacher generalization bound. (*Understand*)
2. Derive, via Massart's lemma and Sauer–Shelah, how Rademacher complexity relates to and generalizes VC-based bounds. (*Analyze*)
3. Compute Rademacher complexity in closed form for a norm-bounded linear class. (*Apply*)
4. Estimate Rademacher complexity by Monte Carlo and compare to the closed-form bound. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Motivation | Why a data-dependent capacity measure can beat a worst-case VC bound |
| 0:15–0:40 | Definition & bound | Empirical/population Rademacher complexity; the generalization bound |
| 0:40–1:05 | Massart's lemma | Statement and proof sketch; linking Rademacher complexity back to the growth function |
| 1:05–1:15 | Break | — |
| 1:15–1:45 | Linear class example | Closed-form derivation for a norm-bounded linear class, dimension-free bound |
| 1:45–2:00 | Synthesis | Rademacher as the generalization of Weeks 2–3's tools |

### Materials/Equipment
- Whiteboard for Massart's lemma and the linear-class derivation
- Jupyter notebook for the Monte Carlo estimator

### Formative Check (in-class)
Students explain, in one sentence, why the Section-5 linear-class Rademacher bound does not grow
with input dimension $d$, unlike the VC bound for the same class.

### Link to Lab/Assessment
Lab 4: Monte Carlo estimate of empirical Rademacher complexity for a linear class, compared
against the closed-form bound. **Assignment 1** (PAC/VC/Rademacher) due this week.
