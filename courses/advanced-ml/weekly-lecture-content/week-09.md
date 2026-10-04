# Week 9 — Lecture Content: Causal Inference in Depth II — Instrumental Variables and Do-Calculus

*(First portion of this week's session is the Midterm Exam, covering Weeks 1–8. The lecture
content below follows the exam.)*

## 1. When Unconfoundedness Fails: Motivating Instrumental Variables
Week 8's propensity-score/IPW approach requires unconfoundedness — no *unobserved* confounder
between treatment and outcome. When an unobserved confounder $U$ affects both $T$ and $Y$ (e.g.,
unmeasured health status affecting both whether a patient seeks a treatment and their outcome),
IPW is biased regardless of how well the propensity score is estimated, because $\hat e(X)$ can
only ever adjust for *observed* $X$. **Instrumental variables (IV)** provide an alternative
identification strategy that tolerates this unobserved confounding, given a suitable instrument.

## 2. The Three IV Assumptions
A variable $Z$ is a valid **instrument** for the effect of $T$ on $Y$ if:
1. **Relevance:** $Z$ affects $T$ (i.e., $\mathrm{Cov}(Z,T)\neq 0$) — the instrument actually
   moves the treatment.
2. **Exclusion restriction:** $Z$ affects $Y$ *only* through $T$ — there is no direct arrow
   $Z\to Y$ and no back-door path from $Z$ to $Y$ other than through $T$.
3. **Independence from the confounder:** $Z$ is independent of the unobserved confounder $U$
   (equivalently, $Z$ is "as good as randomly assigned" with respect to whatever drives the
   $T$–$Y$ confounding).
Under these three conditions, $Z$ identifies the causal effect of $T$ on $Y$ even though $T$
itself remains confounded by $U$.

