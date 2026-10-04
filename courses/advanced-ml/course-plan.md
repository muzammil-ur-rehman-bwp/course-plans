# Course Plan: Advanced Machine Learning (Post Graduate)

## 1. Course Information

| Field | Detail |
|---|---|
| Course Title | Advanced Machine Learning |
| Level | Post Graduate (PhD-track: PhD coursework, or an MS student heading toward a thesis) |
| Credit Hours | 3 (2 hrs lecture + 1 research seminar/lab session of 3 hrs/week) |
| Prerequisites | *Machine Learning* (Graduate), or equivalent: PAC learning and sample complexity, VC dimension and the Sauer–Shelah lemma, Rademacher complexity, the concentration-inequality toolkit (Markov/Chebyshev/Hoeffding/McDiarmid), convex optimization and KKT conditions, kernel methods and RKHS theory (the representer theorem), rigorous ensemble theory (AdaBoost's training-error bound, bagging's variance-reduction argument), Bayesian machine learning (Bayesian linear regression, Gaussian Processes), Conditional Random Fields and structured prediction, rigorous dimensionality-reduction theory (PCA optimality, kernel PCA), a basic causal-inference-introduction unit (correlation vs. causation, confounding, Simpson's paradox, do-notation introduced conceptually only), and model-selection theory (bias-variance decomposition, AIC/BIC, cross-validation). This course assumes every one of these as firm, already-examined foundation and does **not** re-teach any of them. |
| Programming Language | Python 3.x (every technical result in this course is probed empirically in Python; several weeks also expect students to read current NeurIPS/ICML/COLT papers and, where available, their released code) |
| Core Libraries | NumPy, SciPy (`scipy.stats`, `scipy.linalg`, `scipy.optimize`), pandas (causal-inference weeks), Matplotlib. scikit-learn is used sparingly, only to cross-check from-scratch results, never as the primary teaching vehicle |
| Duration | 16 teaching weeks (1 semester) + 1 exam week |
| Delivery Mode | Lecture + research seminar/lab (concept lecture followed by an implementation, derivation-verification, critical-writing, or proposal-development session, depending on the week) |

## 2. Course Description

This is a PhD-track, research-frontier treatment of statistical machine learning theory that
assumes the graduate *Machine Learning* course's entire toolkit — PAC/VC/Rademacher generalization
theory, concentration inequalities, convex optimization and KKT, kernel/RKHS theory, rigorous
ensemble theory, Bayesian ML, structured prediction, dimensionality-reduction theory, a
conceptual-only introduction to causal inference, and model-selection theory — as settled
background, and spends every week pushing into territory that course explicitly does not cover.
The course has five pillars. **(1) Minimax lower bounds and high-dimensional statistics**: the
minimax risk framework, Fano's inequality and the information-theoretic argument behind
estimation lower bounds, sub-Gaussian/sub-exponential concentration beyond Hoeffding, Bernstein's
inequality, and random matrix theory basics (the Marchenko–Pastur law) explaining why sample
covariance estimation misbehaves when the number of features is comparable to the number of
samples. **(2) Full-information online convex optimization**: the online-learning protocol in
which the learner observes the *entire* loss function each round (not merely the payoff of a
chosen action), Follow-The-Regularized-Leader (FTRL), and the derived regret bound for online
gradient descent — a setting explicitly distinct from, and complementary to, the partial-information
bandit/regret territory owned by the sibling postgraduate course, *Advanced Artificial
Intelligence*. **(3) Nonparametric Bayesian methods**: the Dirichlet process as a prior over
infinite discrete distributions, its stick-breaking construction, the Chinese Restaurant Process
as its combinatorial twin, and infinite mixture models. **(4) Causal inference in genuine depth**:
the potential-outcomes (Rubin) framework, propensity-score methods, instrumental variables, and
the do-calculus rules — built rigorously on top of the graduate course's conceptual-only
introduction to confounding and do-notation, which is assumed, not repeated. **(5) Trustworthy and
robust statistical learning**: distribution-shift and domain-adaptation theory, algorithmic
fairness's formal criteria and impossibility results, and robust statistics (median-of-means,
trimmed-mean estimation) under adversarial or heavy-tailed contamination.

The capstone reflects what postgraduate work is for. Where the graduate course's capstone is a
literature review plus a small reproduced or extended experiment, this course's capstone is a
**research proposal** — a PhD-qualifying-exam-style deliverable: a problem statement, a
related-work survey of five or more papers, a proposed novel approach or extension, and either
preliminary results or a rigorous feasibility argument for why the approach should work. It is
evaluated as a thesis proposal would be, not as a course project.

