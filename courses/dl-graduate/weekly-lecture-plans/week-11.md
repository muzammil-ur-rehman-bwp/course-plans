# Week 11 Lecture Plan — Deep Learning (Graduate)
## Topic: Deep Reinforcement Learning II — Policy Gradient and Actor-Critic Methods

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Derive the REINFORCE policy-gradient estimator via the log-derivative trick. (*Analyze*)
2. Implement REINFORCE with a return baseline on a toy environment. (*Apply*)
3. Explain, conceptually, how actor-critic methods reduce variance via a learned baseline.
   (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | Week 10's value-based DQN; today: directly optimizing the policy instead |
| 0:15–0:50 | REINFORCE derivation | The policy-gradient theorem; the log-derivative trick; the REINFORCE estimator, step by step |
| 0:50–1:10 | Variance & baselines | Why the raw estimator is high-variance; subtracting a baseline without introducing bias |
| 1:10–1:20 | Break | — |
| 1:20–1:45 | Actor-critic (conceptual) | The critic as a learned value baseline; the advantage $A(s,a)=G_t-V(s)$; actor and critic losses |
| 1:45–2:00 | Live demo | A minimal REINFORCE training loop on a toy environment |

### Materials/Equipment
- Live-coding environment, PyTorch, `gymnasium` (or custom grid-world/bandit)
- Slide diagram: the policy-gradient derivation steps, written out in full

### Formative Check (in-class)
Students derive, on paper, $\nabla_\theta \mathbb{E}_{\pi_\theta}[R]$ starting from
$\mathbb{E}_{\pi_\theta}[R] = \sum_\tau P_\theta(\tau) R(\tau)$, arriving at the REINFORCE
estimator, with the instructor filling gaps as needed.

### Link to Lab/Assessment
Lab 11: Implementing REINFORCE with a return baseline on a toy environment and comparing
variance with/without the baseline (see `lab-manuals/lab-11.md`).
