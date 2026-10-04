# Week 4 — Lecture Content: Implicit Regularization and the Implicit Bias of Gradient Descent

## 1. The Implicit-Regularization Question
When a model class is large enough to fit the training data in many different ways (any
overparameterized network on separable data, for instance), the training loss alone does not pick
out a unique solution — infinitely many weight settings achieve zero (or near-zero) loss. Yet
gradient descent, with no explicit regularizer at all, reliably converges to *one particular*
solution among these many. **Implicit regularization** is the name for whatever property of the
optimization algorithm itself (not the loss function, not an explicit penalty) is responsible for
that selection. This week derives the cleanest case where the answer is fully known — linear
models — and surveys how much less is settled once nonlinearity or depth enters.

## 2. Setup: Linear Classification on Separable Data
Let $\{(x_i, y_i)\}_{i=1}^n$, $y_i \in \{-1,+1\}$, be linearly separable: there exists $w$ with
$y_i w^\top x_i > 0$ for all $i$. Train a linear classifier $w$ by gradient descent on the
logistic loss
$$
L(w) = \sum_{i=1}^n \ell(y_i w^\top x_i), \qquad \ell(z) = \log(1+e^{-z}),
$$
with no regularization, $w_{t+1} = w_t - \eta \nabla L(w_t)$. Because the data is separable, $L(w)$
can be driven arbitrarily close to $0$ by scaling any separating $w$ up — so $\|w_t\|\to\infty$
along the gradient descent path; there is no finite-norm minimizer. The question is: in what
*direction* does $w_t$ go?

## 3. The Max-Margin Derivation (Sketch)
This result is due to Soudry, Hoffer, Nacson, Gunasekar, and Srebro's analysis of the implicit
bias of gradient descent on separable data; the sketch below follows their argument's structure.

**Step 1 — the loss's exponential tail dominates asymptotic behavior.** For $z\to\infty$,
$\ell(z) = \log(1+e^{-z}) \sim e^{-z}$, and $\ell'(z) = -\sigma(-z) \sim -e^{-z}$ (where $\sigma$
is the sigmoid). So, once $w_t$ has grown large enough to classify every point with growing
margin, the gradient is governed almost entirely by the exponential, not the exact logistic form.

**Step 2 — write $w_t$ as a growing-norm part plus a bounded residual.** Define the hard-margin
(max-margin) direction
$$
\hat w := \arg\min_{w} \|w\|^2 \quad \text{s.t.} \quad y_i\, w^\top x_i \ge 1 \ \ \forall i,
$$
(the normalized hard-SVM solution). Decompose $w_t = g(t)\,\hat w + \rho(t)$, where $g(t)\to\infty$
and $\rho(t)$ is claimed (and, in the cited analysis, proven) to stay **bounded** — all of the
unbounded growth happens along the fixed direction $\hat w$, not in some rotating or wandering
direction.

**Step 3 — why the growth concentrates on $\hat w$.** Substituting the decomposition into the
gradient, the dominant exponential term for point $i$ behaves like
$\exp(-g(t)\, y_i \hat w^\top x_i)$. Points with the *smallest* margin $y_i \hat w^\top x_i$ (the
support vectors of the max-margin problem — by construction, exactly $1$ at the optimum) have
their loss term shrink the *slowest* as $g(t)$ grows, so they dominate the gradient at late $t$,
and the gradient direction asymptotically aligns with $-\nabla$ of a loss supported essentially
only on the support vectors — which is exactly the first-order optimality condition of the
max-margin problem itself. This self-reinforcing loop (large-margin points stop contributing,
small-margin points keep steering $w_t$ toward larger relative margin) is what drives
$w_t / \|w_t\| \to \hat w$.

**Step 4 — norm growth rate.** Matching the loss's required decay rate ($L(w_t)\to 0$) against the
exponential tail gives $g(t) = \Theta(\log t)$: the margin (and hence the norm) grows only
*logarithmically* in the number of gradient steps — convergence in direction is far faster, in
a relative sense, than convergence in norm.

**Conclusion.** Gradient descent on linearly separable data, run with the logistic loss and no
explicit regularization, converges *in direction* to the $L_2$-max-margin (hard-SVM) classifier —
an implicit bias toward the margin-maximizing solution that was never written into the loss.

