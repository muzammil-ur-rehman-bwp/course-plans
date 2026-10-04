# Week 4 — Lecture Content: Rademacher Complexity

## 1. Motivation: A Data-Dependent Capacity Measure
VC dimension is **distribution-free**: it is the worst case over *all possible* point sets, so it
cannot adapt to a "nice" distribution $D$ or a benign actual sample. Rademacher complexity measures
capacity **on the specific sample at hand**, and can be dramatically smaller when the data is
favorable — yielding tighter, more informative generalization bounds.

## 2. Definition
For a class $H$ of real-valued functions and a fixed sample $S=\{x_1,\dots,x_m\}$, the **empirical
Rademacher complexity** of $H$ on $S$ is
$$
\widehat{\mathcal{R}}_S(H) \;=\; \mathbb{E}_{\sigma}\left[\;\sup_{h\in H}\ \frac{1}{m}\sum_{i=1}^m \sigma_i\, h(x_i)\;\right],
$$
where $\sigma_1,\dots,\sigma_m$ are i.i.d. uniform on $\{-1,+1\}$ (Rademacher variables). Intuition:
draw a completely random $\pm1$ "fake labeling" $\sigma$ of the sample, and ask how well the
*best-fitting* $h\in H$ can correlate with pure noise. A class that can correlate well with
arbitrary noise (large $\widehat{\mathcal{R}}_S(H)$) is "too flexible" on this data — a hallmark of
overfitting risk. The **(population) Rademacher complexity** is $\mathcal{R}_m(H) =
\mathbb{E}_{S\sim D^m}[\widehat{\mathcal{R}}_S(H)]$.

## 3. The Rademacher Generalization Bound
For a loss bounded in $[0,1]$, with probability at least $1-\delta$ over the draw of $S$,
**simultaneously for every** $h\in H$,
$$
L_D(h) \;\leq\; \widehat{L}_S(h) \;+\; 2\,\widehat{\mathcal{R}}_S(\ell\circ H) \;+\; \sqrt{\frac{2\ln(2/\delta)}{m}}.
$$
(The $\sqrt{\ln(1/\delta)/m}$ term comes from McDiarmid's inequality, Week 5, applied to the
sample-to-sample fluctuation of $\sup_h(L_D(h)-\widehat{L}_S(h))$ around its expectation, which is
in turn bounded by a symmetrization argument involving $\widehat{\mathcal{R}}_S$.) Because
$\widehat{\mathcal{R}}_S$ depends on the *actual* $x_1,\dots,x_m$, this bound can be far tighter
than the VC bound for the same $H$ whenever the realized sample is more benign than the
distribution-free worst case.

## 4. Relation to VC Dimension — Massart's Lemma
**Massart's lemma:** for a *finite* set $A\subset\mathbb{R}^m$ with every $a\in A$ satisfying
$\|a\|_2\leq r$,
$$
\mathbb{E}_\sigma\Big[\sup_{a\in A}\ \tfrac{1}{m}\sigma^\top a\Big] \;\leq\; \frac{r\sqrt{2\ln|A|}}{m}.
$$
Apply this with $A = \{(h(x_1),\dots,h(x_m)) : h\in H\}$, the set of label vectors $H$ realizes on
$S$. Its size is at most the growth function, $|A|\leq\Pi_H(m)$, and (for $\{-1,+1\}$-valued $h$)
every $a\in A$ has $\|a\|_2=\sqrt{m}$ exactly, so $r=\sqrt{m}$. Massart's lemma gives
$$
\widehat{\mathcal{R}}_S(H) \;\leq\; \frac{\sqrt{m}\cdot\sqrt{2\ln\Pi_H(m)}}{m} \;=\; \sqrt{\frac{2\ln\Pi_H(m)}{m}}.
$$
Plugging in the Sauer–Shelah bound $\Pi_H(m)\leq(em/d)^d$ (Week 3, $d=\mathrm{VCdim}(H)$):
$$
\widehat{\mathcal{R}}_S(H) \;\leq\; \sqrt{\frac{2d\,\ln(em/d)}{m}},
$$
which has exactly the same $\sqrt{d\log(m/d)/m}$ shape as the Week 3 VC bound. **This is the
precise sense in which Rademacher complexity generalizes VC-dimension-based bounds:** the VC bound
is recoverable as the special, distribution-free case of the (tighter, data-dependent) Rademacher
bound, via Massart's lemma and Sauer–Shelah as the bridge.

