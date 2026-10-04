# Lab Manual 3 — Classifier-Free Guidance and Flow Matching

**Duration:** 3 hours | **Prerequisite:** Week 3 lecture, Lab 2 complete

## Objectives
Implement classifier-free guidance on a conditional extension of the Lab 2 toy score model, and a
minimal flow-matching training loop and ODE sampler, comparing both against the Week 2 baseline.

## Setup
1. Reuse your Lab 2 notebook/environment.
2. Create `lab03.ipynb`.

## Procedure
1. **Task A — Conditional data and model:** label each Lab 2 mixture component with a class $c$;
   extend `ScoreNet` to accept a class embedding (or one-hot) input alongside $x,t$, and train
   with 15% condition dropout (randomly replace $c$ with a null embedding).
2. **Task B — Guided sampling:** implement `classifier_free_guidance` exactly as in lecture;
   sample at $w \in \{1, 3, 7\}$ and visualize how samples concentrate toward the target
   component's mean as $w$ increases, including at least one plot showing reduced spread
   (diversity) at high $w$.
3. **Task C — Flow matching:** implement `VelocityNet` and `flow_matching_loss` exactly as in
   lecture; train on the (unconditional) Lab 2 mixture data and implement `flow_matching_sample`.
4. **Task D — Comparison:** for visually comparable sample quality, find the smallest `n_steps`
   that gives acceptable flow-matching samples and compare it to the smallest `n_steps` needed for
   the Lab 2 reverse-SDE sampler; report the ratio and a one-paragraph explanation referencing the
   lecture's discussion of why flow matching can need fewer steps.

## Expected Output
A notebook with four clearly labeled sections (A–D) and the required plots.

## Submission
Submit `lab03.ipynb` via the course submission system by the end of the lab session.
