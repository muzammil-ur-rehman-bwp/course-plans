# Lab Notes 10 — Deep Q-Networks on a Toy Environment

**Concept recap:** DQN regresses $Q_\theta(s,a)$ toward a Bellman target computed from a frozen
target network; experience replay breaks temporal correlation by uniformly resampling past
transitions.

**Common pitfalls:**
- **DQN diverging without a target network or experience replay** (Tasks B/C are designed to
  reproduce this directly) — if divergence does *not* appear on a very simple environment, that
  is a legitimate (if less dramatic) outcome; discuss that instability severity depends on
  environment complexity, not that the mechanism doesn't matter.
- Computing the target using the **online** network instead of the **target** network
  (`q_net` instead of `target_net` inside `torch.no_grad()`) — this silently defeats the entire
  point of Task A's full DQN and degenerates to the (unstable) Task B ablation without anyone
  noticing, since the code still runs without error.
- Forgetting `torch.no_grad()` around the target computation — this lets gradients flow into the
  target network's forward pass unintentionally, which is both wasteful and conceptually wrong
  (the target is supposed to be treated as a fixed label for this update).
- Sampling from a replay buffer before it has enough transitions (`len(buffer.buffer) <
  batch_size`) — the lecture's `dqn_update` guards this, but a custom reimplementation that skips
  the guard will raise a `ValueError` from `random.sample`.

**Debugging tip:** if Task A's full DQN is not noticeably more stable than Task B/C's ablations,
first print whether `target_net`'s parameters actually differ from `q_net`'s at various points in
training — a missing or too-frequent `update_target_network` call is the most common cause.

**Instructor tip:** Task D's discussion works best when students have genuinely observed a
difference between conditions; if the toy environment is too easy to show instability, consider
a slightly harder custom grid-world with sparser rewards for this demonstration.
