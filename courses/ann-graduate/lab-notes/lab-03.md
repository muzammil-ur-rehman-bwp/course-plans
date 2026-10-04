# Lab Notes 3 — Building a Reverse-Mode Autodiff Engine

**Concept recap:** reverse-mode AD computes each node's adjoint as the **sum**, over every child
node it feeds into, of that child's adjoint times the local partial derivative. When one node
feeds into multiple children (a "shared" or "fan-out" node), every one of those contributions must
be **accumulated** (summed), never overwritten.

**Common pitfalls:**
- **The single most common bug in this lab:** writing `self.grad = ...` (assignment) instead of
  `self.grad += ...` (accumulation) inside a `_backward` closure. This passes every test that
  uses a node exactly once, and silently gives a *wrong, smaller* gradient the instant a node
  (like $x$ in Task B) is reused in more than one place — exactly the failure mode Task B is
  designed to catch.
- Forgetting to reset `.grad` to $0$ before a fresh `.backward()` call on a *new* computation
  reusing the same `Value` objects — old gradients silently add onto new ones.
- Building the topological order incorrectly (e.g., not fully recursing into all children before
  appending a node) — this can run a node's `_backward` before all of its own adjoint
  contributions have arrived, understating its gradient.
- In Task D, forgetting `retain_graph`/fresh-tensor requirements, or comparing against a
  `.grad` that was never zeroed between two separate backward calls on the same PyTorch tensors.

**Debugging tip:** Task B is deliberately the simplest possible test that catches the
accumulation bug (`self.grad = ` vs. `+=`). If Task C's more complex network fails but Task B
passes, the bug is elsewhere (e.g., a missing activation-derivative factor); if Task B itself
fails, fix accumulation before debugging anything else.

**Instructor tip:** this is the most consequential bug-finding exercise in the course for autodiff
— spend extra time here, since every later week's code (BatchNorm, Adam, NTK, pruning) sits on
top of correct gradient computation, whether via this hand-built engine or via `torch.autograd`.
