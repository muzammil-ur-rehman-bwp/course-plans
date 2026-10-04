# Week 3 Lecture Plan — Introduction to Machine Learning
## Topic: Regularized Regression

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain why unregularized linear regression overfits with many or correlated features.
   (*Understand*)
2. Apply Ridge, Lasso, and Elastic Net regression and interpret their penalty terms. (*Apply*)
3. Analyze how the regularization strength hyperparameter affects coefficients and model
   complexity. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | Why regularize | Overfitting with many/correlated features; the variance side of bias-variance |
| 0:20–0:45 | Ridge regression (L2) | Penalty term; closed-form solution; shrinkage |
| 0:45–0:55 | Break | — |
| 0:55–1:20 | Lasso regression (L1) | Penalty term; sparsity and automatic feature selection |
| 1:20–1:40 | Elastic Net | Combining L1 and L2; the mixing parameter |
| 1:40–2:00 | Feature scaling | Why regularization requires scaled features; `StandardScaler` |

### Materials/Equipment
- Live-coding environment, scikit-learn; a dataset with many/correlated features.

### Formative Check (in-class)
Exercise: fit Ridge and Lasso at several regularization strengths and observe how coefficients
shrink (Ridge) or are zeroed out (Lasso).

### Link to Lab/Assessment
Lab 3: Ridge/Lasso/Elastic Net regression lab, with feature scaling and a coefficient-path plot.
**Assignment 1 assigned this week.**
