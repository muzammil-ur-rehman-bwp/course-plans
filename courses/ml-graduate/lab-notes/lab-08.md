# Lab Notes 8 — AdaBoost From Scratch

**Concept recap:** AdaBoost reweights training points after each round to focus subsequent weak
learners on previously misclassified points; training error is provably bounded by a product
that shrinks exponentially; margins keep growing after training error reaches zero, which is the
mechanistic explanation for boosting's resistance to overfitting.

**Common pitfalls:**
- Letting $\epsilon_t$ (weighted error) reach exactly $0$ or $0.5$, which makes $\alpha_t =
  \frac12\ln\frac{1-\epsilon_t}{\epsilon_t}$ blow up to $\pm\infty$ or become undefined — always
  clip $\epsilon_t$ to a small range away from $0$ and $0.5$ (as in the lecture code) before
  computing $\alpha_t$.
- Forgetting to re-normalize the sample-weight distribution $D_t$ after each reweighting step,
  which silently breaks every subsequent round's weak-learner fitting.
- Measuring "margin" as the raw weighted vote $\sum_t\alpha_th_t(x_i)$ without normalizing by
  $\sum_t\alpha_t$ — the *normalized* margin (bounded in $[-1,1]$) is what the margin-theory bound
  in the lecture is stated in terms of; comparing unnormalized margins across different numbers of
  rounds is not meaningful.

**Debugging tip:** if training error does not reach zero within 40 rounds, print the per-round
$\epsilon_t$ sequence — if $\epsilon_t$ is consistently close to $0.5$, the weak learner (decision
stump) may not be expressive enough for the dataset, or the dataset may not be linearly
stump-separable with the current feature set.

**Instructor tip:** have students separately plot training error and mean margin on dual y-axes
over the same x-axis (round number) — seeing both curves on one plot makes the "training error
flat, margin still rising" phenomenon immediately visible, which is the central point of this
week's theory.
