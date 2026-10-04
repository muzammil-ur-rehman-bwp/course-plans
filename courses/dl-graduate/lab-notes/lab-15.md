# Lab Notes 15 — Capstone Experiment Execution and Draft Paper

**Concept recap:** this lab executes the approved capstone proposal's experiment and produces a
written draft; it is graded as part of the Research Capstone, not as a standalone lab.

**Common pitfalls:**
- Running a stochastic experiment (e.g., an RL training run, a GAN/diffusion training run) with
  only **one** random seed and reporting that single run's numbers as "the result" — report at
  least 3 seeds and the spread, exactly as emphasized throughout this course and in the sibling
  graduate AI course's research-methods week.
- Quietly changing the experiment's scope mid-way without updating the written draft's method
  section to match what was *actually* run — the paper must describe the experiment as executed,
  not as originally proposed, if anything changed.
- Treating a negative or partial result as something to hide or downplay rather than analyze —
  per the capstone rubric, a well-analyzed negative result scores as well as a positive one; an
  unexplained or glossed-over negative result scores poorly.
- Leaving the practice-talk outline (Task D) as an afterthought produced in the last 10 minutes —
  this week's peer-feedback session is only useful if the outline reflects the actual draft paper,
  not a stale earlier plan.

**Debugging tip:** not code-specific; the equivalent check is: does the results section's
reported numbers match what Task A's experiment logs actually show, with no silently dropped or
cherry-picked runs?

**Instructor tip:** this is the last lab before capstone presentations — spend instructor time
primarily on reviewing whether each draft's results section honestly matches its logged
experiment output, since this is the single most common point where capstone write-ups drift from
what was actually run.
