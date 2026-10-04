# Course Plan: Introduction to Machine Learning

## 1. Course Information

| Field | Detail |
|---|---|
| Course Title | Introduction to Machine Learning |
| Level | Undergraduate (3rd/4th year, BS Computer Science / Software Engineering / AI) |
| Credit Hours | 3 (2 hrs lecture + 1 lab session of 3 hrs/week) |
| Prerequisites | Python programming (equivalent to *Programming for Artificial Intelligence* or a standalone Python course); basic linear algebra (vectors, matrices, dot products); basic probability & statistics (mean, variance, distributions, conditional probability) |
| Programming Language | Python 3.x |
| Core Libraries | NumPy, pandas, Matplotlib/Seaborn, scikit-learn |
| Duration | 16 teaching weeks (1 semester) + 1 exam week |
| Delivery Mode | Lecture + Lab (hands-on, Jupyter/Colab based) |

## 2. Course Description

This course is a full semester dedicated entirely to classical and statistical machine learning.
Where *Programming for Artificial Intelligence* gives applied ML a brief, four-week treatment
(regression, classification, clustering, and evaluation) as one topic among many in a broader
programming course, this course starts from the same families of algorithms and goes substantially
deeper and wider: it derives the mathematics behind linear and regularized regression, logistic
regression and classification metrics, k-nearest neighbors and Naive Bayes, decision trees, bagging
and boosting ensembles, and support vector machines with kernels; it treats model evaluation and
selection, feature engineering, and preprocessing as first-class topics in their own right; and it
covers unsupervised learning (clustering, dimensionality reduction, anomaly detection) and
recommender systems before closing with ML systems practice (pipelines, basic deployment, and
ethics/fairness). This course deliberately does not cover artificial neural networks in any depth
— that material belongs to, and is covered in full, by the dedicated *Introduction to Artificial
Neural Networks* course. Every week of lecture is paired with a hands-on lab, and the course
culminates in a capstone project in which students build, compare, and rigorously evaluate several
models from the course on a real or realistic dataset.

## 3. Goals

- Build a solid mathematical and practical understanding of the core families of classical machine
  learning algorithms: linear/regularized models, instance-based and probabilistic classifiers,
  tree-based models, ensembles, and kernel methods.
- Develop rigor in model evaluation and selection: cross-validation, hyperparameter search, learning
  curves, and the bias-variance tradeoff, so that reported results can be trusted.
- Gain practical skill in feature engineering and preprocessing, which in practice determines model
  quality as much as algorithm choice.
- Understand unsupervised learning (clustering, dimensionality reduction, anomaly detection) and a
  simple recommender system, broadening the toolkit beyond supervised learning.
- Acquire working fluency with scikit-learn as a production-grade toolkit, including pipelines and
  basic deployment considerations, and develop an awareness of ML ethics and fairness.
- Design, build, and critically evaluate an original end-to-end ML project as a capstone, comparing
  multiple algorithm families and discussing their tradeoffs honestly.

## 4. Course Learning Outcomes (CLOs) — Mapped to Bloom's Taxonomy

| CLO | Statement | Bloom's Level(s) |
|---|---|---|
| CLO1 | Recall the supervised/unsupervised/reinforcement learning taxonomy and explain the standard ML workflow (split, train, evaluate, iterate). | Remember, Understand |
| CLO2 | Apply linear, regularized, and logistic regression models to real data, and derive their cost functions and gradients. | Apply, Analyze |
| CLO3 | Apply and compare instance-based, probabilistic, tree-based, ensemble, and kernel-based classifiers, selecting an appropriate model for a given problem. | Apply, Analyze |
| CLO4 | Evaluate model performance using appropriate metrics (precision/recall/F1/ROC-AUC) and selection procedures (k-fold and nested cross-validation, grid/random search), and diagnose overfitting/underfitting from learning curves. | Analyze, Evaluate |
| CLO5 | Apply feature engineering and preprocessing techniques (encoding, scaling, missing-data handling, feature selection) to prepare real-world data for modeling. | Apply, Analyze |
| CLO6 | Apply unsupervised learning (clustering, PCA, anomaly detection) and a simple recommender system to uncover structure in unlabeled data. | Apply, Analyze |
| CLO7 | Evaluate the fairness and deployment considerations of an ML system, and design, build, and present an original end-to-end ML capstone project comparing multiple algorithm families. | Evaluate, Create |

### Bloom's Taxonomy progression across the semester

| Phase | Weeks | Dominant Bloom's Levels | Focus |
|---|---|---|---|
| Foundations | 1–4 | Remember, Understand, Apply | ML taxonomy and workflow; linear and regularized regression; logistic regression and classification metrics |
| Core Supervised Learning | 5–9 | Apply, Analyze | k-NN, Naive Bayes, decision trees, bagging/boosting ensembles, SVMs; midterm |
| Rigor & Data | 10–11 | Analyze, Evaluate | Cross-validation, hyperparameter search, learning curves; feature engineering and preprocessing |
| Beyond Supervised Learning | 12–14 | Apply, Analyze | Clustering, dimensionality reduction, anomaly detection, recommender systems |
| Systems & Synthesis | 15–16 | Evaluate, Create | Pipelines, deployment, ethics/fairness; capstone project |

