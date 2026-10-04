# Week 2 — Lecture Content: PAC Learning in Depth

## 1. The PAC Learnability Definition
A hypothesis class $H$ is **Probably Approximately Correct (PAC) learnable** if there exists a
learning algorithm $A$ and a function $m_H(\epsilon,\delta)$ such that: for every $\epsilon,\delta
\in(0,1)$, every distribution $D$ over $\mathcal{X}\times\mathcal{Y}$ for which some $h^\star\in H$
achieves $L_D(h^\star)=0$ (the **realizability assumption**), and every sample size $m\geq
m_H(\epsilon,\delta)$, running $A$ on $m$ i.i.d. examples from $D$ returns $\hat h$ satisfying
$$
\Pr_{S\sim D^m}\big[\,L_D(\hat h)\leq\epsilon\,\big]\;\geq\;1-\delta .
$$
Read this as: "approximately correct" ($L_D(\hat h)\leq\epsilon$) holds "probably" (with
probability $\geq1-\delta$ over the random training sample). $H$ is PAC learnable only if
$m_H(\epsilon,\delta)$ is **polynomial** in $1/\epsilon$, $1/\delta$, and (for infinite classes) a
complexity measure of $H$ — an exponential sample requirement would make the guarantee
practically useless. This week derives $m_H$ for the simplest case: finite $H$.

## 2. Setup for the Finite-Class Bound
Call $h\in H$ **bad** if $L_D(h)>\epsilon$. ERM fails (outputs an $\hat h$ with $L_D(\hat h)>
\epsilon$) only if *some* bad hypothesis achieves zero empirical risk on $S$ (under realizability,
ERM always finds *some* $h$ with $\widehat{L}_S(h)=0$, since $h^\star$ achieves it; ERM could pick
a bad $h$ only if a bad $h$ also achieves $\widehat{L}_S(h)=0$ and is returned instead). So:
$$
\Pr_S\big[L_D(\hat h)>\epsilon\big] \;\leq\; \Pr_S\Big[\exists\, h\in H:\; L_D(h)>\epsilon \text{ and } \widehat{L}_S(h)=0\Big].
$$

## 3. Step 1 — Bound One Bad Hypothesis via Hoeffding
Fix one bad $h$ with $L_D(h)>\epsilon$. Each $\ell(h(x_i),y_i)$ is an i.i.d. Bernoulli-type
variable in $\{0,1\}$ with mean $L_D(h)$ (for 0/1 loss). Hoeffding's inequality (proved in full in
Week 5; used here as a tool) gives, for i.i.d. $[0,1]$-bounded variables with mean $\mu$,
$$
\Pr\big[\widehat{L}_S(h) \leq \mu - t\big] \leq e^{-2mt^2}.
$$
Set $t=\mu=L_D(h)$ so the event $\widehat{L}_S(h)=0$ is exactly $\widehat{L}_S(h)\leq \mu-t$ for
this $t$ (since $L_D(h)>\epsilon$ we also have $t>\epsilon$, so this is an upper bound on a rarer
event, hence still valid):
$$
\Pr_S\big[\widehat{L}_S(h)=0\big] \;\leq\; e^{-2m\epsilon^2}\qquad\text{(using } \mu>\epsilon\text{, so } t>\epsilon\text{)}.
$$
In words: a hypothesis that is genuinely bad ($L_D(h)>\epsilon$) is exponentially unlikely, in the
sample size $m$, to look perfect ($\widehat{L}_S(h)=0$) on the sample.

## 4. Step 2 — Union Bound Over All of $H$
A union bound over the (at most $|H|$) bad hypotheses gives
$$
\Pr_S\Big[\exists\, h\in H:\, L_D(h)>\epsilon,\ \widehat{L}_S(h)=0\Big]
\;\leq\; \sum_{h\in H:\,L_D(h)>\epsilon} \Pr_S\big[\widehat{L}_S(h)=0\big]
\;\leq\; |H|\, e^{-2m\epsilon^2}.
$$
Combining with Section 2: $\Pr_S[L_D(\hat h)>\epsilon] \leq |H| e^{-2m\epsilon^2}$.

