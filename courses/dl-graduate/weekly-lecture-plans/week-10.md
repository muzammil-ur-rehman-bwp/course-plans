# Week 10 Lecture Plan — Deep Learning (Graduate)
## Topic: Deep Reinforcement Learning I — Deep Q-Networks

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Extend tabular Q-learning (assumed from *Artificial Intelligence*, Graduate) to function
   approximation via a neural network. (*Apply*)
2. Explain why naive function approximation combined with Q-learning can diverge. (*Analyze*)
3. Implement DQN with experience replay and a target network in PyTorch. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap (not re-derived) | The Bellman equation and tabular Q-learning update, stated as assumed from the prerequisite AI course |
| 0:15–0:40 | From tables to networks | Why a $Q$-table is infeasible for large/continuous state spaces; $Q_\theta(s,a)$ as a function approximator |
| 0:40–1:05 | Instability & experience replay | Correlated sequential data, catastrophic forgetting; the replay buffer fix |
| 1:05–1:15 | Break | — |
| 1:15–1:40 | Target networks | The moving-target problem in bootstrapped regression; periodic target-network updates |
| 1:40–2:00 | The DQN loss, assembled | Full loss expression; live-coding a minimal training step |

### Materials/Equipment
- Live-coding environment, PyTorch, `gymnasium` (or custom grid-world)
- Slide diagram: DQN data flow (replay buffer → online network → target network → loss)

### Formative Check (in-class)
Given a transition $(s,a,r,s')$ and current $Q_\theta$, $Q_{\theta^-}$ values, students compute
the DQN target and the squared-error loss term by hand.

### Link to Lab/Assessment
Lab 10: Implementing DQN with an experience replay buffer and a target network on a toy
environment, and comparing training stability with and without each component (see
`lab-manuals/lab-10.md`).
