# Week 8 — Lecture Content: Causal Inference in Depth I — Potential Outcomes and Propensity Scores

## 1. From Conceptual to Rigorous: What the Graduate Course Assumed
The graduate course introduced correlation vs. causation, confounding, Simpson's paradox, and
do-notation **conceptually only** — enough to recognize a confounded scenario and to read
$P(Y\mid\mathrm{do}(X=x))$ as "what $Y$ would be if we set $X=x$." None of that machinery was
formalized or used to derive an actual estimator. This week and next make it rigorous and
operational.

## 2. The Potential-Outcomes (Rubin Causal Model) Framework
For a binary treatment $T\in\{0,1\}$ and outcome $Y$, each unit $i$ has **two potential
outcomes**: $Y_i(1)$ (the outcome unit $i$ *would* have under treatment) and $Y_i(0)$ (the outcome
it *would* have under control). The **fundamental problem of causal inference**: only one is ever
observed, depending on the treatment unit $i$ actually received,
```
Y_i = T_i · Y_i(1) + (1 − T_i) · Y_i(0)
```
The **individual treatment effect** $Y_i(1)-Y_i(0)$ is therefore never directly observable for any
single unit. What *can* be estimated, under assumptions, is the population-level **average
treatment effect**:
```
ATE = E[ Y(1) − Y(0) ] = E[Y(1)] − E[Y(0)]
```

## 3. Formalizing Confounding: Unconfoundedness and Overlap
**Confounding**, stated precisely, is a violation of $T \perp (Y(0),Y(1))$ — treatment assignment
is statistically dependent on the potential outcomes themselves (e.g., sicker patients are more
likely to receive a treatment, so treated and untreated groups are not comparable even before any
causal effect is considered). The naive estimator $\mathbb{E}[Y\mid T=1]-\mathbb{E}[Y\mid T=0]$
equals the ATE only when $T\perp(Y(0),Y(1))$ marginally (as in a randomized experiment); under
confounding it is biased by the systematic difference between who gets treated and who does not.

Two assumptions license recovering the ATE from **observational** (non-randomized) data using
observed covariates $X$:
- **Unconfoundedness / conditional ignorability:** $T \perp (Y(0),Y(1)) \mid X$ — treatment
  assignment is as good as random *once we condition on* the observed covariates $X$ (no
  *unobserved* confounding remains after conditioning on $X$).
- **Overlap / positivity:** $0 < \mathbb{P}(T=1\mid X=x) < 1$ for (almost) every $x$ in the
  support of $X$ — every covariate stratum has some chance of receiving either treatment arm, so
  there are comparable treated and untreated units at every $x$.
Both are needed: unconfoundedness alone does not help if some covariate stratum is never (or
always) treated (no comparison is possible there); overlap alone does not help if treatment
assignment still depends on an *unobserved* factor correlated with the outcome.

## 4. The Propensity Score and Inverse-Propensity Weighting (IPW)
The **propensity score** is $e(x) = \mathbb{P}(T=1\mid X=x)$. The **IPW estimator** of the ATE is
built from the identity (derived below):
```
E[ T·Y / e(X)  −  (1−T)·Y / (1−e(X)) ]  =  ATE
```
**Derivation.** Using $Y = TY(1)+(1-T)Y(0)$, so $TY = TY(1)$ (since $T\in\{0,1\}$):
```
E[ T·Y / e(X) ] = E[ T·Y(1) / e(X) ]
                = E_X[ E[ T·Y(1) / e(X) | X ] ]                     (tower property)
                = E_X[ E[T|X]·E[Y(1)|X] / e(X) ]                   (unconfoundedness: T ⊥ Y(1) | X)
                = E_X[ e(X)·E[Y(1)|X] / e(X) ]                      (since E[T|X] = e(X))
                = E_X[ E[Y(1)|X] ]  =  E[Y(1)]
```
by the tower property of expectation and the definition of conditional expectation. The key step
is $\mathbb{E}[T\,Y(1)\mid X] = \mathbb{E}[T\mid X]\cdot\mathbb{E}[Y(1)\mid X]$, which holds
precisely *because* unconfoundedness gives $T\perp Y(1)\mid X$. The symmetric argument gives
$\mathbb{E}[(1-T)Y/(1-e(X))] = \mathbb{E}[Y(0)]$. Subtracting recovers the ATE identity above.
Overlap is what makes the estimator well-defined and low-variance in practice: $e(X)$ or
$1-e(X)$ appearing in a denominator near 0 makes the corresponding term blow up — so overlap is
not merely a technical nicety but is exactly what IPW's derivation (and finite-sample behavior)
needs.

