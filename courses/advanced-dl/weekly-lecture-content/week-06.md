# Week 6 — Lecture Content: The Bradley-Terry Model and the RLHF Pipeline

## 1. Why Pairwise Preferences
Asking a human annotator to assign an absolute quality score to a single model output is noisy and
poorly calibrated across annotators ("is this a 7 or an 8?"). Asking them to compare **two**
outputs for the same prompt and say which is better is far more reliable — humans are
comparatively good at relative judgments. This motivates building a reward signal from pairwise
comparison data rather than absolute scores.

## 2. The Bradley-Terry Model, Derived
Posit a latent scalar "quality" $r(x,y) \in \mathbb R$ for every (prompt, output) pair. The
**Bradley-Terry model** assumes the odds of $y_1$ being preferred over $y_2$ are exactly the
exponentiated difference in quality:
```
P(y₁ ≻ y₂ | x) / P(y₂ ≻ y₁ | x) = exp( r(x,y₁) − r(x,y₂) )
```
Since $P(y_1\succ y_2\mid x) + P(y_2\succ y_1\mid x) = 1$, write $p := P(y_1\succ y_2\mid x)$; the
odds assumption gives $p/(1-p) = e^{\Delta r}$ where $\Delta r := r(x,y_1)-r(x,y_2)$. Solving:
```
p = e^{Δr} / (1 + e^{Δr}) = 1 / (1 + e^{-Δr}) = σ(Δr)
```
```
P(y₁ ≻ y₂ | x) = σ( r(x,y₁) − r(x,y₂) )
```
where $\sigma$ is the logistic function. This is exactly the standard pairwise-comparison model
used outside ML as well (e.g., in rating systems for competitive games): only the *difference* in
latent quality matters, not its absolute scale — $r$ and $r+c$ for any constant $c$ induce
identical preference probabilities, a useful invariance to remember when interpreting a fitted
reward model's absolute values.

## 3. Fitting a Reward Model by Maximum Likelihood
Given a dataset of labeled pairs $(x, y_w, y_l)$ — $y_w$ preferred over $y_l$ — parameterize
$r_\phi$ (a neural network, the **reward model**) and maximize the Bradley-Terry log-likelihood of
the observed preferences, equivalently minimizing the negative log-likelihood:
```
L(φ) = - E_{(x,y_w,y_l)} [ log σ( r_φ(x,y_w) − r_φ(x,y_l) ) ]
```
This is exactly the binary cross-entropy/logistic loss, with the "label" always being
"$y_w$ wins." Its gradient pushes $r_\phi(x,y_w)$ up and $r_\phi(x,y_l)$ down whenever the model's
current predicted preference probability for the observed winner is less than 1 — standard
logistic-regression-style gradient behavior, now over a learned, high-dimensional feature
representation rather than a fixed feature vector.

```python
import torch
import torch.nn as nn
import torch.nn.functional as F

class RewardModel(nn.Module):
    """Shared encoder + scalar head. encoder(x, y) -> a fixed-size representation; here a toy
    stand-in (in a real system, this is the language model's own hidden representation of the
    (prompt, response) pair)."""
    def __init__(self, input_dim, hidden=128):
        super().__init__()
        self.net = nn.Sequential(
            nn.Linear(input_dim, hidden), nn.ReLU(),
            nn.Linear(hidden, hidden), nn.ReLU(),
            nn.Linear(hidden, 1),
        )

    def forward(self, features):
        return self.net(features).squeeze(-1)     # scalar reward per (x, y)

def bradley_terry_loss(reward_model, feats_win, feats_lose):
    r_win = reward_model(feats_win)
    r_lose = reward_model(feats_lose)
    return -F.logsigmoid(r_win - r_lose).mean()
```

## 4. The RLHF Pipeline, Stage by Stage
1. **Supervised fine-tuning (SFT).** Fine-tune a pretrained language model on a curated dataset of
   high-quality demonstrations (prompt → good response pairs), via ordinary next-token cross-
   entropy — a direct application of the graduate course's standard fine-tuning procedure, with
   nothing new here. The result, $\pi_{\mathrm{ref}}$, is both the starting point for stage 3 and
   the fixed baseline that stage 3's KL penalty measures drift against.
2. **Reward-model training.** Collect preference pairs — typically by sampling multiple
   completions from $\pi_{\mathrm{ref}}$ (or a close variant) for the same prompts and having
   humans compare them — and fit $r_\phi$ via the Bradley-Terry loss of Section 3.
3. **RL fine-tuning.** Optimize a policy $\pi_\theta$, initialized from $\pi_{\mathrm{ref}}$, to
   maximize the learned reward model's score, **regularized back toward $\pi_{\mathrm{ref}}$ by a
   KL penalty**:
```
max_θ  E_{x, y~π_θ} [ r_φ(x,y) ]  −  β · E_x [ D_KL( π_θ(·|x) ‖ π_ref(·|x) ) ]
```

## 5. Why the KL Term Is Necessary
If $\beta = 0$, the optimization is free to push $\pi_\theta$ arbitrarily far from
$\pi_{\mathrm{ref}}$ in pursuit of reward-model score alone. Since $r_\phi$ is only an imperfect,
learned proxy for the true human preference signal (it was fit on a finite sample of comparisons
and may have systematic blind spots or exploitable regions it scores highly despite those outputs
being, by the true human standard, bad — a specification-gaming failure mode), an unconstrained
optimizer will tend to find and exploit exactly those blind spots, a phenomenon generally called
**reward over-optimization** or "reward hacking" against the learned proxy. The KL penalty keeps
$\pi_\theta$ close enough to $\pi_{\mathrm{ref}}$ — a policy already known to produce reasonable,
in-distribution outputs — that it cannot drift arbitrarily far into the reward model's
unvalidated, likely-exploitable regions. $\beta$ is therefore a genuine tradeoff knob: too small
risks reward hacking, too large prevents the policy from improving past $\pi_{\mathrm{ref}}$ at
all.

## 6. In-Class/Lab Exercise
Generate synthetic preference pairs from a known ground-truth reward function $r^*(x,y)$ (with
Bradley-Terry-consistent noise: sample the preference label from $\sigma(r^*(x,y_1)-r^*(x,y_2))$
rather than deterministically). Train `RewardModel` via `bradley_terry_loss` and verify the fitted
model's induced ranking over a held-out set of items correlates strongly (e.g., Spearman
correlation) with the ground-truth ranking. Write out, symbolically, the full stage-3 objective
for a toy $\pi_\theta$, and state in one paragraph what concretely goes wrong in your own toy
setup if $\beta$ is set to 0.
