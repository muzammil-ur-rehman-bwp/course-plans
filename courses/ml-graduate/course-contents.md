# Course Contents: Machine Learning (Graduate)

Detailed per-week breakdown of topics, subtopics, and resources. Companion to `course-plan.md`.
Each week lists: **Topics**, **Subtopics/Skills**, **Readings**, **Software/Libraries used**.

---

## Week 1 — Graduate ML Overview: The Statistical Learning Framework
- **Topics:** What this course assumes (applied ML from *Introduction to Machine Learning*) and
  what it adds (the theory behind it); the statistical learning setup — an instance space
  $\mathcal{X}$, a label space $\mathcal{Y}$, an unknown data distribution $D$ over
  $\mathcal{X}\times\mathcal{Y}$, a loss function, the true risk $L_D(h)=\mathbb{E}_{(x,y)\sim D}[\ell(h(x),y)]$,
  the empirical risk $\widehat{L}_S(h)$ over a sample $S$, and the Empirical Risk Minimization
  (ERM) principle; why ERM alone can overfit (a first look at overfitting as a theory problem, not
  just a practical symptom); course roadmap; explicit scope boundaries against *Artificial
  Neural Network* (Graduate), *Artificial Intelligence* (Graduate), and the forthcoming *Deep
  Learning* (Graduate).
- **Subtopics/Skills:** stating risk and empirical risk precisely for a toy hypothesis class;
  distinguishing a learning algorithm from a hypothesis class; identifying, for a given result,
  which sibling graduate course (if any) owns it.
- **Readings:** Shalev-Shwartz & Ben-David (SSBD), Ch. 1–2.
- **Software:** Python 3.10+, NumPy, Jupyter/Colab.

## Week 2 — PAC Learning in Depth
- **Topics:** The Probably Approximately Correct (PAC) learnability definition (accuracy
  $\epsilon$, confidence $1-\delta$, sample complexity $m(\epsilon,\delta)$); the realizability
  assumption; deriving the finite-hypothesis-class generalization bound: fix a "bad" hypothesis
  (one with true risk $>\epsilon$), bound the probability it looks good on the sample via
  Hoeffding's inequality, then unionize over all $|H|$ hypotheses to bound the probability *any*
  bad hypothesis looks good; solving the resulting inequality for $m$.
- **Subtopics/Skills:** reproducing the finite-class sample-complexity bound
  $m \geq \frac{1}{2\epsilon^2}\ln\frac{2|H|}{\delta}$ step by step; applying it to toy finite
  hypothesis classes; explaining why PAC learnability requires the bound to be polynomial in
  $1/\epsilon$, $1/\delta$, and the representation size of $H$.
- **Readings:** SSBD Ch. 2–4.
- **Software:** NumPy, Matplotlib.

## Week 3 — VC Dimension in Depth
- **Topics:** Shattering a set of points; the VC dimension of a hypothesis class as the largest
  shattered set size; worked examples — linear threshold classifiers in $\mathbb{R}^d$
  ($\mathrm{VCdim}=d+1$), intervals on the real line ($\mathrm{VCdim}=2$), axis-aligned rectangles
  in $\mathbb{R}^2$; the VC generalization bound (fundamental theorem of statistical learning,
  statement); the Sauer–Shelah lemma (conceptual: it bounds the growth function
  polynomially in $m$ once $m$ exceeds the VC dimension, which is what makes the VC bound
  possible for infinite hypothesis classes).
- **Subtopics/Skills:** proving $\mathrm{VCdim}(H)=3$ for linear halfplane classifiers in
  $\mathbb{R}^2$ by exhibiting a shattered 3-point set and a case-complete argument that no 4-point
  set is shattered; proving $\mathrm{VCdim}=2$ for intervals on $\mathbb{R}$; stating the VC bound
  and identifying what breaks it (infinite VC dimension).
- **Readings:** SSBD Ch. 6.
- **Software:** NumPy, Matplotlib (brute-force shattering verification).
- **Quiz 1** (Week 2 content).

