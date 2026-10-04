# Course Contents: Introduction to Machine Learning

Detailed per-week breakdown of topics, subtopics, and resources. Companion to `course-plan.md`.
Each week lists: **Topics**, **Subtopics/Skills**, **Readings**, **Software/Libraries used**.

---

## Week 1 — Introduction to Machine Learning
- **Topics:** History and motivation for ML; the supervised/unsupervised/reinforcement learning
  taxonomy; the standard ML workflow (collect → split → train → evaluate → iterate); a conceptual
  preview of the bias-variance tradeoff; train/validation/test splits and why each exists.
- **Subtopics/Skills:** distinguishing regression from classification; performing a correct
  `train_test_split`; recognizing when a problem is supervised vs. unsupervised.
- **Readings:** James et al. (ISL) Ch. 1–2; Géron Ch. 1.
- **Software:** Python 3.10+, scikit-learn (installation, first look), Jupyter/Colab.

## Week 2 — Linear Regression in Depth
- **Topics:** Simple linear regression; multiple linear regression; the Mean Squared Error cost
  function; gradient descent derivation (partial derivatives of MSE w.r.t. weights); the
  closed-form normal equation; assumptions of linear regression (linearity, independence,
  homoscedasticity, normality of residuals).
- **Subtopics/Skills:** fitting `LinearRegression`; implementing batch gradient descent from
  scratch in NumPy and confirming it converges to the same solution as the normal equation;
  residual-plot diagnostics.
- **Readings:** ISL Ch. 3; Géron Ch. 4 (gradient descent and normal equation sections).
- **Software:** scikit-learn, NumPy, Matplotlib.

## Week 3 — Regularized Regression
- **Topics:** Why unregularized regression overfits with many/correlated features; Ridge (L2)
  regression; Lasso (L1) regression and its sparsity/feature-selection property; Elastic Net; the
  role of the regularization strength hyperparameter; feature scaling as a prerequisite for
  regularization.
- **Subtopics/Skills:** fitting `Ridge`, `Lasso`, `ElasticNet`; using `StandardScaler` correctly
  (fit on train only); comparing coefficient paths as regularization strength varies.
- **Readings:** ISL Ch. 6 (§6.1–6.2); Géron Ch. 4 (regularized models section).
- **Software:** scikit-learn, NumPy, Matplotlib.
- **Assignment 1 assigned** (regression & regularization).

## Week 4 — Logistic Regression & Classification Metrics in Depth
- **Topics:** The sigmoid function and log-odds; the logistic regression decision boundary;
  precision, recall, F1-score, and the confusion matrix in depth; ROC curves and AUC; multi-class
  strategies (one-vs-rest, one-vs-one); handling class imbalance (class weighting, resampling).
- **Subtopics/Skills:** fitting `LogisticRegression`; computing and interpreting a full
  `classification_report`; plotting an ROC curve and computing AUC; applying `class_weight` to an
  imbalanced dataset.
- **Readings:** ISL Ch. 4 (§4.1–4.3); Géron Ch. 3 (classification metrics sections).
- **Software:** scikit-learn, Matplotlib.

## Week 5 — k-Nearest Neighbors and Naive Bayes
- **Topics:** Lazy (instance-based) learning vs. probabilistic learning; the k-NN algorithm and
  distance metrics; the curse of dimensionality (brief); choosing k via cross-validation; Bayes'
  rule recap; Gaussian Naive Bayes and Multinomial Naive Bayes, and the conditional-independence
  assumption.
- **Subtopics/Skills:** fitting `KNeighborsClassifier` and tuning k; fitting `GaussianNB` and
  `MultinomialNB`; comparing decision boundaries of k-NN vs. Naive Bayes on the same dataset.
- **Readings:** ISL Ch. 4 (§4.4, Naive Bayes); Géron Ch. 3 (k-NN sections, selected).
- **Software:** scikit-learn, NumPy, Matplotlib.