## 4. Why the Exponential Tail Specifically Matters
The derivation leans on $\ell(z)\sim e^{-z}$, not on logistic loss's exact form — any loss with
this tail behavior (e.g., the exponential loss used in boosting) produces the same max-margin
bias. A loss with a **polynomial** tail (e.g., $\ell(z) = 1/z$ for large $z$) does *not* produce
this concentration-on-support-vectors behavior in the same way, and is known to converge to a
different (non-max-margin) limit direction — a useful diagnostic for predicting a new loss
function's implicit bias without repeating the full derivation.

## 5. Survey: Implicit Bias in Nonlinear and Deep Settings
The clean result above is specific to **linear** models on **separable** data with a
specific-tail loss. Extending it is a genuinely active, only partially settled research area:
- **Deep linear networks and matrix factorization.** For models that are linear in their *output*
  but reparameterize the linear map as a product of matrices (e.g., matrix factorization,
  deep linear networks trained on a regression loss), a body of work shows an implicit bias toward
  *low-rank* or otherwise "simple" solutions among the many that fit the training data — a
  structurally different implicit bias from max-margin, arising from the reparameterization itself
  rather than from the loss's tail behavior.
- **General deep nonlinear networks.** The proof technique in §3 relies essentially on linearity
  (the ability to decompose $w_t$ along a single fixed direction $\hat w$ and reason about margins
  as linear inner products). For a genuinely nonlinear deep network, no comparably clean,
  general characterization of "the direction gradient descent implicitly prefers" is currently
  established; results exist only for restricted architectures, restricted data distributions, or
  specific training regimes (including, pointedly, this course's own NTK/lazy-training limit from
  Week 2, where the *linearized* dynamics make a margin-style analysis tractable again — directly
  connecting this week back to Week 2's material). This course presents the general deep nonlinear
  case honestly as open, not as a settled extension of §3.

## 6. Code: Observing Max-Margin Convergence
```python
import torch

torch.manual_seed(0)

n, d = 40, 2
X = torch.randn(n, d)
w_true = torch.tensor([1.0, 0.5])
y = torch.sign(X @ w_true)
margin_pad = 0.3
X = X + margin_pad * y.unsqueeze(1) * (w_true / w_true.norm())   # ensure clean separation

w = torch.zeros(d, requires_grad=True)
eta, steps = 0.1, 20000
directions = []
for t in range(steps):
    z = y * (X @ w)
    loss = torch.log1p(torch.exp(-z)).sum()
    grad = torch.autograd.grad(loss, w)[0]
    with torch.no_grad():
        w -= eta * grad
    if t % 2000 == 0:
        directions.append((w / w.norm()).clone())

# Exact max-margin direction via a simple projected-gradient hard-margin solver.
w_svm = torch.zeros(d, requires_grad=True)
for _ in range(5000):
    margins = y * (X @ w_svm)
    violation = torch.clamp(1 - margins, min=0)
    loss = 0.5 * (w_svm ** 2).sum() + 1000.0 * violation.sum()
    grad = torch.autograd.grad(loss, w_svm)[0]
    with torch.no_grad():
        w_svm -= 0.01 * grad
w_svm_dir = (w_svm / w_svm.norm()).detach()

for t_idx, d_vec in enumerate(directions):
    cos_sim = torch.dot(d_vec, w_svm_dir).item()
    print(f"step {t_idx*2000:6d}  cosine similarity to max-margin direction: {cos_sim:.4f}")
```
The cosine similarity should climb toward $1.0$ as training proceeds — the normalized gradient-
descent direction increasingly aligning with the max-margin direction found by the (independent)
penalized hard-margin solver, exactly as §3 predicts.

## 7. In-Class Exercise
A colleague claims: "Since gradient descent converges to the max-margin direction, training longer
with no regularization is just as good as using an explicit max-margin SVM solver." Identify what
this claim gets right and what it elides, using §3 Step 4's norm-growth rate ($g(t)=\Theta(\log
t)$) to argue about how *many* gradient steps are actually needed to get close to the limit
direction in practice.