**This course does not repeat the graduate ML course.** PAC learning, VC dimension, Rademacher
complexity, the Hoeffding/McDiarmid toolkit, convex optimization/KKT, kernel methods/RKHS, AdaBoost
and bagging theory, Bayesian linear regression, Gaussian Processes, CRFs, PCA/kernel-PCA theory,
basic causal-inference concepts, and bias-variance/AIC/BIC/cross-validation theory are assumed
fluent and are referenced only as background when relevant. It also does **not** duplicate the
sibling postgraduate courses: it does not cover the bandit/partial-information regret theory,
multi-agent systems, or game theory owned by *Advanced Artificial Intelligence* (this course's
online-learning week covers *full-information* online convex optimization — a different, though
related, setting, and the distinction is stated explicitly where it arises); it does not cover
neural architectures and training (*Advanced Artificial Neural Network*), deep architectures
(*Advanced Deep Learning*), or deep KR formalisms (*Advanced Knowledge Representation and
Reasoning*). Where one of those topics is relevant to an argument here, it receives at most a
one-sentence pointer, never a lecture.

## 3. Goals

- Master the minimax risk framework and derive, via Fano's inequality, a lower bound on the
  estimation risk for a simple parametric family.
- Master sub-Gaussian and sub-exponential concentration as the natural generalization of
  bounded/Gaussian concentration, and state and apply Bernstein's inequality.
- Explain, via the Marchenko–Pastur law, why sample-covariance eigenstructure is systematically
  distorted in the high-dimensional regime, and connect this to real estimation failure modes.
- Derive the online convex optimization protocol's regret guarantees for Follow-The-Regularized-
  Leader and online gradient descent, and articulate precisely why this full-information setting
  is distinct from the bandit setting.
- Construct the Dirichlet process via its stick-breaking representation and via the Chinese
  Restaurant Process, and apply both to infinite mixture modeling.
- Formalize the potential-outcomes framework, derive propensity-score weighting as a causal-effect
  estimator, state the instrumental-variables identification argument, and apply the core do-calculus
  rules to determine identifiability of a causal query from a graph.
- Formalize covariate shift, derive importance-weighted correction, and state the theoretical
  guarantees (and their limits) for generalization under distribution shift.
- State the formal definitions of demographic parity, equalized odds, and calibration, and prove
  (or carefully state) the impossibility result showing these criteria generally cannot be
  satisfied simultaneously.
- Derive and implement robust mean-estimation procedures (median-of-means, trimmed mean) under
  adversarial or heavy-tailed contamination, and connect this conceptually to adversarial
  robustness in modern ML.
- Read and critique current statistical-learning-theory research, identify genuinely open
  problems, and scope, write, and defend an original PhD-qualifying-style research proposal.

## 4. Course Learning Outcomes (CLOs) — Mapped to Bloom's Taxonomy

| CLO | Statement | Bloom's Level(s) |
|---|---|---|
| CLO1 | Recall the graduate-ML formalism this course assumes, and map the postgraduate research-frontier landscape (minimax/high-dimensional statistics, full-information OCO, nonparametric Bayes, causal inference in depth, trustworthy/robust ML) this course covers, distinguishing it from the sibling postgraduate AI course's bandit territory. | Remember, Understand |
| CLO2 | State the minimax risk framework and derive, via Fano's inequality, an estimation-risk lower bound for a simple parametric family. | Apply, Analyze |
| CLO3 | Define sub-Gaussian and sub-exponential random variables, state and apply Bernstein's inequality, and explain the Marchenko–Pastur law's implications for sample-covariance estimation in high dimensions. | Understand, Apply, Analyze |
| CLO4 | Derive the regret bound for Follow-The-Regularized-Leader and online gradient descent in the full-information online convex optimization setting, and articulate precisely how this setting differs from partial-information bandit regret. | Apply, Analyze |
| CLO5 | Construct the Dirichlet process via stick-breaking and via the Chinese Restaurant Process, and apply both constructions to infinite mixture modeling. | Apply, Analyze |
| CLO6 | Formalize the potential-outcomes framework, derive propensity-score weighting as a causal-effect estimator, and state the instrumental-variables identification argument. | Apply, Analyze |
| CLO7 | Apply the core do-calculus rules to determine the identifiability of a causal query from a causal graph, building rigorously on the graduate course's conceptual-only do-notation introduction. | Apply, Analyze, Evaluate |
| CLO8 | Formalize covariate shift and distribution-shift generalization, derive importance-weighted correction, and critically evaluate the limits of generalization guarantees under shift. | Analyze, Evaluate |
| CLO9 | State formal algorithmic-fairness criteria (demographic parity, equalized odds, calibration), and critically evaluate the impossibility results governing their simultaneous satisfaction. | Analyze, Evaluate |
| CLO10 | Derive and implement robust estimators (median-of-means, trimmed mean) under contamination, and critically connect robust-statistics theory to adversarial robustness in modern ML. | Apply, Analyze, Evaluate |
| CLO11 | Formulate an original research question in statistical learning theory, survey and synthesize 5+ related papers, and construct a rigorous feasibility argument or preliminary result for a proposed novel approach. | Analyze, Evaluate, Create |
| CLO12 | Design, write, and orally defend a PhD-qualifying-exam-style research proposal under questioning, in the manner of a thesis-proposal committee. | Evaluate, Create |

