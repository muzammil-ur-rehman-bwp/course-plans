# Course Contents: Advanced Artificial Intelligence (Post Graduate)

Detailed per-week breakdown of topics, subtopics, and resources. Companion to `course-plan.md`.
Each week lists: **Topics**, **Subtopics/Skills**, **Readings**, **Software/Libraries used**.

---

## Week 1 — Postgraduate Overview: The AI Research Frontier
- **Topics:** The research-frontier landscape this course covers — regret theory and bandits,
  multi-agent reinforcement learning, algorithmic game theory and mechanism design, AI safety and
  alignment, interpretability, and foundational/philosophical debates — and how it is disjoint
  from the sibling postgraduate courses (ANN, ML, DL, KR&R, Advanced); a rapid review of the
  graduate-AI foundations this course assumes (advanced search, game-theory/multi-agent basics,
  SAT/SMT, planning complexity, MDPs/POMDPs/RL fundamentals, AI complexity, research methods) —
  stated explicitly as assumed background, not re-taught; how to scope a research proposal,
  introduced in Week 1 because it drives every week's framing for the rest of the semester.
- **Subtopics/Skills:** self-assessing fluency in each assumed graduate-AI topic against a
  diagnostic checklist; stating, in one sentence each, what a problem statement, a related-work
  survey, a proposed approach, and a feasibility argument are, ahead of the full treatment in
  Week 13.
- **Readings:** Sutton & Barto, Preface and Ch. 1 (reinforcement learning's place in AI, for
  framing); Shoham & Leyton-Brown, Ch. 1 (introduction to multiagent systems).
- **Software:** Python 3.10+, Jupyter/Colab, NumPy (environment setup only).

## Week 2 — Online Learning and Regret Minimization
- **Topics:** The online-learning framework — an algorithm repeatedly chooses an action (or a
  distribution over a fixed set of "experts"), observes a loss, and must perform well without any
  statistical assumption on how losses are generated; **regret** as the standard performance
  measure (the gap between the algorithm's cumulative loss and the best fixed expert's cumulative
  loss, in hindsight); the multiplicative-weights (generalized weighted-majority) algorithm; its
  regret bound, derived via a potential-function argument.
- **Subtopics/Skills:** stating the formal online-learning protocol and the definition of regret
  precisely; deriving the multiplicative-weights regret bound Regret_T ≤ ηT + (ln N)/η and the
  optimized bound O(√(T ln N)); implementing multiplicative weights in Python and empirically
  verifying its regret grows sublinearly in T.
- **Readings:** Arora, Hazan & Kale-style multiplicative-weights survey treatment (as covered in
  lecture); Shoham & Leyton-Brown, Ch. 4 (brief pointer, regret and learning in games, previewing
  Weeks 5–6).
- **Software:** Python, NumPy.

## Week 3 — Multi-Armed Bandits
- **Topics:** The exploration-exploitation tradeoff formalized as the multi-armed bandit problem
  (K arms, unknown reward distributions, sequential pulls, cumulative-regret objective); the
  Upper Confidence Bound (UCB1) algorithm; its regret bound, derived from a Hoeffding
  concentration-inequality argument bounding how far an empirical mean can be from the true mean;
  Thompson sampling (conceptual): a Bayesian alternative that samples from a posterior over each
  arm's reward distribution rather than using a confidence bound.
- **Subtopics/Skills:** stating the Hoeffding bound and using it to justify UCB1's confidence
  radius; deriving the O((K log T)/Δ) expected-regret bound (via the expected-arm-pull-count
  bound) for suboptimal arms with gap Δ; implementing UCB1 and comparing its cumulative regret
  against ε-greedy on simulated Bernoulli bandits; explaining, conceptually, how Thompson
  sampling's posterior sampling achieves a similar exploration effect without an explicit
  confidence bound.
- **Readings:** Sutton & Barto Ch. 2 (§2.1–2.7, bandit problems, UCB, optimistic initial values).
- **Software:** Python, NumPy.