## Week 6 — Decision Trees
- **Topics:** Entropy and information gain; Gini impurity; tree construction conceptually
  (ID3/CART); stopping criteria; overfitting in trees and pruning (`max_depth`,
  `min_samples_leaf`, cost-complexity pruning).
- **Subtopics/Skills:** fitting `DecisionTreeClassifier`/`Regressor`; visualizing a tree with
  `plot_tree`; comparing an unpruned vs. pruned tree's train/test accuracy.
- **Readings:** ISL Ch. 8 (§8.1); Géron Ch. 6.
- **Software:** scikit-learn, Matplotlib.

## Week 7 — Ensemble Methods I: Bagging & Random Forests
- **Topics:** Bootstrap aggregating (bagging) and variance reduction; Random Forests (bagging +
  random feature subsets); out-of-bag error estimation; feature importance.
- **Subtopics/Skills:** fitting `BaggingClassifier` and `RandomForestClassifier`; extracting and
  plotting `feature_importances_`; comparing a single tree, a bagged ensemble, and a random
  forest.
- **Readings:** ISL Ch. 8 (§8.2.1–8.2.2); Géron Ch. 7 (bagging/random forest sections).
- **Software:** scikit-learn, Matplotlib.

## Week 8 — Ensemble Methods II: Boosting; Midterm Review
- **Topics:** Boosting intuition (sequential, error-correcting weak learners); AdaBoost
  (conceptual); Gradient Boosting; a brief, factual mention of XGBoost/LightGBM as widely-used
  production boosting libraries; review session for Weeks 1–8.
- **Subtopics/Skills:** fitting `AdaBoostClassifier` and `GradientBoostingClassifier`; comparing
  boosting vs. bagging on bias/variance behavior; midterm practice problems.
- **Readings:** ISL Ch. 8 (§8.2.3); Géron Ch. 7 (boosting sections).
- **Software:** scikit-learn.
- **Assignment 2 assigned** (classification & ensembles).

## Week 9 — Midterm Exam; Support Vector Machines
- **Topics:** Midterm Exam (covers Weeks 1–8). Afterward: margin maximization and support
  vectors; the kernel trick (linear, polynomial, RBF kernels); soft-margin SVMs and the `C`
  hyperparameter.
- **Subtopics/Skills:** fitting `SVC` with different kernels; tuning `C` and `gamma`; visualizing
  decision boundaries for linear vs. RBF kernels.
- **Readings:** ISL Ch. 9; Géron Ch. 5.
- **Software:** scikit-learn, Matplotlib.
- **Capstone project introduced.**

## Week 10 — Model Evaluation & Selection in Depth
- **Topics:** k-fold cross-validation; nested cross-validation (brief, for honest hyperparameter
  selection); `GridSearchCV` and `RandomizedSearchCV`; learning curves; the bias-variance
  tradeoff revisited quantitatively (expected test error decomposition).
- **Subtopics/Skills:** using `cross_val_score`/`cross_validate`; running `GridSearchCV` with a
  parameter grid; plotting and interpreting a learning curve (`learning_curve`).
- **Readings:** ISL Ch. 5; Géron Ch. 2 (cross-validation), Ch. 4 (bias-variance).
- **Software:** scikit-learn, Matplotlib.
- **Assignment 3 assigned** (SVM & model evaluation). **Capstone project proposal due.**

## Week 11 — Feature Engineering & Preprocessing
- **Topics:** Encoding categorical variables (one-hot, ordinal); handling missing data
  (imputation strategies); feature scaling methods (standardization, min-max, robust scaling);
  feature selection techniques (filter, wrapper, embedded methods).
- **Subtopics/Skills:** using `OneHotEncoder`, `OrdinalEncoder`, `SimpleImputer`; building a
  `ColumnTransformer` for mixed-type data; applying `SelectKBest`/`RFE` for feature selection.
- **Readings:** Géron Ch. 2 (data preparation sections); scikit-learn preprocessing user guide.
- **Software:** scikit-learn, pandas.

