# Week 10 Lecture Plan — Advanced Machine Learning (Post Graduate)
## Topic: Distribution Shift and Domain Adaptation

**Duration:** 2 hours lecture + 3 hour research seminar/lab

### Learning Objectives (Bloom's Level)
1. Formalize covariate shift and derive the importance-weighted risk identity. (*Understand, Apply*)
2. Explain the generalization-bound picture under shift and its limits. (*Analyze, Evaluate*)
3. Estimate density-ratio weights and empirically demonstrate the overlap-driven variance
   blow-up. (*Apply, Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Motivation | Train/test distribution mismatch; covariate shift vs. label shift vs. arbitrary shift |
| 0:15–0:40 | Importance weighting | Deriving the weighted-risk identity from first principles |
| 0:40–1:05 | Estimating the density ratio | Classifier-based estimation; the identity linking $w(x)$ to a classifier's odds |
| 1:05–1:35 | Generalization under shift | The divergence-penalized bound picture; why arbitrary shift has no free lunch |
| 1:35–2:00 | The overlap failure mode | High/infinite-variance weights; why this is structural, not an implementation bug |

### Materials/Equipment
- Slides: "Covariate Shift and Importance Weighting"
- Whiteboard for the importance-weighted risk derivation
- Live-coding environment (Jupyter)

### Formative Check (in-class)
Explain in two sentences why importance weighting requires $P_{\mathrm{tr}}(x)>0$ wherever
$P_{\mathrm{te}}(x)>0$, and what happens to the weighted-risk identity if this fails.

### Link to Lab/Assessment
Lab 10: estimate density-ratio weights on simulated shifted data and empirically probe the
overlap failure mode (see `lab-manuals/lab-10.md`).
