# Week 7 — Lecture Content: The Grokking Phenomenon

## 1. The Observation
Power et al. observed and named **grokking** on certain small, algorithmic tasks — the clearest
reported case is modular arithmetic (e.g., learning $a+b \bmod p$ from a subset of
input-output pairs, with a network trained to classify the correct result among $p$ classes). The
characteristic curve has three phases:
1. **Rapid memorization.** Training accuracy reaches (near) 100% very early in training, while
   test accuracy stays near chance level ($1/p$ for $p$ classes) — the network fits the training
   examples without yet generalizing.
2. **A long plateau.** Training accuracy remains saturated near 100%; test accuracy remains near
   chance for a long subsequent stretch of optimizer steps — often orders of magnitude longer than
   the time it took to reach phase 1.
3. **Sharp, delayed generalization.** Test accuracy then rises sharply, often over a comparatively
   short number of further steps, to near-100% — "grokking" the task, well after training loss had
   already saturated near zero.

## 2. Why This Is Theoretically Puzzling
The classical intuition — once training loss is near zero, training and test performance should
track each other reasonably closely, modulo an overfitting gap predicted by capacity — is directly
violated by phase 2: a model that has long since reached (near) zero training loss continues to
sit at chance-level test accuracy for a large number of further gradient steps, with *no further
improvement in training loss to explain what changes*. Whatever happens during phase 2 that
eventually produces phase 3's sharp transition is not visible in the training loss curve at all —
it must be a change in some other property of the learned solution (its weight-space geometry, its
internal computation, or both) that training loss alone does not register.

## 3. Competing Hypothesis 1: Slow Implicit-Regularization-Driven Transition
This account holds that an implicit-regularization force — commonly, weight decay, interacting
with the kind of margin-maximization dynamics studied in Week 4 — continues to reshape the
solution *within* the zero-training-loss manifold throughout phase 2, slowly moving the network
from a "memorizing" solution (one of the many zero-training-loss solutions, but one that does not
generalize) toward a qualitatively different, lower-complexity (by some norm or margin measure)
"generalizing" solution, with the sharp phase-3 transition marking the point at which the
generalizing solution's structure becomes dominant. Evidence cited for this account typically
includes weight-norm trajectories that continue to shrink (under weight decay) throughout the
plateau, well after training accuracy has saturated, and the empirical sensitivity of whether and
when grokking occurs to the weight-decay strength (stronger decay can shorten the plateau; removing
it entirely can prevent generalization from ever arriving).

## 4. Competing Hypothesis 2: Circuit Formation
This account holds that the network, from early in training, maintains **two internal
computational strategies at once** — something closer to a lookup-table/memorization circuit and
something closer to a genuinely algorithmic, generalizing circuit (e.g., one that has learned a
structure functionally equivalent to the arithmetic operation itself, rather than memorized
input-output pairs) — and that the generalizing circuit is present but *weak* (contributing little
to the output) for most of phase 2, slowly strengthening relative to the memorizing circuit until
it dominates, producing the sharp transition. Evidence cited for this account typically involves
inspecting learned internal representations for structure consistent with the task's actual
algebraic structure (e.g., representations with properties resembling the structure of modular
arithmetic) emerging gradually before the sharp test-accuracy transition, rather than appearing
abruptly at the transition itself.

## 5. Honest Synthesis: What Is and Is Not Settled
Both hypotheses are consistent with the core three-phase curve; they differ in *where the real
causal action is* during phase 2 (global solution geometry vs. two coexisting internal circuits),
and they are not mutually exclusive — a circuit-formation account could itself be *driven by* the
same implicit-regularization forces hypothesis 1 identifies. What current evidence robustly
establishes: (a) the three-phase curve itself is real and reproducible on several small algorithmic
tasks; (b) weight decay strength and training-set size (relative to the task's total input space)
both empirically control whether and how quickly grokking occurs — too little data or absent weight
decay can prevent it. What current evidence does **not** robustly establish: a single, agreed
mechanistic account that distinguishes the two hypotheses above, or confidently predicts, for a
novel task, whether grokking will occur and how long the plateau will last, from first principles
rather than after observing it empirically. Students should state the phenomenon and both
hypotheses precisely, and resist the temptation — common in informal secondary discussion of
grokking — to present either hypothesis as if it were the single settled explanation.

## 6. In-Class/Seminar Exercise
See `lab-manuals/lab-07.md`: write a position paper that (a) restates the grokking phenomenon
precisely, (b) identifies one piece of evidence that would favor hypothesis 1 over hypothesis 2 (or
vice versa) if observed, and (c) states explicitly which open question about grokking you find most
interesting as a potential capstone-adjacent research direction, without overstating what is
currently known.
