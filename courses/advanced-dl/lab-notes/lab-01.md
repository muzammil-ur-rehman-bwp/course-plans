# Lab Notes 1 — Environment Setup and Landscape Mapping

**Concept recap:** this course assumes the graduate *Deep Learning* course's seven-item checklist
as firm background; this lab's self-assessment exists to surface gaps early, not to grade
prerequisite knowledge.

**Common pitfalls:**
- Treating Task B as a formality and writing generic "I'm comfortable with everything" answers —
  the self-assessment is only useful if answered honestly; a gap identified in Week 1 is cheap to
  fix, the same gap discovered in Week 7 (mid-DPO-derivation) is expensive.
- In Task C, confusing "this course's territory" with "a topic that sounds technical" — the test
  is not technical difficulty but which specific pillar (this course's eight topics, or a named
  sibling's pillars) the paper's central contribution belongs to.
- Skipping the GPU check and assuming a GPU is available in every later lab session — several
  labs (Weeks 8–9 especially) run noticeably faster with one, and discovering its absence in
  Week 8 rather than Week 1 wastes lab time.

**Debugging tip:** if `torch.cuda.is_available()` returns `False` unexpectedly on a machine you
believe has a GPU, check the installed PyTorch build matches your CUDA driver version before
assuming the hardware itself is the problem.

**Instructor tip:** use Task B's responses to identify, early, any student who should be directed
to specific graduate-course review material before Week 2's SDE derivation — this is far more
useful as a diagnostic in Week 1 than as a post-hoc explanation for later struggles.
