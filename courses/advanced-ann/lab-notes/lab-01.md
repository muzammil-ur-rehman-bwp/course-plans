# Lab Notes 1 — Environment Setup & Open-Problems Landscape Mapping

**Concept recap:** this course assumes the graduate ANN course as settled background; Task B's
checklist is a diagnostic, not a quiz — its purpose is to surface gaps for independent review, not
to be re-taught in class.

**Common pitfalls:**
- Treating Task D's research-interest paragraph as a commitment — it is a first draft; students
  often under-write it out of a mistaken worry about "locking in" a capstone topic too early. The
  opposite failure (overthinking it for the full 3-hour session) is equally common; cap it at 30
  minutes and move on.
- Skipping the GPU-availability check and assuming code "will just work" later — several later
  labs (Weeks 2, 5, 10) run noticeably faster with a GPU but are designed to also run on CPU at
  the course's small scale; confirm which you have now rather than discovering it during a later,
  time-boxed lab session.
- Being overly generous on the Task B self-assessment ("I've heard of NTK" is not the same as "I
  can state what the graduate course's NTK week established") — under-reporting gaps now means
  discovering them unprepared in Week 2.

**Debugging tip:** if `torch` import fails, check for a Python version mismatch between the
virtual environment and the Jupyter kernel it is launched from — a very common first-week issue.

**Instructor tip:** collect (anonymized) Task B gap reports across the class to identify any
graduate-course topic several students are shaky on, and send a short optional review pointer
before Week 2 or Week 4 (whichever week's new material depends on it most directly) rather than
re-teaching it in class.
