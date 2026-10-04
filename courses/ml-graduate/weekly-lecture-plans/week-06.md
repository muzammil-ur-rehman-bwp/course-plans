# Week 6 Lecture Plan — Machine Learning (Graduate)
## Topic: Convex Optimization for ML

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. State the definitions of convex sets/functions, strong convexity, and smoothness. (*Understand*)
2. Derive the $O(1/T)$ gradient-descent convergence rate for convex, smooth objectives. (*Apply, Analyze*)
3. Derive the linear convergence rate for strongly-convex, smooth objectives. (*Apply, Analyze*)
4. Derive the KKT conditions and apply them to the SVM primal/dual. (*Apply, Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | Convexity definitions | Sets, functions, first/second-order conditions, strong convexity, smoothness |
| 0:20–0:50 | GD convergence, convex case | Descent lemma; telescoping sum; $O(1/T)$ rate |
| 0:50–1:10 | GD convergence, strongly convex case | Contraction lemma; linear rate |
| 1:10–1:20 | Break | — |
| 1:20–1:45 | Lagrangian duality | Primal/dual, weak duality, Slater's condition |
| 1:45–2:00 | KKT & SVM | Deriving the SVM dual via stationarity and complementary slackness |

### Materials/Equipment
- Whiteboard for both convergence-rate derivations
- Jupyter notebook comparing empirical convergence curves

### Formative Check (in-class)
Students explain why complementary slackness implies that only support vectors get nonzero
$\alpha_i$ in the SVM dual.

### Link to Lab/Assessment
Lab 6: implement gradient descent and empirically verify the $O(1/T)$ vs. linear convergence
rates on synthetic convex objectives.
