# Week 10 — Lecture Content: Partially Observable MDPs (POMDPs)

## 1. Why Partial Observability Complicates Planning
A standard MDP assumes the agent always knows its exact current state s. In many real problems —
a robot with noisy sensors, a diagnostic system that only sees symptoms rather than the true
underlying condition, a dialogue system that only hears an utterance rather than knowing the
user's true intent — the agent instead receives an **observation** o that gives only partial,
possibly noisy information about the true state. A **Partially Observable MDP (POMDP)** extends
the MDP tuple ⟨S, A, P, R, γ⟩ with:
- Ω: a set of possible observations,
- Z(o | s', a): the observation model, the probability of observing o after action a lands the
  agent in (unobserved) state s'.

Because the agent cannot condition its action directly on the true state (it does not know it),
it must instead condition on everything it *does* know: its full history of actions and
observations, summarized compactly as a **belief state**.

## 2. Belief States
A **belief state** b is a probability distribution over the underlying states S: b(s) is the
agent's current probability that the true state is s, given everything observed so far. The
belief state is a **sufficient statistic** for the history — two different histories that
produce the same belief state are equivalent for all decision-making purposes, which is what
makes the belief-state formulation tractable in principle (even though, as discussed below, it is
not tractable in the same sense a finite-state MDP is).

## 3. The Belief-State Update (a Bayesian Filter)
After taking action a in belief state b and observing o, the new belief state b'(s') is computed
in two steps — a **prediction** step (apply the transition model) and an **update** step (apply
Bayes' rule using the observation model):
```
b_pred(s') = Σ_s  b(s) · P(s' | s, a)                        (prediction)

b'(s') = η · Z(o | s', a) · b_pred(s')                        (Bayesian update)
```
where η is a normalizing constant chosen so that Σ_s' b'(s') = 1. This is exactly a discrete
Bayesian filter (the same structure underlying, e.g., a Hidden Markov Model's forward algorithm):
predict forward using the dynamics, then reweight by how consistent each predicted state is with
the observation actually received.

```python
def belief_update(belief, action, observation, transition_model, observation_model):
    """belief: dict state -> probability. transition_model(s, a) -> list of (prob, next_state).
    observation_model(obs, next_state, a) -> probability P(obs | next_state, a)."""
    predicted = {}
    for s, p_s in belief.items():
        for p_trans, s2 in transition_model(s, action):
            predicted[s2] = predicted.get(s2, 0.0) + p_s * p_trans

    unnormalized = {
        s2: observation_model(observation, s2, action) * p_pred
        for s2, p_pred in predicted.items()
    }
    total = sum(unnormalized.values())
    if total == 0:
        raise ValueError("Observation has zero probability under this belief/model.")
    return {s2: p / total for s2, p in unnormalized.items()}
```

## 4. Worked Example: A Small Two-State POMDP
Consider two underlying states {Healthy, Sick} with a "stay" action that leaves the true state
unchanged (P(s'|s,"stay") = 1 for s'=s), and a noisy observation model for a "Positive"/"Negative"
test: Z(Positive | Sick) = 0.9, Z(Negative | Sick) = 0.1, Z(Positive | Healthy) = 0.2,
Z(Negative | Healthy) = 0.8. Starting from a prior belief b = {Healthy: 0.9, Sick: 0.1} and
observing "Positive" after the "stay" action:
```
b_pred = {Healthy: 0.9, Sick: 0.1}                       (unchanged; "stay" is identity)
unnormalized(Healthy) = 0.2 * 0.9 = 0.18
unnormalized(Sick)    = 0.9 * 0.1 = 0.09
total = 0.27
b'(Healthy) = 0.18 / 0.27 ≈ 0.667
b'(Sick)    = 0.09 / 0.27 ≈ 0.333
```
A single positive test result shifts belief in "Sick" from 0.1 to about 0.33 — an increase, but
still less than 50%, because the prior was strongly skewed toward "Healthy" and the test has a
nontrivial false-positive rate. This is the same Bayesian reasoning pattern as the undergraduate
course's Bayes'-rule diagnostic-test example, now embedded inside a sequential decision process.

## 5. Why POMDPs Are Much Harder Than the Underlying MDP
Even if the underlying state space S is small and finite, the **belief space** — the space of all
probability distributions over S — is continuous (a |S|−1-dimensional simplex). An optimal POMDP
policy is a mapping from this continuous belief space to actions, which cannot in general be
represented or computed exactly by the same finite-state methods (value/policy iteration) used
for MDPs. Exact POMDP solution methods exist (e.g., representing the optimal value function as a
piecewise-linear convex function over belief space) but scale very poorly; in practice, POMDPs
are usually solved approximately (e.g., by discretizing or sampling the belief space, or by
look-ahead search from the current belief) rather than exactly. This course treats POMDPs only at
the level of the formalism and the belief update — exact or approximate POMDP *solution*
algorithms are an advanced topic beyond this course's scope.

## 6. In-Class/Lab Exercise
Extend the worked example: compute the belief update for a second consecutive "Positive"
observation starting from the updated belief above, and discuss how belief in "Sick" evolves
after two positive tests in a row versus one.