### Bloom's Taxonomy progression across the semester

| Phase | Weeks | Dominant Bloom's Levels | Focus |
|---|---|---|---|
| Postgraduate Orientation | 1 | Remember, Understand | Research-frontier landscape; assumed-foundations review; introducing proposal scoping |
| Minimax Theory & High-Dimensional Statistics | 2–4 | Apply, Analyze | Minimax risk; Fano's inequality; sub-Gaussian/sub-exponential concentration; Bernstein's inequality; random matrix theory basics |
| Online Convex Optimization & Nonparametric Bayes | 5–7 | Apply, Analyze | Full-information OCO; FTRL and online gradient descent; the Dirichlet process; the Chinese Restaurant Process |
| Causal Inference in Depth | 8–9 | Apply, Analyze, Evaluate | Potential outcomes; propensity scores; midterm; instrumental variables; do-calculus |
| Distribution Shift, Fairness, Robustness & Research Methods | 10–13 | Analyze, Evaluate | Covariate shift and domain adaptation; algorithmic-fairness impossibility results; robust statistics; postgraduate research methods |
| Frontier Survey & Capstone | 14–16 | Evaluate, Create | Open-problems survey; proposal drafting and peer feedback; proposal presentations |

Evaluate and Create dominate from Week 8 onward, and the capstone (CLO11–CLO12, both
Evaluate/Create) is weighted accordingly — postgraduate work is judged chiefly on the ability to
formulate and scope original research, not on recall or routine application.

## 5. Weekly Topic Overview (16 Weeks)

| Week | Topic | Bloom's Focus |
|---|---|---|
| 1 | Postgraduate overview: the statistical-learning-theory research frontier; rapid review of assumed graduate foundations; how to scope a research proposal (introduced early) | Remember, Understand |
| 2 | Minimax lower bounds: the minimax risk framework; Fano's inequality; a worked parametric-estimation lower bound | Apply, Analyze |
| 3 | High-dimensional statistics I: sub-Gaussian and sub-exponential concentration; Bernstein's inequality | Apply, Analyze |
| 4 | High-dimensional statistics II: random matrix theory basics; the Marchenko–Pastur law; sample-covariance pathologies in high dimensions | Understand, Apply, Analyze |
| 5 | Full-information online convex optimization: the OCO protocol (distinct from bandits); Follow-The-Regularized-Leader; online gradient descent's regret bound, derived | Apply, Analyze |
| 6 | Nonparametric Bayesian methods I: the Dirichlet process; the stick-breaking construction | Understand, Apply |
| 7 | Nonparametric Bayesian methods II: the Chinese Restaurant Process; infinite mixture models | Apply, Analyze |
| 8 | Causal inference in depth I: the potential-outcomes framework; formalizing confounding; propensity-score methods; midterm review | Apply, Analyze |
| 9 | **Midterm Exam** + Causal inference in depth II: instrumental variables; the do-calculus rules | Apply, Analyze |
| 10 | Distribution shift and domain adaptation: covariate shift formalized; importance weighting; generalization guarantees under shift (and their limits) | Apply, Analyze |
| 11 | Algorithmic fairness: formal fairness criteria (demographic parity, equalized odds, calibration); the impossibility results | Analyze, Evaluate |
| 12 | Robust statistics: median-of-means and trimmed-mean estimation under contamination; connection to adversarial robustness | Apply, Analyze, Evaluate |
| 13 | Research methods for statistical learning theory at the postgraduate level: reading cutting-edge theory papers; identifying open problems; structured capstone work time | Analyze, Evaluate |
| 14 | Current open problems survey (grounded, explicitly flagged as fast-moving) | Understand, Evaluate |
| 15 | Research proposal work session: drafting, refining, peer feedback | Apply, Create |
| 16 | Capstone research proposal presentations + course review | Evaluate, Create |
| 17 | Final Exam Week | — |