## 3. The Linear IV Identification Argument
Consider the linear structural model $Y = \beta T + \gamma U + \epsilon_Y$ and
$T = \delta Z + \lambda U + \epsilon_T$, where $U$ is unobserved and confounds $T,Y$, and $Z$
satisfies the three IV assumptions (in particular $\mathrm{Cov}(Z,U)=0$ and $Z$ has no direct
effect on $Y$). Then:
```
Cov(Z, Y) = Cov(Z, βT + γU + ε_Y) = β·Cov(Z,T) + γ·Cov(Z,U) + Cov(Z,ε_Y)
```
By the exclusion restriction and instrument independence, $\mathrm{Cov}(Z,U)=0$ and
$\mathrm{Cov}(Z,\epsilon_Y)=0$ (no path from $Z$ to $Y$'s idiosyncratic noise), so this collapses
to $\mathrm{Cov}(Z,Y) = \beta\cdot\mathrm{Cov}(Z,T)$. Since relevance guarantees
$\mathrm{Cov}(Z,T)\neq0$, we can solve directly:
```
β = Cov(Z, Y) / Cov(Z, T)
```
This identifies the causal effect $\beta$ purely from observable covariances of $Z$, $T$, and
$Y$ — **without ever observing $U$** — which is exactly the sense in which IV "gets around"
unobserved confounding where the Week 8 approach cannot. In practice, this is estimated via
**two-stage least squares (2SLS)**: regress $T$ on $Z$ to get fitted values $\hat T$ (stage 1),
then regress $Y$ on $\hat T$ (stage 2); the stage-2 coefficient is exactly the sample analogue of
$\mathrm{Cov}(Z,Y)/\mathrm{Cov}(Z,T)$ above.

## 4. The Do-Calculus
Pearl's **do-calculus** builds rigorously on the graduate course's conceptual-only
$P(Y\mid\mathrm{do}(X))$ introduction, giving three precise syntactic rules for manipulating
interventional expressions on a causal DAG $G$, for disjoint node sets $X,Y,Z,W$. Write
$G_{\overline X}$ for $G$ with all edges *into* $X$ deleted (simulating that $X$ was externally
set, cutting off its usual causes), and $G_{\underline X}$ for $G$ with all edges *out of* $X$
deleted.

- **Rule 1 (Insertion/deletion of observations):**
  $P(y\mid\mathrm{do}(x),z,w) = P(y\mid\mathrm{do}(x),w)$ if $Y\perp Z \mid X,W$ in
  $G_{\overline X}$.
  *(An observed variable $Z$ can be dropped from the conditioning set if it is
  $d$-separated from $Y$, given $X,W$, once $X$'s incoming edges are removed — i.e., $Z$ carries
  no information about $Y$ beyond what $X,W$ already give, in the post-intervention world.)*
- **Rule 2 (Action/observation exchange):**
  $P(y\mid\mathrm{do}(x),\mathrm{do}(z),w) = P(y\mid\mathrm{do}(x),z,w)$ if $Y\perp Z\mid X,W$ in
  $G_{\overline X\,\underline Z}$.
  *(An intervention $\mathrm{do}(z)$ can be replaced by passively observing $Z=z$ — i.e.,
  $Z$'s causal effect on $Y$ is indistinguishable from $Z$'s mere statistical association with
  $Y$ — exactly when $Z$ is $d$-separated from $Y$ in the graph with both $X$'s incoming and
  $Z$'s outgoing edges removed — intuitively, when no back-door path from $Z$ to $Y$ remains once
  $Z$'s own downstream causal effects are cut off.)*
- **Rule 3 (Insertion/deletion of actions):**
  $P(y\mid\mathrm{do}(x),\mathrm{do}(z),w) = P(y\mid\mathrm{do}(x),w)$ if $Y\perp Z\mid X,W$ in
  $G_{\overline X\,\overline{Z(W)}}$, where $Z(W)$ denotes the subset of nodes in $Z$ that are
  *not* ancestors of any node in $W$ (in $G_{\overline X}$).
  *(An intervention $\mathrm{do}(z)$ can simply be dropped — it has no effect on $Y$ at all —
  when $Z$ is $d$-separated from $Y$ in the graph with $X$'s incoming edges, and the incoming
  edges of any non-$W$-ancestor part of $Z$, removed.)*

**Why the rules are sound.** Each rule is a direct translation of a $d$-separation statement in
a graph that has been "surgically" modified to reflect exactly what an intervention does
structurally (cutting incoming edges to a do-variable removes its usual causes, formalizing that
it was *set*, not *caused*, in that state) — soundness is inherited from the standard
$d$-separation-implies-conditional-independence theorem for DAGs (assumed background: the
graduate course's conceptual causal-graph introduction). **Why the rules are complete:** Pearl's
completeness theorem (stated here, proof omitted) establishes that if a causal query
$P(y\mid\mathrm{do}(x))$ is identifiable from a DAG $G$ at all (expressible purely in terms of the
observed joint distribution), then some finite sequence of applications of Rules 1–3 reduces it to
such a purely observational expression — so the do-calculus is not just a useful heuristic, it is
a *complete* procedure for identification whenever identification is possible, and a failure to
find such a sequence (after exhausting the relevant cases) is itself a certificate of
non-identifiability.

## 5. Worked Example 1: An Identifiable Confounded Graph
Graph: $X \leftarrow Z \rightarrow Y$, $X\rightarrow Y$ (a single observed confounder $Z$ of
$X,Y$, plus $X$'s own direct effect on $Y$). Goal: identify $P(y\mid\mathrm{do}(x))$.
```
P(y|do(x)) = Σ_z P(y|do(x), z) P(z|do(x))                         [law of total probability]
           = Σ_z P(y|do(x), z) P(z)                                [Rule 3: Z ⟂ X in G_X̄ (no path X→Z); drop do(x)]
           = Σ_z P(y|x, z) P(z)                                    [Rule 2: Y ⟂ X | Z in G_X̲ (X's only path to Y is the direct edge, now cut); exchange do(x) for observing x]
```
This recovers exactly the familiar **back-door adjustment formula**
$P(y\mid\mathrm{do}(x)) = \sum_z P(y\mid x,z)P(z)$ — now derived from first principles via the
do-calculus rules, rather than asserted conceptually as in the graduate course.

## 6. Worked Example 2: A Non-Identifiable Graph
Graph: $X \leftarrow U \rightarrow Y$, $X\rightarrow Y$, with $U$ **unobserved** (no graph node
available to condition or sum over). Attempting the §5 derivation fails at the first step: there
is no observed $Z$ that blocks the back-door path $X \leftarrow U \rightarrow Y$, and no sequence
of Rules 1–3 can introduce one (the rules only ever manipulate conditioning/action sets using
*existing*, *observed* graph nodes). $P(y\mid\mathrm{do}(x))$ is therefore **not identifiable**
from the observational distribution over $X,Y$ alone in this graph — exactly the scenario that
motivates needing an instrument $Z$ satisfying §2's three conditions (which, if added to this
graph as $Z\to X$ with no other edges to $Y$ or $U$, *would* restore identifiability via the IV
route of §3, a different identification strategy than back-door adjustment).

## 7. Python: A From-Scratch Two-Stage-Least-Squares IV Estimator
```python
import numpy as np

rng = np.random.default_rng(0)
n = 5000
true_beta = 2.0

U = rng.normal(0, 1, size=n)                 # unobserved confounder
Z = rng.normal(0, 1, size=n)                 # valid instrument
T = 0.9 * Z + 0.7 * U + rng.normal(0, 0.5, size=n)     # Z affects T (relevance); U confounds
Y = true_beta * T + 1.2 * U + rng.normal(0, 0.5, size=n)  # U also affects Y; no direct Z->Y edge

ols_beta = np.cov(T, Y)[0, 1] / np.var(T)                      # naive OLS: biased by U

# Two-stage least squares
T_hat = np.polyval(np.polyfit(Z, T, 1), Z)                      # stage 1: regress T on Z
iv_beta = np.cov(T_hat, Y)[0, 1] / np.var(T_hat)                 # stage 2: regress Y on T_hat
# equivalently, the direct covariance-ratio identity:
iv_beta_direct = np.cov(Z, Y)[0, 1] / np.cov(Z, T)[0, 1]

print(f"True beta:        {true_beta:.3f}")
print(f"Naive OLS beta:    {ols_beta:.3f}  (biased by unobserved confounder U)")
print(f"2SLS IV beta:      {iv_beta:.3f}")
print(f"Direct Cov-ratio:  {iv_beta_direct:.3f}  (should match 2SLS closely)")
```
The naive OLS estimate should be visibly biased away from `true_beta` (inflated here, since $U$
drives $T$ and $Y$ in the same direction), while both IV estimates should sit close to the true
value, confirming the §3 identity empirically.

## 8. In-Class/Lab Exercise
By hand, apply the do-calculus rules to the graph $X\to M\to Y$, $X\to Y$ (a mediator $M$ plus a
direct effect, no confounding at all) to confirm $P(y\mid\mathrm{do}(x)) = P(y\mid x)$ reduces
immediately (Rule 2, with an empty adjustment set, since there is no back-door path from $X$ to
$Y$ at all once $X$'s incoming edges are cut — there are none to cut, as $X$ has no parents). Then
contrast this with Worked Example 1, where an adjustment set was required. State in one sentence
why a mediator (on a front-door, causal path) is treated completely differently from a confounder
(on a back-door, non-causal path) by the do-calculus.
