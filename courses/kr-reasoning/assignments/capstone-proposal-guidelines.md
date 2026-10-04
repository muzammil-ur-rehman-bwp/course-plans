# Capstone Project — Proposal Guidelines

**Due:** Week 11 | **Weight:** 2% of the Capstone grade (20% total course weight)

## What to Submit
A 1-page proposal (`capstone_proposal.pdf` or `.md`) covering:
1. **Problem statement**: what are you building a knowledge-based reasoning system to solve (a
   diagnostic task, a configuration/scheduling task, a classification task, an eligibility or
   planning task)?
2. **Formalisms integrated**: **at least two** distinct formalisms from this course, named
   explicitly (e.g., "a rule-based engine (Week 5) with a frame-based taxonomy (Week 6)," or "a
   CSP solver (Week 8) with an ontology-style taxonomy of item types (Week 6/7)"). A project built
   on only one formalism does not meet the capstone requirement.
3. **Domain/scope**: the specific domain and knowledge you will encode, with enough detail that
   scope is clear and achievable in the time available (e.g., "a fault-diagnosis expert system
   for home appliances, with a 15–20 rule base and a 3-level appliance-type frame hierarchy," not
   "a general diagnostic AI").
4. **Evaluation plan**: how will you demonstrate your system works (e.g., it correctly answers N
   hand-checked test queries; it finds a valid configuration/schedule for M test scenarios; its
   derivation traces correctly justify every answer tested)?
5. **Team**: individual or pair, with each member's planned contribution if a pair.

## Approval
The instructor will respond within one week with approval or requested revisions. Projects must
integrate at least two formalisms covered in this course; techniques beyond the syllabus require
instructor pre-approval and are not required for full credit.

## Example Topics (for inspiration, not a closed list)
A rule-based expert system over a narrow domain (e.g., simple fault diagnosis or plant/animal
identification) whose facts are organized as a frame-based taxonomy with inheritance and defaults
(Weeks 5+6); a CSP-based configuration or scheduling tool whose item types are organized under an
ontology-style taxonomy with inherited constraints (Weeks 6/7+8); a small STRIPS planner whose
initial-state facts are derived by a non-monotonic default-reasoning step before planning begins
(Weeks 9+10); an integrated knowledge-based agent (Week 14 style) extended with a Bayesian-network
module for one uncertain sub-decision (Weeks 5/6+12).

**Explicitly out of scope for this capstone:** a project built on only one formalism (e.g., a
rule engine alone, with no second representation integrated), or any project whose core technique
is a scikit-learn/ML workflow rather than the symbolic/probabilistic KR&R formalisms from this
course. That kind of project belongs to the applied *Programming for Artificial Intelligence*
course instead.
