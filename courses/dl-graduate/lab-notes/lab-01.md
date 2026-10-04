# Lab Notes 1 — Environment Setup and Prerequisite Fluency Warm-Up

**Concept recap:** this lab verifies tooling and prerequisite fluency only; no new theory is
introduced.

**Common pitfalls:**
- Colab GPU runtime not selected (Runtime → Change runtime type → GPU) before checking
  `torch.cuda.is_available()`, leading students to wrongly conclude PyTorch/CUDA is broken.
- `torch_geometric` installation failures in some environments (version/CUDA-build mismatches) —
  this is expected and explicitly handled by the from-scratch fallback in Weeks 8–9; do not let
  students burn lab time debugging the install.
- Task B's "from memory" CNN reimplementation revealing genuine prerequisite gaps (e.g.,
  forgetting `model.eval()` for test accuracy, or incorrect `Flatten`/`Linear` shape matching) —
  this is useful diagnostic signal, not a lab failure; flag students who struggle here for a
  prerequisite-review pointer.

**Debugging tip:** if `torch.cuda.is_available()` returns `False` unexpectedly on Colab, the most
common cause is simply not having switched the runtime type yet — check that first before any
driver/installation debugging.

**Instructor tip:** Task C (topic mapping) is a useful early signal of whether students have
read the syllabus; a student who cannot place "a paper using a replay buffer" in Week 10 may need
extra syllabus orientation before Week 2.
