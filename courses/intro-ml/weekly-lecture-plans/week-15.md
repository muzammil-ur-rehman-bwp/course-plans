# Week 15 Lecture Plan — Introduction to Machine Learning
## Topic: ML Systems in Practice

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Apply scikit-learn's `Pipeline` to chain preprocessing and modeling into one deployable
   object. (*Apply*)
2. Apply model persistence (`joblib`) and basic input-validation practices for deployment.
   (*Apply*)
3. Evaluate an ML system for fairness and discuss responsible deployment. (*Evaluate*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:25 | The full Pipeline | Combining `ColumnTransformer` + model; using it inside `GridSearchCV` |
| 0:25–0:45 | Model persistence | Saving/loading with `joblib`; versioning considerations |
| 0:45–0:55 | Break | — |
| 0:55–1:15 | Deployment considerations | Input validation, monitoring for data/model drift (conceptual) |
| 1:15–1:40 | Bias in training data | How historical/sampling bias enters a trained model |
| 1:40–2:00 | Fairness metrics & responsible deployment | Group fairness metrics; a short case discussion |

### Materials/Equipment
- Live-coding environment, scikit-learn, joblib; a dataset with a sensitive attribute column for
  the fairness exercise.

### Formative Check (in-class)
Exercise: compute a model's accuracy/recall separately for two groups defined by a sensitive
attribute, and discuss whether a meaningful gap exists.

### Link to Lab/Assessment
Lab 15: build, persist, and reload a full pipeline; audit it for a fairness gap.
