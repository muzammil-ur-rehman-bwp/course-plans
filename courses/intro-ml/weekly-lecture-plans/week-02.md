# Week 2 Lecture Plan — Introduction to Machine Learning
## Topic: Linear Regression in Depth

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain the linear regression model, its cost function, and its standard assumptions.
   (*Understand*)
2. Derive the gradient descent update rule for linear regression by differentiating the MSE cost
   function. (*Analyze*)
3. Apply gradient descent and the normal equation to fit linear regression models and compare
   their solutions. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | Simple linear regression | One feature; slope/intercept; the regression line |
| 0:20–0:45 | Multiple linear regression & the cost function | Vector/matrix form; MSE |
| 0:45–0:55 | Break | — |
| 0:55–1:30 | Gradient descent derivation | Partial derivatives of MSE; the update rule; learning rate |
| 1:30–1:50 | The normal equation | Closed-form solution; when it's preferable to gradient descent |
| 1:50–2:00 | Assumptions of linear regression | Linearity, independence, homoscedasticity, normality of residuals |

### Materials/Equipment
- Live-coding environment, scikit-learn, NumPy; a simple regression dataset.

### Formative Check (in-class)
Exercise: implement one full step of batch gradient descent by hand (on paper or in code) for a
two-parameter linear model, given a small dataset.

### Link to Lab/Assessment
Lab 2: implement gradient descent from scratch in NumPy and compare it against
`LinearRegression`'s normal-equation solution.
