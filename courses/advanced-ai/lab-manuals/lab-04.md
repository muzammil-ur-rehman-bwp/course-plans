# Lab Manual 4 — Contextual Bandits vs. a Context-Free Baseline

**Duration:** 3 hours | **Prerequisite:** Week 4 lecture

## Objectives
Implement a simplified LinUCB-style contextual-bandit algorithm and empirically confirm it
achieves lower regret than a context-free UCB1 baseline once it has seen enough contexts to
estimate each arm's reward model.

## Setup
1. Reuse your course virtual environment.
2. Create `lab04.ipynb`.

## Procedure
1. **Task A — Implementation:** implement `LinUCBArm` and `lin_ucb_choose` exactly as in the
   Week 4 lecture content (ridge-regression reward estimate plus a UCB-style confidence bonus).
2. **Task B — Environment:** construct a synthetic contextual bandit with d=4 context features
   and K=3 arms, where each arm a has a known ground-truth weight vector θₐ and
   E[r | x, a] = x · θₐ + noise. Draw a fresh context xₜ ~ N(0, I) each round.
3. **Task C — Comparison:** run the LinUCB-style algorithm for T=3000 rounds, repeated over 20
   random seeds, against a context-free UCB1 baseline that ignores the context entirely (treats
   the problem as a plain K-armed bandit on the context-averaged reward). Plot mean cumulative
   regret with a spread band (±1 standard deviation across seeds) for both on the same axes.
4. **Task D — Mini-challenge:** sweep the LinUCB confidence parameter `alpha` over at least 4
   values and report, in 2–3 sentences, its effect on early-round vs. late-round regret —
   connecting the tradeoff to the Week 3 exploration-exploitation discussion.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output and the comparison plot described in Task C.

## Submission
Export/submit `lab04.ipynb` via the course submission system by the end of the lab session.
