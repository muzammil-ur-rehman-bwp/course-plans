# Course Contents: Advanced Artificial Neural Network (Post Graduate)

Detailed per-week breakdown of topics, subtopics, and resources. Companion to `course-plan.md`.
Each week lists: **Topics**, **Subtopics/Skills**, **Readings**, **Software/Libraries used**.

---

## Week 1 — Course Overview: The Neural-Network-Theory Research Frontier
- **Topics:** How this course differs from the graduate *Artificial Neural Network* course (that
  course taught universal approximation, autodiff formalism, initialization/normalization theory,
  optimization-landscape theory, classical PAC/VC/Rademacher generalization theory, and a first
  survey of NTK/Lottery-Ticket/information-bottleneck ideas; this course assumes every one of
  those as settled background and studies several of the same topics — NTK chief among them — in
  much greater depth, plus genuinely new territory); a rapid, non-re-derived review checklist of
  the assumed graduate foundations; the landscape of open problems in current neural-network-
  theory research this course will cover (NTK in depth, mean-field theory, implicit bias, feature
  learning, sharpness/SAM, grokking, scaling laws, statistical-physics approaches, double descent,
  PAC-Bayes, mode connectivity, the Lottery Ticket Hypothesis revisited); how to scope a research
  proposal, introduced early as a four-part structure (problem statement, related-work survey,
  proposed approach, feasibility argument) that frames the whole semester; explicit scoping against
  the sibling postgraduate courses (Advanced Machine Learning, Advanced Deep Learning, Advanced
  Knowledge Representation and Reasoning).
- **Subtopics/Skills:** self-assessing prerequisite fluency against the graduate-course checklist;
  setting up the course's PyTorch-first environment; reading a short theoretical-research landscape
  map; drafting a first research-interest paragraph.
- **Readings:** students re-skim their own graduate-course notes on NTK, the Lottery Ticket
  Hypothesis, and information bottleneck (no new reading assigned); a short instructor-provided
  landscape map of this course's topics and their place in current theoretical ML research.
- **Software:** Python 3.10+, PyTorch, NumPy, Matplotlib.

## Week 2 — The Neural Tangent Kernel Revisited in Depth
- **Topics:** Recap of the graduate course's NTK definition (the kernel as an inner product of
  parameter-gradients) as a one-sentence pointer, not a re-derivation; the infinite-width-limit
  derivation sketch in depth — why, under the NTK parameterization, the network's output is
  (to leading order) linear in a first-order Taylor expansion of the parameters around
  initialization, why this linearization becomes exact as width → ∞, and why the induced kernel
  consequently stays (approximately) constant through training; the **lazy training** regime
  named precisely (training dynamics that stay within a small neighborhood of initialization in
  parameter space, so the linearization remains valid) and its scaling-with-width intuition;
  sharper critiques of NTK as a complete account of deep learning's success — the fixed-kernel
  regime involves no feature learning, NTK-regime generalization bounds are often far looser than
  observed finite-width generalization, and realistic finite-width networks are empirically shown
  to move substantially outside the lazy regime during training.
- **Subtopics/Skills:** numerically verifying how little parameters move relative to their
  initialization scale as width grows (the empirical signature of lazy training); computing an
  empirical NTK at two different widths and comparing kernel drift during training between them.
- **Readings:** Jacot, Gabriel, and Hongler's Neural Tangent Kernel paper (students locate and read
  at least the abstract, introduction, and main-result statement; already introduced at a survey
  level in the graduate course, read again here for the infinite-width argument's structure).
- **Software:** PyTorch, Matplotlib.

## Week 3 — Mean-Field Theory of Neural Networks
- **Topics:** The mean-field limit as width → ∞: rather than tracking a kernel of pairwise
  gradient inner products (the NTK view), the mean-field view tracks the empirical distribution of
  a layer's weights/pre-activations and studies how that distribution evolves; why both limits are
  valid infinite-width idealizations of the same object (a wide network) under different scaling
  conventions, and the key conceptual contrast: the mean-field limit, under its own natural scaling,
  can admit genuine feature learning (the distribution of a hidden unit's incoming weights moves by
  an amount comparable to its own scale), while the (separately-scaled) NTK limit by construction
  freezes the kernel; signal propagation analysis in the mean-field regime, connecting back
  explicitly to the graduate course's initialization-theory week (Xavier/He variance-preservation
  arguments as the *linear*, small-signal special case of the same propagation analysis mean-field
  theory studies more generally, including through nonlinearities and at finite but large depth).
