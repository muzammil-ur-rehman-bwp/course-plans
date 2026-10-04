# Assignment 2 — Online Convex Optimization & Nonparametric Bayes (Weeks 5–7)

**Weight:** 5% of course grade (one of 2 problem sets, 10% total) | **Assigned:** Week 7 |
**Due:** Start of Week 9 (before the Midterm Exam)

## Instructions
Submit a single Jupyter notebook `assignment02.ipynb` answering all questions below. Show your
work (code + a brief written explanation) for each question.

## Questions
1. **(Online gradient descent, 25 pts)** Implement `online_gradient_descent` exactly as in the
   Week 5 lecture content. Run it on a provided $d=3$, $T=1000$ sequence of convex quadratic
   losses, and plot regret against the best fixed point in hindsight. In 3–5 sentences, derive
   the optimized bound $\mathrm{Regret}_T\leq DG\sqrt T$ from the telescoping-sum argument and
   confirm your empirical regret stays below it at every round.
2. **(FTRL vs. OGD, 20 pts)** Implement `ftrl_quadratic_reg` and compare its regret to OGD's on
   the same loss sequence as Question 1. In 2–3 sentences, explain why the two algorithms' regret
   curves are similar for quadratic losses specifically, referencing the Week 5 lecture content's
   discussion of FTRL's closed form for this loss class.
3. **(Stick-breaking construction, 20 pts)** Implement the truncated stick-breaking sampler from
   the Week 6 lecture content for $\alpha\in\{1,8\}$ with $H=\mathcal N(0,16)$. For each $\alpha$,
   report the number of atoms needed to capture 95% of the total weight, and verify the weights
   sum to within $10^{-4}$ of 1 at truncation level $K=2000$.
4. **(Chinese Restaurant Process, 20 pts)** Implement the CRP sampler from the Week 7 lecture
   content for $\alpha\in\{1,8\}$ and $n=3000$. Report the observed number of occupied tables
   $K_n$ against the predicted $\alpha\ln n$, averaged over 20 independent runs per $\alpha$.
5. **(Full-information vs. bandit, 15 pts)** In 200–300 words, state precisely what additional
   information the full-information OCO protocol (Week 5) gives the learner each round compared
   to the bandit setting (as covered in the sibling *Advanced Artificial Intelligence* course),
   and explain, with reference to your Question 1 implementation, why online gradient descent's
   update rule could not be directly computed if only the realized scalar loss $f_t(x_t)$ (not
   the gradient $\nabla f_t(x_t)$) were observed each round.

## Submission
Upload `assignment02.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