## Week 12 — Unsupervised Learning I: Clustering
- **Topics:** k-means clustering (algorithm, initialization, choosing k via the elbow method);
  hierarchical clustering (agglomerative, linkage criteria, dendrograms); evaluating clusters
  with the silhouette score.
- **Subtopics/Skills:** fitting `KMeans` and `AgglomerativeClustering`; plotting a dendrogram;
  computing `silhouette_score` to compare clustering configurations.
- **Readings:** ISL Ch. 12 (§12.4); Géron Ch. 9 (clustering sections).
- **Software:** scikit-learn, SciPy (`scipy.cluster.hierarchy`), Matplotlib.

## Week 13 — Unsupervised Learning II: Dimensionality Reduction & Anomaly Detection
- **Topics:** Principal Component Analysis (PCA): variance maximization, principal components,
  explained variance ratio; using PCA for visualization and preprocessing; an introduction to
  anomaly/outlier detection (statistical thresholds, `IsolationForest`, `LocalOutlierFactor`
  conceptually).
- **Subtopics/Skills:** fitting `PCA`, plotting cumulative explained variance, projecting data to
  2D for visualization; fitting `IsolationForest` on a dataset with injected outliers.
- **Readings:** ISL Ch. 12 (§12.2); Géron Ch. 8.
- **Software:** scikit-learn, Matplotlib.

## Week 14 — Recommender Systems
- **Topics:** Content-based filtering (similarity between item feature vectors); collaborative
  filtering, user-based and item-based (conceptual, similarity over a user-item ratings matrix); a
  simple worked example combining both ideas.
- **Subtopics/Skills:** computing cosine similarity between item/user vectors; building a small
  user-item ratings matrix and generating top-N recommendations by hand and with
  `NearestNeighbors`.
- **Readings:** Géron Ch. 1 (applications context); supplementary scikit-learn
  `sklearn.neighbors` and `sklearn.metrics.pairwise` documentation.
- **Software:** scikit-learn, pandas, NumPy.
- **Assignment 4 assigned** (unsupervised learning, feature engineering & recommenders).

## Week 15 — ML Systems in Practice
- **Topics:** Building a scikit-learn `Pipeline` (chaining preprocessing and a model); basic
  deployment considerations (`joblib`/`pickle` model persistence, versioning, input validation);
  ML ethics and fairness (bias in training data, fairness metrics, responsible deployment).
- **Subtopics/Skills:** assembling a `Pipeline`/`ColumnTransformer` and running it inside
  `GridSearchCV`; saving and reloading a fitted pipeline with `joblib`; auditing a model's
  predictions for a fairness gap across a sensitive attribute.
- **Readings:** Géron Ch. 2 (pipelines), Ch. 19 (deployment, selected sections); course notes on
  fairness metrics.
- **Software:** scikit-learn, joblib.

## Week 16 — Capstone Presentations & Course Review
- **Topics:** Student capstone project presentations; recap of the course map (regression →
  classification → ensembles → SVMs → evaluation → feature engineering → unsupervised learning →
  recommenders → systems practice); where classical ML ends and the dedicated Deep Learning /
  Neural Networks courses begin.
- **Deliverable:** Capstone project final submission + presentation.

## Week 17 — Final Exam Week
- Comprehensive final exam, weighted toward Weeks 9–16 content (per Assessment Plan).

---

## Capstone Project (introduced Week 9, proposal Week 10, final Week 16)
Students (individually or in pairs) pick a real or realistic dataset and build an end-to-end ML
project: exploratory data analysis → feature engineering → training and comparing models from at
least three distinct algorithm families covered in the course (e.g., a linear/regularized model, a
tree-based or ensemble model, and an SVM or k-NN model) → rigorous evaluation (appropriate metrics,
cross-validation) → a written discussion of tradeoffs (accuracy vs. interpretability, training
time, robustness to the chosen preprocessing) → a short report and a 5–7 minute presentation.
