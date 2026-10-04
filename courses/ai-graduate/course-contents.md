# Course Contents: Artificial Intelligence (Graduate)

Detailed per-week breakdown of topics, subtopics, and resources. Companion to `course-plan.md`.
Each week lists: **Topics**, **Subtopics/Skills**, **Readings**, **Software/Libraries used**.

---

## Week 1 — Graduate AI Overview
- **Topics:** Map of AI research areas (search, logic/automated reasoning, planning, decision
  theory/RL, probabilistic reasoning, learning, multi-agent systems) and where this course sits
  relative to the sibling graduate courses (ANN, ML, DL, KR&R) that own neural networks,
  statistical ML, deep architectures, and deep KR formalisms respectively; a rigorous
  re-formalization of the agent/environment/problem framework at graduate depth (formal
  definitions of a search problem, PEAS revisited with complexity in mind); course expectations —
  reading and critiquing primary research papers from Week 13 onward, the research capstone.
- **Subtopics/Skills:** restating the formal definition of a search problem precisely (state
  space, initial state, actions, transition model, goal test, path-cost function); identifying
  which AI subfields a given research question belongs to.
- **Readings:** Russell & Norvig Ch. 1–3 (review at graduate pace); course syllabus.
- **Software:** Python 3.10+, Jupyter/Colab (environment setup only).

## Week 2 — Advanced Search
- **Topics:** Limitations of plain A* (memory growth); Iterative Deepening A* (IDA*) — repeated
  depth-first iterations bounded by an increasing f-cost threshold; bidirectional search —
  simultaneously searching forward from the start and backward from the goal; Simplified Memory-
  Bounded A* (SMA*) — A* that forgets its worst leaf nodes under a fixed memory budget; formal
  time/space complexity analysis of each algorithm relative to plain A* and to each other.
