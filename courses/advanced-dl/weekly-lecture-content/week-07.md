# Week 7 — Lecture Content: PPO for RLHF and Direct Preference Optimization

## 1. PPO for RLHF (Built on the Graduate Course's Actor-Critic Foundation)
Week 6 set up the stage-3 objective
$\max_\theta \mathbb E_{x,y\sim\pi_\theta}[r_\phi(x,y)] - \beta\,\mathbb E_x D_{\mathrm{KL}}(\pi_\theta\|\pi_{\mathrm{ref}})$.
**We do not re-derive the policy-gradient theorem, REINFORCE, or actor-critic here** — that is
graduate-course material, assumed fluent. RLHF applies that machinery with one change: the reward
fed to the policy-gradient estimator is not the environment's raw reward but the **KL-regularized
reward**,
```
r̂(x, y) = r_φ(x, y) − β log( π_θ(y|x) / π_ref(y|x) )
```
(the per-sequence KL contribution folded directly into the per-episode reward, token by token in
practice), with a learned value-function baseline exactly as in actor-critic, producing an
advantage estimate $\hat A_t$. **PPO** (Proximal Policy Optimization) then replaces the raw
policy-gradient update with its clipped surrogate objective:
```
L^CLIP(θ) = E[ min( ρ_t Â_t,  clip(ρ_t, 1−ε, 1+ε) Â_t ) ],     ρ_t = π_θ(a_t|s_t) / π_θ_old(a_t|s_t)
```
Clipping the probability ratio $\rho_t$ bounds how far a single gradient step can move the policy
relative to the data it was collected under — this is the practical fix for the well-known
instability of applying many gradient steps to reused on-policy rollouts in actor-critic-style
training (if $\rho_t$ moved freely, large steps could collapse or destabilize the policy; clipping
caps the per-step update's effective size without needing a strict trust-region line search).

**Why this stage is engineering-heavy in practice:** running it requires keeping four networks
resident and consistent — the policy $\pi_\theta$, its value-function baseline, the frozen reward
model $r_\phi$, and the frozen reference policy $\pi_{\mathrm{ref}}$ — and the whole pipeline's
stability is sensitive to the KL coefficient $\beta$, to how stale the on-policy rollouts are
allowed to become before updating, and to each of the three prior pipeline stages (SFT quality,
reward-model calibration) having gone well. This operational complexity is the direct motivation
for DPO.

## 2. Motivating DPO
Could we skip training an explicit reward model and running an RL optimizer at all, and instead
directly update the policy from preference data? **Direct Preference Optimization (DPO)** shows
yes — by exploiting a closed-form fact about the KL-regularized objective itself.

## 3. The Closed-Form Optimal Policy
For a *fixed* reward function $r(x,\cdot)$, consider the objective (dropping the outer
expectation over $x$ and writing it per-prompt for clarity):
```
J(π) = E_{y~π} [ r(x,y) ] − β · E_{y~π} [ log( π(y|x) / π_ref(y|x) ) ]
```
Define $Z(x) := \sum_y \pi_{\mathrm{ref}}(y\mid x)\exp(r(x,y)/\beta)$ (a normalizing constant,
fixed given $r$, $\pi_{\mathrm{ref}}$, and $x$). Algebraic rearrangement:
```
J(π) = β · E_{y~π} [ r(x,y)/β − log(π(y|x)/π_ref(y|x)) ]
     = β · E_{y~π} [ log( π_ref(y|x) exp(r(x,y)/β) / π(y|x) ) ]
     = β · E_{y~π} [ log( Z(x) · [π_ref(y|x) exp(r(x,y)/β)/Z(x)] / π(y|x) ) ]
     = β log Z(x) + β · E_{y~π} [ log( π*(y|x) / π(y|x) ) ],   where π*(y|x) := π_ref(y|x)exp(r(x,y)/β)/Z(x)
     = β log Z(x) − β · D_KL( π(·|x) ‖ π*(·|x) )
```
Since $D_{\mathrm{KL}} \geq 0$ with equality iff the two distributions are identical, $J(\pi)$ is
maximized exactly when $\pi = \pi^*$:
```
π*(y|x) = ( 1 / Z(x) ) · π_ref(y|x) · exp( r(x,y) / β )
```
This is the objective's **closed-form optimal policy** — a standard result for any
KL-regularized-reward-maximization objective of this form (sometimes called the Gibbs/Boltzmann
solution).

## 4. Inverting: The Reward Implied By Any Policy
Solve the closed form for $r$ in terms of $\pi^*$:
```
r(x, y) = β log( π*(y|x) / π_ref(y|x) ) + β log Z(x)
```
This says: **any** policy $\pi_\theta$ can be read as *implicitly* encoding a reward function —
simply plug $\pi_\theta$ in for $\pi^*$ above. $\log Z(x)$ depends only on $x$ (and on
$\pi_{\mathrm{ref}}, r$ overall), not on $y$ — this is the fact the next step exploits.

## 5. Substituting into Bradley-Terry: The DPO Loss
Plug this implied reward directly into Week 6's Bradley-Terry preference probability, for a pair
$(y_w, y_l)$ sharing the same prompt $x$ (so the same $\log Z(x)$ term appears in both):
```
P(y_w ≻ y_l | x) = σ( r(x,y_w) − r(x,y_l) )
 = σ( [β log(π_θ(y_w|x)/π_ref(y_w|x)) + β log Z(x)]
      − [β log(π_θ(y_l|x)/π_ref(y_l|x)) + β log Z(x)] )
 = σ( β log(π_θ(y_w|x)/π_ref(y_w|x)) − β log(π_θ(y_l|x)/π_ref(y_l|x)) )
```
**The $\log Z(x)$ terms cancel exactly** — they are identical for $y_w$ and $y_l$ since both share
the same prompt $x$, and a difference of two equal quantities is zero. This is the crux of the
whole derivation: it is *only* because Bradley-Terry depends on $r$ through a *difference* that the
otherwise-intractable (summing over all possible $y$) normalizing constant $Z(x)$ never needs to be
computed. Treating $\pi_\theta$ itself as the (implicitly reward-encoding) object being fit, the
maximum-likelihood loss over preference data is the **DPO loss**:
```
L_DPO(θ) = − E_{(x,y_w,y_l)} [ log σ( β log(π_θ(y_w|x)/π_ref(y_w|x)) − β log(π_θ(y_l|x)/π_ref(y_l|x)) ) ]
```
A single supervised-style classification loss computed directly from the policy's own
log-probabilities under $\pi_\theta$ and the frozen $\pi_{\mathrm{ref}}$ — **no reward model, no
sampling from the policy during training, no RL optimizer.** Note precisely where this derivation
would break: if the Bradley-Terry assumption (preferences are odds-ratio-consistent with *some*
latent scalar reward) did not hold, the inversion in Section 4 would still define *a* quantity, but
substituting it into a Bradley-Terry-shaped loss would no longer be fitting the true preference
model — this is exactly the Week 14 open question about DPO's robustness to Bradley-Terry
violations.

```python
import torch
import torch.nn.functional as F

def dpo_loss(policy_logprob_win, policy_logprob_lose,
             ref_logprob_win, ref_logprob_lose, beta=0.1):
    """All four args: shape (batch,), the *sequence* log-probabilities log pi(y|x) summed over
    tokens, under the policy and the frozen reference model respectively."""
    policy_logratio = policy_logprob_win - policy_logprob_lose
    ref_logratio = ref_logprob_win - ref_logprob_lose
    logits = beta * (policy_logratio - ref_logratio)
    return -F.logsigmoid(logits).mean()

# Training step sketch (policy and ref_model are causal LMs; ref_model is frozen):
def compute_seq_logprob(model, input_ids, response_mask):
    logits = model(input_ids)                               # (batch, seq, vocab)
    logprobs = F.log_softmax(logits, dim=-1)
    token_logprobs = logprobs.gather(-1, input_ids.unsqueeze(-1)).squeeze(-1)
    return (token_logprobs * response_mask).sum(dim=-1)      # sum over response tokens only
```

## 6. PPO-RLHF vs. DPO: What DPO Gives Up
DPO optimizes *the same underlying objective* as PPO-RLHF, exactly, **to the extent the Bradley-
Terry model and the closed-form derivation hold** — it is not an approximation of RLHF, it is an
exact reparameterization under those assumptions. What it gives up: it cannot easily incorporate
a reward signal that is not naturally preference-pair-shaped (e.g., a scalar reward from an
automated metric with no paired comparison), and because its gradient is computed directly from
offline preference pairs (not from on-policy rollouts of the current $\pi_\theta$), it is less
naturally suited to settings where the reward signal itself should adapt to the *current* policy's
outputs during training. In practice, DPO's training-pipeline simplicity (one loss, two frozen/
trainable log-probability computations, ordinary supervised-style optimization) is its main
advantage over PPO-RLHF's four-network, RL-optimizer pipeline.

## 7. In-Class/Lab Exercise
Using the Week 6 reward-model lab's ground-truth reward $r^*$ and preference-pair generator,
instead train a small categorical "policy" directly via `dpo_loss` against a frozen uniform
reference policy. Verify the fitted policy's implied reward
$\beta\log(\pi_\theta(y\mid x)/\pi_{\mathrm{ref}}(y\mid x))$ ranks items consistently with $r^*$.
Write out, symbolically, each step of the Section 3–5 derivation for your own toy setup, marking
precisely where the $Z(x)$ cancellation occurs.
