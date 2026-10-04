# Week 8 — Lecture Content: Regularization Theory; Midterm Review

## 1. The Classical Bias-Variance Tradeoff
Classical statistical learning theory decomposes expected test error, roughly, into
$\text{Bias}^2 + \text{Variance} + \text{Noise}$. As model capacity grows from too small to too
large, bias falls (the model can represent more) while variance rises (the model fits
idiosyncrasies of the particular training set) — producing the familiar **U-shaped** test-error
curve, minimized at some intermediate capacity, with overfitting expected to worsen monotonically
beyond that point.

## 2. Double Descent
Empirically, as model capacity (e.g., network width) is increased *far* past the point where
training error reaches zero (the **interpolation threshold**), test error does **not** keep
rising as the classical U predicts. Instead, a now widely-documented pattern (observed across many
model families, and frequently referred to as **double descent**) occurs: test error rises to a
peak near the interpolation threshold, then **decreases again** as capacity grows further, often
to below the best error achieved in the classical under-parameterized regime. A clean way to see
this intuitively: right at the interpolation threshold, there is typically exactly one way for the
model to fit the training data, and that one way can be a poorly conditioned, high-variance fit
(it must thread through noise exactly). Once capacity exceeds the threshold, there are *many* ways
to fit the training data exactly, and common training procedures (gradient descent from a small
initialization) have an implicit bias toward the smoothest / simplest / lowest-norm such solution
— which tends to generalize well despite interpolating the (possibly noisy) training labels.
This course treats double descent as a robust, widely replicated **empirical phenomenon** that
any complete generalization theory must explain, rather than attaching it to one single settled
mechanism.

## 3. Weight Decay as a Gaussian Prior (MAP View)
Maximum a posteriori (MAP) estimation maximizes $p(\theta\mid D) \propto p(D\mid\theta)p(\theta)$,
equivalently minimizing $-\log p(D\mid\theta) - \log p(\theta)$. If the per-example likelihood
gives rise to a negative log-likelihood equal (up to an additive constant) to a loss $L(\theta)$,
and the prior is an isotropic zero-mean Gaussian, $\theta \sim \mathcal{N}(0,\tau^2 I)$, then
$$
-\log p(\theta) = \frac{1}{2\tau^2}\|\theta\|^2 + \text{const}
$$
so MAP estimation minimizes
$$
L(\theta) + \frac{1}{2\tau^2}\|\theta\|^2
$$
— exactly L2 weight decay, with regularization strength $\lambda = 1/\tau^2$. A **smaller** prior
variance $\tau^2$ (a stronger belief that weights should be near zero) corresponds to a **larger**
$\lambda$ (stronger decay) — the Bayesian and the classical optimization views of weight decay are
the same object, viewed through two different, exactly equivalent, lenses.

## 4. Dropout as Approximate Bayesian Model Averaging
Dropout trains with each unit independently zeroed with probability $p$ at every forward pass,
which can be read as **training an ensemble of exponentially many subnetworks** sharing weights
(one subnetwork per dropout mask realized), each trained with a single gradient step before the
mask is redrawn. At test time, "inverted dropout" scales activations by $(1-p)$ (or scales weights
at test time equivalently) to approximate the **average prediction over all those subnetworks**
without actually having to run and average exponentially many forward passes. This matches the
form of an approximate Bayesian model-averaging procedure: rather than committing to one single
network, dropout approximately averages predictions over an implicit distribution of networks
induced by the mask — a conceptual link (associated with the Bayesian deep learning literature,
e.g. the Monte Carlo dropout interpretation) that is useful for intuition, though it is an
*approximation* to full Bayesian averaging over network weights, not an exact equivalence.

## 5. Code: A Small Double-Descent Curve
```python
import numpy as np

def make_data(n, d, noise_std, seed=0):
    rng = np.random.default_rng(seed)
    X = rng.normal(size=(n, d))
    w_true = rng.normal(size=d)
    y = X @ w_true + noise_std * rng.normal(size=n)
    return X, y, w_true

def min_norm_least_squares(X, y):
    # Minimum-norm solution: exact fit if width >= n (underdetermined), else ordinary least squares.
    return np.linalg.pinv(X) @ y

n_train, d_true, noise_std = 40, 20, 1.0
X_train, y_train, w_true = make_data(n_train, d_true, noise_std, seed=1)
X_test, y_test, _ = make_data(200, d_true, noise_std, seed=2)

widths = [5, 10, 20, 30, 39, 40, 41, 50, 80, 150, 400]
test_errors = []
rng = np.random.default_rng(0)
for width in widths:
    # Project the true d-dim features into a `width`-dim random feature space (a simple way to
    # sweep "capacity" smoothly through the interpolation threshold at width = n_train).
    P = rng.normal(size=(d_true, width)) / np.sqrt(d_true)
    Xtr_w, Xte_w = X_train @ P, X_test @ P
    w_hat = min_norm_least_squares(Xtr_w, y_train)
    test_err = np.mean((X_test @ P @ w_hat - y_test) ** 2)
    test_errors.append(test_err)
    print(f"width={width:4d}  test MSE={test_err:.4f}")
```
Run this and plot `test_errors` against `widths`: expect error to rise sharply as width approaches
`n_train=40` (the interpolation threshold, where the fit must thread through every training point
exactly with little freedom left over), then fall again as width grows well past it — the
double-descent shape, reproduced here with nothing more exotic than minimum-norm linear
regression on random features.

## 6. Midterm Review (Weeks 1–8)
Review, with worked examples where helpful: the Universal Approximation Theorem and its caveats
(Week 2); forward/reverse-mode AD and backprop as reverse-mode AD (Week 3); the Xavier/He
initialization derivations (Week 4); BatchNorm's forward/backward derivation and the two
explanations for why it helps (Week 5); saddle-point dominance and why second-order methods do
not scale (Week 6); Adam's update rule, its known failure case, and warmup theory (Week 7);
bias-variance vs. double descent, and weight decay/dropout's theoretical justifications (Week 8).

## 7. In-Class Exercise
In your own words, state the one sentence that best explains why double descent is surprising
*given* the classical bias-variance curve, and the one sentence that best explains why it is not,
in fact, a contradiction of any theorem (only of an informal expectation).
