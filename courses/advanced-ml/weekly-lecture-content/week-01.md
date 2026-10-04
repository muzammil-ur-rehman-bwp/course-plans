# Week 1 — Lecture Content: Postgraduate Overview — The Statistical-Learning-Theory Research Frontier

## 1. What This Course Assumes, Stated Explicitly
This course assumes the graduate *Machine Learning* course (or equivalent) as settled background,
specifically: the ERM framework and PAC learnability; VC dimension and the Sauer–Shelah lemma;
Rademacher complexity; the concentration-inequality toolkit (Markov, Chebyshev, Hoeffding,
McDiarmid); convex optimization (gradient-descent convergence rates, Lagrangian duality, KKT
conditions); kernel methods and RKHS theory (the representer theorem); rigorous ensemble theory
(AdaBoost's training-error bound, margin theory, bagging's variance-reduction argument); Bayesian
machine learning (Bayesian linear regression, Gaussian Processes); Conditional Random Fields and
the discriminative/generative distinction; rigorous dimensionality-reduction theory (PCA
optimality, kernel PCA); a **conceptual-only** causal-inference introduction (correlation vs.
causation, confounding, Simpson's paradox, do-notation introduced but not built on); and
model-selection theory (the bias-variance decomposition, AIC/BIC, cross-validation). None of this
is re-taught. Where it is relevant, it is cited as background in one sentence.

## 2. The Five Pillars of This Course
| Pillar | Weeks | What It Covers | Builds On |
|---|---|---|---|
| Minimax theory & high-dimensional statistics | 2–4 | Minimax risk, Fano's inequality, sub-Gaussian/sub-exponential concentration, Bernstein's inequality, random matrix theory (Marchenko–Pastur) | Graduate course's Hoeffding/McDiarmid toolkit, now generalized far beyond bounded variables |
| Full-information online convex optimization | 5 | The OCO protocol, FTRL, online gradient descent's regret bound | Graduate course's convex optimization/KKT; explicitly **not** the sibling AI course's bandit/partial-information regret theory |
| Nonparametric Bayesian methods | 6–7 | The Dirichlet process, stick-breaking, the Chinese Restaurant Process, infinite mixtures | Graduate course's Bayesian linear regression/GP machinery, now over an infinite-dimensional parameter |
| Causal inference in depth | 8–9 | Potential outcomes, propensity scores, instrumental variables, the do-calculus | Graduate course's conceptual-only confounding/do-notation introduction, now made rigorous and operational |
| Trustworthy & robust statistical learning | 10–12 | Distribution shift/domain adaptation, algorithmic-fairness impossibility results, robust statistics | Graduate course's generalization theory and model-selection theory, now under adversarial/shifted/contaminated conditions |

This course is one of five sibling postgraduate courses. Each owns a disjoint slice:

| Course | Owns |
|---|---|
| **Advanced Machine Learning (this course)** | Minimax/high-dimensional statistics, full-information OCO, nonparametric Bayes, causal inference in depth, trustworthy/robust ML |
| Advanced Artificial Intelligence | Bandit/partial-information regret theory, multi-agent RL, algorithmic game theory/mechanism design, AI safety/alignment, interpretability, foundational debates |
| Advanced Artificial Neural Network | Neural architectures and training at research depth |
| Advanced Deep Learning | Deep architectures at research depth |
| Advanced Knowledge Representation and Reasoning | Deep KR formalisms at research depth |

**A precise boundary worth stating now:** both this course and *Advanced Artificial Intelligence*
contain a week titled, loosely, "online learning." They are genuinely different settings and do
not overlap. *Advanced Artificial Intelligence*'s Week 2 covers the **partial-information**
(expert/bandit-style) setting, where the learner only ever observes the realized loss of the
action it actually took. This course's Week 5 covers the **full-information** online convex
optimization setting, where the learner observes the entire convex loss *function* each round —
it could, if it wanted, evaluate that function (or its gradient) at any point, not only at the
point it played. Full information is strictly more information per round, and the two settings
use genuinely different algorithmic machinery (multiplicative-weights-style potential functions
for partial information vs. first-order convex-optimization machinery — FTRL and online gradient
descent — for full information). Where the two settings connect (e.g., both ultimately bound
regret via a similar telescoping/potential argument) is worth noticing, but this course will not
re-derive the sibling course's bandit results, and the sibling course will not re-derive this
course's FTRL/OGD results.

## 3. A Diagnostic Self-Check Against the Assumed Foundations
Before Week 2, each student should be able to, without looking anything up: (a) state the
finite-hypothesis-class PAC generalization bound and sketch its Hoeffding-plus-union-bound
derivation; (b) state the VC generalization bound and compute the VC dimension of linear
classifiers in $\mathbb{R}^d$; (c) state the KKT conditions for a convex constrained optimization
problem; (d) state the representer theorem; (e) derive the bias-variance decomposition for squared
error; (f) explain confounding and Simpson's paradox with a concrete numeric example. If any of
these feel shaky, review the corresponding graduate-course week before Week 2 — this course will
not pause to re-teach them.

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
For each of the following research questions, (a) identify which pillar of this course it belongs
to, and (b) name one graduate-ML topic it assumes as background without re-deriving it:
1. "Can the Gaussian-location minimax lower bound be tightened under a sparsity constraint on the
   mean vector?" (Answer: (a) Pillar 1; (b) nothing beyond basic concentration — this is new
   material building on Week 2's Fano argument.)
2. "Does an FTRL variant with a different regularizer achieve better regret when the loss
   sequence is piecewise constant?" (Answer: (a) Pillar 2; (b) convex optimization/KKT, assumed
   from the graduate course, as the machinery FTRL builds on.)
3. "Can a modified do-calculus rule handle a causal query with selection bias alongside
   confounding?" (Answer: (a) Pillar 4; (b) the conceptual do-notation introduction from the
   graduate course, now being pushed past its conceptual-only treatment.)
