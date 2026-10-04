# Lab Notes 4 — Max-Margin Implicit Bias of Gradient Descent

**Concept recap:** gradient descent on separable data with an exponential-tailed loss converges in
direction to the $L_2$ max-margin classifier, with norm growing like $\Theta(\log t)$.

**Common pitfalls:**
- Insufficient margin padding when constructing the synthetic dataset, leaving near-duplicate or
  barely-separable points that make the hard-margin solver's penalty-based approximation unstable;
  keep the `margin_pad` term from the lecture-content code (or increase it) if Task A's solver
  does not converge cleanly.
- Running too few gradient-descent steps in Task B and concluding the bias "doesn't work" — recall
  from Week 4 §3 Step 4 that norm growth (and hence margin growth) is only *logarithmic* in steps;
  direction convergence is faster than norm convergence but still needs a genuinely large step
  count to look clean on a plot.
- In Task C, plotting $\|w_t\|$ against $t$ instead of $\log t$ — the linear relationship Week 4
  predicts is specifically against $\log t$, and plotting against raw $t$ will look like a
  decelerating curve, not a line, which is easy to mistake for a bug.

**Debugging tip:** verify your hard-margin solver independently by checking that every training
point satisfies $y_i\, w_\text{svm}^\top x_i \ge 1 - \epsilon$ for a small tolerance $\epsilon$
before trusting the cosine-similarity comparison in Task B.

**Instructor tip:** have students predict, before running Task D, whether switching to the
exponential loss should change the *limit direction* — reinforcing that the tail behavior, not the
particular loss formula, is the active ingredient in the derivation.
