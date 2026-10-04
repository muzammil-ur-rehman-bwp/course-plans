# Week 3 — Lecture Content: Multi-Armed Bandits

## 1. The Multi-Armed Bandit Problem
There are K arms; pulling arm i at round t yields a reward drawn i.i.d. from an unknown
distribution with mean μᵢ (we take rewards bounded in [0,1] for concreteness). Unlike Week 2's
full-information setting, the learner observes **only** the reward of the arm it actually pulls
(the "bandit feedback" setting). Let μ* = maxᵢ μᵢ. The objective is to minimize expected
cumulative regret:
```
E[Regret_T] = T·μ* − E[ Σ_{t=1}^T reward(t) ] = Σᵢ Δᵢ · E[nᵢ(T)]
```
where Δᵢ = μ* − μᵢ is arm i's **gap** and nᵢ(T) is the number of times arm i has been pulled by
round T. This decomposition is exact and is the key identity the UCB analysis uses: bounding
regret reduces to bounding how often each suboptimal arm is pulled.

## 2. The Hoeffding Bound
For n i.i.d. random variables bounded in [0,1] with mean μ, and empirical mean μ̂ₙ:
```
P( |μ̂ₙ − μ| ≥ ε ) ≤ 2 exp(−2nε²)
```
This is the concentration inequality that makes "confidence bounds" on an unknown mean
meaningful without any distributional assumption beyond boundedness.

## 3. The UCB1 Algorithm
At round t, having pulled arm i a total of nᵢ(t) times so far and observed empirical mean μ̂ᵢ(t),
UCB1 chooses the arm maximizing the **upper confidence bound**:
```
UCBᵢ(t) = μ̂ᵢ(t) + √( 2 ln t / nᵢ(t) )
```
(each arm is pulled once initially to avoid division by zero). The confidence radius
√(2 ln t / nᵢ(t)) is exactly the ε that, via Hoeffding with n = nᵢ(t), makes
2 exp(−2 nᵢ(t) ε²) = 2 exp(−4 ln t) = 2 t⁻⁴ — a probability that shrinks fast enough (summably
in t) that, by a union bound over all rounds and arms, the true mean μᵢ lies below its UCB index
for all but a vanishing fraction of rounds, with high overall probability. UCB1 is "optimistic":
it acts as if each arm's true mean is as high as the data can plausibly support, which
automatically balances exploration (pulling under-sampled arms, whose bound is wide) against
exploitation (pulling high-empirical-mean arms, once the bound has tightened).

```python
import numpy as np

def ucb1(bandit_means, T, rng):
    """bandit_means: true means of K Bernoulli arms (unknown to the algorithm).
    Returns per-round chosen arm and reward."""
    K = len(bandit_means)
    counts = np.zeros(K, dtype=int)
    sums = np.zeros(K)
    chosen = np.zeros(T, dtype=int)
    rewards = np.zeros(T)
    for t in range(1, T + 1):
        if t <= K:
            arm = t - 1  # pull each arm once first
        else:
            means = sums / counts
            bonus = np.sqrt(2 * np.log(t) / counts)
            arm = int(np.argmax(means + bonus))
        r = rng.binomial(1, bandit_means[arm])
        counts[arm] += 1
        sums[arm] += r
        chosen[t - 1] = arm
        rewards[t - 1] = r
    return chosen, rewards
```

## 4. Deriving the Regret Bound
Fix a suboptimal arm i with gap Δᵢ > 0. Arm i is pulled at round t (beyond the initial K rounds)
only if UCBᵢ(t) ≥ UCB*(t) for the optimal arm. A standard argument (Auer, Cesa-Bianchi & Fischer,
2002) shows this can only happen if at least one of three "bad events" holds: the optimal arm's
empirical mean has strayed too far below μ* (Hoeffding-rare), arm i's empirical mean has strayed
too far above μᵢ (Hoeffding-rare), or arm i has already been pulled enough times that its
confidence radius alone is smaller than Δᵢ/2 — specifically, once
nᵢ(t) > (8 ln t) / Δᵢ², arm i's confidence bound can no longer plausibly exceed the optimal arm's
true mean. Summing the (rare) probabilities of the first two bad events over all rounds (each
bounded via Hoeffding as in §2, and summable because of the t⁻⁴-style tail) contributes only an
O(1) constant, giving the standard result:
```
E[nᵢ(T)] ≤ (8 ln T) / Δᵢ²  +  1 + π²/3
```
Multiplying by Δᵢ and summing over all suboptimal arms:
```
E[Regret_T] ≤ Σ_{i: Δᵢ>0}  [ (8 ln T) / Δᵢ  +  Δᵢ (1 + π²/3) ]  =  O( (K log T) / Δ_min )
```
Regret grows only **logarithmically** in T — an exponential improvement over a naive
exploration strategy (e.g., pulling arms uniformly for a fixed exploration budget), at the cost
of a constant that depends inversely on the smallest gap Δ_min.

## 5. Thompson Sampling (Conceptual)
Thompson sampling takes a Bayesian view: maintain a posterior distribution over each arm's true
mean (e.g., a Beta posterior for Bernoulli rewards, updated conjugately after each pull); at each
round, draw one sample from each arm's current posterior and pull the arm with the highest
sampled value. Arms with wide, uncertain posteriors occasionally produce high samples purely from
uncertainty (encouraging exploration), while arms with narrow, high-mean posteriors are pulled
reliably (exploitation) — the same explore/exploit balance UCB achieves via an explicit
confidence bound, achieved here implicitly via posterior sampling. Thompson sampling is known to
achieve regret of the same O(log T) order as UCB-style algorithms on Bernoulli bandits, and
performs very well empirically; this course treats it conceptually rather than re-deriving its
(more involved) regret analysis.

## 6. In-Class/Lab Exercise
Simulate K = 5 Bernoulli arms with means [0.1, 0.3, 0.5, 0.55, 0.6]. Run UCB1 and ε-greedy
(ε = 0.1) for T = 5000 rounds each, repeated over 50 random seeds, and plot mean cumulative
regret with a spread band; confirm UCB1's regret grows logarithmically while ε-greedy's grows
linearly in the long run (since ε-greedy keeps exploring at a constant rate forever).
