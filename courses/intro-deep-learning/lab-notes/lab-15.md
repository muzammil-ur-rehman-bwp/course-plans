# Lab Notes 15 — Case Study: Bias Audit and Training Compute Cost

**Concept recap:** a subgroup accuracy gap is diagnosed by inspecting data composition (class
balance, subgroup representation, proxy features) first, not by assuming the model architecture
itself is at fault; relative training compute scales roughly with parameters × steps × batch size.

**Common pitfalls:**
- Reporting only overall accuracy for Task A and missing a subgroup gap that overall accuracy can
  mask entirely — always compute and report subgroup-level metrics explicitly, as the task
  requires.
- In Task B, jumping to "the model is biased" as a conclusion without first examining the
  dataset's subgroup composition — the lecture's framing is specifically that the cause is usually
  traceable to data composition or proxy features, and the task asks students to make that
  connection explicit, not just name the symptom.
- In Task C, using `relative_training_cost` to produce an absolute real-world FLOPs/energy number
  — the function (as named and documented in lecture) is explicitly a *relative* estimate for
  comparison between two configurations, not a precise compute measurement.
- Treating this lab as a purely numerical exercise and skipping the written reflection (Task D) —
  the reflection is graded and is this lab's primary learning objective; the calculations in
  Tasks A–C exist to support it.

**Debugging tip:** if subgroup accuracy numbers in Task A look identical to overall accuracy,
double-check that the subgroup split was actually applied when computing per-group accuracy
(a common slip is computing the same overall-accuracy calculation twice with different variable
names).

**Instructor tip:** use Task D's written reflections as discussion material for the Week 16
course-review session — several strong reflections make an effective, low-prep way to revisit
this week's material right before the semester ends.
