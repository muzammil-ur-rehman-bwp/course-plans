# Week 13 — Lecture Content: Causal Inference Basics

## 1. Correlation vs. Causation
Two variables $X,Y$ are **correlated** if they are statistically associated — knowing $X$ changes
your belief about $Y$. $X$ **causes** $Y$ if actively *changing* $X$ (holding everything else
fixed) would change $Y$. Correlation can arise from causation in either direction, from a common
cause (confounding), or from selection effects — observational data alone, without further
assumptions, cannot distinguish these cases. This is not a minor technicality: a predictive model
trained purely to exploit correlation can be highly accurate for *prediction* under the same
data-generating conditions, yet give exactly the wrong answer for a *decision* that intervenes on
a feature.

## 2. Confounding
A **confounder** $Z$ is a common cause of both $X$ and $Y$ that, if not accounted for, creates an
association between $X$ and $Y$ even when neither causes the other (or distorts the size/sign of
a real causal effect). Formally, if the true causal structure is $Z\to X$ and $Z\to Y$ with no
direct $X\to Y$ edge, $X$ and $Y$ are still statistically dependent (through $Z$), and a naive
regression of $Y$ on $X$ alone will report a spurious "effect."

## 3. Simpson's Paradox — A Fully Worked Numerical Example
Consider a (fictional) study of two treatments, A and B, with recovery counts stratified by
patient condition severity:

| | Mild cases | | Severe cases | | **Overall** | |
|---|---|---|---|---|---|---|
| | Recovered / Total | Rate | Recovered / Total | Rate | Recovered / Total | Rate |
| Treatment A | 81 / 87 | 93.1% | 192 / 263 | 73.0% | 273 / 350 | 78.0% |
| Treatment B | 234 / 270 | 86.7% | 55 / 80 | 68.8% | 289 / 350 | 82.6% |

Within **each** severity stratum, Treatment A has the **higher** recovery rate (93.1% > 86.7% for
mild; 73.0% > 68.8% for severe). Yet **overall**, Treatment B looks better (82.6% > 78.0%). This
is Simpson's paradox: the aggregate trend **reverses** the within-stratum trend. **Why:**
Treatment A was given to far more severe cases (263 of 350, or 75%) than Treatment B (80 of 350, or
23%) — severity is a confounder of the treatment/outcome relationship here, because it affects
both which treatment a patient was likely to receive (sicker patients were preferentially given
Treatment A, perhaps because it was considered the more aggressive option) and the outcome
(severe cases recover less often regardless of treatment). Aggregating over severity mixes these
two unequal subpopulations and produces a misleading overall comparison. **The correct
comparison is the stratified one** (which uniformly favors Treatment A) — unless severity is
itself a *consequence* of treatment choice (a mediator, not a confounder), in which case the
correct analysis could differ again; this is exactly why identifying the causal role of a variable
(confounder vs. mediator vs. collider) is a prerequisite for correctly interpreting data, not an
afterthought.

## 4. Causal Graphs (Conceptual)
A **causal directed acyclic graph (DAG)** encodes assumed causal structure: an edge $A\to B$ means
$A$ is assumed to be a direct cause of $B$. Three canonical three-variable patterns matter for
reasoning about bias:
- **Confounder:** $Z\to X$, $Z\to Y$ (as in Section 3) — creates spurious $X$–$Y$ association;
  *should* be adjusted for.
- **Mediator:** $X\to Z\to Y$ — $Z$ lies on the causal pathway from $X$ to $Y$; adjusting for it
  would (incorrectly) remove part of $X$'s true causal effect on $Y$.
- **Collider:** $X\to Z\leftarrow Y$ — conditioning on $Z$ can *create* a spurious association
  between $X$ and $Y$ that did not exist unconditionally (a form of **selection bias**).
The same adjustment (controlling for $Z$) is correct for a confounder and actively wrong for a
mediator or a collider — so drawing the DAG, and reasoning about *which* of these three patterns
applies, is a prerequisite to deciding whether to adjust for a variable at all.

## 5. Interventions and Do-Notation (Conceptual)
Pearl's **do-notation** distinguishes the purely observational conditional probability $P(Y\mid
X=x)$ ("what is $Y$'s distribution among the subpopulation that happened to have $X=x$") from the
**interventional** distribution $P(Y\mid \mathrm{do}(X=x))$ ("what would $Y$'s distribution be if
we *forced* $X=x$ on the whole population, severing whatever normally determines $X$"). In the
presence of a confounder $Z$, these two quantities generally differ — $P(Y\mid X=x)$ mixes in
$Z$'s influence on who ends up with $X=x$, while $P(Y\mid\mathrm{do}(X=x))$ does not, by
definition, since the intervention bypasses $Z$'s influence on $X$ entirely. This course
introduces $\mathrm{do}(\cdot)$ only as a conceptual notation for "what an intervention would
show"; the formal rules (e.g., the backdoor criterion, do-calculus) for computing interventional
quantities from observational data and a causal graph are **not** covered here — this is a
one-week conceptual foundation, not a full causal-inference course.

## 6. Why This Matters for Trustworthy ML
A model trained to minimize predictive loss learns whatever statistical regularities are present
in the training distribution — including confounded, spurious ones — with no mechanism to
distinguish a causal pattern from a merely correlational one. This matters whenever a model's
output is used to justify an **intervention** (e.g., "should we change this feature/policy to
improve this outcome?") rather than a passive prediction under unchanged conditions, and it also
matters for fairness: a historical, confounded correlation between a sensitive attribute and an
outcome can be learned and reproduced by a model as if it were a stable, causal pattern, laundering
the confounding into an apparently "data-driven" decision.

## 7. Code: Reproducing the Simpson's Paradox Numbers
```python
import numpy as np
import pandas as pd

data = pd.DataFrame({
    "treatment": ["A"]*350 + ["B"]*350,
    "severity":  (["mild"]*87 + ["severe"]*263) + (["mild"]*270 + ["severe"]*80),
    "recovered": (
        [1]*81 + [0]*6 + [1]*192 + [0]*71          # Treatment A: mild then severe
        + [1]*234 + [0]*36 + [1]*55 + [0]*25        # Treatment B: mild then severe
    ),
})

stratified = data.groupby(["treatment", "severity"])["recovered"].mean().unstack()
overall = data.groupby("treatment")["recovered"].mean()

print("Recovery rate by treatment and severity stratum:\n", stratified.round(3), "\n")
print("Overall recovery rate by treatment:\n", overall.round(3))
print("\nWithin every stratum, A > B:",
      bool((stratified.loc["A"] > stratified.loc["B"]).all()))
print("Overall, A > B:", bool(overall["A"] > overall["B"]))
```
Expect the printout to confirm Treatment A wins in both strata individually, while Treatment B
wins overall — reproducing Section 3's paradox numerically from raw per-patient records, and
making clear that the reversal is a pure consequence of the unequal severity mix, not of any
arithmetic error.

## 8. In-Class Exercise
Using the causal-graph patterns in Section 4, classify "severity" in the Section 3 example as a
confounder, mediator, or collider, and explain in one sentence why the stratified comparison
(not the aggregate one) is the one that correctly isolates the treatments' effect, given that
classification.
