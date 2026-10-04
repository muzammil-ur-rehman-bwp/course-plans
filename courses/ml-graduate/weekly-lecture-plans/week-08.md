# Week 8 Lecture Plan — Machine Learning (Graduate)
## Topic: Rigorous Ensemble Theory; Midterm Review

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Derive AdaBoost's exponential training-error bound. (*Apply, Analyze*)
2. Explain margin theory's resolution of "why boosting resists overfitting." (*Analyze, Evaluate*)
3. Derive bagging's variance-reduction formula from the variance of an average of correlated estimators. (*Apply, Analyze*)
4. Self-assess readiness for the midterm across Weeks 1–8. (*Evaluate*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:30 | AdaBoost training-error bound | Deriving $Z_t=2\sqrt{\epsilon_t(1-\epsilon_t)}$ and the product bound |
| 0:30–0:55 | Margin theory | The margin-based generalization bound; why it has no explicit $T$-dependence |
| 0:55–1:05 | Break | — |
| 1:05–1:30 | Bagging variance reduction | Deriving $\mathrm{Var}(\bar h)=\sigma^2/n+\frac{n-1}{n}\rho\sigma^2$ |
| 1:30–2:00 | Midterm review | Practice problems spanning Weeks 1–8; open Q&A |

### Materials/Equipment
- Whiteboard for both derivations
- Midterm review problem set handout
- Jupyter notebook for the from-scratch AdaBoost demo

### Formative Check (in-class)
In-class practice midterm questions (ungraded), peer discussion of answers.

### Link to Lab/Assessment
Lab 8: implement AdaBoost from scratch; plot training-error and margin-distribution curves across
rounds. **Assignment 2** due at the start of this week. **Midterm Exam** next week (Week 9).
