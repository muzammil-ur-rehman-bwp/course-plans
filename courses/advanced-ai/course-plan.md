# Course Plan: Advanced Artificial Intelligence (Post Graduate)

## 1. Course Information

| Field | Detail |
|---|---|
| Course Title | Advanced Artificial Intelligence |
| Level | Post Graduate (PhD-track: PhD coursework, or an MS student heading toward a thesis) |
| Credit Hours | 3 (2 hrs lecture + 1 research seminar/lab session of 3 hrs/week) |
| Prerequisites | *Artificial Intelligence* (Graduate), or equivalent: advanced search (IDA*, bidirectional, SMA*), game theory and multi-agent systems fundamentals, automated reasoning/SAT (DPLL), rigorous classical planning, MDPs/POMDPs and RL fundamentals (Bellman equation, value/policy iteration, Q-learning), computational complexity of AI problems (NP-completeness, PSPACE-completeness), and basic research methods (reading/critiquing a paper). This course assumes every one of those topics as firm foundation and does **not** re-teach any of them. |
| Programming Language | Python 3.x (every algorithm in this course is implemented and empirically probed in Python; this course also expects students to read others' code, since several weeks point at open-source reference implementations and current papers' released code) |
| Core Libraries | Python standard library (`random`, `itertools`, `dataclasses`, `math`); `numpy` for vectorized bandit/game-theory simulations; a linear-programming routine (e.g., `scipy.optimize.linprog`) is discussed conceptually for correlated-equilibrium and zero-sum-game computation but no graded component strictly requires `scipy` to be installed |
| Duration | 16 teaching weeks (1 semester) + 1 exam week |
| Delivery Mode | Lecture + research seminar/lab (concept lecture followed by an implementation, critical-writing, or proposal-development session, depending on the week) |

## 2. Course Description

This is a PhD-track, research-frontier treatment of Artificial Intelligence that assumes graduate
AI (advanced search, game theory, automated reasoning, rigorous planning, MDPs/POMDPs, RL
fundamentals, computational complexity, and basic research methods) as settled background and
spends every week building past it into territory that graduate AI explicitly does not cover.
The course has four pillars. **(1) Regret and sequential decision theory under uncertainty**:
the online-learning framework, regret as a performance measure, the multiplicative-weights
algorithm and its regret bound, multi-armed and contextual bandits, the UCB algorithm and its
regret bound, and how this bandit theory sits on the spectrum between the graduate course's
single-agent tabular Q-learning and full reinforcement learning. **(2) Multi-agent AI at
research depth**: multi-agent reinforcement learning (independent vs. joint-action learners,
non-stationarity, self-play) and algorithmic game theory in depth — the computational complexity
of computing a Nash equilibrium (PPAD-completeness), correlated equilibria, and mechanism design
built up to the Vickrey-Clarke-Groves (VCG) mechanism and its truthfulness guarantee, with real
applications (ad auctions, resource allocation). **(3) AI safety, alignment, and interpretability
as serious technical research areas**, not an ethics afterthought: specification gaming and
reward hacking as a technical phenomenon, the alignment problem formalized, outer vs. inner
alignment, current technical alignment-research directions (reward modeling, scalable oversight),
and interpretability research (feature attribution, mechanistic interpretability, and the gap
between post-hoc explanation and genuine understanding). **(4) Foundational and philosophical
debates treated as live research questions**: the symbol grounding problem, the frame problem,
and the Chinese Room argument and its rebuttals, examined with the same rigor as the rest of the
course and connected explicitly to the architectures and failure modes studied in pillars 1–3,
not presented as settled historical trivia.

The capstone reflects the shift in what postgraduate work is for. Where the graduate course's
capstone is a literature review plus a small reproduced or extended experiment, this course's
capstone is a **research proposal** — a PhD-qualifying-exam-style deliverable: a problem
statement, a related-work survey of five or more papers, a proposed novel approach or extension,
and either preliminary results or a rigorous feasibility argument for why the approach should
work. It is evaluated as a thesis proposal would be, not as a course project.

