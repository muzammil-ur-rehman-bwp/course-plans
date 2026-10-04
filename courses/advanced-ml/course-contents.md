# Course Contents: Advanced Machine Learning (Post Graduate)

Detailed per-week breakdown of topics, subtopics, and resources. Companion to `course-plan.md`.
Each week lists: **Topics**, **Subtopics/Skills**, **Readings**, **Software/Libraries used**.

---

## Week 1 — Postgraduate Overview: The Statistical-Learning-Theory Research Frontier
- **Topics:** The research-frontier landscape this course covers — minimax lower bounds and
  high-dimensional statistics, full-information online convex optimization, nonparametric
  Bayesian methods, causal inference in depth, and trustworthy/robust statistical learning
  (distribution shift, fairness, robust statistics) — and how it is disjoint from the sibling
  postgraduate courses, in particular *Advanced Artificial Intelligence*'s bandit/partial-information
  regret theory and multi-agent/game-theoretic territory; a rapid review of the graduate-ML
  foundations this course assumes (PAC/VC/Rademacher theory, concentration inequalities, convex
  optimization/KKT, kernel methods/RKHS, ensemble theory, Bayesian ML, CRFs, dimensionality
  reduction theory, the conceptual causal-inference introduction, model-selection theory) —
  stated explicitly as assumed background, not re-taught; how to scope a research proposal,
  introduced in Week 1 because it drives every week's framing for the rest of the semester.
- **Subtopics/Skills:** self-assessing fluency in each assumed graduate-ML topic against a
  diagnostic checklist; stating, in one sentence each, what a problem statement, a related-work
  survey, a proposed approach, and a feasibility argument are, ahead of the full treatment in
  Week 13; precisely distinguishing this course's online-learning week (full information) from
  the sibling course's bandit week (partial information).
- **Readings:** Wainwright, Preface and Ch. 1 (framing, for the high-dimensional-statistics arc);
  Hazan, Ch. 1 (the online convex optimization protocol, previewed).
- **Software:** Python 3.10+, Jupyter/Colab, NumPy (environment setup only).

## Week 2 — Minimax Lower Bounds
- **Topics:** The minimax risk framework: for a parameter class $\Theta$ and an estimator
  $\hat\theta$ built from $n$ samples, the minimax risk
  $M_n = \inf_{\hat\theta}\sup_{\theta\in\Theta}\mathbb{E}_\theta[d(\hat\theta,\theta)]$ as the
  worst-case risk of the best possible estimator; why upper bounds (an estimator's analyzed risk)
  and lower bounds (no estimator can do better) are both needed to certify an estimator is
  rate-optimal; Fano's inequality (statement: for a Markov chain $\theta \to X \to \hat\theta$
  with $\theta$ uniform over a finite set of $M$ hypotheses, if an estimator can identify $\theta$
  with error probability $\leq \delta$, then $I(\theta; X) \geq (1-\delta)\log M - \log 2$) and the
  information-theoretic intuition (distinguishing among many well-separated hypotheses requires
  enough mutual information between the data and the hypothesis index); constructing a packing set
  of hypotheses and bounding the mutual information via Kullback-Leibler divergence as the standard
  three-step recipe (reduction to testing, packing construction, information bound); a fully worked
  example lower-bounding the minimax risk for estimating the mean of a Gaussian location family.
- **Subtopics/Skills:** stating Fano's inequality precisely and identifying each of its three
  conditions; constructing a simple finite packing set for a Gaussian location family and applying
  the reduction-to-testing argument; reproducing the $\Omega(1/\sqrt n)$ minimax lower bound for
  estimating a Gaussian mean and matching it against the sample-mean estimator's known
  $O(1/\sqrt n)$ upper bound to certify rate-optimality.
