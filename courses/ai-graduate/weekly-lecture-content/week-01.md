# Week 1 — Lecture Content: Graduate AI Overview

## 1. Why This Course, and Why Now
This course assumes you have already taken a broad undergraduate AI survey — agents, basic
search, basic logic, basic planning, basic uncertainty. That survey necessarily moved fast across
many topics. This course does the opposite: it takes a disjoint, carefully bounded subset of AI —
advanced search, game theory and multi-agent systems, automated reasoning (SAT/SMT), rigorous
planning, Markov Decision Processes and reinforcement-learning fundamentals, POMDPs, the
computational complexity of AI problems, and research methods — and treats each at graduate
depth: formal correctness arguments, real complexity results, and research-level expectations.

## 2. The AI-Family Course Map
This course is one of several graduate AI courses. Each owns a disjoint slice of the field:

| Course | Owns |
|---|---|
| **Artificial Intelligence (this course)** | Advanced search, game theory/multi-agent systems, automated reasoning (SAT/SMT), rigorous planning, MDPs/RL fundamentals, POMDPs, AI complexity, research methods |
| Artificial Neural Network (graduate) | Neural network architectures and training in depth |
| Machine Learning (graduate) | Classical statistical ML (SVMs, ensembles, etc.) |
| Deep Learning (graduate) | Deep architectures (CNNs, RNNs, Transformers) |
| Knowledge Representation and Reasoning (graduate) | Deep KR formalisms (description logics, frames, semantic networks) |

When a topic from one of those courses comes up here — for instance, noting that a value function
in an MDP *could* be approximated with a neural network — this course gives it at most one
sentence and moves on; the depth lives in the owning course.

## 3. The Formal Problem-Solving Framework, Restated at Graduate Rigor
A **search problem** is formally a tuple ⟨S, s₀, A, T, G, c⟩ where:
- S is the state space,
- s₀ ∈ S is the initial state,
- A(s) is the set of actions applicable in state s,
- T(s, a) → s' is the transition model,
- G ⊆ S is the set of goal states (or a goal test function),
- c(s, a, s') ≥ 0 is the step cost.

A **solution** is a sequence of actions a₁,...,aₙ taking s₀ to some s_n ∈ G; an **optimal
solution** minimizes total path cost Σᵢ c(sᵢ₋₁, aᵢ, sᵢ). Every algorithm in Weeks 2–7 is an
algorithm for searching this formal object (or a generalization of it — adversarial search adds
an opponent, planning adds a factored state representation, CSP adds a different goal-test
structure, and so on).

```python
from dataclasses import dataclass, field
from typing import Callable, Hashable

@dataclass
class SearchProblem:
    initial: Hashable
    actions: Callable[[Hashable], list]
    transition: Callable[[Hashable, object], Hashable]
    is_goal: Callable[[Hashable], bool]
    step_cost: Callable[[Hashable, object, Hashable], float] = field(
        default=lambda s, a, s2: 1.0
    )
```
This small `SearchProblem` abstraction is reused, in spirit, throughout Weeks 2–7: every advanced
search algorithm, planning algorithm, and CSP solver in this course is ultimately an algorithm
operating over some instantiation of this tuple.

## 4. What "Graduate Rigor" Means in This Course
Three concrete differences from an undergraduate treatment:
1. **Correctness arguments.** When we introduce alpha-beta pruning (Week 3) or DPLL (Week 6) or
   value iteration (Week 8), we do not just state the algorithm — we sketch *why* it is correct
   (e.g., an induction argument, a contraction-mapping argument).
2. **Complexity results are named and used.** "SAT is NP-complete" and "planning is
   PSPACE-complete" are real, standard results (Weeks 6, 7, 12) that we use to explain *why*
   certain problems need heuristics/approximation rather than treating this as folklore.
3. **Research methods are a graded skill.** Starting Week 13, reading and critiquing a paper, and
   designing a sound small experiment, are explicit learning outcomes — not an afterthought — and
   culminate in the Research Capstone (literature review + experiment + paper + presentation).

## 5. Course Roadmap
- **Weeks 2–4:** Advanced search, then game theory and multi-agent systems.
- **Weeks 5–7:** Rigorous CSP/metaheuristics, automated reasoning (SAT/SMT), rigorous planning.
- **Weeks 8–11:** MDPs, Q-learning, POMDPs, rigorous probabilistic graphical models.
- **Weeks 12–16:** AI complexity, research methods, current-trends survey, and the capstone.

## 6. In-Class Exercise
For each of the following research questions, (a) name the AI subfield(s) from this course's map
it belongs to, and (b) name one sibling graduate course it explicitly does NOT belong to:
1. "Can we prove an upper bound on how much memory IDA* needs relative to A*?"
2. "How do multiple reinforcement-learning agents behave when trained against each other?"
3. "How does a transformer's attention mechanism let it model long-range dependencies?"
   (Answer: (a) none in this course's map — this belongs to Deep Learning; (b) Deep Learning,
   graduate.)