**This course does not repeat the graduate AI course.** Advanced search, minimax/alpha-beta,
SAT/SMT, classical-planning complexity, the MDP/Bellman/value-iteration/policy-iteration/
Q-learning formalism, and POMDP belief states are assumed fluent and are referenced only as
background when relevant (e.g., "recall tabular Q-learning from the graduate course" when
introducing contextual bandits). It also does not duplicate the sibling postgraduate courses:
neural architectures and training (*Advanced Artificial Neural Network*), statistical ML theory
(*Advanced Machine Learning*), deep architectures (*Advanced Deep Learning*), or deep KR
formalisms (*Advanced Knowledge Representation and Reasoning*) — where one of those topics is
relevant to an argument here, it receives at most a one-sentence pointer, never a lecture.

## 3. Goals

- Master the online-learning framework and regret as a performance measure; derive the
  multiplicative-weights (weighted-majority) algorithm's regret bound.
- Formalize the exploration-exploitation tradeoff for multi-armed bandits; derive the UCB
  algorithm's regret bound from a Hoeffding concentration argument, and explain Thompson sampling
  conceptually.
- Situate contextual bandits as a bridge between bandit theory and full reinforcement learning,
  and locate the graduate course's tabular Q-learning precisely on that spectrum.
- Analyze multi-agent reinforcement learning: independent vs. joint-action learners, the
  non-stationarity each agent's partner introduces, and self-play as a training paradigm.
- Reason about the computational complexity of computing a Nash equilibrium (PPAD-completeness)
  and about correlated equilibria, and construct the VCG mechanism and prove (via the standard
  dominant-strategy argument) that it is truthful.
- Treat AI safety and alignment as a technical research area: formalize specification gaming and
  reward hacking, define outer vs. inner alignment, and survey current alignment-research
  directions (reward modeling, scalable oversight, interpretability as a safety tool).
- Evaluate interpretability methods — feature attribution and mechanistic interpretability — and
  critically distinguish post-hoc explanation from genuine mechanistic understanding.
- Critically engage, with technical grounding, the symbol grounding problem, the frame problem,
  and the Chinese Room argument, as live research questions connected to modern AI systems.
- Scope, write, and defend an original PhD-qualifying-style research proposal: a problem
  statement, a 5+ paper related-work survey, a proposed novel approach, and a feasibility
  argument or preliminary result.

## 4. Course Learning Outcomes (CLOs) — Mapped to Bloom's Taxonomy

| CLO | Statement | Bloom's Level(s) |
|---|---|---|
| CLO1 | Recall the graduate-AI formalism this course assumes, and map the postgraduate research-frontier landscape (regret theory, multi-agent systems/game theory, AI safety/alignment, interpretability, foundational debates) this course covers. | Remember, Understand |
| CLO2 | Derive the multiplicative-weights algorithm's regret bound and the UCB algorithm's regret bound from first principles (potential-function and Hoeffding-concentration arguments respectively). | Apply, Analyze |
| CLO3 | Position contextual bandits and multi-agent reinforcement learning relative to the graduate course's MDP/Q-learning formalism, and implement independent and joint-action learners, diagnosing the non-stationarity multi-agent learning introduces. | Apply, Analyze |
| CLO4 | Analyze the computational complexity of Nash-equilibrium computation (PPAD-completeness) and correlated equilibria, and construct and prove the truthfulness of the VCG mechanism for a concrete allocation problem. | Apply, Analyze, Evaluate |
| CLO5 | Formalize specification gaming, reward hacking, and the outer/inner alignment distinction as technical research questions, and critically evaluate current alignment-research directions (reward modeling, scalable oversight, interpretability-as-safety). | Analyze, Evaluate |
| CLO6 | Critically evaluate interpretability methods (feature attribution, mechanistic interpretability) against the standard of genuine mechanistic understanding versus post-hoc explanation. | Analyze, Evaluate |
| CLO7 | Critically evaluate the symbol grounding problem, the frame problem, and the Chinese Room argument (and its standard rebuttals) as live, technically grounded research questions relevant to modern AI architectures. | Evaluate |
| CLO8 | Formulate an original research question, survey and synthesize 5+ related papers, and construct a rigorous feasibility argument or preliminary result for a proposed novel approach. | Analyze, Evaluate, Create |
| CLO9 | Design, write, and orally defend a PhD-qualifying-exam-style research proposal under questioning, in the manner of a thesis-proposal committee. | Evaluate, Create |

### Bloom's Taxonomy progression across the semester

