# Week 8 Lecture Plan — Introduction to Machine Learning
## Topic: Ensemble Methods II — Boosting; Midterm Review

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain the boosting intuition of sequentially correcting errors with weak learners.
   (*Understand*)
2. Apply `AdaBoostClassifier` and `GradientBoostingClassifier`. (*Apply*)
3. Analyze the bias/variance behavior of boosting vs. bagging, and consolidate Weeks 1–8 for the
   midterm. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | Boosting intuition | Sequential weak learners, each correcting prior errors |
| 0:20–0:45 | AdaBoost (conceptual) | Reweighting misclassified examples; weighted vote |
| 0:45–0:55 | Break | — |
| 0:55–1:20 | Gradient Boosting | Fitting new learners to the residual/gradient of the loss |
| 1:20–1:35 | XGBoost/LightGBM (brief) | Real, widely-used production boosting libraries; what they add |
| 1:35–2:00 | Midterm review | Consolidated Q&A across Weeks 1–8 |

### Materials/Equipment
- Live-coding environment, scikit-learn; the same dataset as Weeks 6–7 for continuity.

### Formative Check (in-class)
Exercise: explain, in your own words, the key difference between how bagging and boosting
combine their base models.

### Link to Lab/Assessment
Lab 8: boosting lab comparing AdaBoost and Gradient Boosting to the Week 7 bagging/Random Forest
results. **Assignment 2 assigned this week** (classification & ensembles).
