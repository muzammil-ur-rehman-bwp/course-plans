# Week 13 Lecture Plan — Programming for AI
## Topic: Model Evaluation, Overfitting, and Hyperparameter Tuning

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain the bias-variance tradeoff and overfitting/underfitting. (*Understand*)
2. Apply k-fold cross-validation and grid/random search for hyperparameter tuning. (*Apply*)
3. Analyze learning curves to diagnose overfitting vs. underfitting. (*Analyze*)
4. Evaluate and select a final tuned model based on validation performance. (*Evaluate*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:25 | Bias-variance tradeoff | Conceptual explanation, diagrams |
| 0:25–0:55 | Cross-validation | k-fold CV, `cross_val_score`, live demo |
| 0:55–1:05 | Break | — |
| 1:05–1:35 | Hyperparameter tuning | `GridSearchCV`/`RandomizedSearchCV`, live demo |
| 1:35–2:00 | Regularization & learning curves | L1/L2 intro, plotting/interpreting learning curves |

### Materials/Equipment
- Live-coding environment, scikit-learn
- Dataset reused from Week 11/12

### Formative Check (in-class)
Exercise: diagnose whether a given learning curve indicates overfitting, underfitting, or a
well-fit model.

### Link to Lab/Assessment
Lab 13: Cross-validate and tune a model with `GridSearchCV`. **Assignment 4** assigned this week
(model tuning/evaluation), due start of Week 15.
