# Week 11 — Lecture Content: Algorithmic Fairness

## 1. Formal Fairness Criteria
Let $\hat Y$ be a classifier's (or thresholded score's) prediction, $Y$ the true outcome, and $A$
a protected attribute taking values in a finite set of groups. Three widely used formal criteria:

- **Demographic (statistical) parity:** $\mathbb{P}(\hat Y=1\mid A=a)$ is equal across all groups
  $a$ — the *rate* of positive predictions does not depend on group membership, regardless of the
  true outcome $Y$.
- **Equalized odds:** $\mathbb{P}(\hat Y=1\mid Y=y, A=a)$ is equal across groups, for each
  $y\in\{0,1\}$ separately — equivalently, equal true-positive rate ($\mathrm{TPR} =
  \mathbb{P}(\hat Y=1\mid Y=1)$) and equal false-positive rate ($\mathrm{FPR}=\mathbb{P}(\hat Y=1
  \mid Y=0)$) across groups.
- **Calibration (within groups):** $\mathbb{P}(Y=1\mid \hat Y=s, A=a)=s$ for every score value $s$
  and group $a$ — a predicted score of $s$ means the same thing (the same true probability of
  $Y=1$) regardless of group. The single-threshold special case, **predictive parity**
  (equal positive predictive value, $\mathrm{PPV}=\mathbb{P}(Y=1\mid\hat Y=1)$, across groups), is
  implied by full calibration at the threshold's score level and is what the derivation below uses
  directly (any argument that shows predictive parity and equalized odds are incompatible also
  shows full calibration and equalized odds are incompatible, since calibration is the stronger
  condition).

These are not merely three arbitrary options: each formalizes a different, independently
reasonable intuition about what "fair" should mean (equal treatment rates; equal error rates by
true outcome; a score that means the same thing everywhere) — which is exactly why it matters that
they can conflict.

## 2. The PPV Identity
Let $p_a = \mathbb{P}(Y=1\mid A=a)$ be group $a$'s **base rate**. By Bayes' rule, for a classifier
with true-positive rate $\mathrm{TPR}_a$ and false-positive rate $\mathrm{FPR}_a$ within group $a$:
```
PPV_a = P(Y=1 | Ŷ=1, A=a)
      = P(Ŷ=1|Y=1,A=a)·P(Y=1|A=a) / P(Ŷ=1|A=a)
      = p_a·TPR_a / ( p_a·TPR_a + (1−p_a)·FPR_a )
```
(the denominator follows from the law of total probability:
$\mathbb{P}(\hat Y=1\mid A=a) = p_a\,\mathrm{TPR}_a + (1-p_a)\,\mathrm{FPR}_a$).

## 3. Monotonicity in the Base Rate
Fix $\mathrm{TPR}>0$ and $\mathrm{FPR}>0$ (a non-degenerate classifier that produces both some true
and some false positives), and consider $f(p) = p\cdot\mathrm{TPR} / (p\cdot\mathrm{TPR} +
(1-p)\cdot\mathrm{FPR})$ as a function of $p\in(0,1)$. Rewriting,
```
f(p) = 1 / ( 1 + ((1−p)/p)·(FPR/TPR) )
```
As $p$ increases, $(1-p)/p$ strictly decreases (it is a strictly decreasing function of $p$ on
$(0,1)$), so, since $\mathrm{FPR}/\mathrm{TPR}>0$ is a fixed positive constant, the denominator
$1+((1-p)/p)(\mathrm{FPR}/\mathrm{TPR})$ strictly decreases, and hence $f(p)$ **strictly
increases** in $p$. In particular, $f$ is injective (one-to-one) on $(0,1)$ whenever
$\mathrm{TPR},\mathrm{FPR}>0$: two different base rates can never produce the same PPV under a
fixed, shared $(\mathrm{TPR},\mathrm{FPR})$ pair.

