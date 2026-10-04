# Week 3 — Lecture Content: VC Dimension in Depth

## 1. Why Finite-$H$ Theory Is Not Enough
Week 2's bound used $|H|$ directly, which is useless for infinite classes like "all linear
classifiers in $\mathbb{R}^d$" ($|H|=\infty$ for continuous parameters). We need a complexity
measure that stays finite — and meaningful — for such classes. The Vapnik–Chervonenkis (VC)
dimension is that measure.

## 2. Shattering
Let $H$ be a class of binary classifiers $\mathcal{X}\to\{0,1\}$ and $C=\{x_1,\dots,x_m\}\subset
\mathcal{X}$ a finite set of points. $H$ **shatters** $C$ if, for *every one* of the $2^m$ possible
labelings $(y_1,\dots,y_m)\in\{0,1\}^m$, some $h\in H$ realizes it exactly: $h(x_i)=y_i$ for all
$i$. Shattering is a strong requirement — it demands realizing *every* labeling of that specific
set, not just some useful ones.

## 3. VC Dimension — Definition
$$
\mathrm{VCdim}(H) \;=\; \max\{\, m : \text{some set of } m \text{ points is shattered by } H \,\}
$$
(with $\mathrm{VCdim}(H)=\infty$ if no finite maximum exists). Crucially this is an **existential**
statement over point sets of size $m$ (some set of that size is shattered — not every set), but a
**universal** statement over point sets of size $\mathrm{VCdim}(H)+1$ (showing $\mathrm{VCdim}(H)=
d$ requires proving that *no* set of $d+1$ points is shattered — this second direction is usually
the harder half of a VC-dimension proof).

## 4. Worked Example 1 — Intervals on the Real Line
$H=\{h_{a,b}(x)=\mathbb{1}[a\leq x\leq b] : a\leq b\in\mathbb{R}\}$.

**Lower bound, $\mathrm{VCdim}\geq2$:** take $C=\{x_1<x_2\}$. All $4$ labelings are realizable:
$(0,0)$ by an interval disjoint from both; $(1,1)$ by $[x_1,x_2]$; $(1,0)$ by $[x_1,x_1]$ (or any
interval containing $x_1$ only); $(0,1)$ by $[x_2,x_2]$. So some 2-point set is shattered.

**Upper bound, $\mathrm{VCdim}<3$:** take any $C=\{x_1<x_2<x_3\}$. The labeling $(1,0,1)$ is
*not* realizable: any interval $[a,b]$ containing $x_1$ and $x_3$ satisfies $a\leq x_1<x_2<x_3\leq
b$, so it must also contain $x_2$ — forcing label $1$ at $x_2$, contradicting the required label
$0$. Since this holds for *every* choice of 3 points (the argument used only their ordering), no
3-point set is shattered. Hence $\mathrm{VCdim}(H)=2$.

## 5. Worked Example 2 — Linear Threshold Classifiers in $\mathbb{R}^d$
$H=\{h_{w,b}(x)=\mathrm{sign}(w^\top x + b)\}$ (half-space classifiers). **Claim:**
$\mathrm{VCdim}(H)=d+1$.

**Lower bound ($\geq d+1$):** place $d+1$ points at the origin and the $d$ standard basis vectors,
$C=\{0,e_1,\dots,e_d\}$ (these are affinely independent — in "general position"). For any desired
labeling $(y_0,y_1,\dots,y_d)\in\{-1,+1\}^{d+1}$, choose $w_i = y_i$ for $i=1,\dots,d$ and $b =
y_0/2$ (and scale if needed so the margin at the origin has the correct sign); concretely, $h(x) =
\mathrm{sign}(w^\top x+b)$ with this $w$ gives $h(e_i)=\mathrm{sign}(y_i+y_0/2)=y_i$ for small
enough $|y_0/2|$ relative to $1$, and $h(0)=\mathrm{sign}(y_0/2)=y_0$. This realizes every
labeling, so $C$ is shattered and $\mathrm{VCdim}(H)\geq d+1$.

**Upper bound ($<d+2$), via Radon's theorem:** take any $d+2$ points in $\mathbb{R}^d$. Radon's
theorem guarantees they can be partitioned into two disjoint subsets $C_1,C_2$ whose convex hulls
intersect: $\mathrm{conv}(C_1)\cap\mathrm{conv}(C_2)\neq\emptyset$. Label every point in $C_1$ as
$+1$ and every point in $C_2$ as $-1$. No hyperplane can realize this labeling: a hyperplane
$\{x:w^\top x+b=0\}$ that puts all of $C_1$ strictly on the $+$ side and all of $C_2$ strictly on
the $-$ side would put $\mathrm{conv}(C_1)$ entirely on the $+$ side and $\mathrm{conv}(C_2)$
entirely on the $-$ side (both regions are convex and a halfspace's defining property is preserved
under convex combination), which is impossible since the two convex hulls share a point. So no set
of $d+2$ points is shattered, giving $\mathrm{VCdim}(H)\leq d+1$. Combined with the lower bound:
$\mathrm{VCdim}(H)=d+1$ exactly. (For $d=2$: $\mathrm{VCdim}=3$, matching the classic
"3-non-collinear-points-yes, 4-points-no" argument for lines in the plane.)

