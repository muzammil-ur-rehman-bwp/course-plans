# Week 14 — Lecture Content: Model Selection Theory

## 1. The Bias-Variance Decomposition for Squared Error — Full Derivation
Fix a test point $x$ with true response $y=f(x)+\varepsilon$, $\mathbb{E}[\varepsilon]=0$,
$\mathrm{Var}(\varepsilon)=\sigma^2$, $\varepsilon$ independent of the training sample. Let
$\hat h(x)$ be a model's prediction at $x$, where $\hat h$ is itself random because it depends on
a random training sample $S$. We want $\mathbb{E}_{S,\varepsilon}\big[(y-\hat h(x))^2\big]$.

**Step 1 — split off the noise.**
$$
\mathbb{E}\big[(y-\hat h(x))^2\big] = \mathbb{E}\big[(f(x)+\varepsilon-\hat h(x))^2\big]
= \mathbb{E}\big[(f(x)-\hat h(x))^2\big] + 2\,\mathbb{E}\big[\varepsilon\,(f(x)-\hat h(x))\big] + \mathbb{E}[\varepsilon^2].
$$
Since $\varepsilon$ is independent of $\hat h(x)$ (which depends only on $S$, not on this fresh
test-point noise) and $\mathbb{E}[\varepsilon]=0$, the cross term vanishes:
$\mathbb{E}[\varepsilon(f(x)-\hat h(x))] = \mathbb{E}[\varepsilon]\cdot\mathbb{E}[f(x)-\hat h(x)] = 0$.
And $\mathbb{E}[\varepsilon^2]=\mathrm{Var}(\varepsilon)+\mathbb{E}[\varepsilon]^2=\sigma^2$. So:
$$
\mathbb{E}\big[(y-\hat h(x))^2\big] = \mathbb{E}\big[(f(x)-\hat h(x))^2\big] + \sigma^2.
$$

**Step 2 — split the remaining term around $\mathbb{E}[\hat h(x)]$.** Add and subtract
$\mathbb{E}_S[\hat h(x)]$ (the average prediction over all possible training samples):
$$
\mathbb{E}_S\big[(f(x)-\hat h(x))^2\big] = \mathbb{E}_S\Big[\big((f(x)-\mathbb{E}_S[\hat h(x)]) + (\mathbb{E}_S[\hat h(x)]-\hat h(x))\big)^2\Big].
$$
Expanding the square:
$$
= \big(f(x)-\mathbb{E}_S[\hat h(x)]\big)^2 + 2\big(f(x)-\mathbb{E}_S[\hat h(x)]\big)\mathbb{E}_S\big[\mathbb{E}_S[\hat h(x)]-\hat h(x)\big] + \mathbb{E}_S\big[(\mathbb{E}_S[\hat h(x)]-\hat h(x))^2\big].
$$
The first factor in the cross term is a constant (no $S$-dependence once the expectation over $S$
is taken outside it), and $\mathbb{E}_S[\mathbb{E}_S[\hat h(x)]-\hat h(x)] = \mathbb{E}_S[\hat
h(x)] - \mathbb{E}_S[\hat h(x)] = 0$ exactly — so the cross term vanishes. The two surviving terms
are, by definition, the squared **bias** and the **variance** of $\hat h(x)$:
$$
\mathrm{Bias}[\hat h(x)] := \mathbb{E}_S[\hat h(x)] - f(x), \qquad \mathrm{Var}[\hat h(x)] := \mathbb{E}_S\big[(\hat h(x)-\mathbb{E}_S[\hat h(x)])^2\big].
$$

**Combining both steps:**
$$
\boxed{\mathbb{E}_{S,\varepsilon}\big[(y-\hat h(x))^2\big] \;=\; \underbrace{\mathrm{Bias}[\hat h(x)]^2}_{\text{systematic error}} \;+\; \underbrace{\mathrm{Var}[\hat h(x)]}_{\text{sensitivity to the sample}} \;+\; \underbrace{\sigma^2}_{\text{irreducible noise}}.}
$$
**Reading it:** $\sigma^2$ cannot be reduced by any model. Simpler models (e.g., low-degree
polynomials, heavily regularized, high $\lambda$) tend to have high bias, low variance; flexible
models (high-degree polynomials, weak/no regularization) tend to have low bias, high variance.
This is the formal justification behind the qualitative "bias-variance tradeoff" language used
throughout the prerequisite course.

## 2. Information Criteria — AIC
Given a model with log-likelihood $\hat\ell = \log p(\mathrm{data}\mid\hat\theta)$ at its
maximum-likelihood fit $\hat\theta$ (with $k$ free parameters), the **Akaike Information
Criterion** is
$$
\mathrm{AIC} = -2\hat\ell + 2k.
$$
**Derivation sketch:** Akaike's argument estimates the expected out-of-sample
Kullback–Leibler divergence between the true data-generating distribution and the fitted model.
The in-sample log-likelihood $\hat\ell$ is an *optimistic* (upward-biased) estimate of the
model's true out-of-sample fit, precisely because $\hat\theta$ was chosen to maximize likelihood
*on this same data*. An asymptotic argument (using the fact that $-2$ times a log-likelihood
ratio is asymptotically $\chi^2_k$-distributed under standard regularity conditions) shows this
optimism bias is, in expectation, approximately $k$ nats of log-likelihood per fitted parameter —
i.e., each additional free parameter inflates the in-sample log-likelihood by about $1$ nat on
average even if it captures no real signal. Subtracting off this expected optimism (equivalently,
adding a $2k$ penalty to $-2\hat\ell$) corrects for it, giving an approximately unbiased estimate
of (twice) the expected KL divergence, up to a model-independent constant — so comparing AIC
across candidate models approximates comparing their true predictive quality.

