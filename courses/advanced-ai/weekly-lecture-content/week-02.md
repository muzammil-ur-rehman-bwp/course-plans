# Week 2 — Lecture Content: Online Learning and Regret Minimization

## 1. The Online-Learning Protocol
At each round t = 1, ..., T: the algorithm (the "learner") chooses a probability distribution
p(t) over N fixed experts (or actions); an adversary (who may be adaptive, and makes no
statistical assumptions whatsoever) reveals a loss vector l(t) ∈ [0,1]^N; the learner suffers
expected loss l(t) = Σᵢ pᵢ(t) lᵢ(t). No assumption is made about how l(t) is generated — it could
be chosen by an adversary who has seen the learner's algorithm (though not its random coin
flips, if any) in advance. This is strictly more general than the bandit setting of Week 3, where
only the loss of the *chosen* action is observed; here, the full loss vector is revealed each
round (the "full information" setting).

## 2. Regret
Since no statistical model governs the losses, "optimal expected loss" is undefined; the right
performance measure instead compares the learner against the best *fixed* expert in hindsight:
```
Regret_T = Σ_{t=1}^T l(t)  -  min_i  Σ_{t=1}^T lᵢ(t)
```
A learner is called a **no-regret algorithm** if Regret_T / T → 0 as T → ∞ — it does not need to
know in advance which expert is best, and it still eventually performs, on average, almost as
well as the single best fixed expert chosen in hindsight.

## 3. The Multiplicative Weights (Weighted Majority) Algorithm
Maintain a weight wᵢ(t) ≥ 0 for each expert i, initialized wᵢ(1) = 1. At round t, play
pᵢ(t) = wᵢ(t) / Φ(t), where Φ(t) = Σᵢ wᵢ(t). After observing l(t), update:
```
wᵢ(t+1) = wᵢ(t) · (1 − η · lᵢ(t))        for a fixed learning rate η ∈ (0, 1/2]
```
Experts with high loss have their weight shrunk multiplicatively; experts that have been
reliably low-loss accumulate relatively more weight and so get played more often.

```python
import numpy as np

def multiplicative_weights(loss_matrix, eta):
    """loss_matrix: shape (T, N), entries in [0, 1]. Returns the learner's per-round
    expected loss and the weight trajectory."""
    T, N = loss_matrix.shape
    w = np.ones(N)
    learner_losses = np.zeros(T)
    for t in range(T):
        p = w / w.sum()
        learner_losses[t] = p @ loss_matrix[t]
        w = w * (1 - eta * loss_matrix[t])
    return learner_losses
```

## 4. Deriving the Regret Bound
Let Φ(t) = Σᵢ wᵢ(t). Since pᵢ(t) = wᵢ(t)/Φ(t):
```
Φ(t+1) = Σᵢ wᵢ(t)(1 − η lᵢ(t)) = Φ(t) − η Σᵢ wᵢ(t) lᵢ(t) = Φ(t)(1 − η · l(t))
```
Using 1 − x ≤ e^(−x): Φ(t+1) ≤ Φ(t) · e^(−η·l(t)). Unrolling over all T rounds, with Φ(1) = N:
```
Φ(T+1) ≤ N · exp(−η · L_alg),      where L_alg = Σ_t l(t)  (total learner loss)
```
For a lower bound on Φ(T+1), fix any expert i*; since all weights are nonnegative,
Φ(T+1) ≥ wᵢ*(T+1) = Π_t (1 − η lᵢ*(t)). Using the standard inequality
ln(1 − x) ≥ −x − x² for x ∈ [0, 1/2] (valid here since η ≤ 1/2 and lᵢ*(t) ∈ [0,1]):
```
ln wᵢ*(T+1) = Σ_t ln(1 − η lᵢ*(t)) ≥ −η Σ_t lᵢ*(t) − η² Σ_t lᵢ*(t)²  ≥  −η Lᵢ* − η² T
```
(using lᵢ*(t)² ≤ 1), so Φ(T+1) ≥ wᵢ*(T+1) ≥ exp(−η Lᵢ* − η² T). Combining the upper and lower
bounds on Φ(T+1) and taking logs:
```
ln N − η L_alg ≥ −η Lᵢ* − η² T
⟺  η L_alg ≤ η Lᵢ* + η² T + ln N
⟺  L_alg ≤ Lᵢ* + η T + (ln N) / η
```
So **Regret_T = L_alg − Lᵢ* ≤ η T + (ln N)/η** for any fixed expert i* — in particular for the
best one. This holds for every η ∈ (0, 1/2]; choosing η = √((ln N)/T) (valid for T ≥ 4 ln N, so
that η ≤ 1/2) balances the two terms and gives:
```
Regret_T ≤ 2 √(T ln N)  =  O(√(T log N))
```
This is the celebrated multiplicative-weights regret bound: regret grows only as √T (sublinear —
average regret per round, Regret_T/T, goes to 0), and only *logarithmically* in the number of
experts N, so the bound is essentially unaffected even by an exponentially large expert pool.

## 5. Weighted Majority: The Classical Predecessor
Littlestone & Warmuth's original Weighted Majority algorithm is the 0/1-loss, mistake-bound
special case of this idea (binary prediction, multiply the weight of any expert who was wrong by
a fixed β < 1). It predates and motivates the general multiplicative-weights framework above,
which generalizes it to real-valued losses in [0,1] and an expected-loss (rather than
worst-case-mistake) guarantee.

## 6. In-Class/Lab Exercise
Generate a loss matrix for N = 5 experts over T = 2000 rounds where one expert is secretly
slightly better on average but with high per-round noise. Run `multiplicative_weights` with
η = √(ln 5 / 2000) and plot cumulative regret against the best fixed expert in hindsight;
confirm it grows sublinearly and stays within the derived bound.
