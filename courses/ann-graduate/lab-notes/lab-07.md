# Lab Notes 7 — Adam, Non-Convergence, and Warmup

**Concept recap:** Adam's bias-corrected moment estimates ($\hat m_t, \hat v_t$) require dividing
by $(1-\beta_1^t)$ and $(1-\beta_2^t)$ respectively — correction factors that matter most in early
steps and approach $1$ as $t$ grows. AMSGrad's fix replaces $\hat v_t$ with a running maximum to
force a non-increasing effective learning rate.

**Common pitfalls:**
- Forgetting bias correction entirely (using raw $m_t, v_t$ instead of $\hat m_t, \hat v_t$) —
  this still "sort of trains" on many problems, masking the omission, but behaves measurably
  differently in the first several dozen steps, exactly where Task A/C's comparisons matter most.
- In Task B, implementing AMSGrad by taking the max of $v_t$ (the *uncorrected* second moment)
  rather than of $\hat v_t$ (the bias-corrected one) — both numbers are close for large $t$ but
  diverge for small $t$, which is precisely the regime the non-convergence construction probes.
- In Task C, using `total_steps <= warmup_steps`, so the schedule never actually reaches its
  intended peak learning rate within the run — always verify `warmup_steps < total_steps` by a
  reasonable margin so the post-warmup behavior is actually observed.
- Treating "Adam eventually trains fine on my normal regression task in Task A" as evidence
  against the lecture's non-convergence claim — the non-convergence construction (Task B) is a
  deliberately adversarial, specific sequence of gradients; it does not claim Adam fails on
  typical, well-behaved problems, only that no universal convergence guarantee holds.

**Debugging tip:** if Task B's plain-Adam trajectory does not drift as expected, double check the
oscillating-gradient function's period and magnitude ($C$) match the lecture's construction
closely enough — too small a $C$, or too short a run, can mask the effect.

**Instructor tip:** make sure students can articulate, in Task D's writeup, the difference between
"this schedule happened to work better on my one toy problem" and "this schedule has a theoretical
reason to work better" — the first is an empirical observation, the second needs Section 4 of the
lecture content as justification.
