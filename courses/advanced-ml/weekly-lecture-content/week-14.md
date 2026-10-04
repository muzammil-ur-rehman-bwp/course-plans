# Week 14 — Lecture Content: Current Open Problems Survey

## 1. A Fast-Moving Area, Explicitly Flagged
This week surveys 2–3 currently active, genuinely unsolved (or only partially resolved) questions
relevant to this course's five pillars. **The specific frontier shifts between offerings of this
course** — the instructor selects and refreshes the actual current papers each time (see
`course-plan.md` §9). What follows is the *kind* of question this week surveys, illustrated with
the structure such a question has, not a claim that any specific numeric rate below is the
literature's final word as of any particular date.

## 2. Open Question 1: Statistical–Computational Gaps in High-Dimensional Structured Estimation
For many high-dimensional structured-estimation problems (e.g., sparse linear regression, where
the true coefficient vector has only $s \ll p$ nonzero entries; low-rank matrix recovery), the
**statistical** minimax lower bound (Week 2-style: what rate is information-theoretically
achievable by *any* estimator, with unlimited computation) is often provably faster than the best
rate known to be achievable by any *computationally efficient* (polynomial-time) estimator. **What
is known:** for sparse regression, the information-theoretic minimax rate scales like
$s\log(p/s)/n$, while natural computationally efficient estimators (e.g., the Lasso) are known to
match this rate under additional structural conditions (e.g., restricted eigenvalue/incoherence
conditions on the design), but can provably require strictly more samples when those conditions
fail. **What is not known, and why it remains open:** whether a statistical–computational gap is
*fundamentally unavoidable* for natural problem variants where those structural conditions fail
(i.e., whether *every* polynomial-time estimator must pay a statistical penalty relative to the
unbounded-computation minimax rate, not merely the specific algorithms analyzed so far) is, for
many such variants, not settled — connecting this question to average-case computational
complexity (e.g., reductions from problems conjectured to be hard, such as planted clique) is an
active research technique, but a complete characterization (which problems have a true gap, and
how large) remains open for broad classes of structured-estimation problems.

## 3. Open Question 2: Relaxing Fairness Criteria Under the Impossibility Result
Week 11 established that calibration/predictive parity and equalized odds generally cannot be
simultaneously satisfied when base rates differ. **What is known:** several relaxations have been
proposed — e.g., requiring only *approximate* versions of each criterion within a tolerance
$\gamma$, or requiring the criteria to hold only on average over a specified distribution of
sub-populations, or multi-criteria objectives that explicitly trade off the two kinds of
unfairness along a Pareto frontier rather than requiring either exactly. **What is not known, and
why it remains open:** there is active disagreement in the fairness-ML research community over
which relaxation (if any) captures a normatively defensible notion of "fair enough" without
smuggling back in an arbitrary, unprincipled threshold choice, and whether a *group-robust*
relaxation (one that behaves well simultaneously across many possible ways of defining or
intersecting protected groups, not just the specific groups checked at training time) can be
achieved with meaningful statistical guarantees at all — this remains a genuinely contested
question, not merely a technical gap awaiting a known fix.

## 4. Open Question 3: Scaling Robust-Statistics Guarantees to Modern High-Dimensional Learning
Week 12's median-of-means guarantee was derived for scalar mean estimation. **What is known:**
median-of-means-style and trimmed-mean-style ideas have been extended to vector-valued mean
estimation and to some structured problems (robust covariance estimation, robust regression)
with provable guarantees, and robust-statistics-inspired training procedures (e.g., trimmed-loss
training, robust gradient aggregation) have been proposed for modern, high-dimensional learning
settings. **What is not known, and why it remains open:** whether these robust estimators can be
scaled to the dimensionality and sample sizes typical of modern large-scale learning *while
matching their classical low-dimensional statistical rates and remaining computationally
tractable* is, for many settings, an open tension — robust estimators that are statistically
optimal are often computationally expensive (e.g., requiring estimation procedures with
super-linear or worse dependence on dimension), and computationally efficient robust estimators
often pay a statistical penalty relative to the classical-regime rates from Week 12 — whether this
tradeoff is fundamental (an open question structurally similar to Open Question 1's
statistical–computational gap framing) or can be closed is actively studied and not settled.

## 5. What Makes a Question Genuinely Open
Each of the three questions above has the same shape, worth naming explicitly as a diagnostic for
evaluating *any* claimed open problem (including, eventually, a student's own capstone problem
statement): (a) a precise **best known upper bound or positive result**; (b) a precise **best
known lower bound, negative result, or partial characterization**; (c) an identified **specific
technical obstruction** explaining why (a) and (b) do not yet match — not merely "nobody has had
time to try harder," but a structural reason (a conjectured computational hardness connection, a
genuine normative disagreement, a provable tradeoff) that the gap persists. A claimed "open
problem" that lacks a clear instance of all three is usually either already resolved (reread the
literature more carefully) or not yet precisely enough stated to be a problem at all (apply the
Week 13 precision test).

## 6. In-Class/Lab Exercise
For your instructor-assigned current paper (see `lab-manuals/lab-14.md`), identify which of this
week's three open-question *shapes* (a statistical–computational gap; a normative/relaxation
tension; a statistical–computational tradeoff in robustness/scaling) the paper's own stated open
question most resembles, or explain why it does not fit any of the three and instead names a
different kind of obstruction.