- **Subtopics/Skills:** simulating pre-activation variance/correlation propagation through a wide
  random network's depth and comparing the empirical mean-field prediction to the graduate course's
  Xavier/He formulas in the regime where they should agree; a small experiment contrasting
  parameter movement under NTK-style versus mean-field-style parameterization scalings.
- **Readings:** a conceptual treatment of the mean-field limit of wide neural networks and its
  relationship to the NTK limit; students connect this week explicitly to their own graduate-course
  notes on initialization theory.
- **Software:** PyTorch, NumPy, Matplotlib.
- **Quiz 1** (NTK revisited + mean-field theory).

## Week 4 — Implicit Regularization and the Implicit Bias of Gradient Descent
- **Topics:** Why a model class with many global minima of the training loss still has gradient
  descent converge to one *particular* minimum — the central implicit-regularization question; the
  simple linear-model case worked rigorously: for logistic-regression-style loss on linearly
  separable data, unregularized gradient descent, run long enough, converges (in direction) to the
  max-margin (hard-SVM) solution, derived via the loss's exponential tail and a normalized-margin
  monotonicity argument; why this result is surprising (gradient descent was given no explicit
  margin objective, yet implicitly seeks one); a survey of implicit-bias research extending this
  picture to nonlinear and deep settings (e.g., bias toward low-rank or simple solutions in certain
  matrix-factorization and deep-linear-network settings; the much harder, only partially understood
  status of implicit bias in general deep nonlinear networks), presented as an active research area
  rather than a settled one outside the linear case.
- **Subtopics/Skills:** implementing gradient descent on a separable synthetic dataset and tracking
  the normalized weight vector's convergence toward the max-margin direction found by an exact
  margin solver; visualizing the decision boundary's convergence to the max-margin boundary.
- **Readings:** a conceptual/rigorous treatment of the max-margin implicit bias of gradient descent
  on separable data; a survey-level reading on implicit bias in deep/nonlinear settings.
- **Software:** PyTorch, NumPy, Matplotlib.

## Week 5 — Feature Learning Beyond the Kernel Regime
- **Topics:** How finite-width networks escape the NTK/lazy regime: the mechanisms by which
  parameters move far enough from initialization (relative to their own scale) that the
  first-order/frozen-kernel approximation breaks down, and the network's internal representations
  — not just its final linear readout — adapt to the data; why this matters: feature learning is
  widely believed to be central to why realistic, finite-width networks generalize as well as they
  do in practice, a question the (by-construction feature-free) NTK regime cannot address; concrete
  empirical signatures distinguishing lazy/kernel-regime training from feature-learning-regime
  training (kernel drift over training, hidden-representation alignment with task structure,
  width-dependence of the gap between kernel-regression and actually-trained performance).
- **Subtopics/Skills:** the course's central empirical comparison — training networks at a range of
  widths on the same task, measuring kernel drift and the kernel-regression-vs-trained-network
  performance gap at each width, and showing the gap shrinks as width grows (consistent with the
  NTK limit) while performance at *practical*, finite widths often exceeds the kernel-regression
  baseline (consistent with feature learning mattering at those widths).
- **Readings:** a conceptual/current-literature treatment of feature learning versus the kernel
  (lazy) regime, connecting back to Weeks 2–3's NTK and mean-field material.
- **Software:** PyTorch, Matplotlib.
- **Assignment 1 assigned** (NTK in depth, mean-field theory, implicit bias, feature learning —
  covering Weeks 2–5).