## Week 4 — Rademacher Complexity
- **Topics:** Empirical Rademacher complexity
  $\widehat{\mathcal{R}}_S(H)=\mathbb{E}_\sigma[\sup_{h\in H}\frac{1}{m}\sum_i\sigma_i h(x_i)]$ as a
  data-dependent capacity measure; the Rademacher generalization bound; why Rademacher complexity
  generalizes VC-dimension-based bounds (Massart's lemma connects the two: a finite-growth-function
  class has Rademacher complexity bounded via $\sqrt{2\log(\text{growth function})/m}$, recovering
  the VC bound's shape as a special, distribution-free case); computing Rademacher complexity for
  a simple linear class in closed form.
- **Subtopics/Skills:** computing $\widehat{\mathcal{R}}_S(H)$ by Monte Carlo over $\sigma$ draws
  for a norm-bounded linear class; explaining, in one paragraph, why a data-dependent bound can be
  tighter than a worst-case VC bound for the same class.
- **Readings:** SSBD Ch. 26 (selected); Bishop Ch. 1 (supplementary, on generalization).
- **Software:** NumPy, Matplotlib.
- **Assignment 1 assigned** (PAC learning, VC dimension, Rademacher complexity).

## Week 5 — Concentration Inequalities
- **Topics:** Markov's inequality and Chebyshev's inequality (recap, as the building blocks);
  Hoeffding's inequality for bounded, independent random variables (full proof sketch via the
  moment-generating function and Hoeffding's lemma); McDiarmid's bounded-differences inequality
  (conceptual — Hoeffding's inequality generalized to any function of independent variables whose
  value cannot change too much when one variable is swapped, which is exactly what is needed to
  bound $\sup_{h}(\widehat{L}_S(h)-L_D(h))$ as a function of the sample); why the entire Weeks 2–4
  toolkit rests on these two inequalities.
- **Subtopics/Skills:** deriving Hoeffding's inequality from the Chernoff-bounding technique;
  verifying Hoeffding's bound empirically by simulating coin flips and comparing the empirical
  tail probability against the bound; stating McDiarmid's inequality's assumptions (bounded
  differences, independence) and giving an example where applying Hoeffding's inequality to a
  non-i.i.d. sample would be invalid.
- **Readings:** SSBD Appendix B (concentration bounds).
- **Software:** NumPy, SciPy (`scipy.stats`), Matplotlib.
- **Quiz 2** (Weeks 3–4 content).

## Week 6 — Convex Optimization for ML
- **Topics:** Convex sets and convex functions; first- and second-order conditions for convexity;
  gradient descent convergence rate for convex, Lipschitz-gradient objectives ($O(1/T)$) and for
  strongly-convex objectives (linear/geometric rate), with the rate derivation; constrained
  optimization, Lagrangian duality (primal/dual problems, weak duality), and the Karush-Kuhn-Tucker
  (KKT) conditions (stationarity, primal/dual feasibility, complementary slackness) as necessary
  (and, under convexity + Slater's condition, sufficient) optimality conditions.
- **Subtopics/Skills:** implementing gradient descent and empirically verifying the $O(1/T)$ and
  linear-rate convergence predictions on synthetic convex objectives; deriving the KKT conditions
  for the SVM primal and recovering the dual formulation.
- **Readings:** SSBD Ch. 12 (convexity); Bishop Ch. 7.1 (SVM Lagrangian/KKT, for the Week 6–7
  bridge).
- **Software:** NumPy, SciPy (`scipy.optimize`), Matplotlib.

## Week 7 — Kernel Methods and RKHS Theory
- **Topics:** Positive-definite (Mercer) kernels; Mercer's theorem (conceptual — a continuous,
  symmetric, positive-definite kernel admits an eigen-expansion $k(x,x')=\sum_i\lambda_i\phi_i(x)\phi_i(x')$,
  i.e., an implicit feature map into a possibly infinite-dimensional space); constructing the
  reproducing kernel Hilbert space (RKHS) $\mathcal{H}_k$ from a kernel via the reproducing
  property $\langle f,k(x,\cdot)\rangle_{\mathcal{H}_k}=f(x)$; the representer theorem (statement:
  the minimizer of a regularized empirical risk over $\mathcal{H}_k$ is always expressible as
  $\hat f(\cdot)=\sum_{i=1}^m\alpha_i k(x_i,\cdot)$ — a finite-dimensional problem despite an
  infinite-dimensional hypothesis space) and why it matters (it is *why* kernel SVMs, kernel ridge
  regression, and Gaussian Processes are all tractable); connecting back to kernel SVMs from the
  prerequisite course with the RKHS view now rigorous.
- **Subtopics/Skills:** verifying that a given kernel matrix is positive semi-definite; deriving
  the representer-theorem-based finite-dimensional optimization for kernel ridge regression and
  implementing it from scratch; comparing the from-scratch solution against
  `sklearn.kernel_ridge.KernelRidge`.
- **Readings:** Bishop Ch. 6.1–6.2.
- **Software:** NumPy, SciPy (`scipy.linalg`), scikit-learn (cross-check only).

## Week 8 — Rigorous Ensemble Theory; Midterm Review
- **Topics:** AdaBoost's training-error bound (derivation sketch: each weak learner's weighted
  error $\epsilon_t<1/2$ drives the bound $\prod_t 2\sqrt{\epsilon_t(1-\epsilon_t)}$ on training
  error toward exponentially fast zero); margin theory for boosting (why boosting often continues
  to improve generalization even after training error hits zero — the margin distribution over
  training points keeps increasing, and a generalization bound stated in terms of the margin
  explains the resistance to overfitting that a pure training-error view cannot); bagging's
  variance-reduction argument, derived from $\mathrm{Var}(\bar X)=\frac{1}{n}\sigma^2+\frac{n-1}{n}\rho\sigma^2$
  for the average of $n$ correlated, identically distributed estimators with pairwise correlation
  $\rho$ — showing why bagging reduces variance (shrinking the first term) but cannot remove the
  correlation-driven floor $\rho\sigma^2$; review session for Weeks 1–8.
