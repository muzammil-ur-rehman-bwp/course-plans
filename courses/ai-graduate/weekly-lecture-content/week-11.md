# Week 11 — Lecture Content: Probabilistic Graphical Models at Rigor

## 1. Review: Bayesian Networks as Factored Joint Distributions
A Bayesian network represents a joint distribution over variables X₁, ..., Xₙ compactly via a
directed acyclic graph and a conditional probability table (CPT) for each node given its parents:
```
P(X₁, ..., Xₙ) = Π_i P(Xᵢ | Parents(Xᵢ))
```

## 2. The Computational Complexity of Exact Inference
**Result (stated, with intuition).** Exact inference in a general Bayesian network — computing
P(query variable | evidence) exactly — is **#P-hard** (a counting-complexity class at least as
hard as NP), and even the standard exact algorithms (inference by enumeration, variable
elimination) run in time exponential in the network's **treewidth** in the worst case. Intuition:
variable elimination's cost is dominated by the size of the largest intermediate factor it must
construct, which is governed by how the elimination order groups variables together — for
densely connected networks (high treewidth), no elimination order avoids exponentially large
intermediate factors, because eliminating any variable that is entangled with many others forces
a factor over all of them simultaneously. For networks with low treewidth (e.g., tree-structured
or sparsely connected networks), exact inference is tractable (polynomial); in the worst case
(dense, highly connected networks), it is not. This is the structural reason the field also
relies on approximate, sampling-based inference.

## 3. Rejection Sampling
Rejection sampling estimates P(X | e) (query variable X given evidence e) by repeatedly sampling
a complete assignment to all variables from the joint distribution (using the network's topological
order and each node's CPT), and **discarding** any sample inconsistent with the evidence e.
Among the retained samples, the empirical frequency of each value of X estimates P(X | e).

```python
import random

def sample_bayes_net(nodes_in_topo_order, cpt, parents):
    """nodes_in_topo_order: list of variable names, parents before children.
    cpt[var][tuple(parent_values)] -> probability that var = True (binary variables assumed).
    parents[var] -> list of parent variable names. Returns a dict var -> bool."""
    sample = {}
    for var in nodes_in_topo_order:
        parent_vals = tuple(sample[p] for p in parents[var])
        p_true = cpt[var][parent_vals]
        sample[var] = random.random() < p_true
    return sample

def rejection_sampling(nodes_in_topo_order, cpt, parents, query_var, evidence, n_samples=10000):
    counts = {True: 0, False: 0}
    kept = 0
    for _ in range(n_samples):
        sample = sample_bayes_net(nodes_in_topo_order, cpt, parents)
        if all(sample[var] == val for var, val in evidence.items()):
            counts[sample[query_var]] += 1
            kept += 1
    if kept == 0:
        raise ValueError("No samples consistent with evidence; evidence may be too rare.")
    return {k: v / kept for k, v in counts.items()}
```

**The inefficiency.** If the evidence e is a rare event under the prior, nearly every sample is
discarded, wasting almost all the computation — this gets worse the rarer the evidence, and is
the key weakness rejection sampling has that the next method addresses.

## 4. Likelihood Weighting
Likelihood weighting never discards a sample. Instead, evidence variables are **fixed** to their
observed values (not sampled), every other variable is sampled from its CPT as before, and each
sample is given a **weight** equal to the product of the probabilities of the evidence values
given their parents (how "likely" this sample's context made the fixed evidence):

```python
def likelihood_weighting(nodes_in_topo_order, cpt, parents, query_var, evidence, n_samples=10000):
    totals = {True: 0.0, False: 0.0}
    for _ in range(n_samples):
        sample = {}
        weight = 1.0
        for var in nodes_in_topo_order:
            parent_vals = tuple(sample[p] for p in parents[var])
            p_true = cpt[var][parent_vals]
            if var in evidence:
                sample[var] = evidence[var]
                weight *= p_true if evidence[var] else (1 - p_true)
            else:
                sample[var] = random.random() < p_true
        totals[sample[query_var]] += weight
    total_weight = sum(totals.values())
    return {k: v / total_weight for k, v in totals.items()}
```

Because every sample is used (weighted, not rejected), likelihood weighting is strictly more
efficient than rejection sampling whenever evidence is uncommon — this is the standard
improvement taught alongside rejection sampling, and it is worth implementing both specifically
to observe the variance/efficiency difference empirically (as the lab for this week asks).
Likelihood weighting's own weakness: if the evidence variables are "downstream" of variables that
strongly influence them, most of the sample's weight can still concentrate on a few samples,
giving high-variance estimates — this motivates MCMC methods.

## 5. Markov Chain Monte Carlo (MCMC), Briefly
**Gibbs sampling**, a common MCMC method for Bayesian networks, builds a Markov chain over
complete variable assignments: starting from an arbitrary assignment consistent with the
evidence, it repeatedly resamples one non-evidence variable at a time conditioned on the current
values of *all* other variables (its "Markov blanket"), which can be computed efficiently from
local CPTs. After a "burn-in" period, the chain's samples are distributed according to the true
posterior P(X | e), without ever needing to reject a sample or compute a global likelihood
weight. This course covers Gibbs sampling/MCMC only at this conceptual level — the full theory of
why the Markov chain converges to the correct stationary distribution (detailed balance,
ergodicity) is a specialized topic beyond this course's scope.

## 6. In-Class/Lab Exercise
On a small 4-node Bayesian network with one rare evidence variable (e.g., P(evidence)=0.05 under
the prior), run both `rejection_sampling` and `likelihood_weighting` with the same sample budget
and compare (a) the fraction of samples actually used and (b) the variance of the resulting
estimate across several repeated runs.
