# Assignment 2 — Convex Optimization, Kernel/RKHS Theory, Ensemble Theory (Weeks 5–8)

**Weight:** 5% of course grade (one of 3 problem sets, 15% total) | **Assigned:** Week 8 | **Due:** Start of Week 9

## Instructions
Submit `assignment02.ipynb` (and/or PDF for proof-only questions) with full derivations and
working code for all code questions.

## Questions
1. **(Concentration inequalities, 15 pts)** Prove Hoeffding's lemma's key step: show that
   $g(u)=-pu+\ln(1-p+pe^u)$ satisfies $g''(u)\leq1/4$ for all $u$, where $p\in(0,1)$ (this is the
   step the Week 5 lecture asserted without full proof). Then state McDiarmid's inequality and
   give one concrete ML scenario (not from lecture) where applying it, rather than plain
   Hoeffding, is necessary.
2. **(Convex optimization, 20 pts)** Derive the gradient-descent linear convergence rate for a
   $\mu$-strongly-convex, $L$-smooth objective, showing the contraction-lemma step explicitly.
   Then, for $f(x)=\frac12 x^\top Ax$ with a provided matrix $A$, compute the theoretical
   iteration count to reach $f(x_T)-f^\star<10^{-8}$ and verify it empirically in code.
3. **(KKT/duality, 20 pts)** Derive the Lagrangian dual of the soft-margin SVM primal
   $\min_{w,b,\xi}\ \frac12\|w\|^2+C\sum_i\xi_i$ s.t. $y_i(w^\top x_i+b)\geq1-\xi_i,\ \xi_i\geq0$,
   showing all KKT conditions (stationarity, feasibility, complementary slackness) and the
   resulting box constraint $0\leq\alpha_i\leq C$.
4. **(RKHS / representer theorem, 20 pts)** For the polynomial kernel $k(x,x')=(x^\top x'+1)^2$ in
   $\mathbb{R}^2$, write out an explicit finite-dimensional feature map $\phi(x)$ such that
   $k(x,x')=\phi(x)^\top\phi(x')$ (verify your $\phi$ algebraically). Then implement kernel ridge
   regression from scratch using this kernel via the representer theorem, and verify it matches
   `sklearn.kernel_ridge.KernelRidge` with the equivalent polynomial kernel.
5. **(Ensemble theory, 15 pts)** Derive AdaBoost's per-round normalization constant
   $Z_t=2\sqrt{\epsilon_t(1-\epsilon_t)}$ from the definitions of $\alpha_t$ and $\epsilon_t$,
   showing every algebraic step. Then, for a provided sequence of per-round weighted errors
   $\epsilon_1,\dots,\epsilon_5$, compute the resulting upper bound on training error.
6. **(Bagging, 10 pts)** Derive the bagging variance formula
   $\mathrm{Var}(\bar h)=\sigma^2/n+\frac{n-1}{n}\rho\sigma^2$, and compute it for $\sigma^2=4$,
   $\rho=0.2$, at $n=5,50,500$; discuss the diminishing-returns pattern you observe.

## Submission
Upload `assignment02.ipynb` (and/or PDF) via the course submission system. Late policy per
syllabus (`course-plan.md` §7).
