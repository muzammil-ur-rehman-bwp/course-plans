# Week 5 — Lecture Content: Concentration Inequalities

## 1. Why This Week Comes "After" Weeks 2–4
Weeks 2–4 *used* Hoeffding's inequality as a black box. This week derives it (and its cousin,
McDiarmid's inequality) from first principles, so the toolkit underlying the entire course's
generalization bounds is fully justified, not just invoked.

## 2. Markov's Inequality
For a non-negative random variable $X$ and $a>0$:
$$
\Pr[X\geq a] \leq \frac{\mathbb{E}[X]}{a}.
$$
**Proof:** $\mathbb{E}[X] = \mathbb{E}[X\cdot\mathbb{1}[X\geq a]] + \mathbb{E}[X\cdot\mathbb{1}[X<a]]
\geq \mathbb{E}[X\cdot\mathbb{1}[X\geq a]] \geq a\cdot\mathbb{E}[\mathbb{1}[X\geq a]] = a\Pr[X\geq
a]$, using $X\geq0$ to drop the second term and $X\geq a$ on the event indicated. Divide by $a$.

## 3. Chebyshev's Inequality
For any random variable $X$ with mean $\mu$ and variance $\sigma^2$, and $a>0$:
$$
\Pr[|X-\mu|\geq a] \leq \frac{\sigma^2}{a^2}.
$$
**Proof:** apply Markov's inequality to the non-negative variable $(X-\mu)^2$ with threshold
$a^2$: $\Pr[(X-\mu)^2\geq a^2] \leq \mathbb{E}[(X-\mu)^2]/a^2 = \sigma^2/a^2$, and $(X-\mu)^2\geq
a^2 \iff |X-\mu|\geq a$. Chebyshev already gives a law-of-large-numbers-style concentration
result for an average of i.i.d. variables (variance of the mean of $m$ i.i.d. copies is
$\sigma^2/m$), but its $1/a^2$ tail decay is polynomial — Hoeffding will improve this to
exponential decay for *bounded* variables.