- **Subtopics/Skills:** implementing AdaBoost from scratch and plotting the training-error curve
  against the derived bound; plotting the margin distribution over training rounds; deriving the
  bagging variance formula and relating it to why Random Forests additionally decorrelate trees
  via random feature subsets.
- **Readings:** SSBD Ch. 10 (boosting).
- **Software:** NumPy, Matplotlib.
- **Assignment 2 assigned** (convex optimization, kernel/RKHS theory, ensemble theory).

## Week 9 — Midterm Exam; Bayesian Machine Learning I
- **Topics:** Midterm Exam (covers Weeks 1–8). Afterward: Bayesian linear regression — placing a
  Gaussian prior $w\sim\mathcal{N}(0,\tau^2 I)$ over weights, deriving the Gaussian posterior
  $p(w\mid X,y)$ in closed form via Bayes' rule and completing the square, and the resulting
  posterior predictive distribution; the regularization-as-MAP-estimation connection: showing that
  MAP estimation under this Gaussian prior is *exactly* ridge regression, with the prior's
  precision mapping directly to the ridge penalty $\lambda$.
- **Subtopics/Skills:** deriving the Bayesian linear regression posterior mean and covariance by
  hand; implementing Bayesian linear regression from scratch and verifying its MAP estimate
  matches `sklearn.linear_model.Ridge` for the corresponding $\lambda$.
- **Readings:** Bishop Ch. 3.3.
- **Software:** NumPy, scikit-learn (cross-check only).
- **Capstone project introduced; proposal guidelines distributed.**

## Week 10 — Bayesian Machine Learning II: Gaussian Processes
- **Topics:** The Gaussian Process (GP) as a prior over functions, specified by a mean function
  (typically zero) and a covariance (kernel) function; the defining property that any finite set
  of function values is jointly Gaussian; deriving the GP regression predictive mean and covariance
  by conditioning a joint Gaussian on observed (noisy) training outputs; the role of the
  kernel/covariance function in controlling smoothness and the role of the noise variance
  hyperparameter.
- **Subtopics/Skills:** implementing GP regression from scratch via Cholesky decomposition of the
  training-data covariance matrix; plotting the GP posterior mean and $\pm2\sigma$ credible band on
  toy 1-D data for different kernel length scales; connecting back to Week 7's RKHS view (the GP
  posterior mean is a representer-theorem-style kernel expansion).
