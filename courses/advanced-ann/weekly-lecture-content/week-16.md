# Week 16 — Lecture Content: Capstone Presentations and Course Review

## 1. The Course Map, Recapped
This course opened (Week 1) by stating the graduate course's content as assumed background and
mapping a research-frontier landscape. Sixteen weeks later, that landscape has been covered in
depth:
- **Weeks 2–5 — The infinite-width picture and its limits.** The NTK limit (Week 2, a frozen
  kernel, rigorous trainability, no feature learning by construction) and the mean-field limit
  (Week 3, a moving distribution, feature learning possible, signal propagation generalizing
  Xavier/He) are two different, both-valid idealizations of a wide network. The max-margin
  implicit bias (Week 4) showed what is fully settled in the linear case and open beyond it.
  Feature learning (Week 5) located where practical, finite-width networks actually sit relative
  to both infinite-width idealizations.
- **Weeks 6–9 — Why training finds the solutions it finds.** Sharpness and SAM (Week 6) gave a
  measurable, if reparameterization-contested, account of solution quality. Grokking (Week 7)
  showed training dynamics can hide a slow transition invisible to the training loss curve.
  Scaling laws (Week 8) showed a robust empirical regularity whose theoretical explanation remains
  partial. Statistical-physics approaches (Week 9) traced the saddle-point argument to its source
  and stated the analogy's honest limits.
- **Weeks 10–12 — Generalization and landscape geometry, rigorously.** Double descent (Week 10)
  was organized, across three axes, around one concept: the interpolation threshold. PAC-Bayes
  (Week 11) gave a non-vacuous alternative to classical bounds, with a precise, provable
  connection back to sharpness. Mode connectivity and the Lottery Ticket Hypothesis revisited
  (Week 12) gave a geometric account of why apparently separate minima, and apparently special
  initializations, may be more connected than a naive view suggests.
- **Weeks 13–16 — Doing research.** Research methods (Week 13), the open-problems survey
  (Week 14), and the proposal work session (Week 15) built directly toward this week's capstone
  defenses — the course's actual capstone skill: not reciting results, but formulating, surveying,
  proposing, and defending an original question.

## 2. Where This Leads Next
Several directions this course deliberately left as one-sentence pointers, owned by sibling
postgraduate courses, are now unlocked: *Advanced Machine Learning* for classical statistical-ML
theory at postgraduate depth; *Advanced Deep Learning* for deep architectures at postgraduate
depth; *Advanced Knowledge Representation and Reasoning* for deep KR formalisms. Beyond the
course sequence, the material in Weeks 2–14 is drawn from, and should now be legible as, the
current content of NeurIPS, ICML, and ICLR's theoretical-ML tracks — the primary venues this
course has pointed students toward throughout, rather than a fixed textbook.

## 3. Capstone Defense Format (Recap)
See `presentations/capstone-presentation-template.md` for the full required-slides structure. In
brief: title and pillar; problem statement (apply the Week 13 precision test live, if asked);
related work (ending in a stated gap); proposed approach (your own formulation); feasibility
argument or preliminary results (with a named, honestly-stated risk); anticipated risks; committee
Q&A. Defenses are evaluated per `assignments/capstone-rubric.md` — the soundness of the research
plan matters more than whether anything was run.

## 4. Closing Note
A research proposal that honestly says "the survey doesn't settle this, and here is why I think it
remains open" is a stronger piece of postgraduate work than one that overstates what is known to
appear more conclusive. This standard — stated explicitly in Week 1, applied throughout the
semester's own treatment of NTK, grokking, statistical physics, scaling laws, and the open-
problems survey — is the standard your own proposal is defended against today.
