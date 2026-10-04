# Week 10 Lecture Plan — Introduction to Machine Learning
## Topic: Model Evaluation & Selection in Depth

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain k-fold cross-validation and nested cross-validation. (*Understand*)
2. Apply `GridSearchCV`/`RandomizedSearchCV` for hyperparameter tuning and interpret learning
   curves. (*Apply*)
3. Analyze the bias-variance tradeoff quantitatively via the expected test error decomposition.
   (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:25 | k-fold cross-validation | Why one train/test split isn't enough; `cross_val_score` |
| 0:25–0:45 | Nested cross-validation (brief) | Honest hyperparameter selection without leaking into the final estimate |
| 0:45–0:55 | Break | — |
| 0:55–1:25 | GridSearchCV / RandomizedSearchCV | Hyperparameter search over a grid/distribution |
| 1:25–1:45 | Learning curves | Diagnosing underfitting vs. overfitting from train/validation curves |
| 1:45–2:00 | Bias-variance tradeoff, quantitatively | Expected test error = bias^2 + variance + irreducible error |

### Materials/Equipment
- Live-coding environment, scikit-learn; a dataset with a model prone to both under- and
  overfitting (e.g., decision trees at varying depth).

### Formative Check (in-class)
Exercise: given a learning curve plot, determine whether the model is underfitting, overfitting,
or well-fit, and name the next step to try.

### Link to Lab/Assessment
Lab 10: cross-validation and hyperparameter search lab using `GridSearchCV` and learning curves.
**Assignment 3 assigned this week. Capstone project proposal due this week.**
