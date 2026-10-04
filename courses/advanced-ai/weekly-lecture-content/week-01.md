# Week 1 — Lecture Content: Postgraduate Overview — The AI Research Frontier

## 1. What This Course Assumes, Stated Explicitly
This course assumes the graduate *Artificial Intelligence* course (or equivalent) as settled
background, specifically: advanced search (IDA*, bidirectional search, SMA*, with their
time/space complexity); game theory and multi-agent systems fundamentals (minimax/alpha-beta
correctness, normal-form games, the definition of Nash equilibrium, basic mechanism-design
intuition); automated reasoning (DPLL for SAT, SMT conceptually); rigorous classical planning
(PSPACE-completeness, planning-graph heuristics, HTN decomposition); the MDP/POMDP/RL formalism
(Bellman equation, value iteration, policy iteration, tabular Q-learning, belief states); the
computational complexity of AI problems (NP-completeness, PSPACE-completeness); and basic
research methods (reading and critiquing a paper, reproducibility, experimental design). None of
this is re-taught. Where it is relevant, it is cited as background in one sentence (for example,
Week 4 recalls tabular Q-learning only to place it on a spectrum, not to re-derive it).

## 2. The Four Pillars of This Course
| Pillar | Weeks | What It Covers | Builds On |
|---|---|---|---|
| Regret theory & bandits | 2–4 | Online learning, regret, multiplicative weights, UCB, Thompson sampling, contextual bandits | Graduate course's MDP/Q-learning, as the simpler setting these generalize from below |
| Multi-agent systems & algorithmic game theory | 5–7 | Multi-agent RL, Nash-equilibrium complexity (PPAD), correlated equilibria, VCG mechanism design | Graduate course's minimax/Nash-equilibrium *definitions*, now pushed into *computation* and *design* |
| AI safety, alignment & interpretability | 8–10 | Specification gaming, outer/inner alignment, reward modeling, scalable oversight, feature attribution, mechanistic interpretability | Graduate course's one-paragraph introductory mention of AI safety (Week 14 there); here it is a full technical unit |
| Foundational debates & research methods | 11–16 | Symbol grounding, the frame problem, the Chinese Room, postgraduate proposal methodology, frontier survey, capstone | New material, connected throughout to Pillars 1–3's architectures and failure modes |

This course is one of five sibling postgraduate AI courses. Each owns a disjoint slice:

| Course | Owns |
|---|---|
| **Advanced Artificial Intelligence (this course)** | Regret/bandit theory, multi-agent RL, algorithmic game theory/mechanism design, AI safety/alignment, interpretability, foundational debates |
| Advanced Artificial Neural Network | Neural architectures and training at research depth |
| Advanced Machine Learning | Statistical ML theory at research depth |
| Advanced Deep Learning | Deep architectures at research depth |
| Advanced Knowledge Representation and Reasoning | Deep KR formalisms at research depth |

## 3. A Diagnostic Self-Check Against the Assumed Foundations
Before Week 2, each student should be able to, without looking anything up: (a) state the formal
definition of a Nash equilibrium of a normal-form game; (b) write the Bellman optimality equation
for a finite MDP and explain why value iteration converges; (c) state what it means for a problem
to be NP-complete and what it means for a problem to be PSPACE-complete; (d) implement tabular
Q-learning's update rule from memory. If any of these feel shaky, review the corresponding
graduate-course week before Week 2 — this course will not pause to re-teach them.

## 4. How to Scope a Research Proposal (Introduced Now, Developed in Week 13)
Every week from here on is implicitly building toward the capstone, so the four components of a
research proposal are introduced now, at a high level, and developed fully in Week 13:
1. **Problem statement** — a precise, falsifiable question or gap, not a restatement of a topic.
2. **Related-work survey** — 5+ papers, accurately represented, related to each other, used to
   motivate the stated gap.
3. **Proposed novel approach** — the student's own formulation, clearly distinguishable from
   simply restating one surveyed paper.
4. **Feasibility argument or preliminary result** — either a small pilot showing the approach is
   workable, or a rigorous argument for why it should work and what the main risk is.
Carrying a tentative research interest from Week 1 onward (even a rough one) makes every
subsequent week more useful: as each pillar is covered, ask "does this suggest a gap or an
approach relevant to my interest?"

## 5. In-Class Exercise
For each of the following research questions, (a) identify which pillar of this course it
belongs to, and (b) name one graduate-AI topic it assumes as background without re-deriving it:
1. "Can a regret bound for multiplicative weights be improved when the losses are sparse?"
   (Answer: (a) Pillar 1; (b) nothing beyond basic probability — this is new material.)
2. "Why might two independently trained RL agents fail to settle into a stable joint policy?"
   (Answer: (a) Pillar 2; (b) tabular Q-learning's single-agent convergence guarantee, assumed
   from the graduate course, now shown to break down.)
3. "Is a transformer's attention mechanism a satisfying answer to the symbol grounding problem?"
   (Answer: (a) Pillar 4, with a one-sentence pointer to Advanced Deep Learning for the mechanism
   itself — this course discusses only the philosophical question, not attention's architecture.)
