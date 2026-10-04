# Week 14 Lecture Plan — Machine Learning (Graduate)
## Topic: Model Selection Theory

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Prove the bias-variance decomposition for squared error in full. (*Apply, Analyze*)
2. Derive AIC and BIC via their respective asymptotic arguments (sketch). (*Analyze*)
3. Compare the model each criterion selects for a given log-likelihood/parameter-count pair. (*Evaluate*)
4. Explain the theoretical justification for cross-validation as a risk estimator. (*Analyze, Evaluate*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:35 | Bias-variance decomposition | Full two-step derivation: noise split, then bias/variance split |
| 0:35–1:00 | AIC | Asymptotic KL-divergence argument; the $2k$ penalty |
| 1:00–1:10 | Break | — |
| 1:10–1:30 | BIC | Laplace approximation to the marginal likelihood; the $k\ln n$ penalty |
| 1:30–1:45 | AIC vs. BIC | Consistency vs. predictive-optimality framing |
| 1:45–2:00 | Cross-validation | Why it is an (approximately unbiased) risk estimator; link back to Weeks 2–5 |

### Materials/Equipment
- Whiteboard for both derivations
- Jupyter notebook for the empirical bias-variance decomposition (polynomial degree sweep)

### Formative Check (in-class)
Students compute AIC and BIC for two nested models given sample log-likelihoods and parameter
counts, and state which model each criterion favors.

### Link to Lab/Assessment
Lab 14: empirically decompose bias and variance across polynomial degrees; compute AIC/BIC for a
family of nested models. **Quiz 6** (Weeks 13–14) administered.