## Week 6 — Sharpness and Generalization
- **Topics:** Flat versus sharp minima: the intuition that a minimum whose loss rises slowly under
  parameter perturbation ("flat") should generalize better than one whose loss rises sharply
  ("sharp"), because a flat minimum's prediction is robust to the inevitable mismatch between the
  training-sample loss landscape and the true population loss landscape; how sharpness is measured
  in practice (Hessian spectral norm/top eigenvalues, or a perturbation-based proxy); the empirical
  sharpness-generalization correlation and its **debated reliability** — sharpness as conventionally
  measured is sensitive to reparameterization (e.g., simple rescalings of a ReLU network's weights
  can change measured sharpness without changing the function it computes), which has been used to
  argue the naive flat-minima story is incomplete or measurement-dependent rather than a clean causal
  account; **Sharpness-Aware Minimization (SAM)** introduced as a concrete, research-driven response
  — a training method that explicitly minimizes a worst-case loss over a small neighborhood of each
  parameter point, derived as a min-max objective with a first-order approximation yielding a
  practical two-step gradient update.
- **Subtopics/Skills:** implementing a Hessian-top-eigenvalue estimator (power iteration using
  Hessian-vector products) to measure sharpness at a found minimum; implementing SAM's two-step
  update and comparing a SAM-trained versus SGD-trained model's measured sharpness and test accuracy
  on the same task.
- **Readings:** a conceptual/current-literature treatment of the sharpness-generalization
  relationship, including its reparameterization critique, and of Sharpness-Aware Minimization.
- **Software:** PyTorch, Matplotlib.
- **Quiz 2** (sharpness, generalization, and SAM).

## Week 7 — The Grokking Phenomenon
- **Topics:** Power et al.'s observation and naming of **grokking**: on certain algorithmic tasks
  (e.g., modular arithmetic), a network can reach ~100% training accuracy almost immediately via
  memorization, with test accuracy remaining near chance for a long subsequent stretch of training,
  before test accuracy then rises sharply to near-100% — generalization arriving long after training
  loss has already saturated; why this is theoretically puzzling (the classical story that training
  and test performance should track each other once training loss is near zero is directly
  violated, and the delay can be very large in optimizer-step count); current competing hypotheses
  presented honestly as competing and not fully settled — a "slow implicit-regularization" story
  (weight decay or an implicit bias slowly driving the memorizing solution toward a qualitatively
  different, generalizing one, e.g., with distinctive structure in weight-space norm or
  representation geometry), a "circuit formation" story (a generalizing computational circuit
  co-exists with or is slowly amplified relative to a memorizing one), and the empirical role of
  dataset size and weight decay strength in controlling whether and when grokking occurs.
- **Subtopics/Skills:** critically reading and precisely restating the grokking phenomenon and each
  competing hypothesis's actual claim versus what evidence has and has not established for it.
- **Readings:** Power et al.'s paper describing and naming the grokking phenomenon (read for its
  central empirical observation); a survey of the competing explanatory hypotheses, presented
  explicitly as an open research question.
- **Software:** none required this week (critical-writing/discussion week; an optional, ungraded
  small modular-arithmetic reproduction is suggested for students who want to see the effect).

## Week 8 — Scaling Laws; Midterm Review
- **Topics:** The empirical power-law relationships between model size, dataset size, and compute
  and the resulting loss, broadly attributed to Kaplan et al.'s large-scale empirical study (loss
  decreasing as a power law in each of model size, data size, and compute when the others are not
  the bottleneck); the compute-optimal refinement broadly attributed to Hoffmann et al. (given a
  fixed compute budget, model size and data size should be scaled together rather than scaling
  model size alone, correcting an earlier compute-allocation assumption); theoretical attempts to
  explain *why* scaling laws hold at all (data-manifold/intrinsic-dimension arguments, and
  random-feature/kernel-theoretic arguments connecting back to this course's own NTK and mean-field
  material), presented honestly as partial explanations of an empirically robust but
  theoretically only partially understood phenomenon; review of Weeks 1–8 for the midterm.
- **Subtopics/Skills:** fitting a power-law curve (log-loss vs. log-model-size) to a provided or
  small reproduced loss-vs-size dataset and reading off the fitted scaling exponent; review
  exercises spanning Weeks 1–8.
- **Readings:** Kaplan et al.'s empirical scaling-laws paper and Hoffmann et al.'s compute-optimal
  refinement (read for their central empirical claims and fitted functional form, not for exact
  citation detail); a short theoretical-explanation survey.
- **Software:** PyTorch, NumPy, Matplotlib.