## 6. Assessment Plan

| Component | Weight | Notes |
|---|---|---|
| Lab Work (weekly) | 10% | Graded notebooks/exercises, Labs 1–15 |
| Assignments (2 problem sets) | 10% | Tied to Weeks 4 and 7 |
| Quizzes (6, best 5 counted) | 10% | Short, in-class/online, 15 min each |
| Paper Critique & Presentation | 10% | Week 13 research-methods assignment; written critique + in-class presentation of a current statistical-learning-theory paper |
| Midterm Exam | 10% | Week 9, qualifying-exam style, covers Weeks 1–8 |
| Research Proposal Capstone | 40% | Problem statement, 5+ paper related-work survey, proposed novel approach, feasibility argument/preliminary results, written proposal, and oral defense |
| Final Exam | 10% | Comprehensive, emphasis on Weeks 9–15 |

**Rationale for the postgraduate weighting.** As in the sibling postgraduate AI course, weight
shifts decisively toward independent research. Exams (Midterm + Final) total only 20%, and weekly
Labs are 10%, because postgraduate lab sessions are shorter and increasingly folded into
proposal-development work by the back half of the semester. The Research Proposal Capstone, at
40%, is by far the largest single component and is deliberately judged like a thesis-proposal
defense: a sound problem statement, a genuine survey of the related work, a defensible proposed
approach, and an honest feasibility argument — not exam recall or a fixed, instructor-specified
assignment. The components total exactly 100% (10 + 10 + 10 + 10 + 10 + 40 + 10 = 100).

## 7. Grading Policy

Standard letter grading per institutional policy (e.g., A ≥ 85, B ≥ 70, C ≥ 55, D ≥ 40, F < 40;
adjust to institution). Late submissions: −10% per day up to 3 days, then not accepted unless
documented emergency. Capstone milestones (problem-statement check-in Week 8, draft proposal Week
15, final proposal and defense Week 16) have fixed deadlines because of the downstream defense
schedule; late capstone milestones are handled case-by-case with the instructor.

## 8. Tools & Software

- Python 3.10+, pip/conda
- Jupyter Notebook / Google Colab
- NumPy and SciPy (`scipy.stats`, `scipy.linalg`, `scipy.optimize`) for every from-scratch
  derivation-verification exercise (Fano-bound sanity checks, sub-Gaussian/Bernstein simulations,
  Marchenko–Pastur eigenvalue simulations, FTRL/online-gradient-descent implementations, Dirichlet
  process samplers, propensity-score and instrumental-variable estimators, importance-weighting
  correction, fairness-metric computation, median-of-means estimators)
- pandas, for the causal-inference and distribution-shift weeks' observational-data exercises
- scikit-learn, used sparingly and only to cross-check from-scratch results, never as the primary
  teaching vehicle
- Matplotlib for diagnostic plots (empirical vs. theoretical bounds, regret curves, eigenvalue
  spectra, posterior samples, propensity-score overlap, fairness-metric tradeoff curves)
- Git/GitHub for lab, assignment, and capstone submission

## 9. Reference Textbooks and Reading

- Wainwright, M. J. — *High-Dimensional Statistics: A Non-Asymptotic Viewpoint*. Cambridge
  University Press. The canonical text for the concentration-of-measure and high-dimensional
  statistics material (Weeks 2–4): the minimax framework, Fano's inequality, sub-Gaussian and
  sub-exponential variables, Bernstein's inequality, and random matrix theory basics.
- Pearl, J. — *Causality: Models, Reasoning, and Inference*. Cambridge University Press. The
  canonical reference for the causal-inference material in depth (Weeks 8–9): the structural and
  potential-outcomes frameworks, instrumental variables, and the do-calculus.
- Hazan, E. — *Introduction to Online Convex Optimization* (free online). The canonical reference
  for full-information online convex optimization (Week 5): the OCO protocol, Follow-The-
  Regularized-Leader, and online gradient descent's regret bound.
- **Current papers from NeurIPS, ICML, and COLT are the primary reading material from Week 10
  onward** (distribution shift, algorithmic fairness, robust statistics, and the open-problems
  survey). This is a fast-moving research area; the instructor selects and refreshes the specific
  paper list each offering rather than this syllabus naming a fixed set that would quickly date.
  Students are expected to locate, read, and critically evaluate current primary sources as a core
  postgraduate skill, not merely consume a fixed reading list.
- Official NumPy, SciPy, and pandas documentation (for lab support only).

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
