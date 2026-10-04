# Lab Notes 6 — Hessian Eigenspectrum and Newton's Method

**Concept recap:** a critical point's Hessian eigenvalue signs classify it exactly (all positive:
local min; all negative: local max; mixed: saddle); Newton's method rescales each eigendirection
by $1/|\lambda_i|$ without regard to sign, so near a saddle it can step in the *wrong* direction
along a negative-curvature axis rather than escaping it.

**Common pitfalls:**
- Starting Newton's method/gradient descent *exactly* on the saddle (e.g., exactly at the origin
  for $f(x,y)=x^2-y^2$) — by symmetry, the gradient there is exactly zero and neither method
  moves at all; the lecture's starting point is deliberately offset ($x_0=0.01$) to avoid this
  degenerate case, and students must preserve a small offset too.
- Computing the Hessian only once (at the starting point) and reusing it for every Newton
  iteration instead of recomputing it at the *current* point each step — this is only valid for
  an exactly quadratic $f$ (as in Task A) and will silently give wrong results for Task B/C's less
  trivial losses.
- Confusing "Newton's method didn't converge" with "Newton's method is broken" — near a saddle,
  *not* converging monotonically toward the critical point is the expected, correct behavior this
  lab is designed to surface, not a bug to fix.
- In Task C, treating a *small positive* eigenvalue as effectively zero/flat without checking its
  actual sign — near-zero-but-positive and near-zero-but-negative eigenvalues look superficially
  similar in magnitude but mean very different things for local geometry.

**Debugging tip:** if Task B's classification seems to contradict a direct plot of the loss
surface near that point, re-check the sign convention of `hessian()`'s output (row/column order)
against the function signature actually passed to it.

**Instructor tip:** Task D is where the "why does this matter at scale" connection must be made
explicit — a 50-parameter network's mixed-sign Hessian is a small, directly inspectable preview of
why a million-parameter network's loss surface is assumed, on strong theoretical grounds, to be
saddle-dominated almost everywhere.