## Week 9 — Midterm Exam; Statistical-Physics Approaches to Neural Network Theory
- **Topics:** Midterm Exam (covers Weeks 1–8, qualifying-exam style — emphasis on explaining and
  critiquing, not just stating, each week's central result). Afterward: statistical-physics
  approaches to neural network theory, presented conceptually without the full physics derivation —
  the **replica method** idea (a technique, borrowed from the statistical physics of disordered
  systems, for computing an average over random problem instances — here, random data or random
  initialization — by analytically continuing a trick that replaces a hard disorder-average with
  several coupled, non-disordered copies of the system); **spin-glass analogies** for loss-landscape
  structure (modeling a high-dimensional, highly non-convex loss landscape's critical points using
  the same random-matrix/complexity arguments developed for spin-glass energy landscapes in
  physics, which is one historical root of the graduate course's "saddle points dominate over bad
  local minima" argument, revisited here at its statistical-physics source rather than only its
  random-matrix-theory consequence).
- **Subtopics/Skills:** critically reading a conceptual explanation of the replica method and the
  spin-glass analogy, and precisely stating what each borrowed physics argument does and does not
  establish about real, finite neural networks (which are not literally disordered physical systems
  and do not exactly satisfy the idealized assumptions the physics analysis requires).
- **Readings:** a conceptual survey of statistical-physics approaches to neural-network loss
  landscapes (replica method, spin-glass analogies), presented as an illuminating analogy with
  known limits rather than a literal physical theory of real networks.
- **Software:** none required this week (critical-writing/discussion week).

## Week 10 — Double Descent Revisited Rigorously
- **Topics:** The full double-descent curve, revisited well past the graduate course's conceptual
  introduction: test error as a function of model capacity shows the classical U-shape, then rises
  near the **interpolation threshold** (where the model exactly has enough capacity to fit the
  training data), then descends again into the overparameterized regime — and, crucially, this same
  qualitative shape appears along **three distinct axes**: model size (the classical presentation),
  sample size (for fixed model size, more training data can sometimes transiently *worsen* test
  error near a sample-size-dependent interpolation threshold), and training time/epochs (a model can
  pass through a high-test-error regime partway through training before continuing to improve, an
  "epoch-wise" double descent); Nakkiran et al.'s work establishing this rigorous, broader empirical
  characterization; theoretical explanations that have been proposed (effective-model-capacity
  accounts tied to the interpolation threshold itself rather than to raw parameter count, and
  connections to this course's own bias-variance-breakdown and random-matrix-theoretic material),
  presented as genuine explanations of the phenomenon's *location* rather than a complete first-
  principles theory of its *magnitude* in every setting.
- **Subtopics/Skills:** reproducing a double-descent curve along at least one axis (model-size
  double descent is the most tractable to reproduce at small scale) on a controlled dataset, and
  precisely identifying the interpolation threshold in the resulting curve.
- **Readings:** Nakkiran et al.'s paper rigorously characterizing deep double descent (read for its
  central empirical claims across the three axes).
- **Software:** PyTorch, NumPy, Matplotlib.
- **Assignment 2 assigned** (sharpness/SAM, grokking, scaling laws, statistical-physics approaches,
  double descent — covering Weeks 6–10).

## Week 11 — Modern Generalization Bounds: PAC-Bayes
- **Topics:** Why the graduate course's classical VC-dimension and Rademacher-complexity bounds are
  frequently **vacuous** (numerically far larger than 1, i.e., worse than a trivial bound) when
  applied to real, overparameterized deep networks, despite those networks visibly generalizing
  well in practice — revisiting precisely why this gap motivated the rest of modern generalization
  theory; **PAC-Bayes bounds** introduced conceptually as a tighter alternative framework — rather
  than bounding worst-case generalization over an entire hypothesis class, a PAC-Bayes bound
  compares a learned *posterior* distribution over hypotheses to a fixed, data-independent *prior*,
  and bounds expected generalization error by (roughly) the KL-divergence between posterior and
  prior, divided by sample size, plus a confidence term — a bound that can be numerically
  non-vacuous for deep networks when the posterior is suitably concentrated near a wide/flat region
  the prior already favors, connecting PAC-Bayes explicitly back to Week 6's sharpness material.
- **Subtopics/Skills:** stating and evaluating the PAC-Bayes bound's structure (prior, posterior,
  KL term, confidence term) on a small concrete example; computing a simple PAC-Bayes-style bound
  for a small trained network under a Gaussian-perturbation posterior and comparing its numerical
  value to a classical VC-style bound's (vacuous) value on the same network.
