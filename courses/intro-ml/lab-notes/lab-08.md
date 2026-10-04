# Lab Notes 8 — Boosting

**Concept recap:** boosting trains weak learners sequentially on reweighted data/residuals;
unlike Random Forests, boosting can overfit if given too many rounds or too high a learning rate.

**Common pitfalls:**
- Assuming more boosting rounds is always better, as with Random Forests — boosting's test
  performance can degrade past some `n_estimators` if the learning rate is too high.
- Using a tree that is too deep as the AdaBoost base estimator — AdaBoost is designed around
  genuinely weak learners (shallow stumps), and deep base trees can cause rapid overfitting.
- Comparing boosting and bagging only on test accuracy without also checking the train/test gap —
  the two families can reach similar test accuracy via different bias/variance tradeoffs.

**Debugging tip:** if Gradient Boosting test accuracy decreases as `n_estimators` grows past a
point, lower the `learning_rate` and/or add `max_depth`/`min_samples_leaf` constraints on the
base trees.

**Instructor tip:** have students plot train AND test accuracy against `n_estimators` for
Gradient Boosting at a deliberately high `learning_rate` (e.g., 1.0) to see visible overfitting,
then repeat at a low learning rate (e.g., 0.05) to see the contrast directly.
