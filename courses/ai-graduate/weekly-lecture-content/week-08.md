# Week 8 — Lecture Content: Markov Decision Processes I

## 1. The MDP Formalism
A **Markov Decision Process (MDP)** is a tuple ⟨S, A, P, R, γ⟩:
- S: a set of states.
- A: a set of actions (A(s) if action availability depends on the state).
- P(s' | s, a): the transition model — the probability of landing in state s' after taking
  action a in state s. The **Markov property** means this depends only on the current state and
  action, not on the history that led there.
- R(s, a, s'): the reward received for this transition (sometimes simplified to R(s) or R(s,a)).
- γ ∈ [0, 1): the discount factor, controlling how much the agent prefers immediate reward over
  future reward.

A **policy** π(s) maps states to actions. The goal is to find an optimal policy π* maximizing the
expected sum of discounted future rewards from any starting state.

## 2. The Bellman Equation
Define the **state-value function** under a fixed policy π as the expected discounted return
starting from s and following π thereafter:
```
V^π(s) = E[ R(s, π(s), s') + γ V^π(s') ]
       = Σ_s' P(s' | s, π(s)) [ R(s, π(s), s') + γ V^π(s') ]
```
The **optimal value function** V*(s) satisfies the **Bellman optimality equation**:
```
V*(s) = max_a  Σ_s' P(s' | s, a) [ R(s, a, s') + γ V*(s') ]
```
This is a recursive definition: the optimal value of a state is the best achievable immediate
reward plus the discounted optimal value of wherever that action leads, in expectation over the
transition model. An optimal policy is then recovered by acting greedily with respect to V*:
```
π*(s) = argmax_a  Σ_s' P(s' | s, a) [ R(s, a, s') + γ V*(s') ]
```

## 3. Value Iteration
Value iteration computes V* by repeated Bellman backups, starting from an arbitrary V₀ (commonly
all zeros):
```
V_{k+1}(s) = max_a  Σ_s' P(s' | s, a) [ R(s, a, s') + γ V_k(s') ]    for every state s
```
Repeat until the maximum change across all states, max_s |V_{k+1}(s) − V_k(s)|, falls below a
small threshold ε.

```python
def value_iteration(states, actions, transition_model, reward, gamma, theta=1e-6):
    """transition_model(s, a) -> list of (prob, next_state).
    reward(s, a, s_next) -> float. gamma: discount factor, must be in [0, 1)."""
    V = {s: 0.0 for s in states}
    while True:
        delta = 0.0
        new_V = dict(V)
        for s in states:
            if not actions(s):
                continue
            best = max(
                sum(p * (reward(s, a, s2) + gamma * V[s2])
                    for p, s2 in transition_model(s, a))
                for a in actions(s)
            )
            delta = max(delta, abs(best - V[s]))
            new_V[s] = best
        V = new_V
        if delta < theta:
            break
    return V

def extract_policy(states, actions, transition_model, reward, gamma, V):
    policy = {}
    for s in states:
        if not actions(s):
            continue
        policy[s] = max(
            actions(s),
            key=lambda a: sum(p * (reward(s, a, s2) + gamma * V[s2])
                               for p, s2 in transition_model(s, a))
        )
    return policy
```

## 4. Why Value Iteration Converges (Contraction-Mapping Argument, Conceptual)
Define the **Bellman optimality operator** B on value functions: (BV)(s) = max_a Σ_s' P(s'|s,a)
[R(s,a,s') + γV(s')]. Value iteration is exactly the iteration V_{k+1} = B(V_k). The key fact is
that B is a **contraction mapping** under the max-norm ‖·‖∞ with contraction factor γ: for any
two value functions U, V,
```
‖B(U) − B(V)‖∞ ≤ γ ‖U − V‖∞
```
(intuition: the max over actions and the expectation over the transition model are both
non-expansive operations, so the only "shrinking" or "growing" factor applied to the difference
between U and V is the discount γ multiplying V(s') inside the backup). Because γ < 1, repeated
application of B shrinks the distance to the (unique) fixed point V* geometrically, so V_k → V*
as k → ∞, regardless of the arbitrary starting point V₀. This is also exactly why **the discount
factor must satisfy γ < 1** for this convergence guarantee: if γ = 1, B is no longer guaranteed to
be a strict contraction, and value iteration can fail to converge (or converge only in special
cases, e.g., when every policy is guaranteed to reach a terminal/absorbing state).

## 5. In-Class/Lab Exercise
Implement `value_iteration` on a 4x4 grid-world with a +1 goal reward, a −1 trap, and −0.04 step
cost elsewhere; run it with γ = 0.9 and again with γ = 1.0 on a variant without any absorbing
states, and observe/explain the difference in convergence behavior.
