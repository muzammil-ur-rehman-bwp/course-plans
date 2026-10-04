# Week 16 — Lecture Content: Capstone Presentations + Course Review

## 1. Capstone Presentations
Students present their capstone projects per `presentations/capstone-presentation-template.md`
and are graded per `assignments/capstone-rubric.md`. No new lecture content is introduced during
the presentation sessions; this file instead consolidates the course review material used in the
lab-session slot of Week 16.

## 2. The Course Map, Revisited
The course built up a coherent picture of classical AI, one layer at a time:

| Weeks | Theme | Core Idea |
|---|---|---|
| 1 | Foundations | What intelligence/AI means; PEAS task-environment specification |
| 2 | Agents | Agent architectures; environment properties that determine which architecture fits |
| 3–5 | Search | Formulating problems as search; uninformed, informed, and adversarial search |
| 6–8 | Logic | Representing knowledge precisely; inference by truth tables, resolution, chaining, FOL |
| 9 | Planning | Structuring actions (STRIPS) so a planner can reason about applicability and effects |
| 10–11 | Uncertainty | Reasoning with degrees of belief; Bayes' rule; compact representation via Bayesian networks |
| 12–15 | Survey | Honest, brief look at ML, neural networks, NLP, vision, robotics, and AI ethics |
| 16 | Synthesis | Capstone: applying one classical technique end-to-end |

## 3. Recurring Threads
A few ideas recur across nearly every module, worth naming explicitly during review:
- **Formulate first, solve second.** Every module began by precisely specifying the problem
  (search's `Problem` class, logic's syntax/semantics, STRIPS's action schema, a Bayesian
  network's structure) before any algorithm was introduced.
- **Admissibility / soundness matter more than cleverness.** A*'s optimality guarantee, 
  resolution's soundness, and a Bayesian network's conditional-independence assumptions are all
  about *correctness* conditions under which an algorithm's output can be trusted.
- **Compactness via structure.** Heuristics compress search; Horn clauses compress inference;
  STRIPS schemas compress action representation; Bayesian networks compress joint distributions.
  Finding the right structure is a recurring form of progress throughout AI.
- **Classical and statistical AI are complementary, not opposed.** Week 12's decision tree is a
  search over trees guided by an information-theoretic heuristic; Week 15's robot combines
  reactive control with planning. Modern systems routinely combine both pillars.

## 4. Looking Beyond This Course
Students who want to go deeper have natural next steps within this curriculum: *Introduction to
Machine Learning* and *Introduction to Artificial Neural Networks* extend Weeks 12–13's survey
into full, applied treatments; *Knowledge Representation and Reasoning* extends Weeks 6–9 into
richer logics and more powerful inference and planning algorithms; *Programming for Artificial
Intelligence* provides the applied, library-heavy Python workflow (NumPy/pandas/scikit-learn)
that this conceptual course deliberately did not require.

## 5. Final Exam Preparation
The final exam (Week 17) is comprehensive but weighted toward Weeks 9–16: classical planning,
probability and Bayesian networks, and the ML/NN/NLP/vision/robotics survey, plus ethics. Review
the Week 8 midterm roadmap for Weeks 1–8 material, and this table for Weeks 9–16.
