# Week 5 Lecture Plan — Advanced Artificial Intelligence (Post Graduate)
## Topic: Multi-Agent Reinforcement Learning

**Duration:** 2 hours lecture + 3 hour research seminar/lab

### Learning Objectives (Bloom's Level)
1. Distinguish independent learners from joint-action learners in multi-agent RL. (*Understand*)
2. Explain why multi-agent learning breaks the stationarity assumption single-agent RL
   convergence relies on. (*Analyze*)
3. Implement independent Q-learners in a repeated matrix game and diagnose non-convergent
   behavior. (*Apply, Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap + motivation | From one learning agent (graduate course) to several, simultaneously |
| 0:15–0:45 | Independent vs. joint-action learners | Definitions, worked contrast |
| 0:45–1:20 | Non-stationarity | Why "the environment" is moving under each agent's feet; why Q-learning's convergence proof no longer applies |
| 1:20–1:45 | Self-play | Generic mechanism: training against an improving pool of past/current selves as an automatic curriculum |
| 1:45–2:00 | Synthesis | Recap table: single-agent RL vs. multi-agent RL assumptions |

### Materials/Equipment
- Slides: "Multi-Agent Reinforcement Learning"
- Whiteboard for the non-stationarity argument
- Live-coding environment (Jupyter)

### Formative Check (in-class)
Explain why two independent Q-learners playing a repeated general-sum game can fail to converge
to any fixed joint policy, even though each one's own single-agent Q-learning update is correct.

### Link to Lab/Assessment
Lab 5: implement independent Q-learners for a repeated matrix game (see `lab-manuals/lab-05.md`).