- **Readings:** a conceptual treatment of PAC-Bayes generalization bounds and why they can be
  non-vacuous where classical bounds are not, including their connection to flat minima.
- **Software:** PyTorch, NumPy.

## Week 12 — Loss-Landscape Geometry: Mode Connectivity; The Lottery Ticket Hypothesis Revisited
- **Topics:** **Mode connectivity**: the empirical finding that two independently-trained minima of
  a neural network's loss, which a naive straight-line (linear) interpolation in parameter space
  usually shows to be separated by a high-loss barrier, can nonetheless be connected by a simple
  *nonlinear* low-loss path (e.g., a piecewise-linear path through one or two learned bend points) —
  suggesting the loss landscape's many apparent minima are, in a precise geometric sense, part of a
  single connected low-loss manifold rather than isolated basins; what this does and does not imply
  about landscape "flatness" or the number of distinct solutions a network effectively has. The
  **Lottery Ticket Hypothesis revisited**: recapping Frankle and Carbin's original claim (a sparse
  "winning ticket" subnetwork, trained in isolation from its original initialization, can match the
  full network's accuracy) as a one-paragraph pointer to the graduate course, then surveying current
  refinements and critiques from the literature — evidence that winning tickets are substantially
  harder to find reliably at the scale and learning-rate regimes used for the largest modern
  networks, the "linear mode connectivity" refinement connecting winning tickets back to this week's
  mode-connectivity material (a winning ticket's training trajectory often stays linearly connected
  to its final solution in a way a randomly reinitialized run does not), and open questions about
  how directly the pruning-based evidence for the hypothesis should be read as evidence about an
  "already there at initialization" sparse subnetwork versus an artifact of the iterative pruning
  procedure itself.
- **Subtopics/Skills:** implementing a linear-interpolation-between-minima experiment (train two
  networks from different random initializations, interpolate their weights linearly, and plot
  loss along the interpolation path to observe the barrier); implementing a simple nonlinear
  (quadratic Bezier or piecewise-linear bend-point) path-finding procedure and showing it finds a
  substantially lower-loss path than the naive linear interpolation.