## 4. Hoeffding's Lemma
**Lemma:** if $X\in[a,b]$ almost surely and $\mathbb{E}[X]=0$, then for all $t\in\mathbb{R}$,
$$
\mathbb{E}\big[e^{tX}\big] \;\leq\; \exp\!\left(\frac{t^2(b-a)^2}{8}\right).
$$
**Proof sketch:** $e^{tx}$ is convex in $x$, so on $[a,b]$ it lies below the line segment
connecting $(a,e^{ta})$ and $(b,e^{tb})$: $e^{tx}\leq \frac{b-x}{b-a}e^{ta}+\frac{x-a}{b-a}e^{tb}$.
Taking expectations (using $\mathbb{E}[X]=0$) and writing $p=\frac{-a}{b-a}$, one gets
$\mathbb{E}[e^{tX}]\leq (1-p)e^{ta}+pe^{tb}=e^{g(t(b-a))}$ for $g(u)=-pu+\ln(1-p+pe^u)$; a Taylor
analysis shows $g''(u)\leq 1/4$ everywhere, so $g(u)\leq g(0)+g'(0)u+\frac{u^2}{8} =
\frac{u^2}{8}$ (since $g(0)=0=g'(0)$ by construction), giving the stated bound.

## 5. Hoeffding's Inequality — Full Derivation
Let $X_1,\dots,X_m$ be independent, with $X_i\in[a_i,b_i]$ almost surely. Let $S=\sum_i(X_i -
\mathbb{E}[X_i])$. For any $t>0$, by Markov's inequality applied to $e^{tS}$ (a Chernoff bound):
$$
\Pr[S\geq\epsilon] = \Pr[e^{tS}\geq e^{t\epsilon}] \leq \frac{\mathbb{E}[e^{tS}]}{e^{t\epsilon}}
= e^{-t\epsilon}\prod_{i=1}^m \mathbb{E}\big[e^{t(X_i-\mathbb{E}[X_i])}\big]
\leq e^{-t\epsilon}\prod_{i=1}^m \exp\!\left(\frac{t^2(b_i-a_i)^2}{8}\right)
$$
(independence gives the product of MGFs; Hoeffding's lemma bounds each factor). So
$$
\Pr[S\geq\epsilon] \leq \exp\!\left(-t\epsilon + \frac{t^2}{8}\sum_i(b_i-a_i)^2\right).
$$
Minimize the exponent over $t$: setting the derivative to zero gives $t^\star =
\frac{4\epsilon}{\sum_i(b_i-a_i)^2}$, and substituting back,
$$
\Pr[S\geq\epsilon] \;\leq\; \exp\!\left(\frac{-2\epsilon^2}{\sum_i(b_i-a_i)^2}\right).
$$
For i.i.d. $[0,1]$-bounded $X_i$ with mean $\mu$ and $\widehat{\mu}=\frac1m\sum_i X_i$, this is $S =
m(\widehat\mu-\mu)$, $(b_i-a_i)=1$, giving (and symmetrically for the lower tail, via a union over
both tails):
$$
\boxed{\Pr\big[\,|\widehat\mu-\mu|\geq\epsilon\,\big] \;\leq\; 2\exp(-2m\epsilon^2).}
$$
This is exactly the inequality Week 2 used as a black box.

## 6. McDiarmid's Bounded-Differences Inequality (Conceptual)
Hoeffding's inequality concentrates a specific *linear* statistic (a sample mean). **McDiarmid's
inequality** generalizes this to *any* function $f(X_1,\dots,X_m)$ of independent variables,
provided changing one coordinate cannot change $f$ by much: if
$$
\sup_{x_1,\dots,x_m,x_i'} \big|f(x_1,\dots,x_i,\dots,x_m) - f(x_1,\dots,x_i',\dots,x_m)\big| \leq c_i \quad\text{for each } i,
$$
then
$$
\Pr\big[\,|f(X)-\mathbb{E}[f(X)]|\geq t\,\big] \;\leq\; 2\exp\!\left(\frac{-2t^2}{\sum_i c_i^2}\right).
$$
This is exactly the tool needed to bound $\sup_{h\in H}(L_D(h)-\widehat{L}_S(h))$ as a function of
the *whole sample* $S$ (Weeks 3–4): changing one training point changes this supremum by at most
$1/m$ (for a $[0,1]$-bounded loss), so McDiarmid applies with $c_i=1/m$ for all $i$, giving a
concentration result for a complicated, non-linear statistic of the sample — something plain
Hoeffding cannot directly handle, since $\sup_h(\cdot)$ is not a sum of independent terms.

## 7. Where the Assumptions Matter — A Cautionary Example
Both inequalities **require independence and boundedness (or sub-Gaussianity)**. Applying
Hoeffding's inequality to a time series with strong serial correlation (e.g., $X_{i+1}=X_i+\text{small
noise}$) is invalid: the "effective sample size" behind such a sequence is far smaller than $m$,
and the stated $2\exp(-2m\epsilon^2)$ bound can be badly wrong (the true tail probability can be
much larger than the bound claims), because the proof's key step — the product of $m$ independent
MGFs in Section 5 — does not hold when the $X_i$ are dependent.

## 8. Code: Verifying Hoeffding's Bound by Simulation
```python
import numpy as np

def hoeffding_bound(m, eps):
    return 2 * np.exp(-2 * m * eps**2)

rng = np.random.default_rng(0)
m, eps, n_trials = 50, 0.1, 200_000
mu = 0.5  # fair coin

draws = rng.integers(0, 2, size=(n_trials, m))
emp_means = draws.mean(axis=1)
empirical_tail_prob = np.mean(np.abs(emp_means - mu) >= eps)
bound = hoeffding_bound(m, eps)
print(f"empirical P(|mean-mu|>=eps) = {empirical_tail_prob:.4f}   Hoeffding bound = {bound:.4f}")

# Cautionary: correlated (random-walk-like) sequence — Hoeffding's bound no longer a valid guarantee
corr = np.zeros((n_trials, m))
corr[:, 0] = rng.integers(0, 2, size=n_trials)
for t in range(1, m):
    flip = rng.random(n_trials) < 0.05           # label mostly repeats previous value -> strong correlation
    corr[:, t] = np.where(flip, 1 - corr[:, t - 1], corr[:, t - 1])
corr_means = corr.mean(axis=1)
corr_tail_prob = np.mean(np.abs(corr_means - mu) >= eps)
print(f"correlated-sequence P(|mean-mu|>=eps) = {corr_tail_prob:.4f}  (Hoeffding bound {bound:.4f} may not hold)")
```
Expect the i.i.d. empirical tail probability to sit well under the Hoeffding bound, while the
correlated sequence's tail probability can exceed it — the independence assumption is doing real
work, not just a technical nicety.

## 9. In-Class Exercise
Using Section 5's derivation, show what changes if the $X_i$ are bounded in $[-1,1]$ instead of
$[0,1]$, and re-derive the constant inside the exponent of the final two-sided bound.
