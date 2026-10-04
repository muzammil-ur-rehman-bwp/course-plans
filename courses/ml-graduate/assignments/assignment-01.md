# Assignment 1 — PAC Learning, VC Dimension, Rademacher Complexity (Weeks 1–4)

**Weight:** 5% of course grade (one of 3 problem sets, 15% total) | **Assigned:** Week 4 | **Due:** Start of Week 5

## Instructions
Submit `assignment01.ipynb` (or a PDF for proof-only questions plus a notebook for code
questions) with full derivations shown, not just final answers, and working code for all code
questions.

## Questions
1. **(ERM, 10 pts)** For a hypothesis class $H$ of size $|H|=4$ over a 2-point domain, list all
   hypotheses, and for a given target concept and training sample of size $m=2$, show explicitly
   which hypotheses are ERM-consistent (zero empirical risk) and compute each one's true risk
   under a specified distribution $D$.
2. **(PAC sample complexity, 20 pts)** Re-derive the finite-class sample-complexity bound
   $m_H(\epsilon,\delta)$ from Hoeffding's inequality and a union bound, showing every algebraic
   step (do not simply cite the final formula). Then compute $m_H(\epsilon,\delta)$ for
   $|H|=10^6$, $\epsilon=0.05$, $\delta=0.01$.
3. **(VC dimension proof, 25 pts)** Prove that the VC dimension of axis-aligned rectangles in
   $\mathbb{R}^2$ (hypotheses of the form $\mathbb{1}[a_1\leq x_1\leq b_1,\ a_2\leq x_2\leq b_2]$)
   is exactly 4. Give both a shattering construction (4 points) and a complete impossibility
   argument for 5 points.
4. **(VC bound application, 15 pts)** Using the VC generalization bound, compute the sample size
   needed for a class with $\mathrm{VCdim}(H)=10$ to guarantee a generalization gap of at most
   $0.1$ with 95% confidence (state and justify any constants you use, consistent with the
   lecture's stated bound form).
5. **(Rademacher complexity, 20 pts)** Derive, from first principles (not by citation), the
   closed-form empirical Rademacher complexity bound for the norm-bounded linear class
   $H=\{x\mapsto w^\top x : \|w\|_2\leq B\}$, following the Week 4 lecture's method. Then
   implement a Monte Carlo estimator in code and verify it against your closed-form bound for
   $m\in\{50,500\}$, $B=2$, on a provided dataset.
6. **(Synthesis, 10 pts)** For the same hypothesis class, compare the VC-dimension-based bound and
   the Rademacher-complexity-based bound numerically (using the dataset from Question 5) and
   state which is tighter here and why.

## Submission
Upload `assignment01.ipynb` (and/or PDF) via the course submission system. Late policy per
syllabus (`course-plan.md` §7).
