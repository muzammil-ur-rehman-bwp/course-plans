# Week 10 Lecture Plan — Artificial Intelligence (Graduate)
## Topic: Partially Observable MDPs (POMDPs)

**Duration:** 2 hours lecture + 3 hour lab/seminar

### Learning Objectives (Bloom's Level)
1. Explain why partial observability complicates planning relative to a fully observable MDP.
   (*Understand*)
2. Compute a belief-state update by hand for a small two-state POMDP given a prior, an action,
   and an observation. (*Apply, Analyze*)
3. Articulate the distinction between a policy over states (MDP) and a policy over belief
   states (POMDP). (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | Motivation | Why an agent often cannot observe the true state directly |
| 0:20–0:50 | Belief states | Formal definition; the belief-update (Bayesian filter) equation |
| 0:50–1:25 | Worked example | Hand-traced belief update on a small two-state POMDP |
| 1:25–1:50 | Why POMDPs are hard | Continuous belief space vs. discrete underlying state space |
| 1:50–2:00 | Synthesis | Where POMDPs show up in practice (robotics, dialogue systems) |

### Materials/Equipment
- Slides: "POMDPs and Belief States"
- Whiteboard for the hand-traced belief-update example
- Live-coding environment (Jupyter)

### Formative Check (in-class)
Given a prior belief, a transition model, an observation model, and an observed signal, compute
the updated belief by hand and verify it sums to 1 after normalization.

### Link to Lab/Assessment
Lab 10: implement the belief-state update for a small POMDP (see `lab-manuals/lab-10.md`).
