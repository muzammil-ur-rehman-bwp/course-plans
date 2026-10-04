# Week 3 Lecture Plan — Advanced Artificial Intelligence (Post Graduate)
## Topic: Multi-Armed Bandits

**Duration:** 2 hours lecture + 3 hour research seminar/lab

### Learning Objectives (Bloom's Level)
1. Formalize the exploration-exploitation tradeoff as the multi-armed bandit problem. (*Understand*)
2. Derive UCB1's confidence radius from the Hoeffding bound, and derive its regret bound.
   (*Apply, Analyze*)
3. Explain Thompson sampling's Bayesian mechanism conceptually. (*Understand*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap + motivation | From regret against fixed experts (Week 2) to regret against the single best arm |
| 0:15–0:40 | Hoeffding bound | Board-worked statement and its role as a confidence-interval tool |
| 0:40–1:15 | UCB1 | Algorithm + confidence-radius derivation + regret-bound derivation (arm-pull-count argument) |
| 1:15–1:40 | Thompson sampling | Conceptual walkthrough: posterior sampling vs. confidence bounds |
| 1:40–2:00 | Synthesis | Comparative discussion: ε-greedy vs. UCB1 vs. Thompson sampling |

### Materials/Equipment
- Slides: "Multi-Armed Bandits: UCB and Thompson Sampling"
- Whiteboard for the Hoeffding-bound-to-confidence-radius derivation
- Live-coding environment (Jupyter)

### Formative Check (in-class)
For a suboptimal arm with gap Δ=0.1 after T=10,000 rounds, estimate the UCB1 expected-pull-count
bound (8 ln T)/Δ² and the resulting contribution to regret.

### Link to Lab/Assessment
Lab 3: implement UCB1 and compare cumulative regret against ε-greedy (see `lab-manuals/lab-03.md`).
