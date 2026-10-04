# Week 10 Lecture Plan — Machine Learning (Graduate)
## Topic: Bayesian Machine Learning II — Gaussian Processes

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Define a Gaussian Process as a prior over functions. (*Understand*)
2. Derive the GP regression predictive mean and covariance via Gaussian conditioning. (*Apply, Analyze*)
3. Explain the role of the kernel/covariance function and the noise hyperparameter. (*Understand, Analyze*)
4. Implement GP regression from scratch using a numerically stable Cholesky-based solve. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | GP as a function prior | Defining property: any finite set of values is jointly Gaussian |
| 0:20–0:50 | GP regression setup & derivation | Joint Gaussian of training/test outputs; conditioning formula |
| 0:50–1:05 | Reading the equations | Predictive mean as kernel expansion; predictive covariance shrinkage |
| 1:05–1:15 | Break | — |
| 1:15–1:40 | Hyperparameters | Length scale, signal variance, noise variance; marginal-likelihood tuning |
| 1:40–2:00 | Numerical stability | Why Cholesky, not direct inversion, of $K+\sigma_n^2I$ |

### Materials/Equipment
- Whiteboard for the Gaussian-conditioning derivation
- Jupyter notebook for from-scratch GP regression with posterior bands

### Formative Check (in-class)
Students identify which GP hyperparameter plays the role of the kernel ridge regression penalty
$\lambda$, from Week 7.

### Link to Lab/Assessment
Lab 10: implement GP regression from scratch via Cholesky decomposition on toy 1-D data; plot the
posterior mean and credible band. **Quiz 3** (Weeks 6–7) administered.
