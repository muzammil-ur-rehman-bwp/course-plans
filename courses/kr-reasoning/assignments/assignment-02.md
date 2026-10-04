# Assignment 2 — Representation Schemes: Rules, Frames & Ontologies (Weeks 5–7)

**Weight:** 5% of course grade (one of 4 assignments, 20% total) | **Assigned:** Week 7 | **Due:** Start of Week 9

## Instructions
Submit `assignment02.ipynb` (or notebook + short written answers where indicated) with working
code and written answers for all questions.

## Questions
1. **(Rule Engine, 20 pts)** Design a rule base of at least 6 rules for a domain not used in
   lecture or lab. Using your Lab 5 `forward_chain`/`backward_chain` engine **unmodified**, run
   3 queries by each strategy and report agreement.
2. **(Semantic Networks & Exceptions, 20 pts)** Construct a 4-level IS-A hierarchy with one
   genuine exception case (your own Tweety/penguin-style scenario). Show your Lab 6
   `SemanticNetwork` giving the wrong answer, then your `Frame` hierarchy giving the correct
   (overridden) answer.
3. **(Frame Inheritance Trace, 15 pts)** For a 3-level frame hierarchy where the bottom frame
   sets neither a slot nor a default of its own, trace by hand which ancestor's value `get_slot`
   resolves to, and explain why in 2–3 sentences.
4. **(Description Logic, 25 pts)** Given 2 TBox axioms and a small model (provided by the
   instructor), translate each axiom to FOL by hand, then use your Lab 7 `extension`/`subsumes`
   functions to check whether each axiom holds in the given model. Explain any mismatch between
   your hand translation's intuitive meaning and the toy checker's single-model result.
5. **(Synthesis, 20 pts)** In 1–2 paragraphs, compare how a production rule, a frame's
   inheritance-with-override, and a DL subsumption axiom would each represent the same piece of
   knowledge ("a manager is an employee who supervises at least one other employee"), and discuss
   which desideratum (Week 1) each representation favors for this particular fact.

## Submission
Upload `assignment02.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
