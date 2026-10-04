# Lab Notes 8 — Specification Gaming in a Toy Gridworld

**Concept recap:** specification gaming is a Goodhart's-law effect — an optimizer searches for
and exploits any gap between a measurable proxy reward and the true intended objective; the
gridworld demo's proxy (reward at cell 2) is an outer-alignment failure because the *specified*
objective itself, not the learned policy's fidelity to it, was the poor proxy.

**Common pitfalls:**
- In Task A, inspecting only `max(Q[s], key=Q[s].get)` at a single state and assuming that
  characterizes the whole policy — print the greedy action at *every* state (0–4) to see the full
  looping pattern, since the policy's behavior near the exploited checkpoint is the interesting
  part.
- In Task C, removing the proxy reward entirely without adding any reward signal at the true
  goal — this can produce a different failure (a policy that wanders with no learning signal at
  all) that looks superficially "fixed" (no more looping) but has not actually learned the
  intended task; verify the fixed policy reaches cell 4, not just that it stops looping.
- In Task B, stating the diagnosis as "the reward was wrong" without using the precise
  outer/inner-alignment vocabulary from Week 8, §3 — the task requires the specific
  classification and its justification, not a restatement of the symptom.
- In Task D, constructing a "harder" misspecification that is actually just a smaller version of
  Task A's — the mini-challenge is only informative if Task C's *specific* fix genuinely fails to
  address it; check this explicitly rather than assuming novelty.

**Debugging tip:** print the full Q-table (not just the derived greedy policy) when something
looks wrong — near-tied Q-values at a state can make the "greedy" action flicker between
runs/seeds in a way that looks like a bug but is actually just an underdetermined policy at that
state.

**Instructor tip:** have students rerun Task A with `epsilon` increased (e.g., to 0.3) and
`gamma` decreased, and discuss how each change affects the severity or even the presence of the
looping behavior — this makes the point that specification gaming's *presence* can depend on
training hyperparameters, not only on the reward specification itself, which is itself a useful,
unsettling observation for the Week 9 alignment-research-directions discussion.
