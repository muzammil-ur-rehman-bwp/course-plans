# Course Plan: Machine Learning (Graduate)

## 1. Course Information

| Field | Detail |
|---|---|
| Course Title | Machine Learning |
| Level | Graduate (MS Computer Science / Software Engineering / AI) |
| Credit Hours | 3 (2 hrs lecture + 1 lab/seminar session of 3 hrs/week) |
| Prerequisites | *Introduction to Machine Learning* or equivalent (applied regression, classification, ensembles, SVMs, model evaluation, clustering/PCA, recommenders at a scikit-learn level — assumed and **not** re-taught); linear algebra (eigendecomposition, inner products, positive semi-definiteness); multivariate calculus (gradients, Lagrange multipliers); probability & statistics (random variables, expectation, variance, conditional distributions) |
| Programming Language | Python 3.x |
| Core Libraries | NumPy, SciPy (`scipy.optimize`, `scipy.linalg`), scikit-learn (for cross-checking from-scratch implementations only, not as the primary teaching vehicle), Matplotlib |
| Duration | 16 teaching weeks (1 semester) + 1 exam week |
| Delivery Mode | Lecture + Lab/Seminar (concept lecture followed by a hands-on derivation-verification or implementation session) |

## 2. Course Description

This is a rigorous, research-oriented graduate treatment of the **statistical and theoretical
foundations of machine learning**. It assumes students already know, at an applied level, how to
fit and tune regression, classification, ensemble, and kernel models in scikit-learn, how to
evaluate them, and how to engineer features and reduce dimensionality for them (as covered in
*Introduction to Machine Learning*), and it raises the study of *why these methods work, and how
well, and under what assumptions* to graduate rigor. The course builds a single coherent
statistical-learning-theory toolkit from first principles — the empirical risk minimization (ERM)
framework, PAC learnability and sample complexity, VC dimension, Rademacher complexity, and the
concentration inequalities that underlie all three — and proves or carefully sketches every bound
it states, rather than citing results conceptually. It then turns to the mathematical machinery
that makes modern ML algorithms work: convex optimization (duality, KKT conditions, convergence
rates), kernel methods and reproducing kernel Hilbert space (RKHS) theory (the representer
theorem), and rigorous ensemble-method theory (AdaBoost's training-error bound and margin-based
generalization arguments, bagging's variance-reduction argument). The second half of the course
covers Bayesian machine learning (Bayesian linear regression, Gaussian Processes), structured
prediction (Conditional Random Fields), rigorous dimensionality-reduction theory (PCA re-derived as
an optimal eigendecomposition, kernel PCA), an introduction to causal inference, and model-selection
theory (a full proof of the bias-variance decomposition, information criteria, and the theoretical
case for cross-validation), before closing with research methods and a capstone.

**This course is deliberately scoped to avoid duplicating four sibling courses.** It does **not**
re-teach applied classical ML at the level of *Introduction to Machine Learning* (that course's
regression/classification/ensemble/SVM/evaluation/clustering content is assumed background, not
repeated here — this course instead supplies the theory *behind* those methods). It does **not**
cover neural-network-specific theory such as the Universal Approximation Theorem, backpropagation
as reverse-mode automatic differentiation, initialization/normalization theory, loss-landscape
geometry, or the Neural Tangent Kernel (that is *Artificial Neural Network*, Graduate); notably,
that course treats PAC learning, VC dimension, and Rademacher complexity only *conceptually* and
only as a lens on why classical generalization theory struggles with overparameterized networks —
**this** course is where those tools are built rigorously and generally, with real definitions,
real bounds, and real proof sketches, for arbitrary hypothesis classes, before any such application.
It does **not** cover deep architectures (CNNs, RNNs, Transformers, generative models — that is
*Deep Learning*, Graduate). It does **not** cover general exact/approximate inference algorithms
for probabilistic graphical models, game theory, Markov Decision Processes/reinforcement learning,
or heuristic search (that is *Artificial Intelligence*, Graduate — this course's Conditional Random
Fields week contrasts a CRF's discriminative structured-prediction approach with the generative
joint-distribution approach only briefly, as context, and does not re-teach general Bayesian-network
inference algorithms or their complexity). Where any of these topics is relevant to an argument
here, it receives at most a one-sentence pointer to the course that owns it, never a lecture.

## 3. Goals

- Build the empirical risk minimization (ERM) framework from first principles and state precisely
  what it means for a hypothesis class to be PAC-learnable.
- Derive, step by step, the finite-hypothesis-class PAC generalization bound from Hoeffding's
  inequality and a union bound, and state and apply the VC-dimension and Rademacher-complexity
  generalization bounds, including worked VC-dimension computations.
- Master the concentration-inequality toolkit (Markov, Chebyshev, Hoeffding, McDiarmid) that
  underlies every bound above, including proof technique, not just statement.
- Apply convex optimization theory (convex sets/functions, gradient-descent convergence rates,
  Lagrangian duality, KKT conditions) to machine learning objectives.
- Derive the reproducing kernel Hilbert space (RKHS) formalism, state and explain the representer
  theorem, and connect it rigorously back to kernel support vector machines.
- Derive AdaBoost's training-error bound and margin-based generalization argument, and bagging's
  variance-reduction argument, as the first-principles theory behind ensemble methods.
- Derive Bayesian linear regression's closed-form posterior and the Gaussian Process regression
  predictive equations, and explain regularization as MAP estimation under a Gaussian prior.
- Explain Conditional Random Fields as discriminative structured prediction, distinct from a
  generative joint-distribution approach.
- Re-derive PCA rigorously as the variance-maximizing eigendecomposition (with an optimality proof
  sketch), extend it to kernel PCA, and survey nonlinear manifold learning conceptually.
- Explain confounding and the correlation/causation distinction with a concrete worked example,
  and introduce causal graphs and interventions conceptually.
- Prove the bias-variance decomposition for squared error, derive AIC/BIC, and give the theoretical
  justification for cross-validation as a generalization-risk estimator.
- Read and critique theoretical ML papers, and design, execute, and present an original small
  research-style capstone project in statistical learning theory.

## 4. Course Learning Outcomes (CLOs) — Mapped to Bloom's Taxonomy

| CLO | Statement | Bloom's Level(s) |
|---|---|---|
| CLO1 | Recall the assumed applied-ML foundation and state the ERM framework (risk, empirical risk, the ERM principle) precisely, mapping this course's scope against its sibling graduate courses. | Remember, Understand |
| CLO2 | State the PAC-learnability definition and derive the finite-hypothesis-class generalization bound from Hoeffding's inequality and a union bound. | Understand, Apply, Analyze |
| CLO3 | Define shattering and VC dimension, compute the VC dimension of standard hypothesis classes by proof, and state and apply the VC generalization bound. | Apply, Analyze |
| CLO4 | Define Rademacher complexity, derive its generalization bound, and analyze how it relates to and can improve upon VC-dimension-based bounds. | Apply, Analyze, Evaluate |
| CLO5 | Prove Hoeffding's inequality (via Markov/Chebyshev and a moment-generating-function argument) and state McDiarmid's inequality, applying both correctly within their i.i.d./boundedness assumptions. | Apply, Analyze |
| CLO6 | Derive gradient-descent convergence rates for convex and strongly-convex objectives, and derive and apply Lagrangian duality and the KKT conditions to constrained ML problems. | Apply, Analyze |
| CLO7 | State Mercer's theorem conceptually, construct the RKHS for a given kernel, state and explain the representer theorem, and connect it to kernel SVMs. | Understand, Apply, Analyze |
| CLO8 | Derive AdaBoost's training-error bound and margin-based generalization argument, and derive bagging's variance-reduction argument from the variance of an average of correlated estimators. | Apply, Analyze, Evaluate |
| CLO9 | Derive the Bayesian linear regression posterior in closed form, explain ridge regression as MAP estimation under a Gaussian prior, and derive the Gaussian Process regression predictive mean and covariance. | Apply, Analyze |
| CLO10 | Explain the discriminative structured-prediction approach of Conditional Random Fields, contrasted with generative joint models, and implement a CRF for sequence labeling. | Understand, Apply |
| CLO11 | Prove PCA's variance-maximization optimality via eigendecomposition, extend PCA to a kernelized form, and critically survey nonlinear manifold-learning methods. | Apply, Analyze, Evaluate |
| CLO12 | Distinguish correlation from causation using confounding and Simpson's paradox, and explain causal graphs and interventions conceptually. | Understand, Analyze |
| CLO13 | Prove the bias-variance decomposition for squared error, derive AIC/BIC, and justify cross-validation theoretically as a generalization-risk estimator. | Apply, Analyze, Evaluate |
| CLO14 | Critique a statistical-learning-theory research paper's claims and methodology, and design, execute, and present an original research-style capstone: literature review, a reproduced or extended experiment/derivation, a written paper, and a conference-style talk. | Analyze, Evaluate, Create |

### Bloom's Taxonomy progression across the semester

| Phase | Weeks | Dominant Bloom's Levels | Focus |
|---|---|---|---|
| Statistical Learning Theory Foundations | 1–5 | Remember, Understand, Apply, Analyze | ERM framework; PAC learning; VC dimension; Rademacher complexity; concentration inequalities |
| Optimization & Kernel Methods | 6–8 | Apply, Analyze, Evaluate | Convex optimization and KKT; RKHS and the representer theorem; rigorous ensemble theory; midterm |
| Bayesian ML & Structured Prediction | 9–11 | Apply, Analyze | Bayesian linear regression; Gaussian Processes; Conditional Random Fields |
| Dimensionality, Causality, Model Selection & Capstone | 12–16 | Analyze, Evaluate, Create | PCA optimality and kernel PCA; causal inference basics; bias-variance/AIC/BIC/cross-validation theory; research methods; capstone |

## 5. Weekly Topic Overview (16 Weeks)

| Week | Topic | Bloom's Focus |
|---|---|---|
| 1 | Graduate ML overview: the statistical learning framework (risk, empirical risk, the ERM principle), course roadmap and scope boundaries vs. sibling graduate courses | Remember, Understand |
| 2 | PAC learning in depth: PAC-learnability definition, sample complexity, the finite-hypothesis-class generalization bound (Hoeffding + union bound, derived) | Understand, Apply |
| 3 | VC dimension in depth: shattering, VC dimension definition, worked examples (linear classifiers in $\mathbb{R}^d$, intervals on the real line), the VC generalization bound, Sauer–Shelah lemma (conceptual) | Apply, Analyze |
| 4 | Rademacher complexity: definition, the Rademacher generalization bound, relation to and generalization of VC-dimension-based bounds | Apply, Analyze |
| 5 | Concentration inequalities: Markov/Chebyshev recap, Hoeffding's inequality (proof sketch), McDiarmid's bounded-differences inequality (conceptual) | Apply, Analyze |
| 6 | Convex optimization for ML: convex sets/functions, gradient-descent convergence rates for convex and strongly-convex objectives, Lagrangian duality and KKT conditions | Apply, Analyze |
| 7 | Kernel methods and RKHS theory: positive-definite kernels (Mercer's theorem, conceptual), the RKHS, the representer theorem, connection to kernel SVMs | Understand, Apply, Analyze |
| 8 | Rigorous ensemble theory: AdaBoost's training-error bound (derivation sketch), margin theory for boosting, bagging's variance-reduction argument; midterm review | Apply, Analyze |
| 9 | **Midterm Exam** + Bayesian Machine Learning I: Bayesian linear regression (prior → posterior derivation), ridge regression as MAP estimation under a Gaussian prior | Remember–Apply |
| 10 | Bayesian Machine Learning II: Gaussian Processes — the GP prior over functions, GP regression predictive equations (derived), the role of the kernel/covariance function | Apply, Analyze |
| 11 | Structured prediction: Conditional Random Fields for sequence/structured labeling, discriminative vs. generative contrast (brief) | Understand, Apply |
| 12 | Dimensionality reduction theory: PCA re-derived as the variance-maximizing eigendecomposition (optimality proof sketch), kernel PCA, nonlinear manifold learning (Isomap, t-SNE ideas, conceptual survey) | Apply, Analyze, Evaluate |
| 13 | Causal inference basics: correlation vs. causation, confounding and Simpson's paradox, causal graphs and interventions (conceptual), relevance to trustworthy ML | Understand, Analyze |
| 14 | Model selection theory: the bias-variance decomposition for squared error (full derivation), information criteria (AIC/BIC derivation sketch), the theoretical justification for cross-validation | Apply, Analyze, Evaluate |
| 15 | Research methods and project work session: reading/critiquing theoretical ML papers, reproducibility practices, structured capstone work time | Analyze, Evaluate, Create |
| 16 | Capstone research presentations + course review | Evaluate, Create |
| 17 | Final Exam Week | — |

## 6. Assessment Plan

| Component | Weight | Notes |
|---|---|---|
| Lab Work (weekly) | 15% | Graded notebooks, submitted weekly (Labs 1–15) |
| Assignments (3 problem sets) | 15% | Tied to Weeks 4, 8, 12 |
| Quizzes (6, best 5 counted) | 10% | Short, in-class/online, 15 min each |
| Paper Critique & Presentation | 10% | Week 15 research-methods assignment; critique of a statistical-learning-theory paper + in-class presentation |
| Midterm Exam | 15% | Week 9, covers Weeks 1–8 |
| Research Capstone | 25% | Literature review + reproduced/extended experiment or derivation + paper + presentation (proposal Wk 8–9, work session Wk 15, presentation Wk 16) |
| Final Exam | 10% | Comprehensive, emphasis on Weeks 9–15 |

**Rationale for the graduate weighting.** As in the sibling graduate AI and ANN courses, weight
shifts away from high-stakes closed-book exams (Midterm + Final total 25%) and toward sustained,
research-style work: a dedicated Paper Critique & Presentation component (10%) and a heavy
Research Capstone (25%) requiring a genuine literature review and a reproduced or extended
experiment or derivation, not just an implementation exercise. Labs are weighted at 15% rather
than the undergraduate course's 20%, reflecting that graduate labs are shorter, theory-verification
exercises that assume strong baseline scikit-learn/NumPy fluency from the prerequisite course.

## 7. Grading Policy

Standard letter grading per institutional policy (e.g., A ≥ 85, B ≥ 70, C ≥ 55, D ≥ 40, F < 40;
adjust to institution). Late submissions: −10% per day up to 3 days, then not accepted unless
documented emergency. Capstone milestones (proposal, draft, final submission) have fixed deadlines
because of the downstream presentation schedule; late capstone milestones are handled case-by-case
with the instructor.

## 8. Tools & Software

- Python 3.10+, pip/conda
- Jupyter Notebook / Google Colab
- NumPy and SciPy (`scipy.optimize`, `scipy.linalg`, `scipy.spatial`) for every from-scratch
  derivation-verification exercise (PAC/VC/Rademacher simulations, gradient descent, kernel
  Gram matrices, AdaBoost, Bayesian linear regression, Gaussian Process regression, PCA via
  eigendecomposition)
- scikit-learn, used sparingly and only to cross-check from-scratch results (e.g., comparing a
  from-scratch PCA against `sklearn.decomposition.PCA`, or a from-scratch kernel ridge regression
  against `sklearn.kernel_ridge`), not as the primary teaching vehicle — this course's labs build
  the algorithms students already know how to call
- Matplotlib for diagnostic plots (learning curves, margin distributions, GP posterior bands)
- Git/GitHub for lab, assignment, and capstone submission

## 9. Reference Textbooks

- Shalev-Shwartz, S. & Ben-David, S. — *Understanding Machine Learning: From Theory to
  Algorithms*. Cambridge University Press (freely available online). The canonical
  statistical-learning-theory textbook and the primary reference for the ERM framework, PAC
  learning, VC dimension, Rademacher complexity, and the concentration-inequality toolkit (Weeks
  1–5), and for the convex-optimization and boosting chapters (Weeks 6, 8).
- Bishop, C. — *Pattern Recognition and Machine Learning*. Springer. Primary reference for kernel
  methods and RKHS theory, Bayesian linear regression, and structured prediction (Weeks 7, 9, 11).
- Rasmussen, C. E. & Williams, C. K. I. — *Gaussian Processes for Machine Learning*. MIT Press
  (freely available online at gaussianprocess.org/gpml). The canonical reference for Gaussian
  Processes (Week 10).
- Official NumPy, SciPy, and scikit-learn documentation (for lab support only; this course does
  not rely on scikit-learn as its primary pedagogical vehicle).

## 10. Academic Integrity

Labs and assignments are individual unless stated otherwise. The research capstone may be done in
pairs with clearly attributed contributions. Any use of another author's ideas, text, code, or
results — including figures or results from a paper being critiqued or reproduced — must be
properly cited; uncredited reuse of a paper's text or another student's code (including uncredited
AI-generated code or text submitted as original work) is handled per institutional academic
integrity policy. Reproducing a published experiment or derivation is expected and encouraged for
the capstone; presenting someone else's reported results as one's own experimental findings is
not.
