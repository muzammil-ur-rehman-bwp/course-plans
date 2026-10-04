# Assignment 2 — Initialization, Normalization, and Optimization-Landscape Theory (Weeks 4–8)

**Weight:** 5% of course grade (one of 3 problem-set assignments, 15% total) | **Assigned:** Week
7 | **Due:** Start of Week 10

## Instructions
Submit a single Jupyter notebook `assignment02.ipynb` answering all questions below. Show your
work (derivations and/or code) for each question.

## Questions
1. **(Initialization derivation, 20 pts)** For a hidden layer with fan-in $n_{in}=200$ using the
   ELU activation (approximately linear for positive inputs, approximately $-1$ for very negative
   inputs), derive, from a variance-preservation argument analogous to Week 4's He derivation,
   what fraction of $z$'s variance you would estimate ELU's output variance to be, under the
   simplifying assumption that roughly half of pre-activations are positive and behave linearly.
   State the resulting recommended $\mathrm{Var}(W)$ and compare it numerically to He's $2/n_{in}$.
2. **(BatchNorm backward, 25 pts)** Starting from $y_i = \gamma\hat x_i+\beta$, re-derive (showing
   every step) the formula for $\bar x_i = \partial L/\partial x_i$ given $\bar y_i$, matching
   Week 5's lecture result. Then implement and finite-difference-check your derivation on a
   5-feature, 8-example synthetic batch.
3. **(Saddle points and Newton's method, 20 pts)** For $f(x,y,z) = x^2 + y^2 - z^2$, find all
   critical points, compute the Hessian, classify the critical point via its eigenvalues, and run
   both gradient descent and Newton's method from $(0.1, 0.1, 1.0)$ for 30 steps; report and plot
   both trajectories' distance to the origin over time, and explain the qualitative difference.
4. **(Adam vs. AMSGrad, 20 pts)** Using the Week 7 oscillating-gradient construction, sweep the
   spike magnitude $C \in \{2, 5, 10, 20\}$ and report, for each, whether plain Adam's final
   $\theta$ value drifts by more than $0.5$ from AMSGrad's; discuss the trend as $C$ grows.
5. **(Double descent, 15 pts)** Using the Week 8 random-feature construction, locate the
   interpolation-threshold peak's width for 3 different noise levels (`noise_std`
   $\in\{0.1, 1.0, 3.0\}$); report whether higher noise makes the peak more or less pronounced.

## Submission
Upload `assignment02.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