## Week 4 — Contextual Bandits and the Bridge to Reinforcement Learning
- **Topics:** Contextual bandits — at each round the algorithm observes a context (side
  information) before choosing an arm, and the optimal arm may depend on the context; contextual
  bandits as an interpolation between the context-free multi-armed bandit (Week 3) and full
  reinforcement learning (no state transitions — the context is drawn independently each round,
  unlike an MDP's state, which the agent's own actions influence); where the graduate course's
  tabular Q-learning sits on this spectrum (full MDP: states persist and transition under the
  agent's control, and the agent must reason about delayed, multi-step consequences, not just a
  single-round reward).
- **Subtopics/Skills:** implementing a simple contextual-bandit algorithm (ε-greedy or UCB over a
  linear reward model, i.e., a simplified LinUCB) and comparing its regret against a context-free
  bandit baseline on the same contextual reward-generating process; articulating precisely, in
  terms of state persistence and delayed reward, why contextual bandits are strictly easier than
  general MDPs despite looking superficially similar.
- **Readings:** Sutton & Barto Ch. 2 (§2.9, associative search / contextual bandits), Ch. 3
  (§3.1, review of the MDP framing, for contrast).
- **Software:** Python, NumPy.
- **Assignment 1 assigned** (online learning & bandit theory, Weeks 2–4).

