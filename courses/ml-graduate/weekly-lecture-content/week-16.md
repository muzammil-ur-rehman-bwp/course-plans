# Week 16 — Lecture Content: Capstone Research Presentations & Course Review

## 1. Capstone Presentations
The lecture/lab slots this week are given over to student capstone presentations (conference-talk
format; see `presentations/capstone-presentation-template.md`). Each presentation is followed by
Q&A from the instructor and peers, using the same critical-reading framework introduced in Week
15.

## 2. Course Map — Full Review
| Block | Weeks | Core result(s) |
|---|---|---|
| ERM framework | 1 | Risk vs. empirical risk; why unrestricted ERM overfits |
| PAC learning | 2 | $m\geq\frac{1}{2\epsilon^2}\ln\frac{|H|}{\delta}$, derived from Hoeffding + union bound |
| VC dimension | 3 | Shattering; $\mathrm{VCdim}$(halfspaces in $\mathbb{R}^d$) $=d+1$; the VC bound; Sauer–Shelah |
| Rademacher complexity | 4 | $\widehat{\mathcal{R}}_S(H)$; Massart's lemma linking it back to VC/Sauer–Shelah |
| Concentration inequalities | 5 | Markov → Chebyshev → Hoeffding (full proof) → McDiarmid |
| Convex optimization | 6 | GD rates ($O(1/T)$ convex, linear strongly-convex); Lagrangian duality; KKT |
| RKHS / kernels | 7 | Mercer's theorem; the RKHS; the representer theorem; kernel SVM link |
| Ensemble theory | 8 | AdaBoost's $\prod_t 2\sqrt{\epsilon_t(1-\epsilon_t)}$ bound; margin theory; bagging's variance formula |
| Bayesian ML I | 9 | Bayesian linear regression's closed-form posterior; ridge = MAP |
| Bayesian ML II | 10 | GP regression's predictive mean/covariance via Gaussian conditioning |
| Structured prediction | 11 | Linear-chain CRFs; discriminative vs. generative (CRF vs. HMM) |
| Dimensionality reduction | 12 | PCA's eigendecomposition optimality; kernel PCA; Isomap/t-SNE (conceptual) |
| Causal inference | 13 | Confounding; Simpson's paradox; causal DAGs; do-notation (conceptual) |
| Model selection | 14 | Bias-variance decomposition (full proof); AIC/BIC derivation sketches; CV justification |
| Research methods | 15 | Critical paper-reading framework; reproducibility; capstone work session |

## 3. How This Course Connects Onward
This course's statistical-learning-theory foundation (ERM, PAC/VC/Rademacher, concentration
inequalities) is the exact toolkit *Artificial Neural Network* (Graduate) invokes — there, only
conceptually, and specifically to explain why these classical, distribution-free bounds become
vacuous for heavily overparameterized networks (recall Week 15's exercise). This course's convex
optimization and RKHS material (Weeks 6–7) underlies the optimization-theory and
kernel-adjacent arguments used throughout ML generally. *Deep Learning* (Graduate) builds
architectures on top of the optimization foundation from Week 6. *Artificial Intelligence*
(Graduate) owns the general probabilistic-graphical-model inference algorithms that Week 11's CRF
treatment deliberately left to it.

## 4. Final Exam Preparation
The final exam (Week 17) is comprehensive, with emphasis on Weeks 9–15: Bayesian linear
regression and Gaussian Processes, Conditional Random Fields, PCA optimality and kernel PCA,
causal inference basics, and model-selection theory (bias-variance, AIC/BIC, cross-validation).
Weeks 1–8 material (the midterm's content) may still appear in conceptual, integrative questions
(e.g., "which Week 2–5 tool would you reach for to bound X, and why").

## 5. Closing Exercise
For your own capstone topic, state in one paragraph which single result from this course's 15
weeks was most load-bearing for your reproduced experiment or derivation, and why.
