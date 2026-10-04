# Lab Notes 14 — End-to-End Transfer Learning Workflow and Debugging

**Concept recap:** the Week 14 debugging checklist runs cheapest-first: data/labels, overfit-one-
batch, train/eval mode, gradient norms, learning rate.

**Common pitfalls:**
- Jumping straight to inspecting gradient norms or tuning the learning rate before checking
  data/labels or attempting to overfit a single batch — those two checks are cheaper and catch a
  larger share of real bugs; following the checklist *in order* is itself part of this lab's
  lesson.
- In Task B, rebuilding the architecture slightly differently before reloading the `state_dict`
  (e.g., a different number of output classes, or a missing final-layer replacement), which
  causes a shape-mismatch error on `load_state_dict` — the architecture must match exactly what
  was saved.
- In Task C, fixing the *symptom* (e.g., just lowering the learning rate) without identifying the
  actual root cause the checklist step revealed — document which specific step and finding led to
  the fix, as the task requires, not just a fix that happens to work.
- Treating "the loss decreased a little" as success in Task C's overfit-one-batch test — the test
  is only informative if the loss approaches near-zero on that tiny batch; a small decrease that
  plateaus usually still indicates a real bug.

**Debugging tip:** `lab14_broken.py`-style bugs are usually one of: mislabeled/misaligned data, a
missing `optimizer.zero_grad()`, a learning rate several orders of magnitude too high or too low,
or a loss/activation mismatch (e.g., applying `softmax` before `nn.CrossEntropyLoss`, which
expects raw logits) — check these in roughly that order.

**Instructor tip:** resist giving away which checklist step reveals the bug when students ask for
a hint — the point of this lab is practicing the checklist's discipline, not finding this specific
bug.