- **Subtopics/Skills:** implementing IDA* in Python; deriving and stating the space complexity of
  IDA* (O(depth) vs. A*'s O(b^d)); explaining under what conditions bidirectional search gives an
  exponential speedup; tracing SMA*'s node-forgetting behavior by hand on a small example.
- **Readings:** Russell & Norvig Ch. 3 (§3.5, memory-bounded search).
- **Software:** Python, `heapq`.

## Week 3 — Game Theory & Adversarial Search I
- **Topics:** Minimax and alpha-beta pruning revisited with a formal correctness argument (why
  alpha-beta provably returns the same value as minimax); a brief look at imperfect-information
  games (why minimax assumes perfect information and what breaks without it); introduction to
  game theory proper — normal-form (strategic-form) games, payoff matrices, dominant strategies,
  and the formal definition of a Nash equilibrium.
- **Subtopics/Skills:** writing and proving (by induction on tree depth) the correctness of
  alpha-beta pruning relative to minimax; computing pure-strategy Nash equilibria of small 2x2
  normal-form games by hand; implementing alpha-beta for a small adversarial game in Python.
- **Readings:** Russell & Norvig Ch. 5 (§5.1–5.4, 5.7 game theory basics).
- **Software:** Python.

## Week 4 — Game Theory II & Multi-Agent Systems
- **Topics:** Cooperative vs. competitive multi-agent settings; zero-sum vs. general-sum games;
  coordination problems in cooperative multi-agent systems (brief); mechanism design and auction
  theory as an AI application — designing rules so that self-interested agents' equilibrium
  behavior achieves a desired outcome (conceptual treatment: first-price vs. second-price/
  Vickrey auctions, truthfulness, brief).
- **Subtopics/Skills:** classifying a multi-agent scenario as cooperative/competitive and
  zero-sum/general-sum; explaining, conceptually, why a second-price (Vickrey) auction incentivizes
  truthful bidding; implementing a small simulation of competing agents bidding in a toy auction.
- **Readings:** Russell & Norvig Ch. 5 (§5.6–5.7), Ch. 18 (§18.6, mechanism design overview).
- **Software:** Python.
- **Assignment 1 assigned** (advanced search & game theory, Weeks 1–4).

## Week 5 — Rigorous CSP & Combinatorial Optimization
- **Topics:** Advanced constraint-satisfaction algorithms beyond basic backtracking — arc
  consistency (AC-3) as a preprocessing/pruning step, variable- and value-ordering heuristics
  (minimum remaining values, least constraining value), forward checking; metaheuristics for
  combinatorial optimization — simulated annealing (the Metropolis acceptance criterion, cooling
  schedules) and genetic algorithms (selection, crossover, mutation); a conceptual discussion of
  convergence (why simulated annealing converges to a global optimum in the limit of an
  infinitely slow cooling schedule, and why genetic algorithms give no such guarantee).
- **Subtopics/Skills:** implementing AC-3 and a heuristic-ordered backtracking solver for a CSP
  (e.g., graph coloring or Sudoku); implementing simulated annealing for a toy combinatorial
  optimization problem (e.g., the traveling salesperson problem on a small instance); discussing,
  at a conceptual level, the exploration/exploitation tradeoff embedded in a cooling schedule.
- **Readings:** Russell & Norvig Ch. 4 (§4.1.2, simulated annealing), Ch. 6 (CSP in depth).
- **Software:** Python, `random`.

## Week 6 — Automated Reasoning: SAT and SMT
- **Topics:** The Boolean satisfiability problem (SAT) in depth; conjunctive normal form (CNF)
  review; the DPLL algorithm — unit propagation, pure-literal elimination, and backtracking
  search over variable assignments; clause learning (conceptual: how a modern CDCL solver learns
  a new clause from a conflict to prune future search); a conceptual overview of Satisfiability
  Modulo Theories (SMT) — SAT extended with background theories (linear arithmetic, arrays,
  uninterpreted functions) via a SAT solver cooperating with theory-specific decision procedures.
- **Subtopics/Skills:** implementing DPLL with unit propagation from scratch in Python; tracing
  DPLL by hand on a small CNF formula; explaining, conceptually, why SMT solvers (e.g., Z3) are
  used for software/hardware verification problems that plain SAT cannot express naturally.
- **Readings:** Russell & Norvig Ch. 7 (§7.6, propositional theorem proving / DPLL).
- **Software:** Python; `python-sat` (PySAT) discussed conceptually as production-grade tooling
  (not required to install).

## Week 7 — Rigorous Classical Planning
- **Topics:** The computational complexity of classical planning — PLAN-SAT and plan existence
  for STRIPS-style planning are PSPACE-complete (conceptual statement and intuition for why,
  without a full complexity-theory proof); the planning graph (alternating layers of literals and
  actions) and how relaxed-planning-graph heuristics (ignoring delete lists) give an informative,
  efficiently computable heuristic for heuristic-search planners; Hierarchical Task Network (HTN)
  planning — decomposing abstract tasks into primitive actions via decomposition methods, and
  why this exploits domain structure that flat STRIPS search does not.
- **Subtopics/Skills:** building a small planning graph by hand and reading off a relaxed-plan
  heuristic value; implementing a simple HTN decomposition for a toy domain (e.g., a travel-
  booking or blocks-world-with-subtasks domain); stating the PSPACE-completeness result for
  planning and explaining, in one or two sentences, why it is believed strictly harder than
  NP-complete problems like SAT.
- **Readings:** Russell & Norvig Ch. 10–11 (planning complexity §10.4, planning graphs §11.1,
  hierarchical planning §11.2).
- **Software:** Python.
- **Capstone topic selection opens** (students begin identifying a subtopic for the research
  capstone; informal proposals discussed with the instructor Weeks 7–8).

## Week 8 — Markov Decision Processes I; Midterm Review
- **Topics:** The MDP formalism — states, actions, a transition model P(s'|s,a), a reward
  function R(s,a,s'), and a discount factor γ; the Bellman equation for the optimal state-value
  function; the value iteration algorithm and its convergence guarantee (a contraction mapping
  argument, conceptual); review session for Weeks 1–7 ahead of the midterm.
- **Subtopics/Skills:** implementing value iteration on a small grid-world MDP in Python; tracing
  one Bellman backup by hand; explaining why value iteration converges (the Bellman backup is a
  contraction under the max-norm when γ < 1) and what goes wrong if γ ≥ 1 or is mis-set.
- **Readings:** Russell & Norvig Ch. 17 (§17.1–17.2); Sutton & Barto Ch. 3–4.
- **Software:** Python, standard library only.
- **Assignment 2 assigned** (CSP/optimization, SAT/SMT, planning, Weeks 5–8).

## Week 9 — Midterm Exam; Markov Decision Processes II
- **Topics:** Midterm Exam (covers Weeks 1–8). Afterward: the policy iteration algorithm
  (alternating policy evaluation and policy improvement) and why it converges in a finite number
  of iterations for a finite MDP; the exploration-exploitation tradeoff in reinforcement learning
  (why an agent that only exploits its current estimate can get stuck in a suboptimal policy);
  Q-learning as a foundational model-free, off-policy reinforcement-learning algorithm and its
  update rule.
- **Subtopics/Skills:** implementing policy iteration on the same grid-world MDP and confirming it
  converges to the same optimal policy as value iteration; implementing tabular Q-learning with an
  ε-greedy exploration policy on a small grid-world; comparing value iteration (requires a known
  model) with Q-learning (model-free, learns from experience).
- **Readings:** Russell & Norvig Ch. 17 (§17.3), Ch. 22 (§22.1–22.3, reinforcement learning
  overview); Sutton & Barto Ch. 4, Ch. 6 (§6.5, Q-learning).
- **Software:** Python, standard library only.

## Week 10 — Partially Observable MDPs (POMDPs)
- **Topics:** Why partial observability complicates planning — the agent no longer knows its
  exact state, only a probability distribution (belief state) over states, conditioned on its
  history of actions and observations; the belief-state update (a Bayesian filter over the
  underlying MDP's transition and observation models); why solving a POMDP exactly is far harder
  than solving the underlying MDP (the belief space is continuous even if the state space is
  discrete); a small conceptual worked example (e.g., a 1-D "tiger problem"-style two-state POMDP)
  tracing a belief update by hand.
- **Subtopics/Skills:** computing a belief-state update by hand for a small two-state, two-
  observation POMDP given a prior belief, an action, and an observation; implementing the belief
  update as a short Python function; articulating, precisely, the difference between a policy
  over states (MDP) and a policy over belief states (POMDP).
- **Readings:** Russell & Norvig Ch. 17 (§17.4, POMDPs).
- **Software:** Python, standard library only.

## Week 11 — Probabilistic Graphical Models at Rigor
- **Topics:** Review of Bayesian networks as compact factored representations of a joint
  distribution; the computational complexity of exact inference (exact inference in general
  Bayesian networks is #P-hard; even inference by enumeration/variable elimination is
  exponential in the worst case, governed by network treewidth); approximate inference via
  sampling — rejection sampling (sampling from the joint, rejecting samples inconsistent with
  evidence), likelihood weighting (weighting samples by evidence likelihood instead of rejecting),
  and a brief, conceptual mention of Markov Chain Monte Carlo (MCMC)/Gibbs sampling as a way to
  sample from the posterior without the inefficiency of rejection sampling.
- **Subtopics/Skills:** implementing rejection sampling and likelihood weighting for query
  inference on a small Bayesian network in Python; comparing the variance/efficiency of the two
  sampling methods empirically on the same network and query; stating why exact inference is
  worst-case intractable while sampling-based methods trade exactness for scalability.
- **Readings:** Russell & Norvig Ch. 13 (§13.1–13.4, exact and approximate inference).
- **Software:** Python, `random`.
- **Assignment 3 assigned** (MDPs, POMDPs, probabilistic inference, Weeks 8–11).

## Week 12 — Computational Complexity of AI Problems
- **Topics:** A unifying look back at the complexity results touched on throughout the course —
  SAT is NP-complete (Cook-Levin theorem, stated and discussed, not proved in full); general CSP
  is NP-complete (by reduction from/to SAT-like reasoning); classical planning is PSPACE-complete
  (Week 7 result revisited); exact inference in Bayesian networks is worst-case intractable (Week
  11 result revisited); discussion of why these results do not make AI hopeless in practice —
  heuristics, approximation, structure exploitation (restricted problem classes, average-case
  behavior) are how real AI systems cope with worst-case intractability.
- **Subtopics/Skills:** stating precisely what "NP-complete" and "PSPACE-complete" mean (decision
  problem, polynomial-time verifiability/reducibility, the relationship NP ⊆ PSPACE); explaining,
  for each of SAT, CSP, and planning, which complexity class it falls in and why that matters for
  algorithm design; writing a short empirical demonstration (runtime vs. problem size) showing
  how a complete SAT or CSP solver's runtime scales on hard vs. easy instances.
- **Readings:** Russell & Norvig Ch. 3 (§3.6, complexity of search, review), Ch. 6 (§6.1, CSP
  complexity), Ch. 10 (§10.4, planning complexity, review); a standard computational-complexity
  reference (e.g., Sipser's *Introduction to the Theory of Computation*) for NP/PSPACE definitions
  is a suggested, non-required supplementary reading.
- **Software:** Python (timing experiments with `time`/`timeit`).

## Week 13 — Research Methods in AI
- **Topics:** How to read a research paper efficiently (abstract → figures/results → methods →
  related work → full read); how to critique a paper — is the claim supported by the evidence,
  are baselines fair and current, is the experimental design sound; reproducibility concerns in
  AI research (missing hyperparameters, cherry-picked seeds, undisclosed compute, non-public
  code/data); experimental design — the role of benchmarks, the importance of strong baselines,
  ablation studies (removing one component at a time to isolate its contribution), and the basics
  of statistical significance when comparing two methods' results (why a single run's numbers are
  not evidence of a real difference).
- **Subtopics/Skills:** critiquing a short paper excerpt as a structured in-class exercise
  (identifying claim, evidence, baseline adequacy, and at least one reproducibility concern);
  designing, on paper, an ablation study for a hypothetical AI system; explaining why comparing
  two algorithms' performance over multiple random seeds (not one) is necessary before claiming
  one is better.
- **Readings:** Guidance handouts on reading/critiquing papers (instructor-provided); students
  begin selecting a paper from a suggested reading list (drawn from AAAI/IJCAI/NeurIPS-style
  venues) for the Paper Critique & Presentation assignment.
- **Software:** none (methods/discussion week).
- **Paper Critique & Presentation assignment assigned.**

## Week 14 — Current Research Topics Survey
- **Topics:** A grounded, non-hype survey of current AI research directions that build directly
  on this course's foundations — multi-agent reinforcement learning (extending single-agent MDPs/
  Q-learning from Weeks 8–9 to multiple interacting learning agents, connecting back to the game
  theory of Weeks 3–4); explainable AI (XAI) at an introductory level (why black-box decisions are
  a problem, simple model-agnostic explanation ideas such as feature-perturbation sensitivity —
  one sentence only on explaining neural-network-specific methods, which belong to the ANN/DL
  courses); AI safety and alignment at an introductory, conceptual level (the specification/reward-
  hacking problem in RL, why a reward function that is "almost right" can produce unintended
  behavior, and why alignment is an active research area).
- **Subtopics/Skills:** explaining, conceptually, what makes multi-agent RL harder than single-
  agent RL (a non-stationary environment from each agent's perspective); implementing a toy
  feature-perturbation-based explanation for a simple rule-based or tabular decision function;
  giving one concrete example of reward hacking/misspecification in a toy RL setting.
- **Readings:** current AAAI/IJCAI/NeurIPS-style papers or surveys on multi-agent RL, XAI, and AI
  safety, selected by the instructor each offering (no fixed citation list — this is a
  fast-moving area and readings are refreshed per term).
- **Software:** Python.

## Week 15 — Research Project Work Session
- **Topics:** Structured, instructor-guided work time for the research capstone: finalizing the
  literature review (3–5 papers), designing and running the small reproduced/extended experiment,
  and drafting the written paper; peer feedback practice — each student/pair gives a short
  practice run of their capstone talk and receives structured peer feedback before Week 16.
- **Subtopics/Skills:** giving and receiving structured, specific feedback on a research talk
  (clarity of problem statement, soundness of experiment, honesty about limitations); revising a
  draft literature review or experiment plan in response to feedback.
- **Readings:** none assigned; working session on students' own capstone materials.
- **Software:** whatever each capstone project requires (see individual proposals).
- **Deliverable:** capstone written-paper draft due; peer-feedback worksheet submitted.

## Week 16 — Capstone Research Presentations; Course Review
- **Topics:** Student capstone research presentations (conference-talk format: problem,
  related work, method/experiment, results, limitations, Q&A); recap of the course map (advanced
  search → game theory/multi-agent systems → automated reasoning → rigorous planning → MDPs/RL/
  POMDPs → graphical-model inference → AI complexity → research methods → current trends);
  closing discussion connecting this course's decision-theoretic and complexity-theoretic
  foundations to the sibling graduate courses (ANN, ML, DL, KR&R) students may take next.
- **Deliverable:** Capstone final paper submission + conference-style presentation.

## Week 17 — Final Exam Week
- Comprehensive final exam, weighted toward Weeks 9–14 content (per Assessment Plan).

---

## Research Capstone (topic selection Weeks 7–8, work session Week 15, presentations Week 16)
Students (individually or in pairs) choose an AI subtopic covered in this course (or adjacent to
it, with instructor approval) and complete a research-style project with four required
components: (1) a **literature review** of 3–5 relevant papers summarizing the state of the art
and the specific gap or question the project addresses; (2) a **small reproduced or extended
experiment** — either reproducing a core result from one of the reviewed papers at small scale,
or extending/varying it in a focused way (e.g., a new heuristic for IDA*, a different exploration
strategy for Q-learning, an additional ablation on a sampling-based inference method); (3) a
**short written paper** (introduction, related work, method, results, discussion of limitations,
in a conference-short-paper style); and (4) a **conference-style presentation** in Week 16. The
capstone is explicitly research-shaped, not a plain coding project: a project that honestly
reports a negative or partial result, correctly analyzed, is graded on the soundness of its
literature review, experimental design, and analysis — not on whether the original paper's result
was fully reproduced. Example topics: comparing IDA*/SMA*/A* memory-time tradeoffs empirically; a
small multi-agent Q-learning experiment and its relation to Nash equilibria; extending a DPLL
implementation with a simple clause-learning heuristic and measuring the speedup; an empirical
study of planning-graph heuristic quality across several STRIPS domains; a comparison of
value/policy iteration convergence rates under different discount factors; an empirical study of
rejection sampling vs. likelihood weighting efficiency as evidence becomes rarer.