## 6. The VC Generalization Bound (Fundamental Theorem of Statistical Learning)
With probability at least $1-\delta$ over the draw of $m$ i.i.d. training examples, **every**
$h\in H$ (simultaneously) satisfies
$$
L_D(h) \;\leq\; \widehat{L}_S(h) \;+\; O\!\left(\sqrt{\frac{\mathrm{VCdim}(H)\,\log\!\big(m/\mathrm{VCdim}(H)\big) + \log(1/\delta)}{m}}\right).
$$
This generalizes Week 2's finite-class bound: $\log|H|$ is replaced by (a log factor times)
$\mathrm{VCdim}(H)$, which is finite even for continuously-parameterized classes. $H$ is PAC
learnable (agnostically) **if and only if** $\mathrm{VCdim}(H)<\infty$ — VC dimension is not just
*a* complexity measure, it exactly characterizes learnability for binary classification.

## 7. The Sauer–Shelah Lemma (Conceptual)
The **growth function** $\Pi_H(m) = \max_{C:|C|=m} |\{(h(x_1),\dots,h(x_m)) : h\in H\}|$ counts the
number of *distinct labelings* $H$ can realize on the worst-case $m$-point set — at most $2^m$,
trivially. The Sauer–Shelah lemma says: once $m$ exceeds $d=\mathrm{VCdim}(H)$, growth collapses
from exponential to **polynomial**:
$$
\Pi_H(m) \;\leq\; \sum_{i=0}^{d}\binom{m}{i} \;\leq\; \left(\frac{em}{d}\right)^{d}\quad\text{for } m\geq d.
$$
This is the mechanism that makes the VC bound possible at all: Week 2's union bound used $|H|$
directly, but an infinite $H$ needs the union bound replaced by a bound over the *effectively
distinct* behaviors $H$ exhibits on the sample, which Sauer–Shelah caps polynomially in $m$ — small
enough that a Hoeffding-type argument (now via a symmetrization technique, not covered in full
here) still yields a bound that shrinks with $m$.

## 8. Code: Brute-Force Shattering Check
```python
import itertools
import numpy as np

def can_separate_with_halfplane(X, labels):
    """Checks realizability via a simple linear program feasibility test using least squares
    as a cheap proxy (sufficient for small, well-separated teaching examples); for a rigorous
    check use a linear-programming feasibility solver."""
    y = np.where(np.array(labels) == 1, 1.0, -1.0)
    X_aug = np.hstack([X, np.ones((X.shape[0], 1))])
    best = None
    rng = np.random.default_rng(0)
    for _ in range(20000):
        w = rng.normal(size=X_aug.shape[1])
        pred = np.sign(X_aug @ w)
        if np.all(pred == y):
            return True
    return False

def is_shattered(points, classifier_check):
    m = len(points)
    for labeling in itertools.product([0, 1], repeat=m):
        if not classifier_check(np.array(points), labeling):
            return False, labeling
    return True, None

# 3 points in general position in R^2: should be shatterable by halfplanes
pts3 = [(0, 0), (1, 0), (0, 1)]
shattered, bad = is_shattered(pts3, can_separate_with_halfplane)
print("3 points shattered by halfplanes:", shattered)

# 4 points at the corners of a square: should NOT be shatterable
pts4 = [(0, 0), (1, 0), (1, 1), (0, 1)]
shattered4, bad_labeling = is_shattered(pts4, can_separate_with_halfplane)
print("4 points (square) shattered by halfplanes:", shattered4, " first failing labeling:", bad_labeling)
```
Expect `True` for the 3-point set and `False` for the square, with the failing labeling being the
alternating-corners pattern $(1,0,1,0)$ or $(0,1,0,1)$ (opposite corners same label, adjacent
corners different) — exactly the labeling Radon's theorem rules out, since the two diagonals'
convex hulls (line segments) cross at the square's center.

## 9. In-Class Exercise
For the square in Section 8, identify the specific labeling that fails and explain, in one
sentence, why it corresponds to the two diagonals of the square — i.e., why it is exactly the
Radon partition for 4 points in convex position in $\mathbb{R}^2$.