**The estimator in practice.** Estimate $\hat e(x)$ from data (e.g., via logistic regression of
$T$ on $X$), then compute the **empirical IPW-ATE**:
```
ATE_hat = (1/n) Σ_i [ T_i Y_i / ê(X_i)  −  (1−T_i) Y_i / (1−ê(X_i)) ]
```

## 5. Python: Propensity-Score Estimation and IPW-Based ATE Estimation
```python
import numpy as np
from sklearn.linear_model import LogisticRegression

rng = np.random.default_rng(0)
n = 5000

X = rng.normal(0, 1, size=n)                               # observed confounder
true_propensity = 1 / (1 + np.exp(-(0.8 * X)))              # e(x): sicker (higher X) -> more likely treated
T = rng.binomial(1, true_propensity)

true_ATE = 2.0
Y0 = 1.0 + 1.5 * X + rng.normal(0, 1, size=n)               # Y(0): depends on X -> confounding
Y1 = Y0 + true_ATE                                           # constant treatment effect for clarity
Y = T * Y1 + (1 - T) * Y0

naive_diff = Y[T == 1].mean() - Y[T == 0].mean()

model = LogisticRegression().fit(X.reshape(-1, 1), T)
e_hat = model.predict_proba(X.reshape(-1, 1))[:, 1]
e_hat = np.clip(e_hat, 0.02, 0.98)                           # guard against near-zero overlap

ipw_ate = np.mean(T * Y / e_hat - (1 - T) * Y / (1 - e_hat))

print(f"True ATE:          {true_ATE:.3f}")
print(f"Naive diff-in-means: {naive_diff:.3f}  (biased by confounding via X)")
print(f"IPW-based ATE:       {ipw_ate:.3f}  (should be much closer to the true ATE)")
```
The naive difference-in-means should be visibly biased away from `true_ATE` (inflated, since $X$
drives both treatment probability and $Y(0)$ in the same direction here), while the IPW estimate
should sit close to the true ATE — the direct empirical confirmation of the §4 identity.

## 6. In-Class/Lab Exercise
Repeat the §5 simulation with a deliberately poor-overlap propensity function (e.g.,
`true_propensity = 1/(1+exp(-(3*X)))`, which pushes $e(x)$ toward 0 or 1 for even moderate $|X|$),
and report how the IPW estimate's variance across 200 repeated simulated datasets compares to the
good-overlap case — a direct, empirical demonstration of why overlap is not a mere technicality.

## 7. Midterm Review (Weeks 1–8)
Recap map: postgraduate overview and scope (Week 1) → minimax risk and Fano's inequality (Week 2)
→ sub-Gaussian/sub-exponential concentration and Bernstein's inequality (Week 3) → random matrix
theory and the Marchenko–Pastur law (Week 4) → full-information OCO, FTRL, and OGD's regret bound
(Week 5) → the Dirichlet process and stick-breaking (Week 6) → the Chinese Restaurant Process and
infinite mixtures (Week 7) → potential outcomes, confounding formalized, and propensity-score/IPW
estimation (Week 8). The midterm (Week 9) draws on all eight weeks and is qualifying-exam style:
expect derivation-reproduction questions (e.g., "derive the OGD regret bound"), not only
definitional recall.