## Week 5 — Multi-Agent Reinforcement Learning
- **Topics:** Extending single-agent RL to multiple simultaneously learning agents; independent
  learners (each agent runs single-agent Q-learning, treating other agents as part of a fixed
  environment) vs. joint-action learners (each agent models and conditions on other agents'
  actions); the fundamental complication of multi-agent RL — from any one agent's perspective,
  the environment is **non-stationary**, because the other agents' policies are changing as they
  too learn, which breaks the stationarity assumption that single-agent RL convergence guarantees
  (e.g., Q-learning's) rely on; a brief, accurate, non-hyped look at self-play — training an agent
  against copies or past versions of itself, as generically used in the training pipelines of
  superhuman game-playing systems — described at the level of the general mechanism (an agent's
  opponent pool improves together with the agent, creating an automatic curriculum) rather than
  citing specific unverified implementation details of any one system.
- **Subtopics/Skills:** implementing independent Q-learners for both players of a small repeated
  matrix game (e.g., repeated matching pennies or a repeated general-sum game) and observing
  non-convergent or cyclic behavior that would not occur against a fixed opponent; explaining,
  precisely, why non-stationarity invalidates the standard single-agent Q-learning convergence
  argument; describing the self-play mechanism and why it provides an automatically scaling
  curriculum of opponents.
- **Readings:** Shoham & Leyton-Brown Ch. 7 (learning and teaching in multiagent systems); Sutton
  & Barto Ch. 6 (§6.5, Q-learning, for direct comparison against the single-agent case).
- **Software:** Python, NumPy.

## Week 6 — Algorithmic Game Theory I: Equilibrium Computation
- **Topics:** Computing Nash equilibria of normal-form games as a computational problem in its
  own right (not just a definition, as in the graduate course); the complexity of Nash-equilibrium
  computation — for two-player **zero-sum** games, an equilibrium is computable in polynomial time
  via linear programming (the minimax theorem); for general (non-zero-sum) games, computing a
  Nash equilibrium is **PPAD-complete** (stated and discussed conceptually: PPAD, "Polynomial
  Parity Argument on Directed graphs," is a complexity class believed to lie strictly between P
  and NP-hardness, and Nash-equilibrium computation sits at its hardest, complete level — meaning
  it is no easier than any other PPAD problem, and no polynomial-time algorithm is known or
  expected); correlated equilibria (brief): a relaxation of Nash equilibrium that is, by contrast,
  computable in polynomial time via linear programming for any number of players.
- **Subtopics/Skills:** implementing a support-enumeration algorithm to find Nash equilibria of
  small bimatrix games; solving a two-player zero-sum game via linear programming (or a
  from-scratch fictitious-play approximation if `scipy` is unavailable); stating precisely what
  "PPAD-complete" means and why it is a meaningfully different kind of hardness result from
  NP-completeness (Week 12 of the graduate course's complexity unit); computing a correlated
  equilibrium of a small game and contrasting its computational tractability with Nash
  equilibrium's.
- **Readings:** Shoham & Leyton-Brown Ch. 3–4 (computing solution concepts; correlated
  equilibrium); Daskalakis–Goldberg–Papadimitriou-style treatment of PPAD-completeness, as
  covered in lecture.
- **Software:** Python, NumPy; `scipy.optimize.linprog` discussed conceptually (optional).

## Week 7 — Algorithmic Game Theory II: Mechanism Design
- **Topics:** Mechanism design in depth — designing the rules of a game (an allocation rule and a
  payment rule) so that self-interested agents' equilibrium behavior, under their own private
  valuations, produces a socially desirable outcome; the Vickrey-Clarke-Groves (VCG) mechanism —
  choose the welfare-maximizing outcome given reported valuations, and charge each agent the
  externality it imposes on the others (the drop in others' welfare caused by that agent's
  presence); the truthfulness (dominant-strategy incentive compatibility) of VCG, derived from
  first principles; the single-item second-price (Vickrey) auction as VCG's simplest special
  case; applications — sponsored-search/ad auctions and resource allocation as real domains where
  VCG-style mechanisms (or practical approximations/variants of them) are used.
- **Subtopics/Skills:** deriving the VCG payment rule and reproducing the standard
  dominant-strategy truthfulness proof (an agent's utility under truthful reporting decomposes
  into a term the mechanism is explicitly maximizing plus a term independent of that agent's own
  report, so no misreport can do better); implementing a VCG mechanism for a small combinatorial
  or single-item allocation problem and empirically verifying that a bidder's utility is maximized
  by truthful reporting; explaining, concretely, how a sponsored-search ad auction or a resource-
  allocation market borrows from (or deliberately departs from) the VCG blueprint.
- **Readings:** Shoham & Leyton-Brown Ch. 10 (mechanism design; VCG; auctions).
- **Software:** Python, NumPy.
- **Assignment 2 assigned** (multi-agent RL & algorithmic game theory, Weeks 5–7).

## Week 8 — AI Safety and Alignment I; Midterm Review
- **Topics:** Specification gaming and reward hacking as a technical phenomenon — when a reward
  function is an imperfect proxy for the true intended objective, an optimizer under sufficient
  optimization pressure tends to exploit any gap between the proxy and the true objective (a
  Goodhart's-law effect), with grounded, well-documented examples (e.g., the widely cited
  CoastRunners boat-racing case, where a reinforcement-learning agent discovered it could loop
  through reward targets in a lagoon indefinitely rather than finishing the race, scoring higher
  than human players who completed the course normally; and the broader catalogue of
  specification-gaming examples documented across many RL benchmarks in the research literature);
  the alignment problem formalized as a research question — ensuring an AI system's actual
  behavior matches the designer's true intent, not merely the letter of its specified objective;
  outer alignment (is the specified/training objective itself a faithful proxy for the intended
  goal?) vs. inner alignment (does the trained system's actual learned objective match the
  specified training objective, or might it have learned a different internal proxy — a
  "mesa-objective" — that merely correlated with reward during training?); midterm review for
  Weeks 1–8.
- **Subtopics/Skills:** classifying a given example of unwanted AI behavior as a failure of outer
  alignment, inner alignment, or both; constructing a toy gridworld with a deliberately
  misspecified reward and demonstrating an agent "gaming" it rather than achieving the intended
  task; summarizing, precisely, the outer/inner alignment distinction and why inner alignment is
  a harder problem to even detect (a system can perform correctly throughout training and still
  harbor a misaligned internal objective that only manifests under distribution shift).
- **Readings:** Current safety/alignment papers and surveys selected by the instructor (see
  `course-plan.md` §9); DeepMind's "Specification gaming" research catalogue and OpenAI's
  "Faulty reward functions" case study, as covered in lecture.
- **Software:** Python, NumPy.

## Week 9 — Midterm Exam; AI Safety and Alignment II
- **Topics:** Midterm Exam (qualifying-exam style, covers Weeks 1–8). Afterward: a grounded,
  technical (non-hype) survey of current alignment-research directions — **reward modeling**
  (learning a reward function from human preference comparisons rather than hand-specifying one,
  as a way to narrow the outer-alignment gap); **scalable oversight** (techniques intended to let
  humans (or AI-assisted humans) supervise AI systems on tasks where direct human evaluation is
  difficult or infeasible, e.g., debate-style and amplification-style approaches, discussed at
  the level of their general mechanism); **interpretability as a safety tool** (the idea that
  mechanistic understanding of a model's internals could let us check its objective directly,
  rather than relying solely on its external behavior — previewing Week 10 in full).
  reward-modeling framework and why it is attractive (it only requires humans to compare outputs,
  not to specify a reward function explicitly); describing, at the level of general mechanism,
  one scalable-oversight approach (e.g., debate: two systems argue opposing sides for a human
  judge, intended to make dishonesty harder to sustain than honesty); explaining why
  interpretability is framed as a *safety* tool, not merely a usability feature.
- **Subtopics/Skills:** implementing a small reward-model lab exercise: fitting a reward model
  from synthetic pairwise preference data via logistic regression over a feature representation,
  and using the fitted model to rank held-out items; articulating one concrete limitation of
  reward modeling (it is only as good as the preference data's coverage and the labelers'
  judgment, so it inherits any blind spots or inconsistencies they have).
- **Readings:** Current reward-modeling and scalable-oversight papers selected by the instructor
  (see `course-plan.md` §9).
- **Software:** Python, NumPy.

## Week 10 — Interpretability Research
- **Topics:** Feature attribution methods (conceptual) — the general family of techniques
  (gradient-based saliency, permutation importance, and perturbation-based methods such as the
  LIME/SHAP family) that attribute a model's output to its input features or to perturbations of
  them; mechanistic interpretability as an emerging research area — the project of reverse-
  engineering the actual algorithm a trained model has learned (identifying interpretable
  internal features and the circuits connecting them), as opposed to only characterizing its
  input-output behavior; the gap between post-hoc explanation and genuine understanding —
  documented findings that some popular attribution methods can be insensitive to randomizing a
  model's own parameters (a basic sanity check a genuinely faithful explanation method should
  fail to pass), which is direct evidence that an attribution map can look plausible without
  actually reflecting the model's real computation.
- **Subtopics/Skills:** implementing a simple permutation-importance attribution method for a
  small trained model and comparing its output to a known ground-truth feature-relevance ranking
  on a synthetic dataset where the ground truth is known by construction; running a basic "sanity
  check" on an attribution method (e.g., checking whether its output changes appropriately when
  model weights are randomized) and interpreting what a failure of this check does and does not
  imply; articulating, precisely, the difference between "this input region correlates with the
  output" (post-hoc attribution) and "this is the mechanism the model uses" (mechanistic
  understanding).