| Phase | Weeks | Dominant Bloom's Levels | Focus |
|---|---|---|---|
| Postgraduate Orientation | 1 | Remember, Understand | Research-frontier landscape; assumed-foundations review; introducing proposal scoping |
| Regret Theory & Bandits | 2–4 | Apply, Analyze | Online learning/regret; multiplicative weights; UCB; contextual bandits vs. tabular RL |
| Multi-Agent Systems & Game Theory | 5–7 | Apply, Analyze, Evaluate | Multi-agent RL; Nash-equilibrium complexity (PPAD); correlated equilibria; VCG mechanism design |
| AI Safety, Alignment & Interpretability | 8–10 | Analyze, Evaluate | Specification gaming; outer/inner alignment; alignment research directions; interpretability |
| Foundational Debates & Research Methods | 11–13 | Evaluate | Symbol grounding; frame problem; Chinese Room; postgraduate proposal-scoping methodology |
| Frontier Survey & Capstone | 14–16 | Evaluate, Create | Current frontier topics; proposal drafting and peer feedback; proposal presentations |

Note the deliberate skew relative to the graduate course: **Evaluate** and **Create** dominate
from Week 5 onward, and the capstone (CLO8–CLO9, both Evaluate/Create) is weighted accordingly —
postgraduate work is judged chiefly on the ability to formulate and scope original research, not
on recall or routine application.

## 5. Weekly Topic Overview (16 Weeks)

| Week | Topic | Bloom's Focus |
|---|---|---|
| 1 | Postgraduate overview: the AI research-frontier landscape; rapid review of assumed graduate foundations; how to scope a research proposal (introduced early) | Remember, Understand |
| 2 | Online learning and regret minimization: the online-learning framework; the multiplicative-weights (weighted-majority) algorithm and its regret bound, derived | Apply, Analyze |
| 3 | Multi-armed bandits: exploration-exploitation formalized; the UCB algorithm and its regret bound, derived from Hoeffding concentration; Thompson sampling (conceptual) | Apply, Analyze |
| 4 | Contextual bandits and the bridge to RL: contextual bandits as an interpolation between bandits and full MDPs; where tabular Q-learning sits on this spectrum | Apply, Analyze |
| 5 | Multi-agent reinforcement learning: independent vs. joint-action learners; non-stationarity; self-play | Apply, Analyze |
| 6 | Algorithmic game theory I: computing Nash equilibria; PPAD-completeness (conceptual); correlated equilibria (brief) | Analyze |
| 7 | Algorithmic game theory II: mechanism design in depth; the VCG mechanism and its truthfulness; applications (ad auctions, resource allocation) | Apply, Analyze, Evaluate |
| 8 | AI safety and alignment I: specification gaming and reward hacking; the alignment problem formalized; outer vs. inner alignment; midterm review | Analyze, Evaluate |
| 9 | **Midterm Exam** + AI safety and alignment II: current alignment-research directions (reward modeling, scalable oversight, interpretability-as-safety) | Analyze, Evaluate |
| 10 | Interpretability research: feature attribution; mechanistic interpretability; post-hoc explanation vs. genuine understanding | Analyze, Evaluate |
| 11 | Foundational and philosophical debates I: the symbol grounding problem; the frame problem, as live research questions | Evaluate |
| 12 | Foundational and philosophical debates II: the Chinese Room argument and its rebuttals; what "understanding" might mean computationally | Evaluate |
| 13 | Research methods at the postgraduate level: scoping a research proposal (problem statement, related-work survey standards, feasibility argument) | Analyze, Evaluate |
| 14 | Current frontier topics survey (grounded, explicitly flagged as fast-moving) | Understand, Evaluate |
| 15 | Research proposal work session: drafting, refining, peer feedback | Apply, Create |
| 16 | Capstone research proposal presentations + course review | Evaluate, Create |
| 17 | Final Exam Week | — |

## 6. Assessment Plan

| Component | Weight | Notes |
|---|---|---|
| Lab Work (weekly) | 10% | Graded notebooks/exercises, Labs 1–15 |
| Assignments (2 problem sets) | 10% | Tied to Weeks 4 and 8 |
| Quizzes (6, best 5 counted) | 10% | Short, in-class/online, 15 min each |
| Paper Critique & Presentation | 10% | Week 13 research-skills assignment; written critique + in-class presentation of a current AI paper |
| Midterm Exam | 10% | Week 9, qualifying-exam style, covers Weeks 1–8 |
| Research Proposal Capstone | 40% | Problem statement, 5+ paper related-work survey, proposed novel approach, feasibility argument/preliminary results, written proposal, and oral defense |
| Final Exam | 10% | Comprehensive, emphasis on Weeks 9–14 |

