# Course Plan: Advanced Artificial Neural Network (Post Graduate)

## 1. Course Information

| Field | Detail |
|---|---|
| Course Title | Advanced Artificial Neural Network |
| Level | Post Graduate (PhD-track: PhD coursework, or an MS student heading toward a thesis) |
| Credit Hours | 3 (2 hrs lecture + 1 research seminar/lab session of 3 hrs/week) |
| Prerequisites | *Artificial Neural Network* (Graduate), or equivalent: the Universal Approximation Theorem and depth-vs-width expressivity; automatic differentiation formalized (forward-/reverse-mode AD, backpropagation as a special case); initialization theory (Xavier/Glorot, He, signal propagation) and normalization theory (BatchNorm/LayerNorm, derived); optimization-landscape theory (saddle points, second-order methods, adaptive optimizers); classical generalization theory (PAC learning, VC dimension, Rademacher complexity) and the double-descent phenomenon at a conceptual level; and a grounded survey-level exposure to the Neural Tangent Kernel, the Lottery Ticket Hypothesis, and information-bottleneck perspectives. **Every one of these is assumed firm background and is not re-taught here** — this course states them as a rapid Week 1 review and then goes considerably deeper into several of the same named topics (the NTK chief among them) while opening genuinely new territory the graduate course only surveyed or did not cover at all. |
| Programming Language | Python 3.x |
| Core Libraries | PyTorch (primary, from Week 1 — this course works with wide networks, trained dynamics, and landscape geometry at a scale where hand-rolled NumPy autodiff is no longer the point), NumPy, Matplotlib, Jupyter/Colab |
| Duration | 16 teaching weeks (1 semester) + 1 exam week |
| Delivery Mode | Lecture + research seminar/lab (concept lecture followed by an implementation, critical-writing, or proposal-development session, depending on the week) |

## 2. Course Description

