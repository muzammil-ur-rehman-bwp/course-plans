# Week 14 Lecture Plan — Artificial Intelligence (Graduate)
## Topic: Current Research Topics Survey

**Duration:** 2 hours lecture + 3 hour lab/seminar

### Learning Objectives (Bloom's Level)
1. Explain what makes multi-agent reinforcement learning harder than single-agent RL.
   (*Understand*)
2. Implement a toy model-agnostic explanation method and describe XAI's motivating problem.
   (*Apply, Understand*)
3. Give a concrete example of reward misspecification/hacking and discuss why alignment is an
   active research area. (*Evaluate*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:30 | Multi-agent RL | Non-stationarity from each agent's perspective; connection to Weeks 3–4 game theory |
| 0:30–1:00 | Explainable AI (XAI) | Why black-box decisions are a problem; model-agnostic explanation ideas (brief) |
| 1:00–1:30 | AI safety/alignment | Reward specification/hacking; why "almost right" reward functions can misfire |
| 1:30–1:55 | Discussion | Open discussion: how this course's foundations (game theory, MDPs/RL, complexity) underlie each topic |
| 1:55–2:00 | Capstone reminder | Week 15 work-session logistics |

### Materials/Equipment
- Slides: "Current Trends: Multi-Agent RL, XAI, and AI Safety" (refreshed with current
  AAAI/IJCAI/NeurIPS-style readings each term)
- Live-coding environment (Jupyter)

### Formative Check (in-class)
Give one concrete scenario where a reward function that looks reasonable on paper could be
"gamed" by an RL agent in an unintended way.

### Link to Lab/Assessment
Lab 14: implement a toy multi-agent Q-learning setup or a feature-perturbation explanation demo
(see `lab-manuals/lab-14.md`).
