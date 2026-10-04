# Week 13 Lecture Plan — Introduction to Machine Learning
## Topic: Unsupervised Learning II — Dimensionality Reduction & Anomaly Detection

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain Principal Component Analysis (PCA) and explained variance. (*Understand*)
2. Apply `PCA` for visualization and as a preprocessing step. (*Apply*)
3. Analyze a dataset for anomalies using `IsolationForest`/statistical thresholds. (*Apply,
   Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:25 | PCA intuition | Variance maximization; principal components as new axes |
| 0:25–0:50 | PCA in scikit-learn | Explained variance ratio; choosing the number of components |
| 0:50–1:00 | Break | — |
| 1:00–1:20 | PCA for visualization | Projecting high-dimensional data to 2D |
| 1:20–1:40 | Anomaly/outlier detection intro | Statistical thresholds (z-score, IQR) |
| 1:40–2:00 | Model-based anomaly detection | `IsolationForest`, `LocalOutlierFactor` (conceptual) |

### Materials/Equipment
- Live-coding environment, scikit-learn; a higher-dimensional dataset for PCA, a dataset with
  injected outliers for anomaly detection.

### Formative Check (in-class)
Exercise: given a scree plot of explained variance ratio, decide how many principal components
to retain to explain at least 90% of the variance.

### Link to Lab/Assessment
Lab 13: PCA and anomaly-detection lab.
