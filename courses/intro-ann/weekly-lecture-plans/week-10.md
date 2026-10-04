# Week 10 Lecture Plan — Introduction to Artificial Neural Networks
## Topic: Optimizers — Momentum, RMSProp, Adam

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain the limitations of plain SGD that motivate momentum and adaptive learning rates.
   (*Understand*)
2. Apply the momentum, RMSProp, and Adam update rules, including Adam's bias correction.
   (*Apply*)
3. Analyze and compare convergence behavior across optimizers on the same problem. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap & motivation | Plain SGD's problems: slow in shallow directions, oscillates in steep ones |
| 0:15–0:40 | Momentum | Velocity term; physical-ball-rolling-downhill intuition |
| 0:40–0:50 | Break | — |
| 0:50–1:20 | RMSProp | Per-parameter adaptive learning rate via a running average of squared gradients |
| 1:20–1:50 | Adam | Combining momentum + RMSProp; bias-corrected moment estimates, derived |
| 1:50–2:00 | Learning rate schedules (brief) | Step decay, cosine decay — why decreasing the rate over training often helps |

### Materials/Equipment
- Live-coding environment, NumPy, Matplotlib
- Plot: convergence paths of SGD vs. Momentum vs. Adam on an elongated 2D quadratic bowl

### Formative Check (in-class)
Given a gradient sequence for 3 steps, compute Adam's $m_t, v_t, \hat m_t, \hat v_t$, and the
resulting parameter update by hand, and compare to plain SGD's update on the same sequence.

### Link to Lab/Assessment
Lab 10: Implement momentum, RMSProp, and Adam from scratch on the Week 8 network; compare
convergence speed/stability.
