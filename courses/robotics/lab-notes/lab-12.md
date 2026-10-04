# Lab Notes 12 — 1D Kalman Filter

**Concept recap:** predict (project forward, increase uncertainty) then update (incorporate
measurement via Kalman gain, decrease uncertainty); the Kalman gain balances trust between the
motion model and the measurement based on their relative noise levels.

**Common pitfalls:**
- Setting `measurement_var` to an unrealistically small value, making the filter essentially
  ignore the motion model and just track raw (still noisy) measurements.
- Forgetting to call `predict()` before `update()` each step, which skips the uncertainty growth
  that predict is responsible for.

**Debugging tip:** if the filtered estimate looks identical to the raw measurement, the Kalman
gain is likely too close to 1 — check `measurement_var` relative to `process_var`.

**Instructor tip:** Task D's quantitative error comparison (filter vs. raw) is the most
convincing evidence students will see all semester that fusion actually helps — don't skip it
for time.