- **Readings:** Wainwright, Ch. 15 (minimax lower bounds via metric entropy and Fano's method).
- **Software:** NumPy, SciPy (`scipy.stats`), Matplotlib.

## Week 3 — High-Dimensional Statistics I: Concentration Beyond Hoeffding
- **Topics:** Why bounded-random-variable concentration (Hoeffding, assumed from the graduate
  course) is too restrictive for high-dimensional statistics; sub-Gaussian random variables
  (definition: $X$ is sub-Gaussian with parameter $\sigma$ if
  $\mathbb{E}[e^{\lambda(X-\mathbb{E}X)}] \leq e^{\lambda^2\sigma^2/2}$ for all $\lambda$,
  equivalently Gaussian-like tail decay $\mathbb{P}(|X-\mathbb{E}X|\geq t)\leq 2e^{-t^2/2\sigma^2}$)
  and why they strictly generalize bounded variables (Hoeffding's lemma shows every bounded
  variable is sub-Gaussian) and Gaussian variables themselves; sub-exponential random variables
  (definition: tails that decay like $e^{-t/\tau}$ beyond a crossover point, the correct model for
  variables such as squares of Gaussians or sums of squares, i.e. $\chi^2$-type quantities) and why
  a single sub-Gaussian-style bound cannot capture their heavier tails; Bernstein's inequality for
  sub-exponential variables (statement and the two-regime — Gaussian-like near the mean,
  exponential in the tail — structure of the bound) and its derivation via a Chernoff/moment-generating-
  function argument analogous to, but more delicate than, Hoeffding's.
- **Subtopics/Skills:** verifying the sub-Gaussian moment-generating-function bound for a bounded
  and for a standard Gaussian random variable; deriving Bernstein's inequality's two-regime form
  from a bound on the moment-generating function of a sub-exponential variable; simulating sums of
  sub-Gaussian and sub-exponential variables and empirically confirming the respective tail bounds
  hold, including the Bernstein bound's crossover between its sub-Gaussian-like and purely
  exponential regimes.
- **Readings:** Wainwright, Ch. 2 (sub-Gaussian and sub-exponential variables, Bernstein's
  inequality).
- **Software:** NumPy, SciPy (`scipy.stats`), Matplotlib.

## Week 4 — High-Dimensional Statistics II: Random Matrix Theory Basics
- **Topics:** Why classical (fixed-dimension, $n\to\infty$) covariance estimation theory breaks
  down when the feature dimension $p$ is comparable to, or exceeds, the sample size $n$; the
  Marchenko–Pastur law (statement, conceptual derivation sketch): for a $p\times n$ data matrix
  of i.i.d. mean-zero, unit-variance entries with $p/n \to \gamma \in (0,\infty)$, the empirical
  spectral distribution of the sample covariance matrix $\hat\Sigma = \frac{1}{n}XX^\top$
  converges to a deterministic limiting distribution supported on
  $[(1-\sqrt\gamma)^2, (1+\sqrt\gamma)^2]$ (for $\gamma \leq 1$) rather than concentrating at 1 (the
  true covariance's eigenvalue), with an explicit density formula; why this means sample-covariance
  eigenvalues are systematically spread out and biased even when the true population covariance is
  the identity — the single most important cautionary fact for any method (PCA, whitening,
  portfolio optimization, LDA) that trusts a high-dimensional sample covariance matrix's
  eigenstructure at face value; the qualitative behavior as $\gamma \to 1$ (the smallest eigenvalue
  approaches zero — the sample covariance becomes nearly singular) and as $\gamma \to 0$ (recovering
  the classical fixed-$p$ regime).
- **Subtopics/Skills:** simulating the empirical spectral distribution of $\hat\Sigma$ for
  increasing $\gamma = p/n$ and overlaying the Marchenko–Pastur density to confirm convergence;
  explaining concretely, for a scenario with $p \approx n$, why the top sample eigenvalues of an
  identity-covariance dataset substantially overstate the true leading variance (a direct, correct
  cautionary consequence for naively applying graduate-course PCA in the $p \approx n$ regime).
- **Readings:** Wainwright, Ch. 6 (random matrix theory and covariance estimation, selected
  sections for the Marchenko–Pastur law).
- **Software:** NumPy, SciPy (`scipy.linalg.eigh`), Matplotlib.
- **Quiz 1** (Week 2 content).
- **Assignment 1 assigned** (minimax lower bounds, high-dimensional statistics, Weeks 2–4).

## Week 5 — Full-Information Online Convex Optimization
- **Topics:** The online convex optimization (OCO) protocol: at each round $t$, the learner
  chooses $x_t$ from a convex feasible set $\mathcal{K}$, and *then* an adversary reveals a convex
  loss function $f_t:\mathcal{K}\to\mathbb{R}$ in full — the learner observes the entire function
  (and hence can compute $f_t$ at any point, in particular its gradient at $x_t$), not merely the
  scalar loss of its own choice; this is the **full-information** setting, explicitly contrasted
  with the sibling postgraduate AI course's bandit setting, where only the realized loss (or
  reward) of the chosen action is observed and the loss function's shape elsewhere is never seen;
  regret $\mathrm{Regret}_T = \sum_t f_t(x_t) - \min_{x\in\mathcal{K}}\sum_t f_t(x)$ as the
  performance measure (matching the definition used for multiplicative weights, now for a
  continuous convex decision set); Follow-The-Regularized-Leader (FTRL): play
  $x_{t+1}=\arg\min_{x\in\mathcal{K}}\big(\eta\sum_{s\leq t} f_s(x) + R(x)\big)$ for a strongly
  convex regularizer $R$; online gradient descent (OGD) as a first-order approximation of FTRL
  using a linearized loss $\langle \nabla f_t(x_t), x\rangle$ in place of $f_t(x)$, with update
  $x_{t+1} = \Pi_{\mathcal{K}}(x_t - \eta_t \nabla f_t(x_t))$; deriving OGD's
  $O(\sqrt T)$ regret bound for convex, bounded-gradient losses over a bounded feasible set, step by
  step, via the standard projection non-expansiveness + telescoping argument.
- **Subtopics/Skills:** stating the OCO protocol and regret precisely, and articulating in one
  paragraph exactly what information the full-information setting reveals that the bandit setting
  does not; deriving the OGD regret bound $O(\sqrt T)$ with the optimized, time-varying step size
  $\eta_t = \Theta(1/\sqrt t)$; implementing OGD from scratch on a synthetic sequence of convex
  losses and empirically verifying sublinear, $\sqrt T$-shaped regret growth; implementing FTRL
  with a simple quadratic regularizer and comparing its empirical regret to OGD's on the same loss
  sequence.
- **Readings:** Hazan, Ch. 2–3 (OCO protocol, OGD, FTRL, and their regret bounds).
- **Software:** NumPy, Matplotlib.

## Week 6 — Nonparametric Bayesian Methods I: The Dirichlet Process
- **Topics:** Motivation — parametric Bayesian mixture models (e.g., a finite Gaussian mixture)
  require fixing the number of components $K$ in advance; the Dirichlet process (DP) as a
  distribution over probability distributions (a prior over an infinite-dimensional object) that
  lets the effective number of mixture components be inferred from data; the DP's defining
  property — for a base distribution $H$ and concentration parameter $\alpha>0$, a draw
  $G\sim\mathrm{DP}(\alpha,H)$ satisfies that for every finite partition $A_1,\dots,A_k$ of the
  sample space, $(G(A_1),\dots,G(A_k)) \sim \mathrm{Dirichlet}(\alpha H(A_1),\dots,\alpha H(A_k))$;
  the stick-breaking construction (Sethuraman's representation), derived and explained: draw
  $\beta_k \sim \mathrm{Beta}(1,\alpha)$ i.i.d. for $k=1,2,\dots$, set the weights
  $\pi_k = \beta_k \prod_{j<k}(1-\beta_j)$ (breaking off a $\beta_k$-fraction of the remaining
  "stick" at each step, so $\sum_k \pi_k = 1$ almost surely), draw atom locations
  $\theta_k \sim H$ i.i.d., and set $G = \sum_{k=1}^\infty \pi_k \delta_{\theta_k}$ — a concrete,
  almost-surely-discrete random measure with exactly the DP's defining property; the role of
  $\alpha$ in controlling how quickly the stick-breaking weights decay (small $\alpha$: a few atoms
  dominate; large $\alpha$: many atoms carry comparable weight, and $G$ approaches $H$).
- **Subtopics/Skills:** verifying by simulation that stick-breaking weights sum to 1 and that their
  decay rate changes correctly with $\alpha$; implementing a truncated stick-breaking sampler and
  drawing samples from $G$ for a Gaussian base measure $H$; explaining, in one paragraph, why the DP
  is the natural infinite-dimensional generalization of a Dirichlet distribution.
- **Readings:** Course notes on the Dirichlet process and Sethuraman's stick-breaking
  representation (standard graduate Bayesian nonparametrics treatment); Hazan, Ch. 1 (closing
  pointer only, no overlap in content).
- **Software:** NumPy, SciPy (`scipy.stats`), Matplotlib.
- **Quiz 2** (Weeks 3–4 content).

## Week 7 — Nonparametric Bayesian Methods II: The Chinese Restaurant Process
- **Topics:** The Chinese Restaurant Process (CRP) as the equivalent combinatorial (exchangeable
  partition) view of the Dirichlet process, obtained by integrating out $G$: customer $n+1$ (the
  $(n+1)$-th data point) joins an existing table (cluster) $k$ with probability
  $n_k/(n+\alpha)$ (proportional to that table's current occupancy $n_k$) or starts a new table
  with probability $\alpha/(n+\alpha)$; the exchangeability of the resulting random partition (the
  distribution over partitions does not depend on the order data arrived in) and why this matters
  for valid Bayesian inference from an arbitrarily ordered dataset; the CRP's expected number of
  occupied tables after $n$ customers grows as $O(\alpha \log n)$ — the formal sense in which a DP
  mixture's effective number of clusters grows (slowly) with more data rather than being fixed;
  the DP mixture model built from the CRP: each table $k$ is assigned a parameter
  $\theta_k \sim H$, and each customer at table $k$ generates its observation from
  $F(\cdot \mid \theta_k)$ — a fully specified infinite (but, for any finite $n$, almost surely
  finite-component) mixture model; a sketch of Gibbs-sampling-style inference (reassigning each
  point to an existing or new table conditional on all others) as the standard computational
  approach, at the conceptual level.
- **Subtopics/Skills:** implementing a CRP sampler and verifying empirically that table-occupancy
  probabilities match $n_k/(n+\alpha)$ and $\alpha/(n+\alpha)$; simulating the growth of the number
  of occupied tables as $n$ increases for several $\alpha$ values and comparing the empirical growth
  rate to the $O(\alpha\log n)$ prediction; building a small CRP-based infinite Gaussian mixture
  generative sampler and visualizing generated cluster structure for two different $\alpha$ values.
- **Readings:** Course notes on the Chinese Restaurant Process and its equivalence to the Dirichlet
  process via de Finetti-style exchangeability (standard graduate Bayesian nonparametrics
  treatment).
- **Software:** NumPy, Matplotlib.
- **Quiz 3** (Weeks 5–6 content).
- **Assignment 2 assigned** (full-information OCO, Dirichlet process/CRP, Weeks 5–7).

## Week 8 — Causal Inference in Depth I: Potential Outcomes and Propensity Scores; Midterm Review
- **Topics:** The potential-outcomes (Rubin causal model) framework, built rigorously on top of
  the graduate course's conceptual-only confounding/do-notation introduction: for a binary
  treatment $T\in\{0,1\}$, each unit $i$ has two potential outcomes $Y_i(1)$ and $Y_i(0)$, only one
  of which is ever observed ($Y_i = T_i Y_i(1) + (1-T_i)Y_i(0)$, the "fundamental problem of causal
  inference"); the individual treatment effect $Y_i(1)-Y_i(0)$ versus the estimable average
  treatment effect $\mathrm{ATE}=\mathbb{E}[Y(1)-Y(0)]$; formalizing confounding precisely as a
  violation of $T \perp (Y(0),Y(1))$ (treatment assignment correlated with potential outcomes), and
  the two standard assumptions that license causal estimation from observational data —
  unconfoundedness/conditional ignorability ($T \perp (Y(0),Y(1)) \mid X$ given observed covariates
  $X$) and overlap/positivity ($0 < \mathbb{P}(T=1\mid X=x) < 1$ for all $x$); the propensity score
  $e(x) = \mathbb{P}(T=1\mid X=x)$ and inverse-propensity weighting (IPW) as an ATE estimator,
  derived: $\mathbb{E}\big[\frac{T Y}{e(X)} - \frac{(1-T)Y}{1-e(X)}\big] = \mathrm{ATE}$ under
  unconfoundedness and overlap, with the derivation shown step by step via the tower property of
  expectation; midterm review session for Weeks 1–8.
- **Subtopics/Skills:** stating the fundamental problem of causal inference and the
  unconfoundedness/overlap assumptions precisely; deriving the IPW-ATE identity from the tower
  property; implementing propensity-score estimation (e.g., logistic regression) and IPW-based ATE
  estimation on simulated observational data with a known, injected confounder and ground-truth
  ATE, and verifying the IPW estimate recovers the true ATE while the naive difference-in-means
  estimate is biased by the confounder.
- **Readings:** Pearl, Ch. 1 and Ch. 3 (structural models and the potential-outcomes/structural
  correspondence); course notes on propensity-score methods (standard graduate causal-inference
  treatment, e.g., Rosenbaum & Rubin's propensity-score framework, discussed conceptually).
- **Software:** NumPy, pandas, scikit-learn (logistic regression for propensity estimation, cross-
  check only), Matplotlib.
- **Capstone problem-statement check-in.**

## Week 9 — Midterm Exam; Causal Inference in Depth II: Instrumental Variables and Do-Calculus
- **Topics:** Midterm Exam (covers Weeks 1–8). Afterward: instrumental variables (IV) — when
  unconfoundedness fails (an unobserved confounder affects both treatment and outcome), an
  instrument $Z$ that (i) affects treatment $T$ (relevance), (ii) affects the outcome $Y$ only
  through $T$ (exclusion restriction), and (iii) is itself independent of the unobserved
  confounder, identifies the treatment effect for compliers; the core identification argument for
  the linear IV/two-stage-least-squares case, derived:
  $\mathrm{Cov}(Z,Y)=\beta\,\mathrm{Cov}(Z,T)$ under the exclusion restriction, so
  $\beta = \mathrm{Cov}(Z,Y)/\mathrm{Cov}(Z,T)$ identifies the causal effect $\beta$ even though
  $T$ is confounded; the do-calculus (Pearl), building rigorously on the graduate course's
  conceptual-only $P(Y\mid\mathrm{do}(X))$ introduction: the three do-calculus rules precisely
  stated for a causal DAG $G$ with disjoint node sets $X,Y,Z,W$ — **Rule 1** (insertion/deletion of
  observations): $P(y\mid \mathrm{do}(x),z,w)=P(y\mid\mathrm{do}(x),w)$ if $Y\perp Z \mid X,W$ in
  $G_{\overline X}$ (the graph with edges into $X$ removed); **Rule 2** (action/observation
  exchange): $P(y\mid\mathrm{do}(x),\mathrm{do}(z),w)=P(y\mid\mathrm{do}(x),z,w)$ if
  $Y\perp Z\mid X,W$ in $G_{\overline X \underline Z}$ (edges into $X$ and out of $Z$ removed);
  **Rule 3** (insertion/deletion of actions): $P(y\mid\mathrm{do}(x),\mathrm{do}(z),w)=
  P(y\mid\mathrm{do}(x),w)$ if $Y\perp Z\mid X,W$ in $G_{\overline{X}\,\overline{Z(W)}}$ (edges into
  $X$ and into $Z$ not an ancestor of $W$ removed) — and why the rules are sound (each corresponds
  to a graph-surgery argument about $d$-separation in the mutilated graph implied by the
  intervention) and complete (a sequence of applications of the three rules can reduce any
  identifiable causal query to a purely observational expression, when identification is possible
  at all); a worked example applying the rules to identify $P(y\mid\mathrm{do}(x))$ in a simple
  confounded graph (recovering the familiar adjustment formula) and a worked example of a graph
  where the query is *not* identifiable by any rule sequence (illustrating that do-calculus also
  certifies non-identifiability, not only identifiability).
- **Subtopics/Skills:** stating the three IV assumptions and deriving the IV/two-stage-least-squares
  identification formula; implementing a from-scratch two-stage-least-squares IV estimator on
  simulated data with a known unobserved confounder and verifying it recovers the true effect while
  ordinary least squares does not; applying the three do-calculus rules by hand to two small graphs
  (one identifiable, one not) and justifying each rule application by the corresponding
  $d$-separation condition in the correct mutilated graph.
- **Readings:** Pearl, Ch. 3 (instrumental variables) and Ch. 3.4/Ch. 4 (the do-calculus rules and
  their soundness/completeness, at the level of statement and worked application).
- **Software:** NumPy, SciPy (`scipy.stats`), pandas.
- **Quiz 4** (Weeks 7–8 content).

## Week 10 — Distribution Shift and Domain Adaptation
- **Topics:** Covariate shift formalized: training distribution $P_{\mathrm{tr}}(x,y)$ and test
  distribution $P_{\mathrm{te}}(x,y)$ that share the same conditional $P(y\mid x)$ but differ in the
  marginal $P(x)$ (contrasted with label shift and the fully general, unidentifiable-without-
  assumptions case of arbitrary distribution shift); importance weighting as the correction:
  reweighting each training point by $w(x)=P_{\mathrm{te}}(x)/P_{\mathrm{tr}}(x)$ makes the
  weighted training risk an unbiased estimator of the test risk, derived directly from the
  definition of expectation
  ($\mathbb{E}_{P_{\mathrm{te}}}[\ell(f(x),y)] = \mathbb{E}_{P_{\mathrm{tr}}}[w(x)\,\ell(f(x),y)]$);
  practical estimation of the density ratio $w(x)$ (e.g., via a probabilistic classifier
  distinguishing training from test covariates, using the standard
  $w(x) = \frac{P(\mathrm{te}\mid x)}{P(\mathrm{tr}\mid x)}\cdot\frac{P(\mathrm{tr})}{P(\mathrm{te})}$
  identity); the generalization-bound picture under shift — a PAC-style bound (building on the
  graduate course's Rademacher/VC toolkit) degrades by a divergence term between
  $P_{\mathrm{tr}}(x)$ and $P_{\mathrm{te}}(x)$ (e.g., in terms of a suitable $f$-divergence or
  integral probability metric), making precise why "no free lunch" holds for arbitrary shift and
  why covariate shift with bounded, well-estimated importance weights is the tractable special
  case; the practical failure mode of importance weighting when $w(x)$ has high or infinite
  variance (near-zero $P_{\mathrm{tr}}(x)$ support where $P_{\mathrm{te}}(x)$ is not negligible) and
  why this is a structural limit on domain adaptation guarantees, not just an implementation detail.
- **Subtopics/Skills:** deriving the importance-weighted risk identity from first principles;
  estimating density-ratio weights via a classifier-based estimator on simulated shifted data and
  verifying that weighted empirical risk tracks true test risk more closely than unweighted
  training risk; constructing a deliberately pathological shift scenario with poor overlap and
  empirically demonstrating the resulting high-variance weight blow-up and its effect on estimator
  quality.
- **Readings:** Wainwright, Ch. 2–3 (concentration tools reused for the generalization-under-shift
  bound); course notes on covariate shift and importance weighting (standard graduate domain-
  adaptation treatment).
- **Software:** NumPy, pandas, scikit-learn (classifier-based density-ratio estimation, cross-check
  only), Matplotlib.
- **Quiz 5** (Weeks 9 content).

## Week 11 — Algorithmic Fairness
- **Topics:** Formal fairness criteria for a binary classifier/score $\hat Y$ predicting outcome
  $Y$ with a protected attribute $A$: **demographic (statistical) parity**,
  $\mathbb{P}(\hat Y=1\mid A=a)$ equal across groups $a$; **equalized odds**,
  $\mathbb{P}(\hat Y=1\mid Y=y,A=a)$ equal across groups for each $y\in\{0,1\}$ (equal true-positive
  and false-positive rates across groups); **calibration (within groups)**,
  $\mathbb{P}(Y=1\mid \hat Y=s, A=a) = s$ for every score value $s$ and group $a$ (a predicted score
  means the same thing regardless of group); the impossibility result (Chouldechova 2017;
  Kleinberg, Mullainathan & Raghavan 2016, stated accurately as the standard
  calibration/balance-error-rate incompatibility result): except in degenerate cases (equal base
  rates $\mathbb{P}(Y=1\mid A=a)$ across groups, or a perfect classifier), calibration and equalized
  odds (equivalently, calibration and balance for the positive/negative class) cannot all be
  satisfied simultaneously when base rates differ across groups — stated and proved for the simple
  case via a direct algebraic argument relating group base rates, calibration, and the false-
  positive/false-negative rates; why this is a genuine mathematical incompatibility (not a
  fixable engineering gap) whenever groups have different base rates and the classifier is
  imperfect, and the practical implication that fairness-criterion selection is a value-laden,
  context-dependent choice, not a purely technical optimization.
- **Subtopics/Skills:** computing demographic parity, equalized-odds, and calibration metrics for
  a classifier on simulated data with two groups and different base rates; empirically
  demonstrating the impossibility result by showing that enforcing calibration on such data
  provably leaves a measurable equalized-odds gap (and vice versa), and relating the size of the
  gap algebraically to the base-rate difference; articulating, for a concrete scenario (e.g., a
  risk-assessment score), which fairness criterion is being prioritized and what is explicitly
  being traded away.
- **Readings:** Course notes on formal fairness criteria and the calibration/equalized-odds
  impossibility result (standard graduate/postgraduate algorithmic-fairness treatment, following
  Chouldechova and Kleinberg–Mullainathan–Raghavan's results, discussed at the level of statement
  and the base-rate-driven algebraic argument, without claiming a specific page/edition citation).
- **Software:** NumPy, pandas, Matplotlib.

## Week 12 — Robust Statistics
- **Topics:** The classical sample mean's catastrophic sensitivity to even a single arbitrarily
  large outlier (breakdown point $1/n \to 0$), motivating robust mean estimation; the trimmed-mean
  estimator (discard the top and bottom $\epsilon$-fraction of order statistics and average the
  rest) and its breakdown point $\epsilon$; the median-of-means estimator — partition $n$ samples
  into $k$ groups, compute each group's mean, and return the median of the $k$ group means —
  derived and analyzed: for i.i.d. data with finite variance $\sigma^2$, median-of-means with
  $k = O(\log(1/\delta))$ groups achieves error $O(\sigma\sqrt{k/n})$ with probability $1-\delta$,
  a *sub-Gaussian-type* confidence interval even when the underlying data is only known to have
  finite variance (heavy-tailed), strictly better than the sample mean's Chebyshev-only guarantee in
  this regime — the argument: each group mean concentrates around the true mean via Chebyshev with
  constant probability $\geq 3/4$ for a suitable group size, and a Chernoff/Hoeffding argument on the
  binary "group mean is close" indicator across the $k$ independent groups boosts this to
  $1-\delta$ confidence via a majority-vote argument; adversarial contamination — when an
  $\epsilon$-fraction of the $n$ samples is adversarially corrupted (not just heavy-tailed), the
  trimmed mean and median-of-means remain bounded-error ($O(\sigma\sqrt\epsilon)$-type guarantees)
  while the sample mean's error is unbounded; a conceptual connection to adversarial robustness in
  modern ML — adversarial examples and data poisoning are both, at a structural level, a
  worst-case-contamination problem, and robust-statistics-style estimators (trimmed/ winsorized
  losses, median-of-means gradient estimation) are an active line of provably robust learning
  research, discussed at the conceptual level (not a full adversarial-ML treatment, which is out
  of scope).
- **Subtopics/Skills:** deriving the median-of-means error bound via the group-concentration-plus-
  majority-vote argument; implementing the trimmed mean and median-of-means estimators from scratch
  and empirically comparing their error against the sample mean under (a) heavy-tailed
  (e.g., Pareto/Cauchy-adjacent finite-variance) data and (b) adversarial contamination injected at
  a known rate $\epsilon$; articulating, in one paragraph, the structural analogy between
  statistical outlier-contamination robustness and adversarial-example robustness.
- **Readings:** Wainwright, Ch. 2 (tail bounds reused), with course notes on robust mean estimation
  (median-of-means, trimmed mean; standard graduate/postgraduate robust-statistics treatment,
  following the Lugosi–Mendelson-style median-of-means analysis, discussed at the level of
  statement and derivation without claiming a specific page/edition citation).
- **Software:** NumPy, SciPy (`scipy.stats`), Matplotlib.
- **Quiz 6** (Weeks 10–11 content).

## Week 13 — Research Methods for Statistical Learning Theory at the Postgraduate Level
- **Topics:** How to read a cutting-edge statistical-learning-theory paper: identify the precise
  claimed result (an upper bound? a lower bound? both, matching rates?), the assumptions it depends
  on (distributional, boundedness, independence), and whether the proof or experiments actually
  support the claim at the stated generality; writing a precise, falsifiable problem statement (the
  precision test: could a knowledgeable reader state, after reading only the statement, what
  evidence would resolve it?); related-work survey standards (accurately represent each paper's
  claim and finding, relate the papers to each other, use the survey to motivate a specific stated
  gap); constructing a rigorous feasibility argument (what must be true for an approach to work,
  the single most likely failure mode named honestly, and a reasoned case for why that risk is
  manageable) as a substitute for a full experiment; how postgraduate evaluation differs from the
  graduate standard — a research proposal is judged on the soundness of a plan for research not yet
  completed, not on whether a fixed experiment was executed; structured in-class capstone work time
  with instructor feedback on each student's problem-statement draft.
- **Deliverable:** Paper Critique & Presentation assignment assigned (student selects a real
  statistical-learning-theory paper from a suggested-topics list and, by Week 15, submits a written
  critique plus a short in-class presentation).
- **Readings:** None assigned beyond the research-methods course notes; students begin reading
  candidate papers for their own capstone survey and for the Paper Critique assignment.
- **Software:** As needed for capstone/critique-paper work.
- **Capstone workshop: problem-statement drafting and related-work annotation.**

## Week 14 — Current Open Problems Survey
- **Topics:** A grounded survey of 2–3 currently active, unsolved (or only partially resolved)
  questions relevant to this course's topics, explicitly flagged as a fast-moving area whose
  specific frontier shifts between offerings of this course — illustrative examples of the *kind*
  of open question this week surveys (the instructor selects and refreshes the specific current
  papers each offering): (i) how tight are known minimax lower bounds for high-dimensional
  structured-estimation problems (e.g., sparse regression, low-rank matrix recovery) relative to
  the best known computationally efficient estimators — where a statistical/computational gap is
  conjectured but not proven; (ii) whether and how formal fairness criteria can be relaxed or
  made group-robust under the Week 11 impossibility result without sacrificing meaningful
  guarantees, an active area of disagreement in the fairness-ML research community; (iii) how
  robust-statistics-style estimators (Week 12) can be scaled to high-dimensional, modern-ML-scale
  learning with provable guarantees matching their classical low-dimensional rates. Each surveyed
  question is treated as genuinely open — this week models, rather than resolves, the kind of
  question a capstone proposal should target.
- **Subtopics/Skills:** for each surveyed open question, stating precisely what is known (the best
  current upper and lower bounds, or the best current partial result), what is *not* known, and why
  it remains open (a genuine technical obstruction, not merely "nobody has gotten to it yet");
  relating at least one surveyed open question to the student's own emerging capstone interest.
- **Readings:** Instructor-assigned current NeurIPS/ICML/COLT papers (refreshed each offering; see
  `course-plan.md` §9).
- **Software:** As needed for any accompanying demonstration.

## Week 15 — Research Proposal Work Session
- **Topics:** Structured, dedicated class time for drafting and refining the capstone research
  proposal's four required components (problem statement, 5+ paper related-work survey, proposed
  novel approach, feasibility argument or preliminary results); a thesis-committee-style
  peer-feedback protocol applied to full draft proposals (not just the problem statement, as in
  Week 13): each reviewer assesses whether the problem statement is precise and genuinely open,
  whether the survey correctly represents and motivates from the cited papers, whether the
  proposed approach is genuinely distinguishable from the surveyed work, and whether the
  feasibility argument honestly names and engages its main risk; producing an explicit, specific
  revision plan from the feedback received.
- **Deliverable:** Draft capstone proposal due (all four required components); Paper Critique &
  Presentation written critique due, presented in-class this week.
- **Readings:** None assigned; students work from their own draft and survey materials.
- **Software:** As needed for capstone work.

## Week 16 — Capstone Research Proposal Presentations + Course Review
- **Topics:** Student capstone research-proposal presentations, delivered and defended in a
  qualifying-exam/thesis-proposal-defense format (problem statement, related work, proposed
  approach, feasibility argument or preliminary results, anticipated risks, committee-style Q&A);
  recap of the course map (minimax lower bounds/Fano → sub-Gaussian/sub-exponential concentration
  and Bernstein's inequality → random matrix theory/Marchenko–Pastur → full-information OCO/FTRL →
  the Dirichlet process/CRP → potential outcomes/propensity scores → instrumental
  variables/do-calculus → distribution shift → algorithmic fairness → robust statistics); where
  this course's frontier statistical-learning-theory foundation connects into the sibling
  postgraduate *Advanced Artificial Intelligence*, *Advanced Artificial Neural Network*, and
  *Advanced Deep Learning* courses.
- **Deliverable:** Final capstone research-proposal submission + oral defense.

## Week 17 — Final Exam Week
- Comprehensive final exam, weighted toward Weeks 9–15 content (per Assessment Plan).

---

## Research Proposal Capstone (introduced Week 1, problem-statement check-in Week 8, workshop
Week 13, draft due Week 15, final submission + defense Week 16)
Students individually formulate an original research question in statistical learning theory
connected to one of this course's five pillars (minimax/high-dimensional statistics;
full-information online convex optimization; nonparametric Bayesian methods; causal inference in
depth; trustworthy/robust statistical learning), or to adjacent territory with instructor
approval, and complete: a precise, falsifiable problem statement; a related-work survey of 5 or
more papers that accurately represents and synthesizes the cited work and motivates a specific
stated gap; a proposed novel approach or extension that is the student's own formulation; and
either preliminary results from a small pilot or a rigorous feasibility argument (what must be
true for the approach to work, the main technical risk named honestly, and a reasoned case for why
the risk is manageable). It is evaluated as a PhD-qualifying-exam-style research proposal, not as
a completed project. See `assignments/capstone-proposal-guidelines.md` and
`assignments/capstone-rubric.md`.
