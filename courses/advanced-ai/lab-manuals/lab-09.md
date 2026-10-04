# Lab Manual 9 — Reward Modeling from Pairwise Preferences

**Duration:** 3 hours | **Prerequisite:** Week 9 lecture

## Objectives
Fit a reward model from synthetic pairwise-preference data, recover a known ground-truth
weight vector, and empirically demonstrate how a systematic labeler bias distorts the learned
reward model.

## Setup
1. Reuse your course virtual environment.
2. Create `lab09.ipynb`.

## Procedure
1. **Task A — Synthetic data:** generate n_pairs=500 feature-vector pairs (d=4) and a known
   ground-truth weight vector `w_true`. Simulate noisy pairwise preferences via the Bradley-Terry
   model P(A ≻ B) = σ(w_true · (featuresA − featuresB)), sampling `preferred_is_a` accordingly.
2. **Task B — Fit and evaluate:** implement `fit_reward_model` exactly as in the Week 9 lecture
   content. Fit it on the Task A data, compare the learned weight vector to `w_true` (e.g., via
   cosine similarity), and report ranking agreement between the learned and ground-truth reward
   on a held-out set of items.
3. **Task C — Labeler bias:** introduce a systematic bias: for pairs where item A has a positive
   value on one specific feature, flip the recorded preference away from what the Bradley-Terry
   model would have generated, for a given fraction of such pairs. Refit the reward model on the
   biased data and quantify the degradation (cosine similarity to `w_true`) relative to Task B.
4. **Task D — Mini-challenge:** sweep the fraction of biased pairs introduced in Task C over at
   least 4 values (e.g., 0%, 10%, 30%, 60%) and plot the resulting degradation (cosine similarity
   to `w_true`) as a function of the bias fraction.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output and the Task D degradation plot.

## Submission
Export/submit `lab09.ipynb` via the course submission system by the end of the lab session.