- **Readings:** Current interpretability papers and surveys selected by the instructor, including
  sanity-check-style critiques of attribution methods (see `course-plan.md` §9).
- **Software:** Python, NumPy.

## Week 11 — Foundational and Philosophical Debates I
- **Topics:** The symbol grounding problem (Harnad, 1990) — how do the symbols a computational
  system manipulates acquire meaning connected to the world, rather than referring only to other
  symbols within the system (a purely syntactic system risks being "meaning" only in the sense
  that a dictionary defines words using other words)? The frame problem (McCarthy & Hayes, 1969)
  — how can an agent reasoning about the effects of actions represent, tractably, which facts
  remain unchanged by an action, without explicitly enumerating every fact that did *not* change?
  Both are presented as live, technically grounded research questions, not historical trivia:
  the symbol-grounding question resurfaces directly in any system whose internal representations
  are learned from data vs. hand-specified, and in questions about whether large learned models'
  internal representations are "grounded" in any sense beyond statistical co-occurrence; the
  frame problem resurfaces in any agent architecture that must efficiently track and update world
  state across actions (planning representations, world models, and the state-persistence issue
  raised in Week 4's contrast between bandits and MDPs are all, at bottom, engineering responses
  to the frame problem).
- **Subtopics/Skills:** stating the symbol grounding problem and the frame problem precisely, in
  the original terms in which they were posed; identifying, for a given modern AI system
  (described in a short case prompt), which design choices are implicit answers to one or both
  problems, and whether those answers are satisfying or merely practical workarounds.
- **Readings:** Harnad, "The Symbol Grounding Problem" (1990); McCarthy & Hayes, "Some
  Philosophical Problems from the Standpoint of Artificial Intelligence" (1969), as covered in
  lecture and handout excerpts.
- **Software:** none (critical-writing/discussion week; see `lab-manuals/lab-11.md`).

