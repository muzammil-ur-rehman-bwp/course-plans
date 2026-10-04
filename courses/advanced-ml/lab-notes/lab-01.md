# Lab Notes 1 — Research Landscape Mapping & Environment Setup

**Concept recap:** this course has five pillars (minimax/high-dimensional statistics,
full-information OCO, nonparametric Bayes, causal inference in depth, trustworthy/robust ML), each
building on a specific, disjoint slice of the assumed graduate-ML toolkit.

**Common pitfalls:**
- Confusing this course's full-information online-learning pillar with the sibling *Advanced
  Artificial Intelligence* course's bandit/regret territory in Task B — if a prompt mentions
  "observing only the reward of the action taken," it belongs to the sibling course, not this
  one's Pillar 2.
- Writing an overly broad keyword list in Task D (e.g., including "learning" as a Pillar 2
  keyword) that causes `pillar_keywords` to match every prompt — keep keyword lists specific to
  each pillar's actual terminology (e.g., "Fano," "minimax," "sub-Gaussian" for Pillar 1; "FTRL,"
  "regret," "online convex" for Pillar 2).
- Treating Task A's self-assessment as a formality — students who honestly flag a shaky topic
  (e.g., KKT conditions) and review it before Week 2 consistently do better on Weeks 5 and 9's
  material, which leans directly on it.

**Debugging tip:** test `pillar_keywords` on a prompt you have manually classified first, and
confirm the function returns exactly the pillar you expect before testing the full Task B set.

**Instructor tip:** have students read their Task C research-interest paragraph aloud in pairs —
this is deliberately low-stakes and tentative, but normalizing early, rough articulation of a
research interest makes the Week 8 problem-statement check-in far less daunting.
