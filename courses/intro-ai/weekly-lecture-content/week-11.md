# Week 11 — Lecture Content: Bayesian Networks

## 1. Why Bayesian Networks?
A joint distribution over `n` binary variables needs `2^n - 1` numbers in general — intractable
even for modest `n`. A **Bayesian network** exploits conditional independence to represent the
same joint distribution with far fewer numbers: one conditional probability table (CPT) per
node, conditioned only on its direct parents in a directed acyclic graph.

## 2. Network Structure
Classic textbook example: `Burglary` and `Earthquake` can each (independently) trigger an
`Alarm`; if the `Alarm` sounds, neighbors `JohnCalls` or `MaryCalls` might call. Edges encode
direct causal/probabilistic influence; a node's CPT gives `P(node | parents)`.

```python
# Simplified 3-node version: Burglary, Earthquake -> Alarm
cpt_burglary = {True: 0.001, False: 0.999}
cpt_earthquake = {True: 0.002, False: 0.998}

# P(Alarm | Burglary, Earthquake)
cpt_alarm = {
    (True,  True):  0.95,
    (True,  False): 0.94,
    (False, True):  0.29,
    (False, False): 0.001,
}
```

The key structural claim encoded by the graph: `Burglary` and `Earthquake` are independent of
each other (no edge between them), and `Alarm` depends on both, but **given Alarm**, nothing
downstream needs to know about `Burglary`/`Earthquake` directly — all relevant information flows
through `Alarm`. This is the conditional-independence assumption that makes the network compact.

## 3. Inference by Enumeration
To compute `P(query | evidence)`, sum the full joint probability (expressed as a product of
CPT entries, by the chain rule for Bayesian networks) over all values of the hidden variables,
then normalize.

```python
def joint_prob(b, e, a):
    pb = cpt_burglary[b]
    pe = cpt_earthquake[e]
    pa = cpt_alarm[(b, e)] if a else (1 - cpt_alarm[(b, e)])
    return pb * pe * pa

def p_burglary_given_alarm(alarm_observed=True):
    numerator = sum(joint_prob(True, e, alarm_observed) for e in [True, False])
    denominator = sum(joint_prob(b, e, alarm_observed) for b in [True, False] for e in [True, False])
    return numerator / denominator

print(f"P(Burglary | Alarm) = {p_burglary_given_alarm():.4f}")
```

## 4. Hand-Worked Example
By hand: `P(B=T, A=T) = P(B=T) * [P(E=T) P(A=T|B=T,E=T) + P(E=F) P(A=T|B=T,E=F)]`
`= 0.001 * [0.002 * 0.95 + 0.998 * 0.94] ≈ 0.001 * 0.9394 ≈ 0.0009394`.
Similarly compute `P(B=F, A=T)`, then `P(B=T | A=T) = P(B=T, A=T) / [P(B=T, A=T) + P(B=F, A=T)]`.
Working through these sums by hand (even for this tiny 3-node network) is the point of the
exercise — it makes concrete why inference by enumeration, while always correct, grows
expensive quickly as more variables are added (a motivation, not implemented in this course, for
more advanced inference algorithms like variable elimination or sampling).

## 5. Conditional Independence From Structure
A useful reading skill: `JohnCalls` and `MaryCalls` (if added to the network) would be
conditionally independent given `Alarm` — once we know whether the alarm sounded, knowing that
John called tells us nothing extra about whether Mary called. This separates "what's connected
in the graph" from "what's independent given evidence," a distinction worth practicing on paper
before trusting any inference code.

## 6. Capstone Kickoff
The capstone project (introduced in Week 9, proposal due this week) asks you to apply **one**
classical technique from this course end-to-end. A Bayesian-network-based diagnostic tool (e.g.,
a small fault-diagnosis network with 4–6 nodes) is one of the suggested capstone directions —
see `assignments/capstone-proposal-guidelines.md`.

## 7. In-Class Exercise
Given the 3-node network above and CPTs, compute `P(Burglary | Alarm = true)` by hand via
enumeration, then verify with the code.