## 3. Information Criteria — BIC
The **Bayesian Information Criterion** is
$$
\mathrm{BIC} = -2\hat\ell + k\ln n.
$$
**Derivation sketch:** BIC approximates the (negative, doubled) log **marginal likelihood**
(model evidence) $p(\mathrm{data}) = \int p(\mathrm{data}\mid\theta)\,p(\theta)\,d\theta$, which
is what a fully Bayesian model-comparison procedure (choosing the model with highest evidence)
would use. Applying a **Laplace approximation** to this integral — approximating the integrand by
a Gaussian centered at the maximum-likelihood $\hat\theta$, matching its curvature (Hessian) there
— gives
$$
\log p(\mathrm{data}) \approx \hat\ell - \frac{k}{2}\ln n + O(1),
$$
where the $-\frac{k}{2}\ln n$ term arises because the determinant of the Hessian of the
log-likelihood scales like $n^k$ for $n$ i.i.d. observations and $k$ parameters (each parameter's
curvature grows linearly in $n$), and $\log(n^{-k/2})=-\frac{k}{2}\ln n$. Multiplying by $-2$ and
dropping the $O(1)$ term (which does not grow with $n$) gives $\mathrm{BIC}=-2\hat\ell+k\ln n$.

## 4. AIC vs. BIC
Both penalize complexity, but BIC's penalty ($k\ln n$) grows with sample size while AIC's ($2k$)
does not — for $n>e^2\approx7.39$, BIC penalizes additional parameters more heavily than AIC, so
BIC tends to select *simpler* models than AIC, especially as $n$ grows. This also reflects a
difference in what each criterion is asymptotically trying to do: AIC targets good **predictive**
performance (minimizing expected KL divergence to the truth, even if the truth is not exactly
in the model family); BIC targets identifying the **true** model (it is consistent — if the true
model is among the candidates, BIC selects it with probability $\to1$ as $n\to\infty$ — whereas
AIC is not guaranteed to be model-selection-consistent in that sense).

## 5. The Theoretical Justification for Cross-Validation
$k$-fold cross-validation estimates generalization risk by repeatedly holding out a fold, training
on the rest, and averaging the held-out error. **Why this is a sound estimator of
$L_D(\hat h)$:** each fold's held-out error is computed on data the model being evaluated on that
fold never saw during its training — making it, for that fold, an unbiased estimate of the risk of
a model trained on $n(k-1)/k$ points (slightly fewer than the full $n$, so it has a small, usually
negligible pessimistic bias relative to the full-data model's true risk). Averaging over $k$ folds
reduces the variance of this estimate relative to a single train/test split, at the cost of $k$-fold
extra computation. Leave-one-out CV ($k=n$) has the least bias (each held-out model is trained on
$n-1$ points, nearly the full sample) but the highest variance among common choices (the $n$
held-out errors are highly correlated with each other, since the $n$ training sets overlap almost
entirely). **This is precisely the complementary piece to Weeks 1–5:** the PAC/VC/Rademacher
bounds of Weeks 2–4 give *worst-case, distribution-free* guarantees that hold uniformly over all
$h\in H$ before any data is seen; cross-validation gives an *empirical, data-dependent* estimate of
risk for the *one* $\hat h$ actually produced — the two are not competing theories of
generalization, but two different and complementary ways of controlling or estimating the same
underlying quantity, $L_D(\hat h)$.

## 6. Code: Empirically Decomposing Bias and Variance
```python
import numpy as np

def true_f(x):
    return np.sin(x)

def fit_polynomial(x, y, degree):
    coeffs = np.polyfit(x, y, degree)
    return np.poly1d(coeffs)

rng = np.random.default_rng(0)
x_test = np.linspace(-3, 3, 50)
f_test = true_f(x_test)
sigma = 0.3
n_train, n_resamples = 20, 500

for degree in [1, 3, 9]:
    preds = np.zeros((n_resamples, len(x_test)))
    for r in range(n_resamples):
        x_train = rng.uniform(-3, 3, n_train)
        y_train = true_f(x_train) + rng.normal(scale=sigma, size=n_train)
        model = fit_polynomial(x_train, y_train, degree)
        preds[r] = model(x_test)

    mean_pred = preds.mean(axis=0)
    bias_sq = np.mean((mean_pred - f_test) ** 2)
    variance = np.mean(preds.var(axis=0))
    print(f"degree={degree}: bias^2={bias_sq:.4f}  variance={variance:.4f}  "
          f"bias^2+variance+sigma^2={bias_sq+variance+sigma**2:.4f}")
```
Expect `degree=1` (too simple) to show high `bias^2`, low `variance`; `degree=9` (too flexible for
only 20 points) to show low `bias^2`, high `variance`; and `degree=3` (close to the problem's
effective complexity near the sampled range) to show the best balance — a direct empirical
reconstruction of Section 1's decomposition.

## 7. In-Class Exercise
Compute AIC and BIC (Sections 2–3) for two nested linear-regression models fit to the same
20-point dataset, one with $k=2$ parameters and one with $k=6$, given their fitted log-likelihoods
$\hat\ell=-15.0$ and $\hat\ell=-11.0$ respectively, and state which model each criterion selects.
