# Lab Manual 10 — Deep Q-Networks on a Toy Environment

**Duration:** 3 hours | **Prerequisite:** Week 10 lecture

## Objectives
Implement DQN with an experience replay buffer and a target network; empirically compare
training stability with and without each component.

## Setup
Create `lab10.ipynb`. Start from the lecture's `QNetwork`, `ReplayBuffer`, `dqn_update`, and
`update_target_network`. Use `gymnasium`'s `CartPole-v1` if available, otherwise a small custom
grid-world.

## Procedure
1. **Task A — Full DQN:** implement the full DQN training loop (replay buffer + target network);
   train until the agent reaches a reasonable return threshold; plot episode return over
   training.
2. **Task B — No target network ablation:** retrain using the online network as its own target
   (remove the target-network freeze); plot episode return and compare stability against Task A.
3. **Task C — No replay buffer ablation:** retrain updating directly on each new transition
   (no buffer, batch size 1, no resampling); plot episode return and compare stability against
   Task A.
4. **Task D — Discussion:** in a markdown cell, relate the Task B and Task C instability (or lack
   thereof, on this simple environment) to the three failure factors from lecture.

## Expected Output
A notebook with Tasks A–D; all three training curves plotted on one chart for comparison.

## Submission
Submit `lab10.ipynb` by the end of the lab session.