This is the most advanced course in the Artificial Neural Network sequence (undergraduate
*Introduction to Artificial Neural Networks* → graduate *Artificial Neural Network* → this
postgraduate course), and it is a PhD-track, research-frontier treatment of **neural network
theory**. It assumes the graduate course's content — approximation theory, automatic
differentiation, initialization/normalization theory, optimization-landscape theory, and classical
generalization theory, plus a first survey of the Neural Tangent Kernel (NTK), the Lottery Ticket
Hypothesis, and information-bottleneck ideas — as settled background, stated once in a rapid Week
1 review and never re-derived. From there the course pushes into territory the graduate course
explicitly only surveyed briefly or did not cover: the NTK revisited with real depth (the
infinite-width derivation sketch, the "lazy training" regime, and a sharper critique of its limits
as an account of deep learning's success); **mean-field theory** of wide networks, connected to
and contrasted with the NTK limit; the **implicit regularization / implicit bias** of gradient
descent, derived rigorously in the linear case and surveyed in nonlinear settings; how finite-width
networks escape the lazy/kernel regime into genuine **feature learning**; the empirical
**sharpness-generalization** relationship and Sharpness-Aware Minimization; the **grokking**
phenomenon and its competing explanations; the theoretical angle on **scaling laws**;
**statistical-physics** approaches to the loss landscape (the replica method, spin-glass
analogies); **double descent** revisited rigorously across model size, data size, and training
time; modern **PAC-Bayes** generalization bounds as a tighter alternative to classical VC/Rademacher
theory; and **loss-landscape geometry** — mode connectivity and a current-literature revisiting of
the Lottery Ticket Hypothesis. The capstone reflects what postgraduate work is for: a genuine
**research proposal**, evaluated as a PhD qualifying exam or thesis-proposal committee would
evaluate it, not as a completed course project.

**This course does not repeat the graduate course.** Universal approximation, autodiff formalism,
initialization/normalization derivations, saddle-point/second-order-method theory, classical
PAC/VC/Rademacher bounds, and the *introductory* treatments of NTK, the Lottery Ticket Hypothesis,
and the information bottleneck are assumed fluent and referenced only as background ("recall the
graduate course's NTK week" when revisiting the topic in depth). It also does not duplicate the
sibling postgraduate courses — classical statistical ML theory (*Advanced Machine Learning*), deep
architectures (*Advanced Deep Learning*), and deep KR formalisms (*Advanced Knowledge
Representation and Reasoning*) — where one of those topics is relevant to an argument here, it
receives at most a one-sentence pointer, never a lecture.

## 3. Goals

- Explain the infinite-width NTK derivation sketch and the lazy-training regime in depth, and
  critically evaluate the NTK framework's limits as an account of deep learning's empirical success.
- Explain the mean-field limit of wide neural networks, connect and contrast it with the NTK limit,
  and analyze signal propagation in the mean-field regime, building on the graduate course's
  initialization theory.
- Derive the implicit bias of gradient descent toward the max-margin solution on linearly separable
  data, and critically survey the implicit-bias literature in nonlinear and deep settings.
- Analyze how finite-width networks escape the NTK/lazy regime to learn data-dependent features,
  and argue why this matters for explaining deep learning's empirical success beyond kernel theory.
- Analyze the empirical sharpness-generalization relationship, including its debated reliability,
  and evaluate Sharpness-Aware Minimization as a concrete research-driven training method.
- Critically evaluate competing hypotheses for the grokking phenomenon (delayed generalization long
  after training accuracy saturates) against the available evidence.
- Explain the empirical form of neural scaling laws and evaluate theoretical attempts to explain
  them, including statistical-physics (replica-method, spin-glass) approaches to the loss landscape.
- Rigorously analyze the full double-descent curve across model size, sample size, and training
  time, and evaluate the theoretical explanations proposed for it.
- Evaluate modern PAC-Bayes generalization bounds as a tighter alternative to classical VC-dimension
  and Rademacher-complexity bounds, which are often vacuous for deep networks in practice.
- Analyze loss-landscape geometry through mode connectivity, and critically evaluate current
  refinements of and critiques of the Lottery Ticket Hypothesis.
- Formulate an original, falsifiable research question in neural-network theory; survey 5+ related
  papers; propose a genuinely novel approach; and construct a rigorous feasibility argument or
  preliminary result — then design, write, and orally defend a PhD-qualifying-exam-style research
  proposal.

## 4. Course Learning Outcomes (CLOs) — Mapped to Bloom's Taxonomy

| CLO | Statement | Bloom's Level(s) |
|---|---|---|
| CLO1 | Recall the graduate-level ANN-theory formalism this course assumes, and map the open-problems landscape of current neural-network-theory research this course covers, including how to scope a research proposal. | Remember, Understand |
| CLO2 | Explain the infinite-width NTK derivation sketch and the lazy-training regime in depth, and critically evaluate the NTK framework's limits as an explanation of deep learning's success. | Understand, Analyze, Evaluate |
| CLO3 | Explain the mean-field limit of wide neural networks, connect and contrast it with the NTK limit, and analyze signal propagation in the mean-field regime. | Understand, Analyze |
| CLO4 | Derive the max-margin implicit bias of gradient descent on linearly separable data from first principles, and critically survey implicit-bias research in nonlinear and deep settings. | Apply, Analyze, Evaluate |
| CLO5 | Analyze how finite-width networks escape the NTK/lazy regime into feature learning, and evaluate why this matters for explaining deep learning's empirical success. | Analyze, Evaluate |
| CLO6 | Analyze the empirical sharpness-generalization relationship, including its debated reliability, and evaluate Sharpness-Aware Minimization as a concrete research response. | Analyze, Evaluate |
| CLO7 | Critically evaluate competing hypotheses for the grokking phenomenon against the available evidence, correctly stating what each hypothesis does and does not establish. | Analyze, Evaluate |
| CLO8 | Explain the empirical form of neural scaling laws and evaluate theoretical and statistical-physics attempts (replica method, spin-glass analogies) to explain the loss landscape and why scaling laws hold. | Understand, Analyze, Evaluate |
| CLO9 | Rigorously analyze the full double-descent curve across model size, sample size, and training time, and evaluate modern PAC-Bayes generalization bounds against classical VC/Rademacher theory. | Analyze, Evaluate |
| CLO10 | Analyze loss-landscape geometry via mode connectivity, and critically evaluate current refinements of and critiques of the Lottery Ticket Hypothesis. | Analyze, Evaluate |
| CLO11 | Formulate an original research question in neural-network theory, survey 5+ related papers, propose a novel approach, and construct a feasibility argument or preliminary result; design, write, and orally defend a PhD-qualifying-exam-style research proposal under questioning. | Evaluate, Create |

### Bloom's Taxonomy progression across the semester

| Phase | Weeks | Dominant Bloom's Levels | Focus |
|---|---|---|---|
| Orientation, NTK, Mean-Field & Implicit Bias | 1–5 | Remember, Understand, Analyze | Frontier landscape and proposal scoping (previewed early); NTK revisited in depth; mean-field theory; implicit bias; feature learning beyond the kernel regime |
| Sharpness, Grokking, Scaling & Statistical Physics | 6–9 | Analyze, Evaluate | Sharpness-generalization and SAM; grokking; scaling laws; midterm; replica-method/spin-glass approaches |
| Double Descent, Generalization Bounds & Landscape Geometry | 10–12 | Analyze, Evaluate | Double descent revisited rigorously; PAC-Bayes bounds; mode connectivity; the Lottery Ticket Hypothesis revisited |
| Research Methods, Open Problems & Capstone | 13–16 | Evaluate, Create | Reading frontier theory papers; current open problems (flagged as fast-moving); proposal drafting, peer feedback, and defense |

Note the same deliberate skew as the sibling postgraduate courses: **Evaluate** and **Create**
dominate from the midpoint of the semester onward, and the capstone (CLO11, Evaluate/Create) is
weighted accordingly — postgraduate work is judged chiefly on the ability to formulate and scope
original research, not on recall or routine application.

## 5. Weekly Topic Overview (16 Weeks)

| Week | Topic | Bloom's Focus |
|---|---|---|
| 1 | Course overview: rapid review of assumed graduate foundations (stated as assumed, not re-taught); the landscape of open problems in neural-network-theory research; how to scope a research proposal (introduced early) | Remember, Understand |
| 2 | The Neural Tangent Kernel revisited in depth: the infinite-width-limit derivation sketch; the "lazy training" regime; critiques and limits of NTK as an explanation of deep learning's success | Understand, Analyze |
| 3 | Mean-field theory of neural networks: the mean-field limit as width→∞ (conceptual, contrasted with the NTK limit); signal propagation analysis in the mean-field regime | Understand, Analyze |
| 4 | Implicit regularization and the implicit bias of gradient descent: the linear-model max-margin case, derived; survey of implicit bias in nonlinear/deep settings | Apply, Analyze |
| 5 | Feature learning beyond the kernel regime: how finite-width networks escape NTK/lazy dynamics; why this matters for explaining deep learning's success | Analyze, Evaluate |
| 6 | Sharpness and generalization: flat vs. sharp minima; the empirical sharpness-generalization correlation and its debated reliability; Sharpness-Aware Minimization (SAM) | Analyze, Evaluate |
| 7 | The grokking phenomenon: delayed generalization long after training accuracy saturates; current competing hypotheses for why it occurs | Analyze, Evaluate |
| 8 | Scaling laws: empirical power-law relationships between model size/dataset size/compute and loss; theoretical attempts to explain why scaling laws hold; midterm review | Understand, Analyze |
| 9 | **Midterm Exam** + Statistical-physics approaches to neural network theory: the replica-method idea (conceptual); spin-glass analogies for loss-landscape structure | Understand, Analyze |
| 10 | Double descent revisited rigorously: the full curve across model size, sample size, and training time/epochs; theoretical explanations proposed | Analyze, Evaluate |
| 11 | Modern generalization bounds: why classical VC/Rademacher-style bounds are often vacuous for deep networks; PAC-Bayes bounds as a tighter alternative framework | Analyze, Evaluate |
| 12 | Loss-landscape geometry: mode connectivity (linear and nonlinear low-loss paths between independently trained minima); the Lottery Ticket Hypothesis revisited with current refinements and critiques | Analyze, Evaluate |
| 13 | Research methods for theoretical ML at the postgraduate level: reading cutting-edge theory papers; identifying genuinely open problems vs. incremental questions; structured capstone work time | Analyze, Evaluate |
| 14 | Current open problems survey (grounded, explicitly flagged as a fast-moving area): 2–3 currently active unsolved questions in neural network theory | Understand, Evaluate |
| 15 | Research proposal work session: drafting and refining the capstone research proposal; peer feedback on proposal drafts | Apply, Create |
| 16 | Capstone research proposal presentations + course review | Evaluate, Create |
| 17 | Final Exam Week | — |

## 6. Assessment Plan

| Component | Weight | Notes |
|---|---|---|
| Lab Work (weekly) | 10% | Graded notebooks/exercises, Labs 1–15 |
| Assignments (2 problem sets) | 10% | Tied to Weeks 5 and 10 |
| Quizzes (6, best 5 counted) | 10% | Short, in-class/online, 15 min each |
| Paper Critique & Presentation | 10% | Week 13 research-skills assignment; written critique + in-class presentation of a current theoretical ML paper |
| Midterm Exam | 10% | Week 9, qualifying-exam style, covers Weeks 1–8 |
| Research Proposal Capstone | 40% | Problem statement, 5+ paper related-work survey, proposed novel approach, feasibility argument/preliminary results, written proposal, and oral defense |
| Final Exam | 10% | Comprehensive, emphasis on Weeks 9–14 |

**Rationale for the postgraduate weighting.** As in the sibling postgraduate courses, weight shifts
decisively toward independent research. Exams (Midterm + Final) total only 20%, and weekly Labs
are 10% — postgraduate lab sessions are shorter and more exploratory, and the back half of the
semester increasingly folds lab time into proposal-development work. The Research Proposal
Capstone, at 40%, is by far the largest single component and is deliberately judged like a
thesis-proposal defense: a sound problem statement, a genuine survey of the related work, a
defensible proposed approach, and an honest feasibility argument — not exam recall or a fixed,
instructor-specified assignment. The components total exactly 100% (10 + 10 + 10 + 10 + 10 + 40 +
10 = 100).

## 7. Grading Policy

Standard letter grading per institutional policy (e.g., A ≥ 85, B ≥ 70, C ≥ 55, D ≥ 40, F < 40;
adjust to institution). Late submissions: −10% per day up to 3 days, then not accepted unless
documented emergency. Capstone milestones (problem-statement check-in Week 8, research-methods
workshop Week 13, draft proposal Week 15, final proposal + defense Week 16) have fixed deadlines
because of the downstream defense schedule; late capstone milestones are handled case-by-case with
the instructor.

## 8. Tools & Software

- Python 3.10+, pip/conda
- Jupyter Notebook / Google Colab
- PyTorch (primary library from Week 1 — wide-network experiments, autograd-based NTK/Jacobian
  computation, training-dynamics and landscape-geometry experiments)
- NumPy, Matplotlib
- Git/GitHub for lab, assignment, and capstone submission

## 9. Reference Textbooks and Reading

- Goodfellow, I., Bengio, Y., & Courville, A. — *Deep Learning*. MIT Press. (background reference
  only, for material this course assumes from the graduate course; this course does not re-teach
  its chapters).
- **Current theoretical ML papers from NeurIPS, ICML, and ICLR are the primary reading material in
  this course, from Week 2 onward.** This is standard postgraduate practice in a fast-moving
  research area; the instructor selects and refreshes the specific reading list each offering
  rather than this syllabus naming a fixed set that would quickly date. Students are expected to
  locate, read, and critically evaluate current primary sources as a core postgraduate skill.
- Several results are named accurately by author/idea because they are well-established, settled
  foundational results that this course explicitly does not re-derive in full technical detail but
  builds on and critiques: Jacot, Gabriel, and Hongler's Neural Tangent Kernel framework; Frankle
  and Carbin's Lottery Ticket Hypothesis; Nakkiran et al.'s rigorous empirical characterization of
  deep double descent; Power et al.'s observation and naming of the grokking phenomenon; Kaplan et
  al.'s empirical neural scaling laws, and Hoffmann et al.'s compute-optimal refinement of them.
  Students are expected to locate and read each primary paper themselves as part of the
  corresponding week's work and the capstone, rather than rely on a secondary citation given here;
  where this syllabus is not fully confident of an exact year, venue, or identifier, it states the
  result by idea and author only rather than guess at a citation detail.
- Official PyTorch and NumPy documentation.

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
