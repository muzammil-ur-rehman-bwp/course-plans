# Week 10 — Lecture Content: Distribution Shift and Domain Adaptation

## 1. Formalizing Covariate Shift
The graduate course's generalization theory (PAC/VC/Rademacher) assumes training and test data
are drawn from the **same** distribution. **Distribution shift** breaks this. The tractable
special case this week focuses on is **covariate shift**: training distribution
$P_{\mathrm{tr}}(x,y)$ and test distribution $P_{\mathrm{te}}(x,y)$ satisfy
```
P_tr(y|x) = P_te(y|x)      (the labeling function is unchanged)
P_tr(x)   ≠  P_te(x)        (only the covariate marginal shifts)
```
This is distinct from **label shift** (the marginal $P(y)$ changes but $P(x\mid y)$ is shared —
relevant when the *outcome* prevalence changes, e.g., a disease's base rate, but how the disease
manifests in features does not) and from fully general, **arbitrary** distribution shift (both
$P(x)$ and $P(y\mid x)$ may change arbitrarily between train and test) — in the fully general
case, no correction is possible without further structural assumptions, since nothing about the
test conditional $P_{\mathrm{te}}(y\mid x)$ can be inferred from training data alone if it is
allowed to differ arbitrarily. Covariate shift's assumption that $P(y\mid x)$ is preserved is
exactly what makes a principled correction possible.

## 2. Importance Weighting: The Correction, Derived
Define the **density ratio** (importance weight) $w(x) = P_{\mathrm{te}}(x)/P_{\mathrm{tr}}(x)$
(assuming $P_{\mathrm{tr}}(x)>0$ wherever $P_{\mathrm{te}}(x)>0$ — this is exactly the covariate-
shift analogue of Week 8's overlap condition, and is addressed in §5). Then, directly from the
definition of expectation:
```
E_{P_te}[ ℓ(f(x),y) ] = ∫ ℓ(f(x),y) P_te(x,y) dx dy
                       = ∫ ℓ(f(x),y) · (P_te(x)/P_tr(x)) · P_tr(x,y) dx dy    [using P_te(y|x)=P_tr(y|x)]
                       = ∫ ℓ(f(x),y) · w(x) · P_tr(x,y) dx dy
                       = E_{P_tr}[ w(x) · ℓ(f(x),y) ]
```
So **the test risk equals the $w(x)$-weighted training risk** — an exact identity, not an
approximation, under the covariate-shift and overlap assumptions. In practice, this licenses
training by minimizing the **importance-weighted empirical risk**
$\frac1n\sum_i \hat w(x_i)\,\ell(f(x_i),y_i)$ using training data alone, as an unbiased estimator
of the true test risk, correcting exactly for the covariate marginal's shift.

## 3. Estimating the Density Ratio via a Classifier
Directly estimating $P_{\mathrm{tr}}(x)$ and $P_{\mathrm{te}}(x)$ separately (both are typically
high-dimensional densities) is hard. The standard trick avoids it: label each training point with
a domain label $0$, each test (covariate-only, unlabeled-$y$ is fine) point with domain label $1$,
and train a probabilistic classifier $c(x)=\hat{\mathbb{P}}(\mathrm{domain}=1\mid x)$ to
distinguish them. By Bayes' rule,
```
P(domain=1|x) = P(x|domain=1)·P(domain=1) / P(x)
              = P_te(x)·P(domain=1) / ( P_te(x)·P(domain=1) + P_tr(x)·P(domain=0) )
```
Solving for the ratio $w(x)=P_{\mathrm{te}}(x)/P_{\mathrm{tr}}(x)$:
```
w(x) = ( c(x) / (1−c(x)) ) · ( P(domain=0) / P(domain=1) )
```
i.e., the estimated odds of the domain classifier, rescaled by the (known, from the sampling
design) ratio of domain-label base rates. This turns a density-ratio estimation problem into an
ordinary binary classification problem — directly reusing the graduate course's classifier
toolkit (e.g., logistic regression) for a new purpose.

## 4. Generalization Guarantees Under Shift, and Their Limits
The graduate course's Rademacher-complexity-based generalization bound (for same-distribution
train/test) can be extended to the covariate-shift setting, but picks up an explicit penalty term
reflecting how different $P_{\mathrm{tr}}(x)$ and $P_{\mathrm{te}}(x)$ are — schematically,
```
L_{P_te}(f) ≤ L̂_{w,S}(f) + O( Rademacher-complexity term ) + (divergence term between P_tr, P_te)
```
where the divergence term grows with a suitable measure of train/test mismatch (e.g., related to
an $f$-divergence or an integral probability metric between $P_{\mathrm{tr}}(x)$ and
$P_{\mathrm{te}}(x)$, scaled by how extreme the importance weights $w(x)$ become — precisely
captured by quantities like $\mathbb{E}_{P_{\mathrm{tr}}}[w(x)^2]$, which blows up exactly when
overlap is poor). **Why there is "no free lunch" for arbitrary shift:** if $P_{\mathrm{te}}(x)$
can place mass anywhere $P_{\mathrm{tr}}(x)$ has none, no amount of training data constrains
$P_{\mathrm{te}}(y\mid x)$ there at all (even under the covariate-shift assumption that $P(y|x)$
is shared, there is no training data to estimate it from in that region) — so a generalization
guarantee *requires* bounded importance weights (equivalently, sufficient overlap) to be
non-vacuous. Covariate shift with bounded, well-estimated weights is exactly the tractable special
case where meaningful guarantees exist; it is not a fully general solution to domain adaptation.