**Rationale for the postgraduate weighting.** The weight shifts decisively further toward
independent research than even the graduate course does. Exams (Midterm + Final) total only 20%
(vs. 25% at the graduate level and 30% at the undergraduate level), and weekly Labs drop to 10%
(vs. 15% at the graduate level) because postgraduate lab sessions are shorter, more exploratory,
and increasingly folded into proposal-development work by the back half of the semester. The
Research Proposal Capstone, at 40% (vs. 25% at the graduate level), is by far the largest single
component and is deliberately judged like a thesis-proposal defense: postgraduate work should be
judged mostly on the ability to formulate and scope original research — a sound problem
statement, a genuine survey of the related work, a defensible proposed approach, and an honest
feasibility argument — not on exam recall or on completing a fixed, instructor-specified
assignment. The components total exactly 100% (10 + 10 + 10 + 10 + 10 + 40 + 10 = 100).

## 7. Grading Policy

Standard letter grading per institutional policy (e.g., A ≥ 85, B ≥ 70, C ≥ 55, D ≥ 40, F < 40;
adjust to institution). Late submissions: −10% per day up to 3 days, then not accepted unless
documented emergency. Capstone milestones (problem-statement check-in Week 8, draft proposal Week
13, final proposal Week 15, defense Week 16) have fixed deadlines because of the downstream
defense schedule; late capstone milestones are handled case-by-case with the instructor.

## 8. Tools & Software

- Python 3.10+ (standard library plus `numpy` for vectorized bandit and game-theory simulations)
- Jupyter Notebook / Google Colab, or a plain text editor and terminal
- A linear-programming routine (e.g., `scipy.optimize.linprog`) is discussed conceptually for
  correlated-equilibrium computation and for solving zero-sum games via linear programming; it is
  **not** required to be installed, and all graded code in this course that needs it is either
  a from-scratch Python implementation or ships with a pure-Python fallback
- Git/GitHub for lab, assignment, and capstone submission

## 9. Reference Textbooks and Reading

- Sutton, R. & Barto, A. — *Reinforcement Learning: An Introduction* (free online; the canonical
  reference for the online-learning/bandit material in Weeks 2–4, extending the graduate course's
  use of this same text for the MDP/Q-learning chapters).
- Shoham, Y. & Leyton-Brown, K. — *Multiagent Systems: Algorithmic, Game-Theoretic, and Logical
  Foundations* (free online; the canonical textbook for the algorithmic-game-theory and
  multi-agent-systems material in Weeks 5–7 — Nash-equilibrium computation, correlated
  equilibria, and mechanism design).
- **Current papers from NeurIPS, ICML, AAAI, and IJCAI are the primary reading material from
  Week 8 onward** (AI safety/alignment, interpretability, and the frontier-topics survey). This
  is a fast-moving research area; the instructor selects and refreshes the specific paper list
  each offering rather than this syllabus naming a fixed set that would quickly date. Students
  are expected to locate, read, and critically evaluate current primary sources as a core
  postgraduate skill, not merely consume a fixed reading list.
- Official Python and NumPy documentation (for lab support only).

## 10. Academic Integrity

Labs and assignments are individual unless stated otherwise. The research proposal capstone is
individual work, reflecting its role as a qualifying-exam-style assessment of each student's own
ability to formulate research; any collaboration on the capstone (e.g., discussing a problem
statement with a peer) must be disclosed in the proposal's acknowledgments. Postgraduate work is
held to an **original-contribution standard**: a related-work survey must accurately represent
what each cited paper actually claims and found, a proposed approach must be the student's own
formulation (extending or combining existing ideas in a way the student can clearly explain and
defend, not a restatement of a single paper's contribution as if it were novel), and any
feasibility argument or preliminary result must be the student's own reasoning or own
experimentation. Uncredited reuse of another author's ideas, text, code, or results — including
uncredited AI-generated text or code submitted as original work, or presenting a paper's reported
results as the student's own preliminary findings — is handled per institutional academic
integrity policy and, for the capstone specifically, is treated with the same seriousness as
plagiarism in a thesis proposal.