## 5. Step 3 — Solve for the Sample Complexity
We want this failure probability $\leq\delta$:
$$
|H| e^{-2m\epsilon^2} \leq \delta
\;\;\Longleftrightarrow\;\; e^{-2m\epsilon^2}\leq \frac{\delta}{|H|}
\;\;\Longleftrightarrow\;\; -2m\epsilon^2 \leq \ln\frac{\delta}{|H|}
\;\;\Longleftrightarrow\;\; m \geq \frac{1}{2\epsilon^2}\ln\frac{|H|}{\delta}.
$$
So **finite** $H$ (realizable case) is PAC learnable by ERM with sample complexity
$$
\boxed{m_H(\epsilon,\delta) = \left\lceil \frac{1}{2\epsilon^2}\ln\frac{|H|}{\delta} \right\rceil}
$$
— polynomial in $1/\epsilon$, $\log(1/\delta)$, and $\log|H|$ (the "description length" of $H$).
This is exactly the sense in which a finite hypothesis class is "simple": its sample complexity
grows only *logarithmically* in its size.

## 6. Non-Realizable (Agnostic) Case, Stated
Dropping realizability, a symmetric two-sided Hoeffding bound plus the same union bound gives:
with probability $\geq 1-\delta$, **simultaneously for every** $h\in H$,
$$
\big|\widehat{L}_S(h) - L_D(h)\big| \leq \sqrt{\frac{\ln(2|H|/\delta)}{2m}}.
$$
This is a **uniform convergence** bound — it is the general template every bound in Weeks 3–4
(VC dimension, Rademacher complexity) specializes for infinite $H$, where $|H|$ cannot be used
directly.

## 7. Code: Verifying the Finite-Class Bound Empirically
```python
import numpy as np

def run_trial(H_size, m, true_h_idx, rng):
    """H = H_size hypotheses; one (true_h_idx) matches the (noiseless) target exactly;
    the rest predict uniformly at random, independent of x. Realizable setting."""
    X = rng.normal(size=m)
    y_true = (X > 0).astype(int)                       # target concept
    preds = rng.integers(0, 2, size=(H_size, m))
    preds[true_h_idx] = y_true                           # the realizable hypothesis
    emp_risk = (preds != y_true).mean(axis=1)
    consistent = np.flatnonzero(emp_risk == 0.0)
    chosen = rng.choice(consistent)                       # ERM breaks ties uniformly at random
    return chosen

def true_risk_of_random_hyp():
    return 0.5   # a random-guess hypothesis disagrees with the deterministic target half the time

rng = np.random.default_rng(0)
H_size, eps, delta = 50, 0.1, 0.05
m_bound = int(np.ceil(np.log(H_size / delta) / (2 * eps ** 2)))
print("Sample complexity bound m_H(eps,delta) =", m_bound)

n_trials = 2000
failures = 0
for _ in range(n_trials):
    chosen = run_trial(H_size, m_bound, true_h_idx=0, rng=rng)
    true_risk = 0.0 if chosen == 0 else true_risk_of_random_hyp()
    failures += true_risk > eps
print(f"empirical failure rate over {n_trials} trials: {failures / n_trials:.3f}  (bound: delta={delta})")
```
Expect the empirical failure rate to sit comfortably below `delta` — the bound is deliberately
conservative (it holds for *every* choice of target and *every* consistent-but-wrong hypothesis
mix), so the true failure rate observed here is typically far smaller than the bound guarantees.

## 8. In-Class Exercise
Re-derive Step 5 but for a target failure probability of $\delta=0.01$ instead of $\delta=0.05$,
with $|H|=1000$, $\epsilon=0.05$. By what factor does $m_H$ grow? Explain why $m_H$ depends only
*logarithmically*, not linearly, on $1/\delta$ and $|H|$.