- **Readings:** a current-literature treatment of mode connectivity; a current-literature survey of
  refinements and critiques of the Lottery Ticket Hypothesis (students locate at least one recent
  paper extending or critiquing Frankle & Carbin's original claim).
- **Software:** PyTorch, Matplotlib.
- **Quiz 5** (mode connectivity and the Lottery Ticket Hypothesis revisited) — note: Quizzes 3 and 4
  fall in Weeks 8 and 10 respectively (see those weeks).

## Week 13 — Research Methods for Theoretical ML at the Postgraduate Level
- **Topics:** How to read a cutting-edge theory paper efficiently and critically (identifying the
  precise claim, the assumptions — often an idealized limit such as infinite width or a specific
  data distribution — the claim depends on, the strength of the supporting theoretical argument or
  empirical evidence, and its stated and unstated limitations); how to distinguish a genuinely open
  research problem from an incremental question already well-addressed by existing work (a
  precision test: could a knowledgeable reader state, after reading only the proposed problem
  statement, what evidence would resolve it, and is that evidence already available in the
  literature this course or the student's own survey has covered?); writing a precise, falsifiable
  problem statement and a related-work survey that synthesizes rather than merely lists sources, and
  constructing a rigorous feasibility argument (what must be true, the main technical risk named
  honestly, and why that risk is judged manageable) — the same four-part proposal standard
  previewed in Week 1, now treated in full; structured in-class capstone work time.
- **Subtopics/Skills:** critiquing a provided short theoretical ML paper excerpt as a guided
  in-class exercise; drafting a one-paragraph problem statement for the student's own capstone
  candidate topic and subjecting it to the precision test via structured peer critique; annotating
  5+ candidate related-work papers using the synthesize-don't-just-list standard.
- **Readings:** none new; students apply the critique framework to their own chosen capstone papers.
- **Software:** whatever the student's capstone topic requires (typically PyTorch).
- **Paper Critique & Presentation assigned** (due Week 15).

## Week 14 — Current Open Problems Survey
- **Topics:** A grounded survey of 2–3 currently active, unsolved questions in neural-network
  theory relevant to this course — for example: whether a unified theory can explain feature
  learning's benefits beyond the NTK/mean-field limits in a way that yields practically predictive
  (not just qualitative) guidance; whether the sharpness-generalization relationship or PAC-Bayes
  bounds can be made simultaneously tight, reparameterization-invariant, and practically computable
  at scale; and whether grokking-style delayed generalization and double descent share a common
  underlying mechanism or are better understood as distinct phenomena that happen to produce
  superficially similar non-monotonic curves — explicitly flagged as a fast-moving area where the
  specific state of open-ness may date quickly, and where the instructor refreshes the exact set of
  questions each offering rather than this syllabus fixing them permanently.
- **Subtopics/Skills:** for each surveyed open question, precisely stating what would count as
  progress or resolution, and what the strongest current partial answer actually establishes versus
  what it is sometimes informally taken to establish.
- **Readings:** instructor-curated current papers/preprints representing the frontier of each
  surveyed open question (refreshed each offering).
- **Software:** none required this week (critical-writing/discussion week).
- **Quiz 6** (open problems survey + research-methods standards from Week 13).

## Week 15 — Research Proposal Work Session
- **Topics:** Structured work time for drafting and refining the capstone research proposal, with
  instructor consultation; a thesis-committee-style structured peer-review workshop on complete
  proposal drafts, applying the Week 13 precision test and survey-synthesis standard to a peer's
  draft and producing specific, actionable feedback; producing a written revision plan in response
  to feedback received.
- **Subtopics/Skills:** giving and receiving structured, specific peer feedback on a problem
  statement's precision, a survey's synthesis quality, a proposed approach's novelty, and a
  feasibility argument's honesty about risk; revising one's own draft in response.
- **Readings:** none new; peers' own draft proposals serve as this week's material.
- **Software:** whatever the student's capstone topic requires.
- **Paper Critique & Presentation due. Draft capstone proposal due** (full written draft, all four
  required components).

## Week 16 — Capstone Research Proposal Presentations; Course Review
- **Topics:** Student capstone research-proposal presentations, delivered and defended in a
  qualifying-exam/thesis-proposal-defense format (problem statement, related work, proposed
  approach, feasibility argument or preliminary results, anticipated risks, committee-style Q&A);
  recap of the course map (NTK revisited → mean-field theory → implicit bias → feature learning →
  sharpness/SAM → grokking → scaling laws → statistical-physics approaches → double descent revisited
  → PAC-Bayes → mode connectivity/Lottery-Ticket-revisited → research methods → open problems);
  discussion of where this course's theory leads next (the sibling postgraduate courses, and current
  theoretical ML research venues such as NeurIPS, ICML, and ICLR).
- **Deliverable:** Final written research proposal + oral defense.

## Week 17 — Final Exam Week
- Comprehensive final exam, weighted toward Weeks 9–14 content (per Assessment Plan).

---

## Research Proposal Capstone (introduced Week 1 overview; problem-statement check-in Week 8;
## research-methods workshop Week 13; draft due Week 15; final proposal + defense Week 16)
This capstone is a **research proposal**, not a completed project — a PhD-qualifying-exam-style
deliverable evaluated the way a thesis-proposal committee evaluates a candidate's proposal before
the dissertation work has happened. Each student formulates an original, falsifiable research
question connected to one of this course's topics (NTK/mean-field/implicit bias/feature learning;
sharpness and SAM; grokking; scaling laws; statistical-physics approaches; double descent; PAC-Bayes
bounds; mode connectivity; the Lottery Ticket Hypothesis revisited) or to adjacent territory with
instructor approval, conducts a related-work survey of 5+ papers that synthesizes rather than lists,
proposes a novel approach or extension of their own formulation, and provides either preliminary
results from a small pilot or a rigorous feasibility argument (what must be true, the main technical
risk named honestly, and why that risk is judged manageable). Results — including negative or
partial ones — must be reported honestly and analyzed soundly; see `assignments/capstone-rubric.md`
and `assignments/capstone-proposal-guidelines.md`.
