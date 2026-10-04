# Lab Notes 8 — Value Iteration on a Grid-World MDP

**Concept recap:** value iteration repeatedly applies the Bellman backup until values stabilize;
convergence is guaranteed by the Bellman operator's contraction property with factor γ under the
max-norm, requiring γ < 1.

**Common pitfalls:**
- **Incorrect discount factor leading to non-convergent value iteration**: setting γ = 1 (or
  accidentally leaving it unset/defaulting to 1) removes the contraction guarantee; on a grid
  without guaranteed-absorbing terminal states this can cause values to grow without bound or
  oscillate instead of converging — always explicitly pass γ < 1 and verify convergence actually
  happens within a reasonable iteration count.
- **Off-by-one in the Bellman backup**: using the *previous* iteration's value for the state
  being updated itself (should always use `V_k`, never a partially-updated `V_{k+1}`, within a
  single synchronous sweep) — mixing old and new values mid-sweep is a subtle bug that still
  often "looks like" convergence but converges to the wrong fixed point.
- Forgetting the stochastic "slip" transitions in Task A's transition model — if `transition_model`
  only returns the intended direction with probability 1, the lab silently degenerates to a
  deterministic grid-world and the slip probability's effect on the optimal policy (e.g.,
  avoiding cells adjacent to the trap) will not show up.
- Reporting convergence "iteration count" inconsistently (some students count the first sweep as
  iteration 0, others as iteration 1) — state the convention used.

**Debugging tip:** print the value function after each iteration for the first 3–4 iterations on
a tiny 2x2 or 3x3 grid and verify by hand that the first backup's result for each cell matches
a manual Bellman-equation calculation.

**Instructor tip:** have students run Task D's γ = 1.0-adjacent case (e.g., γ = 0.9999 on a grid
variant with no guaranteed terminal state reachable) deliberately, to see slow or non-convergent
behavior firsthand rather than only being told about it.
