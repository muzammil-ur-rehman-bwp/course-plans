# Week 5 Lecture Plan — Introduction to Machine Learning
## Topic: k-Nearest Neighbors and Naive Bayes

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain lazy (instance-based) learning vs. probabilistic learning, and the curse of
   dimensionality. (*Understand*)
2. Apply k-NN and Naive Bayes classifiers, and choose `k` via validation performance. (*Apply*)
3. Analyze the decision boundaries and assumptions of k-NN vs. Naive Bayes on the same dataset.
   (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | Lazy learning & k-NN | No training phase; distance metrics; majority vote |
| 0:20–0:40 | Choosing k & the curse of dimensionality | Bias-variance in k; why distances degrade in high dimensions (brief) |
| 0:40–0:50 | Break | — |
| 0:50–1:10 | Bayes' rule recap | Prior, likelihood, posterior |
| 1:10–1:40 | Naive Bayes | Conditional independence assumption; Gaussian vs. Multinomial variants |
| 1:40–2:00 | Comparing decision boundaries | k-NN vs. Naive Bayes vs. logistic regression, side by side |

### Materials/Equipment
- Live-coding environment, scikit-learn; a 2D dataset for decision-boundary visualization, a text
  dataset for Multinomial Naive Bayes.

### Formative Check (in-class)
Exercise: for `k=1` vs. `k=25` on the same dataset, predict (then verify) which gives a smoother
decision boundary and why.

### Link to Lab/Assessment
Lab 5: k-NN and Naive Bayes lab, tuning `k` and comparing Gaussian/Multinomial Naive Bayes.
