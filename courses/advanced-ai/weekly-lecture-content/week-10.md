# Week 10 — Lecture Content: Interpretability Research

## 1. Feature Attribution Methods (Conceptual)
**Feature attribution** methods attempt to explain a single prediction by assigning each input
feature a score reflecting how much it contributed to that output. Three common families,
described conceptually:
- **Gradient-based saliency**: use the gradient of the output with respect to each input feature
  (for differentiable models) as a local sensitivity measure — large-magnitude gradient on a
  feature suggests the output is locally sensitive to it.
- **Permutation importance**: for a given feature, shuffle (permute) its values across a dataset
  and measure how much the model's performance degrades; a feature whose permutation causes a
  large performance drop is deemed important, without requiring the model to be differentiable.
- **Perturbation/surrogate-model methods (the LIME/SHAP family)**: fit a simple, interpretable
  surrogate model (e.g., a local linear model) to approximate the complex model's behavior in the
  neighborhood of one specific input, and read feature importance off the simple surrogate.

```python
import numpy as np

def permutation_importance(model_predict, X, y, loss_fn, n_repeats=10, seed=0):
    """model_predict: X -> predictions. loss_fn: (y_true, y_pred) -> scalar (lower is better).
    Returns an importance score per feature (mean increase in loss when that feature is shuffled)."""
    rng = np.random.default_rng(seed)
    baseline_loss = loss_fn(y, model_predict(X))
    n_features = X.shape[1]
    importances = np.zeros(n_features)
    for j in range(n_features):
        increases = []
        for _ in range(n_repeats):
            X_perm = X.copy()
            rng.shuffle(X_perm[:, j])
            increases.append(loss_fn(y, model_predict(X_perm)) - baseline_loss)
        importances[j] = np.mean(increases)
    return importances
```

## 2. Mechanistic Interpretability
**Mechanistic interpretability** is the research program of reverse-engineering the actual
algorithm a trained model has learned — identifying specific interpretable internal features and
the "circuits" (sub-networks of connected features) that compute them — rather than only
characterizing the model's input-output behavior from the outside. This is a fundamentally
different kind of claim from feature attribution: an attribution score says "this input region
correlates with (or is locally sensitive to) this output," while a mechanistic account aims to
say "this specific internal computation, built from these specific identified components, is
what produces this output, and here is how it generalizes to other inputs." Mechanistic
interpretability is an emerging, labor-intensive research area, and current techniques can
usually only fully characterize relatively small, targeted pieces of a model's behavior rather
than its computation as a whole — a genuine and acknowledged limitation of the field's current
state, not a solved problem.

## 3. The Post-Hoc Explanation vs. Genuine Understanding Gap
A documented and important finding in the attribution-methods literature is that some popular
saliency-style attribution methods can be **insensitive to randomizing the model's own
parameters** — a basic sanity check that any attribution method claiming to reflect what the
*trained* model actually does should, at minimum, pass (if the explanation looks essentially the
same whether the model's weights are the trained ones or pure random noise, the explanation
cannot be reflecting anything specific to what the model learned). Several sanity-check studies
in the literature (e.g., work examining saliency maps under model- and data-randomization tests)
have found that certain attribution methods fail checks of exactly this kind. This is direct,
concrete evidence of the gap this week is about: a feature-attribution map can *look* visually
plausible and intuitive to a human observer without actually reflecting the specific mechanism
the model is using — attribution and mechanism are not the same kind of claim, and passing a
sanity check is necessary, though not sufficient, evidence that an explanation reflects the
model's real computation.

```python
import numpy as np

def randomization_sanity_check(attribution_fn, model_predict, X, y, randomize_model_fn, seed=0):
    """A minimal sanity check: compare an attribution method's output on the real, trained
    model vs. on a version of the model with randomized parameters. If the two attribution
    maps are highly similar, the method is not sensitive to what the model actually learned."""
    rng = np.random.default_rng(seed)
    real_attr = attribution_fn(model_predict, X, y)
    randomized_predict = randomize_model_fn(model_predict, rng)
    random_attr = attribution_fn(randomized_predict, X, y)
    correlation = np.corrcoef(real_attr, random_attr)[0, 1]
    return correlation  # high correlation here is a red flag, not a reassurance
```

## 4. Why This Distinction Matters for AI Safety (Callback to Week 9)
If interpretability is to serve as a genuine safety tool — detecting a misaligned internal
objective that behavior alone would not reveal — it needs mechanistic-style understanding, not
merely post-hoc attribution. A plausible-looking saliency map that fails a basic sanity check
would give false confidence that a model's reasoning has been inspected and found acceptable,
when in fact nothing about the model's actual computation has been verified. This is precisely
why current AI-safety research treats mechanistic interpretability as a distinct, harder, and
more valuable target than feature attribution alone.

## 5. In-Class/Lab Exercise
Build a small synthetic classification dataset where the true decision rule depends on exactly
2 of 6 features (the rest are pure noise, known by construction). Fit a simple model, compute
`permutation_importance`, and check that it correctly ranks the 2 true features above the 4 noise
features. Then run `randomization_sanity_check` against a trivial attribution method that simply
returns each feature's raw correlation with the model's output (ignoring the model entirely) and
discuss why this naive method fails the sanity check by construction.