## 4. The Impossibility Result
Suppose a classifier satisfies **equalized odds** across two groups $a,b$ (so
$\mathrm{TPR}_a=\mathrm{TPR}_b=\mathrm{TPR}$ and $\mathrm{FPR}_a=\mathrm{FPR}_b=\mathrm{FPR}$) and
**predictive parity** (so $\mathrm{PPV}_a=\mathrm{PPV}_b$). By §2, $\mathrm{PPV}_a=f(p_a)$ and
$\mathrm{PPV}_b=f(p_b)$ for the *same* function $f$ (since $\mathrm{TPR},\mathrm{FPR}$ are shared
across groups by the equalized-odds assumption). By §3's injectivity (whenever
$\mathrm{TPR},\mathrm{FPR}>0$),
```
PPV_a = PPV_b   ⟹   f(p_a) = f(p_b)   ⟹   p_a = p_b
```
**Contrapositive statement (the impossibility result, following Chouldechova 2017 and
Kleinberg–Mullainathan–Raghavan 2016):** if the groups' base rates differ ($p_a\neq p_b$), then a
classifier cannot simultaneously satisfy equalized odds and predictive parity/calibration, *unless*
it is degenerate in the sense ruled out by §3's non-degeneracy assumption — specifically, unless
$\mathrm{FPR}=0$ (so $f(p)\equiv 1$ regardless of $p$, i.e. the classifier never produces a false
positive — a form of "perfect" precision) or $\mathrm{TPR}=0$ (the classifier never predicts
positive on an actual positive, a degenerate/useless classifier). Excluding these degenerate
edge cases, **the three criteria of §1 — equalized odds, calibration, and (as a consequence of
equal base rates being required) implicitly even demographic parity whenever true base rates
genuinely differ by group — cannot all be satisfied simultaneously whenever groups have different
base rates.** This is a genuine mathematical incompatibility, not an engineering gap awaiting a
cleverer algorithm: no classifier, however sophisticated, escapes this algebra once base rates
differ and the classifier is informative.

## 5. The Practical Implication
Because the impossibility result is unconditional (given differing base rates and an informative
classifier), choosing which fairness criterion to prioritize is **necessarily a value-laden,
context-dependent decision**, not a purely technical optimization that a better model or more
data could resolve. For example, a recidivism-risk score calibrated to mean the same thing across
racial groups (predictive parity/calibration) will, whenever base rates differ across those
groups, necessarily have unequal false-positive or false-negative rates between them
(violating equalized odds) — and vice versa, enforcing equal error rates will break calibration.
Neither choice is "more correct" in a purely mathematical sense; the choice reflects which kind of
unfairness a given application is more willing to accept, and this is squarely a normative
judgment the modeling choice cannot avoid making, implicitly or explicitly.

## 6. Python: Computing Fairness Metrics and Confirming the Impossibility Result
```python
import numpy as np

rng = np.random.default_rng(0)

def simulate_group(n, base_rate, tpr, fpr, rng):
    Y = rng.binomial(1, base_rate, size=n)
    Yhat = np.where(Y == 1, rng.binomial(1, tpr, size=n), rng.binomial(1, fpr, size=n))
    return Y, Yhat

def fairness_metrics(Y, Yhat):
    ppv = Yhat[Yhat == 1].size and Y[Yhat == 1].mean()
    tpr = Y[Y == 1].size and Yhat[Y == 1].mean()
    fpr = Y[Y == 0].size and Yhat[Y == 0].mean()
    dp = Yhat.mean()
    return {"PPV": ppv, "TPR": tpr, "FPR": fpr, "demographic_parity_rate": dp}

n = 20000
tpr, fpr = 0.75, 0.15                          # SAME TPR/FPR enforced across both groups
Y_a, Yhat_a = simulate_group(n, base_rate=0.10, tpr=tpr, fpr=fpr, rng=rng)
Y_b, Yhat_b = simulate_group(n, base_rate=0.40, tpr=tpr, fpr=fpr, rng=rng)

metrics_a = fairness_metrics(Y_a, Yhat_a)
metrics_b = fairness_metrics(Y_b, Yhat_b)
print("Group A (base rate 0.10):", {k: round(v, 3) for k, v in metrics_a.items()})
print("Group B (base rate 0.40):", {k: round(v, 3) for k, v in metrics_b.items()})

def ppv_formula(p, tpr, fpr):
    return p * tpr / (p * tpr + (1 - p) * fpr)

print("Predicted PPV_A:", round(ppv_formula(0.10, tpr, fpr), 3))
print("Predicted PPV_B:", round(ppv_formula(0.40, tpr, fpr), 3))
```
Even though TPR and FPR are *enforced identically* for both groups (equalized odds holds exactly
by construction), the printed PPVs for Group A and Group B should differ substantially, matching
the `ppv_formula` prediction — the direct empirical confirmation that equalized odds and
predictive parity/calibration cannot coexist when base rates differ.

## 7. In-Class/Lab Exercise
Using the §6 simulation, now instead *enforce* equal PPV across the two groups (by finding, for
Group B, a TPR/FPR pair with the same PPV as Group A's default TPR=0.75, FPR=0.15 — several pairs
work; pick one with FPR fixed at 0.15 and solve for the required TPR via the §2 formula), and
report the resulting TPR/FPR gap between groups. Confirm the gap is non-zero, completing the
other direction of the impossibility result (enforcing calibration breaks equalized odds, just as
enforcing equalized odds broke calibration in §6).