## 5. The Overlap Failure Mode
When $P_{\mathrm{tr}}(x)$ is very small (but non-zero) in some region where $P_{\mathrm{te}}(x)$
is not negligible, $w(x) = P_{\mathrm{te}}(x)/P_{\mathrm{tr}}(x)$ becomes very large there — a
handful of training points in that region receive enormous weight, dominating the weighted
empirical risk and driving up its variance (in the limit $P_{\mathrm{tr}}(x)\to 0$ with
$P_{\mathrm{te}}(x)$ fixed and positive, the weight — and the estimator's variance — diverges).
This is a **structural** limit of domain adaptation, directly implied by §4's divergence term: no
algorithmic cleverness removes it, because it reflects a genuine lack of training information
about test-relevant regions, not merely a correctable implementation detail.

## 6. Python: Density-Ratio Estimation and the Overlap Failure Mode
```python
import numpy as np
from sklearn.linear_model import LogisticRegression

rng = np.random.default_rng(0)

def covariate_shift_experiment(train_shift_strength, n=4000):
    X_tr = rng.normal(0, 1, size=n)
    X_te = rng.normal(train_shift_strength, 1, size=n)   # test covariates shifted in mean
    y_fn = lambda x: 2.0 + 1.5 * x
    y_tr = y_fn(X_tr) + rng.normal(0, 1, size=n)
    y_te = y_fn(X_te) + rng.normal(0, 1, size=n)

    X_domain = np.concatenate([X_tr, X_te]).reshape(-1, 1)
    domain_label = np.concatenate([np.zeros(n), np.ones(n)])
    clf = LogisticRegression().fit(X_domain, domain_label)
    c_tr = np.clip(clf.predict_proba(X_tr.reshape(-1, 1))[:, 1], 1e-3, 1 - 1e-3)
    w = c_tr / (1 - c_tr)                                  # base rates equal (n==n), ratio omitted

    unweighted_risk = np.mean((y_fn(X_tr) - (2.0 + 1.5 * X_tr)) ** 2)  # ~ irreducible noise only
    weighted_mean_w = w.mean()
    weight_variance = w.var()
    print(f"shift={train_shift_strength:4.1f}  mean(w)={weighted_mean_w:7.2f}  "
          f"var(w)={weight_variance:10.2f}  max(w)={w.max():8.2f}")

for shift in [0.5, 1.5, 3.0, 5.0]:
    covariate_shift_experiment(shift)
```
As `train_shift_strength` increases (training and test covariate distributions drift further
apart, with worsening overlap), the importance weights' mean, variance, and maximum should all
grow sharply — the direct empirical signature of the §5 failure mode.

## 7. In-Class/Lab Exercise
Using the §6 setup at `shift=3.0`, fit a linear model by (a) ordinary (unweighted) least squares
on the training data and (b) importance-weighted least squares using the estimated $w(x)$, then
evaluate both models' mean-squared error on a freshly sampled test set at the same shift. Report
which model performs better and relate the result, in 2–3 sentences, to the weight-variance
diagnostic from §6 at that shift level — if weights are extremely high-variance, importance
weighting's theoretical unbiasedness may be undercut in practice by its own high variance, a
direct illustration of §4–5's "structural limit" point.
