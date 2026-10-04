# Week 4 Lecture Plan — Advanced Artificial Intelligence (Post Graduate)
## Topic: Contextual Bandits and the Bridge to Reinforcement Learning

**Duration:** 2 hours lecture + 3 hour research seminar/lab

### Learning Objectives (Bloom's Level)
1. Define the contextual-bandit protocol and explain how it generalizes the context-free bandit.
   (*Understand*)
2. Implement a contextual-bandit algorithm and compare its regret to a context-free baseline.
   (*Apply*)
3. Precisely locate tabular Q-learning on the bandit-to-RL spectrum, in terms of state
   persistence and delayed reward. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap + motivation | Why side information (context) should change which arm is best |
| 0:15–0:50 | Contextual bandits | Protocol + a simplified linear contextual-bandit algorithm (LinUCB-style) |
| 0:50–1:25 | The bandit-to-RL spectrum | Board diagram: context-free bandit → contextual bandit → full MDP; what changes at each step |
| 1:25–1:50 | Q-learning revisited | Recap (not re-derivation) of graduate-course tabular Q-learning, placed explicitly on this spectrum |
| 1:50–2:00 | Synthesis | Recap table: what persists across rounds, and what each setting's regret/convergence guarantee requires |

### Materials/Equipment
- Slides: "Contextual Bandits & the Road to Full RL"
- Whiteboard for the spectrum diagram
- Live-coding environment (Jupyter)

### Formative Check (in-class)
Explain, in two or three sentences, why a contextual bandit's optimal "policy" can be learned
without any notion of a transition model, while an MDP's cannot.

### Link to Lab/Assessment
Lab 4: implement a contextual bandit and compare it to a context-free baseline (see
`lab-manuals/lab-04.md`). **Assignment 1 assigned** (Weeks 2–4).
