# Week 11 — Lecture Content: Scaling Laws from a Systems/Engineering Perspective

## 1. Scoping This Week Precisely
The sibling *Advanced Artificial Neural Network* course's Week 8 asks a **theoretical** question:
*why* does loss fall as a power law in model size, data size, and compute at all — data-manifold/
intrinsic-dimension arguments, random-feature/kernel-theoretic arguments. **This week takes the
power law as a given empirical fact and asks a systems/engineering question instead: given that
fact, and a fixed compute budget, how should a real training run actually allocate it?** Different
question, same underlying empirical phenomenon, no content overlap — if you find yourself deriving
*why* a power law holds this week, you have wandered into the sibling course's territory.

## 2. The Training-FLOPs Approximation
For a Transformer-style model, the standard approximation for the total floating-point operations
to train on $D$ tokens is $C \approx 6ND$, where $N$ is the parameter count and $D$ the number of
training tokens (the factor of 6 comes from approximately 2 FLOPs per parameter per forward pass
and a backward pass costing roughly twice the forward pass, $2 + 4 = 6$; this is already available
from the graduate course's large-scale-training material and is not re-derived here). This
approximation is the lever the rest of this week turns on: for a **fixed** compute budget $C$,
$N$ and $D$ are not independent — increasing one, at fixed $C$, forces decreasing the other.

## 3. Compute-Optimal Allocation
Empirically fitted scaling relationships (broadly attributed to Hoffmann et al.'s refinement of
earlier scaling-law work) show that loss, as a function of $N$ and $D$ at fixed total compute $C$,
is minimized not by maximizing $N$ alone (holding $D$ fixed or letting it be whatever $C/6N$
implies as an afterthought) but by growing **both** $N$ and $D$ together, at comparable rates, as
$C$ grows — the compute-optimal allocation is (approximately) a power-law split
$N^* \propto C^{a}$, $D^* \propto C^{b}$ for fitted exponents $a,b$ with $a+b\approx 1$ (consistent
with $C\approx 6ND$), and — crucially for a systems engineer — $a$ and $b$ are **not** $1$ and
$0$: both exponents are meaningfully positive, refuting the simpler, earlier assumption that model
size should be scaled aggressively while data is treated as a secondary concern.

```python
import numpy as np

def compute_optimal_allocation(C, a=0.5, b=0.5, N_ref=1.0, D_ref=1.0, C_ref=1.0):
    """Toy illustrative allocation: N*, D* scale as power laws in C with fitted exponents a, b
    (a+b should be close to 1, consistent with C ~ 6*N*D); N_ref, D_ref, C_ref anchor the fit."""
    N_star = N_ref * (C / C_ref) ** a
    D_star = D_ref * (C / C_ref) ** b
    return N_star, D_star

def naive_allocation(C, N_ref, D_ref, C_ref):
    """Naive baseline: grow N proportionally to C, hold the ratio N/D fixed at the reference
    ratio -- i.e., scale N and D by the SAME factor, rather than the fitted, generally unequal
    compute-optimal exponents a, b."""
    scale = C / C_ref
    return N_ref * scale, D_ref * scale
```
**The direct practical consequence:** a model trained with $N$ substantially larger than its
data-optimal size for the compute spent (equivalently, trained on too few tokens relative to its
size) is wasting compute relative to a smaller model trained on proportionally more data at the
same total cost — a concrete, actionable allocation decision for anyone planning a training run
on a fixed budget, not a theoretical curiosity about *why* the exponents take the values they do.

## 4. Checkpointing
Long training runs periodically save full model/optimizer state (a **checkpoint**) so a failure
does not require restarting from scratch. The tradeoff: checkpointing too rarely risks losing a
large amount of compute if a failure occurs just before the next scheduled checkpoint; checkpointing
too often wastes compute/storage/time on the checkpointing operation itself (writing a large
model's full state to persistent storage is not free). For a checkpoint interval $\tau$, a
per-checkpoint cost $c$, and a failure process with rate $\lambda$ (failures per unit compute-time),
the expected compute lost per unit time is approximately $c/\tau + \lambda\tau/2$ (the first term
from checkpointing overhead happening every $\tau$, the second from losing, on average, half an
interval's worth of work when a failure lands uniformly within it) — minimizing over $\tau$ gives
$\tau^* = \sqrt{2c/\lambda}$, by the standard first-derivative-zero argument
($d/d\tau\,[c/\tau + \lambda\tau/2] = -c/\tau^2 + \lambda/2 = 0 \Rightarrow \tau^2 = 2c/\lambda$).

```python
def optimal_checkpoint_interval(checkpoint_cost, failure_rate):
    """tau* = sqrt(2c/lambda), minimizing expected lost compute per unit time c/tau + lambda*tau/2."""
    return (2 * checkpoint_cost / failure_rate) ** 0.5

def expected_lost_compute(tau, checkpoint_cost, failure_rate):
    return checkpoint_cost / tau + failure_rate * tau / 2
```

## 5. Fault Tolerance (Conceptual)
At the node-count and duration of a modern large-scale training run (potentially thousands of
accelerators for weeks), at least one hardware failure somewhere in the cluster over the run's
lifetime is close to a certainty, not an edge case — a qualitatively different reliability regime
than a single-GPU experiment. Production large-scale training systems are therefore engineered
around **elastic, fault-tolerant restart**: the training job is designed to detect a failed
worker, drop it (or replace it), and resume from the last checkpoint with the remaining/replacement
resources, rather than assuming uninterrupted execution for the run's full duration. This is the
systems reality the Section 3 compute-optimal-allocation math has to survive in practice — an
allocation that is mathematically optimal on paper but assumes zero downtime is not, in fact,
optimal once real failure rates are accounted for.

## 6. In-Class/Lab Exercise
Using `compute_optimal_allocation` and `naive_allocation` with illustrative fitted exponents
$a=0.46,b=0.54$ (consistent with $a+b\approx1$ and noticeably closer to equal than to $(1,0)$),
compute both allocations' $(N,D)$ at several compute budgets and plot the resulting (toy, provided)
loss curve each would achieve, showing the naive allocation's growing gap from compute-optimal as
$C$ increases. Using `optimal_checkpoint_interval`, compute $\tau^*$ for a toy training-run
specification (checkpoint cost, assumed node failure rate) and plot `expected_lost_compute` as a
function of $\tau$ to confirm the computed $\tau^*$ sits at the curve's minimum. Finally, write a
precise, 100–150 word statement distinguishing this week's compute-allocation question from the
sibling *Advanced Artificial Neural Network* course's theoretical "why do power laws hold"
question.