- **Readings:** Rasmussen & Williams (GPML), Ch. 2.
- **Software:** NumPy, SciPy (`scipy.linalg.cho_factor`/`cho_solve`), Matplotlib.
- **Quiz 3** (Weeks 6–7 content, administered alongside Week 9's midterm-review cycle).

## Week 11 — Structured Prediction: Conditional Random Fields
- **Topics:** Structured prediction as predicting a whole labeled sequence/structure rather than a
  single label; Conditional Random Fields (CRFs) as a discriminative model
  $p(y\mid x)=\frac{1}{Z(x)}\exp(\sum_k \theta_k f_k(y,x))$ over a linear-chain structure for
  sequence labeling; a brief contrast between a CRF's *discriminative* approach (model $p(y\mid x)$
  directly) and a *generative* joint-distribution approach (model $p(x,y)$, as a Hidden Markov
  Model would) — this contrast is kept brief and is **not** a treatment of general Bayesian-network
  exact/approximate inference, which belongs to *Artificial Intelligence* (Graduate).
  Feature functions, the linear-chain structure, and (conceptually) the Viterbi-style decoding and
  forward-backward-style training that CRFs share with HMMs.
- **Subtopics/Skills:** fitting a linear-chain CRF (e.g., via `sklearn-crfsuite` or an equivalent
  library) to a small sequence-labeling task (e.g., part-of-speech tagging on a toy corpus) and
  inspecting learned feature weights; articulating, in one paragraph, the discriminative/generative
  distinction with the CRF/HMM pair as the concrete example.
- **Readings:** Bishop Ch. 8.1–8.2 (chain-structured graphical models, for context only).
- **Software:** Python, `sklearn-crfsuite` (or equivalent), NumPy.
- **Quiz 4** (Weeks 9–10 content).

## Week 12 — Dimensionality Reduction Theory
- **Topics:** Re-deriving PCA rigorously: the variance-maximization formulation
  ($\max_{\|u\|=1} u^\top \Sigma u$), the Lagrangian argument that the optimal directions are
  eigenvectors of the covariance matrix $\Sigma$, and the optimality proof sketch that the top-$k$
  eigenvectors maximize retained variance among all rank-$k$ orthogonal projections (via the
  Courant-Fischer / Rayleigh-quotient characterization and an inductive deflation argument); kernel
  PCA (performing PCA implicitly in an RKHS via the kernel trick, operating on the centered kernel
  (Gram) matrix); a conceptual survey of nonlinear manifold learning — Isomap (preserving
  geodesic/graph distances) and the idea behind t-SNE (preserving local neighborhood probabilities
  under a heavy-tailed embedding distribution) as methods that go beyond PCA's linear-subspace
  assumption.
- **Subtopics/Skills:** implementing PCA from scratch via eigendecomposition of the covariance
  matrix and verifying it matches `sklearn.decomposition.PCA` on the same data; implementing kernel
  PCA from scratch for an RBF kernel; explaining, for a concrete nonlinear dataset (e.g., a Swiss
  roll), why linear PCA fails where Isomap/t-SNE-style methods succeed.
- **Readings:** SSBD Ch. 23 (dimensionality reduction, PCA); Bishop Ch. 12.1, 12.3.
- **Software:** NumPy, SciPy (`scipy.linalg.eigh`), scikit-learn (cross-check and Isomap/t-SNE
  demonstration), Matplotlib.
- **Assignment 3 assigned** (Bayesian ML, GPs, dimensionality-reduction theory).

## Week 13 — Causal Inference Basics
- **Topics:** Correlation vs. causation; confounding variables and why they create spurious
  association; Simpson's paradox as a concrete, fully worked numerical example (an aggregate trend
  reversing within every subgroup once a confounder is stratified on); a conceptual introduction to
  causal graphs (directed acyclic graphs encoding assumed causal structure) and the idea of an
  intervention, introduced via Pearl's do-notation ($P(Y\mid \mathrm{do}(X=x))$ as "what would $Y$
  be if we *set* $X=x$," contrasted with the purely observational $P(Y\mid X=x)$); why this matters
  for trustworthy ML (a predictive model that only captures correlation can mislead a decision-maker
  who intends to *intervene* on a feature, and can encode and launder confounded, spurious
  patterns — including discriminatory ones — as if they were causal). This is a conceptual
  introduction, not a full causal-inference course.
