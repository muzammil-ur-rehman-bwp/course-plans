# Lab Manual 6 — Bradley-Terry Reward Model

**Duration:** 3 hours | **Prerequisite:** Week 6 lecture

## Objectives
Train a reward model via the Bradley-Terry logistic loss on synthetic preference pairs with a
known ground-truth reward, and verify the fitted model's induced ranking.

## Setup
1. Reuse your Week 1 virtual environment.
2. Create `lab06.ipynb`.

## Procedure
1. **Task A — Synthetic data:** define a ground-truth reward function $r^*(x,y) =
   \theta^{*\top}\phi(x,y)$ over a toy feature space; generate preference pairs by sampling the
   preference label from $\sigma(r^*(x,y_1)-r^*(x,y_2))$ (Bradley-Terry-consistent noise, not a
   deterministic comparison).
2. **Task B — Reward model:** implement `RewardModel` and `bradley_terry_loss` exactly as in the
   Week 6 lecture content; train on the Task A data.
3. **Task C — Ranking verification:** on a held-out set of items, compute the fitted model's
   induced ranking and the ground-truth ranking, and report their Spearman rank correlation.
4. **Task D — KL-penalty reasoning:** write out, symbolically, the full stage-3 RLHF objective for
   a toy categorical policy over the same item set, and state in one paragraph what concretely
   goes wrong (with a specific, constructed example) if $\beta=0$.

## Expected Output
A notebook with four clearly labeled sections (A–D) and the reported correlation value.

## Submission
Submit `lab06.ipynb` via the course submission system by the end of the lab session.
