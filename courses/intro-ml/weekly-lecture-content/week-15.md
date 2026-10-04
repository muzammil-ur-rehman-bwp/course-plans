# Week 15 — Lecture Content: ML Systems in Practice

## 1. The Full Pipeline
Week 11 introduced `ColumnTransformer` for per-column preprocessing. Combining it with a model
in one `Pipeline` produces a single object that takes raw data in and returns predictions,
and can be tuned, evaluated, and saved as a single unit:
```python
from sklearn.pipeline import Pipeline
from sklearn.model_selection import GridSearchCV

full_pipeline = Pipeline([
    ("preprocess", preprocessor),             # the ColumnTransformer from Week 11
    ("model", RandomForestClassifier(random_state=42)),
])

param_grid = {"model__n_estimators": [100, 200], "model__max_depth": [5, 10, None]}
grid_search = GridSearchCV(full_pipeline, param_grid, cv=5)
grid_search.fit(X_train, y_train)    # preprocessing is refit correctly within each CV fold
```
Note the `model__` prefix: parameters of a named pipeline step are addressed as
`stepname__paramname` when passed through `GridSearchCV`. Wrapping preprocessing inside the
`Pipeline` (rather than doing it separately beforehand) is what keeps cross-validation leak-free,
as emphasized since Week 1 and formalized in Week 10.

## 2. Model Persistence
Once a final pipeline is selected, it is saved so it can be reloaded for inference later without
retraining:
```python
import joblib

joblib.dump(grid_search.best_estimator_, "model_pipeline.joblib")

# Later, in a different process/session:
loaded_pipeline = joblib.load("model_pipeline.joblib")
predictions = loaded_pipeline.predict(X_new)
```
**Versioning considerations:** record the scikit-learn version, the training data snapshot (or
its hash/date), and the exact feature schema (column names, order, dtypes) alongside the saved
model — a pipeline silently fails or gives wrong predictions if later fed data with a different
schema than it was trained on.

## 3. Basic Deployment Considerations
- **Input validation:** check that incoming data has the expected columns, types, and reasonable
  value ranges before prediction; fail loudly rather than silently producing a nonsensical
  prediction on malformed input.
- **Data/model drift (conceptual):** the real-world data distribution a deployed model sees can
  shift over time away from its training distribution, degrading performance; production systems
  typically monitor key input statistics and model performance over time and retrain
  periodically.
- **Reproducibility:** fixed `random_state`s, pinned library versions, and saved preprocessing
  parameters (means, encodings) are what make a deployed model's behavior reproducible and
  debuggable.

## 4. Bias in Training Data
A model trained on historical data reproduces (and can amplify) whatever biases are present in
that data. Common sources:
- **Historical bias:** past decisions recorded as labels may themselves reflect discrimination
  (e.g., historical lending decisions).
- **Sampling bias:** training data that underrepresents some group leads to a model that
  performs worse for that group.
- **Measurement bias:** features or labels are measured or defined differently (or less
  reliably) across groups.
None of these are fixed by a better algorithm alone — they require examining and, where
possible, correcting the data and labeling process itself.

## 5. Fairness Metrics
Given predictions across two or more groups defined by a sensitive attribute (e.g., a
demographic group), common group fairness notions include:
- **Demographic parity:** the positive-prediction rate should be similar across groups:
  `P(y_hat=1 | group=A) ≈ P(y_hat=1 | group=B)`.
- **Equal opportunity:** the true positive rate (recall) should be similar across groups:
  `P(y_hat=1 | y=1, group=A) ≈ P(y_hat=1 | y=1, group=B)`.
- **Equalized odds:** both the true positive rate and false positive rate should be similar
  across groups.
These notions can conflict with each other and with overall accuracy — there is no single
"correct" fairness metric independent of the application's context and stakes, which is itself an
important lesson.
```python
# Computing group-wise recall as one concrete fairness check
from sklearn.metrics import recall_score

for group in sensitive_attribute.unique():
    mask = (sensitive_attribute == group)
    print(group, "recall:", recall_score(y_test[mask], y_pred[mask]))
```

## 6. Responsible Deployment
Before deploying a model that affects people, document: what the model does and does not do,
what data it was trained on, known performance gaps across groups, and a plan for monitoring and
recourse if it makes harmful errors. This is a direct extension of the evaluation rigor built
throughout the course (Week 4 metrics, Week 10 evaluation) applied to the question "performance
for whom," not only "performance overall."

## 7. In-Class Exercise
On the provided dataset with a sensitive attribute column, compute accuracy and recall
separately for each group for a trained classifier; discuss whether the gap (if any) would be
acceptable for a real deployment, and why.