## 5. Weekly Topic Overview (16 Weeks)

| Week | Topic | Bloom's Focus |
|---|---|---|
| 1 | Introduction to ML: history, supervised/unsupervised/reinforcement taxonomy, the ML workflow, bias-variance preview, train/validation/test splits | Remember, Understand |
| 2 | Linear regression in depth: simple/multiple regression, cost function, gradient descent, the normal equation, assumptions | Understand, Apply |
| 3 | Regularized regression: Ridge (L2), Lasso (L1), Elastic Net, why regularization helps, feature scaling | Apply, Analyze |
| 4 | Logistic regression & classification metrics in depth: sigmoid, decision boundary, precision/recall/F1/ROC/AUC, multi-class strategies, class imbalance | Apply, Analyze |
| 5 | k-Nearest Neighbors and Naive Bayes: lazy vs. probabilistic learning, curse of dimensionality, choosing k, Gaussian/Multinomial Naive Bayes | Apply, Analyze |
| 6 | Decision Trees: entropy, information gain, Gini impurity, tree construction, overfitting and pruning | Understand, Apply |
| 7 | Ensemble methods I: bagging, Random Forests, feature importance | Apply, Analyze |
| 8 | Ensemble methods II: boosting (AdaBoost, Gradient Boosting, XGBoost/LightGBM); midterm review | Apply, Analyze |
| 9 | **Midterm Exam** + Support Vector Machines: margin maximization, the kernel trick, soft margins | Remember–Analyze |
| 10 | Model evaluation & selection in depth: k-fold and nested cross-validation, GridSearchCV/RandomizedSearchCV, learning curves, bias-variance revisited | Analyze, Evaluate |
| 11 | Feature engineering & preprocessing: encoding, missing data, scaling, feature selection | Apply, Analyze |
| 12 | Unsupervised learning I: k-means clustering, hierarchical clustering, silhouette score | Apply, Analyze |
| 13 | Unsupervised learning II: PCA and dimensionality reduction, anomaly/outlier detection | Apply, Analyze |
| 14 | Recommender systems: content-based filtering, collaborative filtering, a worked example | Apply, Analyze |
| 15 | ML systems in practice: scikit-learn `Pipeline`, basic deployment, ML ethics and fairness | Apply, Evaluate |
| 16 | Capstone project presentations; course review | Evaluate, Create |
| 17 | Final Exam Week | — |

## 6. Assessment Plan

| Component | Weight | Notes |
|---|---|---|
| Lab Work (weekly) | 20% | Graded notebooks/scripts, Labs 1–15 |
| Assignments (4) | 20% | Problem sets tied to Weeks 3, 8, 10, 14 |
| Quizzes (6, best 5 counted) | 10% | Short, in-class/online, 15 min each |
| Midterm Exam | 15% | Week 9, covers Weeks 1–8 |
| Capstone Project | 20% | Proposal (Wk 10) + implementation + presentation (Wk 16) |
| Final Exam | 15% | Comprehensive, emphasis on Weeks 9–16 |

## 7. Grading Policy

Standard letter grading per institutional policy (e.g., A ≥ 85, B ≥ 70, C ≥ 55, D ≥ 40, F < 40;
adjust to institution). Late submissions: −10% per day up to 3 days, then not accepted unless
documented emergency.

## 8. Tools & Software

- Python 3.10+, pip/conda
- Jupyter Notebook / Google Colab
- NumPy, pandas, Matplotlib/Seaborn
- scikit-learn (primary modeling library throughout)
- Git/GitHub for lab and project submission

## 9. Reference Textbooks

- James, G., Witten, D., Hastie, T., & Tibshirani, R. — *An Introduction to Statistical Learning*
  (with Applications in Python/R). Primary reference for regression, classification, and
  resampling-based evaluation.
- Géron, A. — *Hands-On Machine Learning with Scikit-Learn, Keras, and TensorFlow*. O'Reilly.
  Primary reference for the scikit-learn workflow, ensembles, SVMs, and unsupervised learning
  chapters.
- Bishop, C. — *Pattern Recognition and Machine Learning*. Springer. Optional, more mathematically
  rigorous reference for students who want deeper theoretical grounding.
- Official scikit-learn, NumPy, and pandas documentation.

## 10. Academic Integrity

Labs and assignments are individual unless stated otherwise. The capstone project may be done in
pairs with clearly attributed contributions. Code plagiarism (including uncredited AI-generated
code submitted as original work) is handled per institutional academic integrity policy.