## 5. Rademacher Complexity of a Norm-Bounded Linear Class, in Closed Form
Let $H=\{x\mapsto w^\top x : \|w\|_2\leq B\}$. By Cauchy-Schwarz,
$\sup_{\|w\|\leq B} \frac{1}{m}\sigma^\top Xw = \frac{B}{m}\sup_{\|w\|\le1}\sigma^\top Xw \cdot \|w\|/B$... more directly:
$$
\sup_{\|w\|\leq B}\ \frac{1}{m}\sum_i \sigma_i w^\top x_i \;=\; \frac{B}{m}\Big\|\textstyle\sum_i \sigma_i x_i\Big\|_2,
$$
attained at $w = B\cdot\frac{\sum_i\sigma_i x_i}{\|\sum_i\sigma_i x_i\|_2}$. So
$$
\widehat{\mathcal{R}}_S(H) = \frac{B}{m}\,\mathbb{E}_\sigma\Big[\big\|\textstyle\sum_i\sigma_i x_i\big\|_2\Big]
\;\leq\; \frac{B}{m}\sqrt{\mathbb{E}_\sigma\Big[\big\|\textstyle\sum_i\sigma_i x_i\big\|_2^2\Big]}
\;=\; \frac{B}{m}\sqrt{\sum_i \|x_i\|_2^2}
$$
(Jensen's inequality for the first step; $\mathbb{E}[\sigma_i\sigma_j]=0$ for $i\neq j$ kills
cross terms in the second, leaving $\sum_i\|x_i\|^2$). If $\|x_i\|_2\leq R$ for all $i$, this gives
the clean, dimension-free bound $\widehat{\mathcal{R}}_S(H)\leq BR/\sqrt{m}$ — notably with **no
explicit dependence on the input dimension**, which is a key advantage of Rademacher-complexity
arguments for high-dimensional linear classes (VC dimension of this class is dimension-dependent).

## 6. Code: Estimating Rademacher Complexity by Monte Carlo
```python
import numpy as np

def rademacher_estimate_linear(X, B=1.0, n_draws=500, seed=0):
    """Empirical Rademacher complexity of H = {x -> w.x : ||w||_2 <= B} on sample X."""
    rng = np.random.default_rng(seed)
    m = X.shape[0]
    vals = []
    for _ in range(n_draws):
        sigma = rng.choice([-1, 1], size=m)
        corr_vec = X.T @ sigma                      # sum_i sigma_i x_i
        sup_val = B * np.linalg.norm(corr_vec) / m   # closed-form sup, Section 5
        vals.append(sup_val)
    return float(np.mean(vals))

rng = np.random.default_rng(0)
for m in [10, 50, 200, 1000]:
    X = rng.normal(size=(m, 5))
    R_hat = rademacher_estimate_linear(X, B=1.0, seed=1)
    bound = 1.0 * np.sqrt(np.mean(np.sum(X**2, axis=1))) / np.sqrt(m)   # BR/sqrt(m), R ~ typical norm
    print(f"m={m:5d}  Rademacher estimate={R_hat:.4f}  closed-form-order bound~{bound:.4f}")
```
Expect both quantities to shrink roughly as $1/\sqrt{m}$, and the Monte Carlo estimate to track the
closed-form order-of-magnitude bound from Section 5.

## 7. In-Class Exercise
Using Section 5's bound $\widehat{\mathcal{R}}_S(H)\leq BR/\sqrt m$, write down the resulting
Rademacher generalization bound (Section 3) for a norm-bounded linear class, and explain why this
bound does **not** blow up as the input dimension $d\to\infty$ with $B,R,m$ held fixed — contrast
this with how the Week 3 VC bound for unrestricted linear classifiers *does* grow with $d$ (since
$\mathrm{VCdim}=d+1$ there).
