# Week 16 — Lecture Content: Capstone Presentations & Course Review

## 1. Course Review: The Full Map
This course moved through classical/statistical machine learning in roughly this order:
1. **Foundations (Week 1):** the ML taxonomy, workflow, and train/validation/test splits.
2. **Linear models (Weeks 2–4):** linear regression (cost function, gradient descent, normal
   equation), Ridge/Lasso/Elastic Net regularization, logistic regression, and classification
   metrics (precision/recall/F1/ROC-AUC).
3. **Instance-based and probabilistic models (Week 5):** k-NN and Naive Bayes.
4. **Tree-based models and ensembles (Weeks 6–8):** decision trees (entropy/Gini, pruning),
   bagging/Random Forests, and boosting (AdaBoost, Gradient Boosting).
5. **Kernel methods (Week 9):** SVMs, margins, and the kernel trick.
6. **Rigor (Weeks 10–11):** cross-validation, hyperparameter search, learning curves, the
   bias-variance tradeoff quantitatively, and feature engineering/preprocessing.
7. **Beyond supervised learning (Weeks 12–14):** k-means/hierarchical clustering, PCA, anomaly
   detection, and recommender systems.
8. **Systems practice (Week 15):** pipelines, model persistence, deployment considerations, and
   ML ethics/fairness.

Across this progression, a small number of ideas recur repeatedly and are worth naming
explicitly as the course's throughline: **never let test data influence training decisions**
(Week 1's leakage warning, repeated through scaling, cross-validation, and pipelines); **every
model trades bias against variance**, and most of the course's hyperparameters (`alpha`, `k`,
tree depth, `C`, `n_estimators`) are concretely tuning that one tradeoff; and **a metric must
match the problem's actual costs**, not just be "accuracy" by default.

## 2. Where This Course Ends
This course deliberately does not cover artificial neural networks beyond, at most, a passing
mention — that is the subject of a dedicated *Introduction to Artificial Neural Networks* course,
which starts from the single perceptron and builds up through backpropagation, modern
optimizers, and a deep learning framework. Students who want to extend the ML systems practice of
Week 15 (pipelines, deployment, fairness) to deep learning, or who want the mathematical depth of
gradient-based learning for neural networks specifically, should take that course next; a further
dedicated Deep Learning course extends into CNNs/RNNs and modern architectures in more depth
still.

## 3. Capstone Project Presentations
Each student/pair presents their end-to-end project: problem statement, data, approach
(comparing at least three algorithm families from the course), results, and an honest discussion
of limitations, following `presentations/capstone-presentation-template.md`. Presentations are
evaluated against `assignments/capstone-rubric.md`; peers complete a short feedback form for at
least two other presentations.

## 4. Closing Discussion
A brief closing discussion revisits the ML ethics and fairness themes from Week 15 in light of
the specific capstone projects presented, asking: for each project, who would be affected by this
model's errors, and what would responsible deployment require?
