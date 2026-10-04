# Course Contents: Programming for Artificial Intelligence

Detailed per-week breakdown of topics, subtopics, and resources. Companion to `course-plan.md`.
Each week lists: **Topics**, **Subtopics/Skills**, **Readings**, **Software/Libraries used**.

---

## Week 1 — Python Essentials & the AI Tooling Landscape
- **Topics:** Python syntax recap; variables, types, operators; control flow (if/for/while);
  functions & scope; the Python AI/ML ecosystem overview (NumPy, pandas, scikit-learn, PyTorch).
- **Subtopics/Skills:** writing/running `.py` scripts and Jupyter notebooks; virtual environments
  (venv/conda); `pip install`; reading documentation.
- **Readings:** Python official tutorial §1–4; VanderPlas Ch. 1 (IPython/Jupyter).
- **Software:** Python 3.10+, Jupyter/Colab.

## Week 2 — Data Structures & OOP in Python
- **Topics:** Lists, tuples, dicts, sets; list/dict comprehensions; strings & string methods;
  intro to classes/objects, `__init__`, methods, inheritance basics.
- **Subtopics/Skills:** choosing the right data structure for a task; writing a simple class
  (e.g., a `Graph` or `Dataset` class) to be reused in later AI algorithms.
- **Readings:** Python official tutorial §5, 9.
- **Software:** Python 3.10+.

## Week 3 — NumPy for Numerical Computing
- **Topics:** ndarray creation, shape/dtype, indexing & slicing, broadcasting, vectorized
  operations, basic linear algebra (`dot`, `matmul`, `linalg`), random number generation.
- **Subtopics/Skills:** replacing Python loops with vectorized NumPy operations; performance
  comparison (loop vs. vectorized).
- **Readings:** VanderPlas Ch. 2; NumPy Quickstart (official docs).
- **Software:** NumPy.

## Week 4 — pandas & Exploratory Data Analysis
- **Topics:** Series, DataFrame; reading CSV/JSON; indexing (`loc`/`iloc`); filtering, grouping,
  aggregation; handling missing data; merging/joining; basic plotting with Matplotlib.
- **Subtopics/Skills:** building an end-to-end EDA notebook on a public dataset.
- **Readings:** VanderPlas Ch. 3; pandas "Getting Started" docs.
- **Software:** pandas, Matplotlib/Seaborn.
- **Assignment 1 assigned** (Python + NumPy/pandas problem set).

## Week 5 — Problem Solving as Search: State Spaces, BFS/DFS
- **Topics:** Formulating problems as search (states, actions, goal test, path cost); uninformed
  search: Breadth-First Search, Depth-First Search; graph vs. tree search.
- **Subtopics/Skills:** implementing a generic `Problem` class and BFS/DFS solvers in Python
  (e.g., 8-puzzle, maze solving).
- **Readings:** Russell & Norvig Ch. 3 (§3.1–3.4).
- **Software:** Python, `collections.deque`.

## Week 6 — Informed Search: Greedy & A*
- **Topics:** Heuristic functions, admissibility/consistency, Greedy Best-First Search, A*
  search; priority queues.
- **Subtopics/Skills:** implementing A* with `heapq`; designing heuristics for a grid/puzzle
  problem; comparing search strategies by nodes expanded.
- **Readings:** Russell & Norvig Ch. 3 (§3.5–3.6).
- **Software:** Python, `heapq`.
- **Assignment 2 assigned** (search algorithms).

## Week 7 — Constraint Satisfaction & Local Search
- **Topics:** CSP formulation (variables, domains, constraints); backtracking search with
  forward checking; local search: hill climbing, simulated annealing.
- **Subtopics/Skills:** solving N-Queens / map-coloring / Sudoku as a CSP; implementing simulated
  annealing for an optimization problem.
- **Readings:** Russell & Norvig Ch. 4 (§4.1), Ch. 6.
- **Software:** Python.

## Week 8 — Probability & Uncertainty for AI; Midterm Review
- **Topics:** Probability basics, conditional probability, Bayes' rule; Naive Bayes classifier
  (from scratch, conceptually); review session for Weeks 1–7.
- **Subtopics/Skills:** implementing a simple Naive Bayes text classifier; practice problems for
  the midterm.
- **Readings:** Russell & Norvig Ch. 13 (intro); VanderPlas Ch. 5 (Naive Bayes section, preview).
- **Software:** Python, NumPy.