## Week 12 — Foundational and Philosophical Debates II
- **Topics:** Critiques of claimed AI capabilities — the Chinese Room argument (Searle, 1980):
  a thought experiment arguing that syntactic symbol manipulation (running a program), however
  behaviorally convincing, is not sufficient for genuine semantic understanding, challenging the
  claim that a sufficiently sophisticated program thereby *understands*; the standard rebuttals,
  presented even-handedly — the Systems Reply (the room-plus-rulebook-plus-person *system* as a
  whole may understand, even if the person inside does not), the Robot Reply (grounding the
  system's symbols in real sensorimotor interaction with the world might supply what the
  thought experiment's ungrounded symbol manipulation lacks — connecting directly back to Week
  11's symbol grounding problem), and the Brain Simulator Reply (if the simulation replicates the
  brain's causal structure precisely, on what principled basis would it lack understanding?); a
  critical-thinking-focused discussion of what "understanding" might mean for a computational
  system at all, and whether the question is empirically resolvable or a conceptual/definitional
  dispute dressed as an empirical one.
- **Subtopics/Skills:** reconstructing the Chinese Room argument's premises and conclusion
  precisely enough to state exactly where each rebuttal targets the argument; taking and
  defending a reasoned position (not necessarily a strong one) on whether behavioral
  indistinguishability is or is not sufficient evidence for understanding, and identifying what
  kind of evidence (if any) could in principle settle the question.
- **Readings:** Searle, "Minds, Brains, and Programs" (1980), as covered in lecture and handout
  excerpts, together with the standard replies literature.
- **Software:** none (critical-writing/discussion week; see `lab-manuals/lab-12.md`).

## Week 13 — Research Methods at the Postgraduate Level
- **Topics:** Scoping a research proposal in depth — writing a precise, falsifiable **problem
  statement** (not "I want to study X," but a specific question or gap, stated precisely enough
  that a reader could say what evidence would answer it); related-work-survey standards at the
  postgraduate level (accurately representing what each surveyed paper claims and found, placing
  papers in relation to each other rather than summarizing them in isolation, and using the
  survey to motivate a specific, stated gap); constructing a rigorous **feasibility argument**
  for a proposed approach that has not yet been fully run (what would have to be true for it to
  work, what the main technical risk is, and why the risk is judged manageable); how postgraduate
  evaluation differs from the graduate course's literature-review-plus-small-experiment standard
  — a postgraduate proposal is judged on the soundness of a plan for research not yet completed,
  much as a PhD qualifying exam or thesis-proposal defense is.
- **Subtopics/Skills:** drafting a one-paragraph, falsifiable problem statement for a candidate
  capstone topic and subjecting it to a structured peer critique (is it precise? is it actually
  open, i.e., not already settled by a paper the student has read?); annotating 5+ candidate
  papers for the capstone's related-work survey, stating for each what it claims, what it found,
  and how it relates to the student's proposed gap; drafting a feasibility argument's skeleton
  (what must be true, main risk, why the risk is manageable) for the student's own proposal.
- **Readings:** Instructor-provided guidance handouts on problem-statement writing, related-work
  survey standards, and feasibility arguments; students begin selecting their paper for the Paper
  Critique & Presentation assignment from current AAAI/IJCAI/NeurIPS/ICML-style venues.
- **Software:** none (methods/workshop week).
- **Paper Critique & Presentation assignment assigned.**

## Week 14 — Current Frontier Topics Survey
- **Topics:** A grounded, non-hype snapshot of 2–3 currently active AI research directions
  relevant to this course's pillars (the instructor selects specific current directions each
  offering — candidates include, e.g., scalable-oversight methods maturing beyond the Week 9
  survey, mechanistic-interpretability techniques extending Week 10, or multi-agent/self-play
  training approaches extending Week 5); **this week is explicitly flagged as covering a
  fast-moving area where specific techniques, benchmarks, and claims may date quickly** — the
  graded skill is locating, reading, and critically situating current primary sources against
  this course's foundations, not memorizing any particular current result.
- **Subtopics/Skills:** for each surveyed direction, stating which of this course's foundational
  pillars (regret/bandit theory, multi-agent RL/game theory, alignment, interpretability,
  foundational debates) it builds on, and what open question it leaves unresolved; practicing the
  Week 13 skill of a precise problem-statement extraction on a current paper, as direct
  preparation for the capstone.
