# Lab Manual 7 — Direct Preference Optimization

**Duration:** 3 hours | **Prerequisite:** Week 7 lecture, Lab 6 complete

## Objectives
Implement the DPO loss and train a small policy against a frozen reference policy on synthetic
preference pairs, verifying the fitted policy's implied reward ranking.

## Setup
1. Reuse your Lab 6 notebook/environment.
2. Create `lab07.ipynb`.

## Procedure
1. **Task A — Setup:** reuse Lab 6's ground-truth reward $r^*$ and preference-pair generator;
   define a small categorical "policy" $\pi_\theta(y\mid x)$ (a softmax over a learned logit per
   item) and a frozen uniform reference policy $\pi_{\mathrm{ref}}$.
2. **Task B — DPO loss:** implement `dpo_loss` exactly as in the Week 7 lecture content; train
   $\pi_\theta$ on the Lab 6 preference pairs.
3. **Task C — Verification:** compute the fitted policy's implied reward
   $\beta\log(\pi_\theta(y\mid x)/\pi_{\mathrm{ref}}(y\mid x))$ for each item, and report its
   Spearman rank correlation with $r^*$ (compare against Lab 6's reward-model correlation).
4. **Task D — Derivation trace:** write out, symbolically, each step of the closed-form-optimal-
   policy derivation and the Bradley-Terry substitution for your own toy setup, explicitly marking
   where the $Z(x)$ cancellation occurs.

## Expected Output
A notebook with four clearly labeled sections (A–D) and the derivation trace (markdown cells are
acceptable for Task D).

## Submission
Submit `lab07.ipynb` via the course submission system by the end of the lab session.
