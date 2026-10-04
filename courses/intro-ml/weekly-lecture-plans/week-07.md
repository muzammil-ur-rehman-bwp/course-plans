# Week 7 Lecture Plan — Introduction to Machine Learning
## Topic: Ensemble Methods I — Bagging & Random Forests

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain bootstrap aggregating (bagging) and how it reduces variance. (*Understand*)
2. Apply `BaggingClassifier` and `RandomForestClassifier`, and extract feature importances.
   (*Apply*)
3. Analyze the variance-reduction benefit of a random forest relative to a single decision tree.
   (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:25 | Bootstrap sampling & bagging | Sampling with replacement; averaging/voting across models |
| 0:25–0:45 | Why bagging reduces variance | Averaging decorrelated high-variance models |
| 0:45–0:55 | Break | — |
| 0:55–1:25 | Random Forests | Bagging + random feature subsets at each split; decorrelating trees further |
| 1:25–1:45 | Out-of-bag error | Free validation estimate from unused bootstrap samples |
| 1:45–2:00 | Feature importance | Mean impurity decrease; caveats of interpretation |

### Materials/Equipment
- Live-coding environment, scikit-learn; the same dataset as Week 6 for direct comparison.

### Formative Check (in-class)
Exercise: explain, in your own words, why averaging predictions from several decorrelated
high-variance models reduces overall variance.

### Link to Lab/Assessment
Lab 7: comparing a single tree, a bagging ensemble, and a Random Forest on the same dataset.