- **Readings:** Current AAAI/IJCAI/NeurIPS/ICML-style papers or surveys selected by the
  instructor each offering (no fixed citation list — this is explicitly a fast-moving area and
  readings are refreshed per term; see `course-plan.md` §9).
- **Software:** Python, as needed for any accompanying small demonstration.

## Week 15 — Research Proposal Work Session
- **Topics:** Structured, instructor-guided time for drafting and refining the capstone research
  proposal: finalizing the problem statement, completing the 5+ paper related-work survey,
  developing the proposed novel approach, and building the feasibility argument or preliminary
  result; structured peer feedback on proposal drafts, modeled on a thesis-committee's style of
  questioning (is the problem statement precise and genuinely open? does the related-work survey
  correctly represent the cited papers and motivate the stated gap? is the proposed approach
  actually novel relative to the surveyed work, or a restatement of it? is the feasibility
  argument honest about the main risk?).
- **Subtopics/Skills:** giving and receiving structured, critical (not merely encouraging) peer
  feedback on a proposal draft, in the manner of a qualifying-exam committee member; revising a
  proposal draft in direct response to specific, itemized feedback.
- **Readings:** none assigned; working session on students' own capstone materials.
- **Software:** whatever each proposal's feasibility argument or preliminary result requires.
- **Deliverable:** capstone written-proposal draft due; peer-feedback worksheet submitted.

## Week 16 — Capstone Research Proposal Presentations; Course Review
- **Topics:** Student capstone research-proposal presentations, in a qualifying-exam/thesis-
  proposal-defense format (problem statement, related work, proposed approach, feasibility
  argument or preliminary results, anticipated risks, committee-style Q&A); recap of the course
  map (regret theory and bandits → multi-agent RL and algorithmic game theory → AI safety,
  alignment, and interpretability → foundational/philosophical debates → research-proposal
  methodology); closing discussion connecting this course's research-frontier foundations to the
  sibling postgraduate courses (ANN, ML, DL, KR&R, Advanced) and to an actual thesis or
  dissertation trajectory.
- **Deliverable:** Capstone final research-proposal submission + oral defense.

## Week 17 — Final Exam Week
- Comprehensive final exam, weighted toward Weeks 9–14 content (per Assessment Plan).

---

## Research Proposal Capstone (problem-statement check-in Week 8, draft Week 13/15, defense Week 16)
Each student (individual work — see `course-plan.md` §10) formulates an original research
question connected to this course's material (or to adjacent territory, with instructor
approval) and produces a PhD-qualifying-exam-style **research proposal** with four required
components: (1) a precise, falsifiable **problem statement** — a specific question or gap, not a
restatement of a topic; (2) a **related-work survey of 5 or more papers** that accurately
represents what each paper claims and found, relates the papers to each other, and uses the
survey to motivate the stated gap; (3) a **proposed novel approach or extension** — the student's
own formulation, which may combine or extend existing ideas but must be clearly distinguishable
from simply restating one surveyed paper's contribution; and (4) either **preliminary results**
(a small implemented pilot demonstrating the approach is workable) or a **rigorous feasibility
argument** (what must be true for the approach to work, the main technical risk, and a reasoned
case for why that risk is manageable) when running a full pilot is not feasible within the
semester. This capstone is explicitly a **proposal**, not a completed project: it is evaluated on
the soundness of the problem statement, the survey, the proposed approach, and the feasibility
reasoning — much as a PhD qualifying exam or thesis-proposal committee would evaluate a proposal,
not on whether a full experiment was run to completion. Example topics: a new regret bound or
algorithm variant for a structured bandit setting; a multi-agent RL training scheme for a
specific non-stationarity failure mode identified in Week 5; a mechanism-design variant for a
resource-allocation setting where strict VCG truthfulness is impractical; a proposed reward-
modeling or scalable-oversight refinement addressing a specific gap identified in Week 9; a new
or combined interpretability method targeted at the post-hoc/mechanistic gap from Week 10; a
proposal for empirically probing a modern system's representations for evidence relevant to the
symbol grounding problem (Week 11).
