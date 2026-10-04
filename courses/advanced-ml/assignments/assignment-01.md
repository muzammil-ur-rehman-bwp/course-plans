# Assignment 1 — Minimax Lower Bounds & High-Dimensional Statistics (Weeks 2–4)

**Weight:** 5% of course grade (one of 2 problem sets, 10% total) | **Assigned:** Week 4 |
**Due:** Start of Week 6

## Instructions
Submit a single Jupyter notebook `assignment01.ipynb` answering all questions below. Show your
work (code + a brief written explanation) for each question.

## Questions
1. **(Fano's inequality and minimax rates, 25 pts)** Implement `two_point_test_error(n, eps,
   n_trials, rng)` exactly as in the Week 2 lecture content. For $n\in\{30,100,300,1000\}$,
   compute the critical separation $\epsilon^\star=\sqrt{\ln 2/(4n)}$ and the empirical testing
   error at $\epsilon^\star$ and at $5\epsilon^\star$. In 3–5 sentences, explain how this
   empirically confirms the $\Omega(1/\sqrt n)$ minimax lower bound derived in lecture, and state
   what additional fact (about the sample mean's upper bound) is needed to conclude the sample
   mean is rate-optimal.
2. **(Bernstein's inequality, 20 pts)** Implement the sub-Gaussian (Rademacher) and
   sub-exponential (centered $\chi^2_1$) simulations from the Week 3 lecture content. For
   $n=300$, tabulate the empirical tail probability, the sub-Gaussian bound, and Bernstein's
   two-regime bound at $t\in\{0.2,0.5,0.8,1.2\}$ for the $\chi^2_1$ case. In 2–3 sentences,
   identify the smallest $t$ at which the sub-Gaussian bound is violated and explain why
   Bernstein's bound is not.
3. **(Random matrix theory, 25 pts)** Implement the Marchenko–Pastur simulation from the Week 4
   lecture content for $\gamma\in\{0.3,0.6,0.9\}$. For each $\gamma$, report the predicted support
   edges $a,b$ and the empirically observed minimum and maximum sample eigenvalues. In 3–5
   sentences, explain concretely why a data analyst at $\gamma=0.9$ who observes a sample
   eigenvalue near the predicted upper edge $b$ should not necessarily conclude it reflects real
   signal.
4. **(Synthesis, 30 pts)** In 250–350 words, compare the *type* of guarantee a minimax lower
   bound (Week 2) gives versus the type of guarantee the Marchenko–Pastur law (Week 4) gives:
   one is a statement about what *no estimator* can do; the other is a statement about what a
   *specific, familiar* estimator (the sample covariance) actually does in a specific regime.
   Explain, with a concrete hypothetical example, how both kinds of results could be relevant to
   critiquing a single applied high-dimensional PCA analysis.

## Submission
Upload `assignment01.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