- **Subtopics/Skills:** constructing a worked Simpson's-paradox numerical example (e.g., a
  treatment that looks harmful overall but helpful in every subgroup, or vice versa) and
  identifying the confounder; sketching a simple causal DAG for a given scenario and identifying
  which variable is a confounder versus a mediator.
- **Readings:** Course notes on confounding and Simpson's paradox (standard treatment, e.g., as in
  Pearl's *Causality* framework, discussed conceptually; no specific chapter assigned).
- **Software:** NumPy, pandas, Matplotlib.
- **Quiz 5** (Week 11–12 content).

## Week 14 — Model Selection Theory
- **Topics:** The bias-variance decomposition for squared error, derived in full: for a fixed test
  point, $\mathbb{E}[(y-\hat f(x))^2] = \mathrm{Bias}[\hat f(x)]^2 + \mathrm{Var}[\hat f(x)] +
  \sigma^2$ (irreducible noise), with every step of the expansion of the squared expectation shown;
  information criteria — the Akaike Information Criterion (AIC) and Bayesian Information Criterion
  (BIC), derivation sketch (AIC from an asymptotic approximation to out-of-sample
  Kullback-Leibler divergence via the log-likelihood penalized by the number of parameters; BIC from
  a Laplace approximation to the Bayesian model-evidence integral, penalizing more heavily by
  $\log n$) and how they trade off fit against complexity differently from each other; the
  theoretical justification for cross-validation as an (approximately unbiased) estimator of
  out-of-sample generalization risk, and why that justification is why Weeks 1–5's PAC/VC/Rademacher
  bounds and cross-validation are two complementary, not competing, ways of controlling the same
  generalization gap.
- **Subtopics/Skills:** reproducing the bias-variance algebra symbolically and numerically (fitting
  polynomials of increasing degree to noisy samples many times and empirically decomposing the
  mean-squared error into its bias² and variance components); computing AIC/BIC for a family of
  nested models and comparing the model each criterion selects.
- **Readings:** SSBD Ch. 5 (bias-complexity tradeoff); Bishop Ch. 1.3, 3.4 (Bayesian model
  comparison and BIC).
- **Software:** NumPy, SciPy, Matplotlib.
- **Quiz 6** (Week 13–14 content).

## Week 15 — Research Methods and Project Work Session
- **Topics:** How to read a theoretical ML paper (identify the claimed result, the assumptions it
  depends on, and whether the proof or experiments actually support the claim at the stated
  generality); reproducibility practices for theory-adjacent empirical work (reporting exact
  assumptions, random seeds, and the gap between a toy illustrative experiment and a paper's full
  claim); structured in-class capstone work time with instructor feedback.
- **Deliverable:** Paper Critique & Presentation assignment due (student selects a real
  statistical-learning-theory paper from a suggested-topics list and submits a written critique
  plus a short in-class presentation).
- **Readings:** None assigned; students read their chosen capstone/critique paper(s).
- **Software:** As needed for capstone work.

## Week 16 — Capstone Research Presentations & Course Review
- **Topics:** Student capstone project presentations; recap of the course map (ERM → PAC → VC →
  Rademacher → concentration inequalities → convex optimization → RKHS/kernels → ensemble theory →
  Bayesian ML → GPs → CRFs → dimensionality-reduction theory → causal inference → model-selection
  theory); where this course's statistical-learning-theory foundation connects into the sibling
  *Artificial Neural Network* and *Deep Learning* graduate courses.
- **Deliverable:** Capstone project final submission + presentation.

## Week 17 — Final Exam Week
- Comprehensive final exam, weighted toward Weeks 9–15 content (per Assessment Plan).

---

## Research Capstone (introduced Week 9, proposal due Week 9–10, work session Week 15, presented Week 16)
Students (individually or in pairs) choose a statistical-learning-theory subtopic from this course
and complete: a literature review of 3–5 papers on that subtopic; a small reproduced or extended
experiment or derivation (e.g., empirically verifying a generalization bound's scaling, extending
the bias-variance decomposition to a different loss, implementing and stress-testing a from-scratch
Gaussian Process on a new kernel, or reproducing a margin-based boosting generalization experiment);
a short written paper in a conference-style format; and a conference-style presentation. See
`assignments/capstone-proposal-guidelines.md` and `assignments/capstone-rubric.md`.