## Week 9 — Midterm Exam; Intro to Machine Learning
- **Topics:** Midterm Exam (covers Weeks 1–8). Afterward: ML taxonomy (supervised, unsupervised,
  reinforcement — overview only); the ML workflow (data → features → train → evaluate → deploy);
  train/test split.
- **Readings:** Géron Ch. 1.
- **Software:** scikit-learn (installation, first look).

## Week 10 — Regression with scikit-learn
- **Topics:** Linear regression (cost function, gradient descent intuition), logistic regression
  for binary classification; feature scaling.
- **Subtopics/Skills:** fitting `LinearRegression`/`LogisticRegression` in scikit-learn; plotting
  predictions vs. actuals; interpreting coefficients.
- **Readings:** Géron Ch. 4 (selected sections).
- **Software:** scikit-learn, Matplotlib.
- **Capstone project proposal due.**

## Week 11 — Classification & Model Evaluation Metrics
- **Topics:** k-Nearest Neighbors, Decision Trees, (brief) SVM; evaluation metrics — accuracy,
  precision, recall, F1, confusion matrix, ROC/AUC.
- **Subtopics/Skills:** training/comparing multiple classifiers on the same dataset; building a
  confusion matrix and classification report.
- **Readings:** Géron Ch. 3, Ch. 6 (Decision Trees intro).
- **Software:** scikit-learn.
- **Assignment 3 assigned** (classification + evaluation).

## Week 12 — Unsupervised Learning: Clustering & Dimensionality Reduction
- **Topics:** k-means clustering (algorithm, choosing k via elbow method); Principal Component
  Analysis (PCA) for dimensionality reduction/visualization.
- **Subtopics/Skills:** clustering a dataset and visualizing clusters; reducing a high-dimensional
  dataset to 2D with PCA for plotting.
- **Readings:** Géron Ch. 8, Ch. 9 (selected sections).
- **Software:** scikit-learn, Matplotlib.

## Week 13 — Model Evaluation, Overfitting, and Hyperparameter Tuning
- **Topics:** Bias-variance tradeoff, overfitting/underfitting, cross-validation (k-fold),
  hyperparameter search (grid search, random search), regularization (L1/L2 intro).
- **Subtopics/Skills:** using `cross_val_score`, `GridSearchCV`; plotting learning curves.
- **Readings:** Géron Ch. 4 (regularization), Ch. 2 (cross-validation sections).
- **Software:** scikit-learn.
- **Assignment 4 assigned** (model tuning/evaluation).

## Week 14 — Neural Networks I: Perceptron & Forward Pass
- **Topics:** Biological inspiration (brief), the perceptron, activation functions (sigmoid,
  ReLU, softmax), forward propagation through a simple feedforward network, loss functions.
- **Subtopics/Skills:** implementing a single-layer perceptron and a 2-layer forward pass in
  NumPy "from scratch".
- **Readings:** Géron Ch. 10 (selected sections).
- **Software:** NumPy.

## Week 15 — Neural Networks II: Backpropagation & Training with PyTorch/Keras
- **Topics:** Backpropagation intuition, gradient descent variants (SGD, Adam — conceptual),
  building/training a small feedforward network with a deep learning framework; GPU/CPU note.
- **Subtopics/Skills:** training an MNIST-scale classifier end-to-end; plotting training/validation
  loss curves.
- **Readings:** Géron Ch. 10–11 (selected sections); PyTorch "60 Minute Blitz" or Keras
  Sequential API guide.
- **Software:** PyTorch or TensorFlow/Keras.

## Week 16 — Capstone Presentations, Course Review, AI Ethics
- **Topics:** Student capstone mini-project presentations; recap of the course map (search → CSP
  → probability → ML → neural nets); brief discussion of AI ethics (bias, fairness, data privacy,
  responsible use of AI-generated code).
- **Deliverable:** Capstone project final submission + presentation.

## Week 17 — Final Exam Week
- Comprehensive final exam, weighted toward Weeks 9–16 content (per Assessment Plan).

---

## Capstone Mini-Project (introduced Week 9, proposal Week 10, final Week 16)
Students (individually or in pairs) pick a dataset/problem and apply the course's techniques
end-to-end: data cleaning (pandas) → a classical AI or ML technique → evaluation → a short
written report and a 5–7 minute presentation. Example topics: spam classifier, simple game-
playing agent via search, handwritten digit recognizer, basic recommendation system.
