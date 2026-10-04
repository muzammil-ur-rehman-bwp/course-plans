# Course Contents: Knowledge Representation and Reasoning

Detailed per-week breakdown of topics, subtopics, and resources. Companion to `course-plan.md`.
Each week lists: **Topics**, **Subtopics/Skills**, **Readings**, **Software/Libraries used**.

---

## Week 1 — Introduction to Knowledge Representation
- **Topics:** What knowledge representation is and why it is a distinct problem from search or
  learning; the KR hypothesis (a system's behavior can be explained/predicted by the knowledge it
  has and how it reasons with it); desiderata for a good representation — expressiveness (can it
  say what needs to be said?), inferential efficiency (can conclusions be drawn tractably?), and
  naturalness (does it map cleanly onto how humans think about the domain?); an overview map of
  the representation schemes covered this semester — logic (propositional, first-order),
  semantic networks, frames, production rules, description logics/ontologies — and how they
  trade off the three desiderata against each other.
- **Subtopics/Skills:** classifying a given representation choice (e.g., "store facts as a flat
  list of English sentences" vs. "store them as logical formulas") against the three desiderata;
  sketching, for one small domain, how it would look under three different schemes.
- **Readings:** Brachman & Levesque, introductory chapter on the role and goals of KR; Russell &
  Norvig Ch. 7 (§7.1, motivation for logic as a representation).
- **Software:** Python 3.10+, Jupyter/Colab (environment setup only).

## Week 2 — Propositional Logic in Depth
- **Topics:** Recap of propositional syntax/semantics; normal forms — conjunctive normal form
  (CNF) and disjunctive normal form (DNF), and the conversion procedure (eliminate
  `↔`/`→`, push negation inward via De Morgan's, distribute `∨` over `∧` for CNF or `∧` over `∨`
  for DNF); the resolution rule in depth, with multiple worked derivations; resolution refutation
  as a sound and complete decision procedure for propositional entailment; the Boolean
  satisfiability problem (SAT) and a brief, honest look at its complexity (NP-completeness, why
  this matters for the scalability of resolution-based reasoning).
- **Subtopics/Skills:** implementing a CNF/DNF converter; implementing resolution refutation with
  a trace of every resolvent derived; implementing a brute-force SAT checker and discussing why it
  does not scale.
- **Readings:** Russell & Norvig Ch. 7 (§7.5); Brachman & Levesque Ch. 2 (propositional logic as a
  representation language).
- **Software:** Python.

## Week 3 — First-Order Logic in Depth
- **Topics:** Why propositional logic cannot represent general relational knowledge; FOL syntax
  in depth — terms (constants, variables, function applications), atomic sentences (predicates
  applied to terms), complex sentences, quantifiers (`∀`, `∃`) and their scope; FOL semantics —
  models (a domain of objects plus an interpretation of constants/predicates/functions over it)
  and satisfaction of a sentence in a model; translating English sentences to FOL, including
  nested-quantifier sentences and the `∀∃` vs. `∃∀` scope trap.
- **Subtopics/Skills:** representing a tiny FOL model in Python (a finite domain plus dicts for
  predicate/function interpretations) and evaluating whether a quantified sentence is satisfied in
  it by brute-force enumeration over the domain; translating a batch of English sentences to FOL
  and back.
- **Readings:** Russell & Norvig Ch. 8 (§8.1–8.2); Brachman & Levesque Ch. 3 (first-order logic).
- **Software:** Python.

## Week 4 — First-Order Inference
- **Topics:** Why propositional resolution does not directly apply to FOL (variables need to be
  matched, not just compared); the unification algorithm (most general unifier, occurs check,
  substitution composition); resolution for FOL (unify complementary literals instead of requiring
  exact string match); Skolemization at a conceptual level (replacing existentially quantified
  variables with Skolem functions/constants to remove `∃` before resolution, with a worked example
  rather than a full proof); soundness and completeness of FOL resolution (brief, result-level
  statement, not a full proof).
- **Subtopics/Skills:** implementing the unification algorithm from scratch, including variable
  substitution and the occurs check; implementing a FOL resolution step that unifies complementary
  literals before resolving; tracing Skolemization by hand on a small `∃`-containing sentence.
- **Readings:** Russell & Norvig Ch. 9 (§9.1–9.2, unification and resolution); Brachman & Levesque
  Ch. 3 (FOL inference).
- **Software:** Python.
- **Assignment 1 assigned** (logic foundations, Weeks 1–4).

## Week 5 — Rule-Based Systems
- **Topics:** Production systems as a representation/architecture (a working memory of facts, a
  rule base of condition-action rules, and a recognize-act inference cycle); forward chaining as a
  data-driven production-system inference engine (match-resolve-act, conflict resolution briefly);
  backward chaining as goal-directed, recursive reasoning over the same rule base; building a
  general-purpose Python rule engine from scratch that supports both strategies over an arbitrary
  rule base (not hard-coded to one domain, unlike the single Horn-clause example a brief survey
  course would use).
- **Subtopics/Skills:** designing a `Rule` representation (premises, conclusion) decoupled from any
  specific domain; implementing a reusable `forward_chain` and `backward_chain` function that both
  operate over the same rule-base representation; applying the engine to two different toy
  knowledge bases (e.g., animal identification and a simple fault-diagnosis domain) without
  changing the engine code.
- **Readings:** Brachman & Levesque Ch. 7 (rules and production systems); Russell & Norvig Ch. 7
  (§7.5, forward/backward chaining, as a starting point extended here to a general engine).
- **Software:** Python.

## Week 6 — Semantic Networks and Frames
- **Topics:** Semantic networks as a graph-structured representation of objects, categories, and
  relations (IS-A and part-of links as the canonical cases); inheritance of properties down an
  IS-A hierarchy; the exceptions problem — strict inheritance breaks when a subtype needs to
  override an inherited property (the classic "Tweety is a bird, birds fly, but Tweety is a
  penguin" case), motivating **non-monotonic inheritance**; frames as a richer representation —
  objects as named bundles of slots, each slot with a value or a default value and optional
  attached procedures; frame-based reasoning that resolves a slot's value by checking the frame
  itself, then its parent frames in order, stopping at the first value or default found (and
  overriding defaults with any more specific value).
- **Subtopics/Skills:** implementing a semantic network as a Python graph (dict of nodes to
  IS-A/part-of edges) with a property-inheritance query function; implementing a frame system with
  slots, defaults, and inheritance that correctly overrides a default when a more specific frame
  provides its own value; constructing the Tweety/penguin exceptions case and showing the frame
  system resolves it correctly while naive strict inheritance does not.
- **Readings:** Brachman & Levesque Ch. 4 (semantic networks) and Ch. 5 (frames and defaults).
- **Software:** Python.

## Week 7 — Description Logics and Ontologies
- **Topics:** Description logics (DLs) as a family of decidable, structured fragments of FOL
  purpose-built for representing taxonomic/conceptual knowledge; basic DL syntax — concepts
  (unary predicates, e.g. `Person`), roles (binary predicates, e.g. `hasChild`), and basic
  constructors (conjunction `⊓`, negation `¬`, existential restriction `∃R.C`, universal
  restriction `∀R.C`) in a small fragment such as `ALC`; the relationship between DL and FOL (every
  DL concept/role assertion translates to a FOL formula; DLs trade some FOL expressiveness for
  guaranteed decidable reasoning — concept subsumption, instance checking); an overview of
  OWL (Web Ontology Language) and RDF (Resource Description Framework) as the Semantic Web's
  standardized realization of DL-style ontologies, and the Semantic Web vision of machine-readable,
  linked knowledge on the web; real-world DL tooling (Protégé as an ontology editor, Pellet as a
  DL reasoner) discussed conceptually as context, not required software.
- **Subtopics/Skills:** translating a handful of DL concept expressions to and from FOL by hand;
  implementing a toy subsumption checker in Python for a small, fixed DL fragment, representing
  concepts as Python sets/structures and checking `C ⊑ D` by a direct semantic (model-based) test
  over a small finite domain rather than a general DL tableau algorithm; sketching an RDF triple
  (`subject, predicate, object`) representation of a small ontology fragment.
- **Readings:** Brachman & Levesque Ch. 9 (description logics); van Harmelen, Lifschitz & Porter
  (eds.), *Handbook of Knowledge Representation*, description logics chapter (reference/overview).
- **Software:** Python.
- **Assignment 2 assigned** (representation schemes, Weeks 5–7).

## Week 8 — Constraint Satisfaction in Depth; Midterm Review
- **Topics:** Recap of CSP formulation (variables, domains, constraints) as a representation for
  problems defined by what is *not* allowed rather than by explicit search successors; constraint
  graphs; arc consistency and the AC-3 algorithm in depth (the revise procedure, the worklist of
  arcs, termination, what "arc consistent" guarantees and does not guarantee); backtracking search
  for CSPs with variable-ordering heuristics (minimum-remaining-values, degree heuristic) and
  value-ordering heuristics (least-constraining-value), and the benefit of interleaving AC-3-style
  propagation (forward checking) with backtracking; review session for Weeks 1–7 ahead of the
  midterm.
- **Subtopics/Skills:** implementing AC-3 from scratch, including the `revise(csp, Xi, Xj)`
  subroutine and the worklist/queue of arcs; implementing backtracking search with MRV and
  least-constraining-value heuristics; applying both to a map-coloring and a small scheduling CSP;
  practice problems for the midterm.
- **Readings:** Russell & Norvig Ch. 6 (CSPs, in full — this week treats the chapter at full
  depth rather than the brief recap given elsewhere).
- **Software:** Python.

## Week 9 — Midterm Exam; Non-Monotonic Reasoning
- **Topics:** Midterm Exam (covers Weeks 1–8). Afterward: why classical (monotonic) logic cannot
  model everyday common-sense reasoning, where conclusions must sometimes be retracted as new
  information arrives (monotonicity means `KB ⊨ α` implies `KB ∪ {β} ⊨ α` for any `β` — nothing
  already derived can ever be taken back); the closed-world assumption (CWA) — treating any atomic
  fact not derivable from the KB as false, rather than unknown, which is how most databases and
  rule engines implicitly behave; default logic (Reiter) — default rules of the form "if
  prerequisite holds and justification is consistent with what is believed, conclude the
  consequent," and how a default can later be blocked by new, conflicting information; a
  conceptual introduction to circumscription (minimizing the extension of certain predicates to
  formalize "assume no more abnormality than necessary") without a full formal treatment.
- **Subtopics/Skills:** implementing a CWA query engine over a small fact base; implementing a
  default-logic extension builder that applies defaults unless their justification is blocked by a
  derived fact, and showing a worked case where adding one new fact changes (retracts) a
  previously drawn conclusion — something resolution refutation from Weeks 2 and 4 structurally
  cannot do.
- **Readings:** Brachman & Levesque Ch. 10 (non-monotonic reasoning); van Harmelen, Lifschitz &
  Porter (eds.), *Handbook of Knowledge Representation*, non-monotonic reasoning chapter.
- **Software:** Python.

## Week 10 — Planning in Depth
- **Topics:** The STRIPS representation revisited in depth (action schemas with variables,
  preconditions, add-lists, delete-lists; grounding a schema into concrete actions over a domain's
  objects); limitations of pure forward state-space search as action/object counts grow; partial-
  order planning (POP) — building a partially ordered, partially specified plan by resolving open
  preconditions and detecting/repairing threats between steps, rather than committing to a total
  action order up front; a conceptual introduction to planning graphs and the GraphPlan algorithm
  (alternating proposition/action levels, mutex relations between actions and between propositions,
  and extracting a plan by searching backward through the graph) — introduced at the level of how
  the graph is built and what it is used for, not a full GraphPlan implementation.
- **Subtopics/Skills:** extending the STRIPS action-schema implementation to support variables and
  grounding; implementing a small partial-order planner (open-precondition resolution and threat
  detection) for a toy domain; constructing the first one or two levels of a planning graph by hand
  for a small domain and identifying mutex pairs.
- **Readings:** Russell & Norvig Ch. 10 (§10.3–10.4, partial-order planning and planning graphs);
  Brachman & Levesque Ch. 8 (action and planning representations).
- **Software:** Python.
- **Capstone project introduced** (proposal due Week 11).

## Week 11 — Temporal and Spatial Reasoning
- **Topics:** Why point-based time (a single timestamp per fact) is often too weak a
  representation; representing and reasoning about time with intervals — Allen's interval algebra
  and its thirteen base relations (`before`, `after`, `meets`, `met-by`, `overlaps`,
  `overlapped-by`, `starts`, `started-by`, `finishes`, `finished-by`, `during`, `contains`,
  `equals`); composing interval relations and propagating constraints over a small temporal
  constraint network (path consistency); a brief, conceptual look at basic spatial relations
  (topological relations such as disjoint, touches, overlaps, contains — the core idea behind
  formalisms like the Region Connection Calculus) as the spatial analogue of Allen's algebra.
- **Subtopics/Skills:** implementing Allen's thirteen relations as a function that, given two
  intervals `(start, end)`, returns the relation that holds between them; implementing constraint
  propagation (path consistency) over a small network of interval constraints to detect
  inconsistency or infer new relations; sketching the topological spatial relations for a small
  worked example (e.g., regions on a toy map).
- **Readings:** van Harmelen, Lifschitz & Porter (eds.), *Handbook of Knowledge Representation*,
  temporal reasoning chapter (Allen's interval algebra; spatial reasoning overview).
- **Software:** Python.
- **Assignment 3 assigned** (constraints, non-monotonic reasoning, planning, Weeks 8–10).
  **Capstone proposal due.**

## Week 12 — Probabilistic Reasoning and Bayesian Networks in Depth
- **Topics:** Recap of representing a joint distribution compactly with a Bayesian network (DAG
  structure, conditional probability tables); **exact inference by variable elimination** — a
  level deeper than enumeration alone: eliminating hidden variables one at a time by summing out
  (marginalizing) each from a running product of factors, exploiting the network structure so that
  not every full joint entry needs to be formed; factor representation, factor multiplication,
  and summing out a variable from a factor; choosing an elimination ordering and why it affects
  efficiency (brief).
- **Subtopics/Skills:** implementing a small `Factor` class (a function from a tuple of variable
  assignments to a probability, with multiply and sum-out operations) and a `variable_elimination`
  query function built from it; running it on the same Burglary/Earthquake/Alarm-style network
  used as a motivating example, and on a slightly larger 5-node network, comparing the number of
  numbers manipulated against plain enumeration.
- **Readings:** Russell & Norvig Ch. 13 (§13.1–13.3, review) and Ch. 14 (§14.4, exact inference by
  variable elimination — the chapter this course implements, versus the enumeration-only
  treatment given elsewhere).
- **Software:** Python.

## Week 13 — Reasoning with Uncertainty Beyond Bayes
- **Topics:** Limits of pure Bayesian networks (hard to encode general logical structure with
  exceptions) and of pure logic (cannot represent degrees of belief); Markov logic networks (MLNs)
  as a conceptual survey of combining first-order logic with probability — weighted first-order
  formulas, where a world's probability depends on how many formula instances it satisfies (higher
  weight, stronger preference, but no formula is strictly required as in classical FOL); fuzzy
  logic basics — fuzzy sets and membership functions (a value's degree of membership in a set
  ranges over `[0, 1]`, not just `{0, 1}`), standard fuzzy set operations (fuzzy AND as `min`,
  fuzzy OR as `max`, fuzzy NOT as `1 - x`), and a simple fuzzy rule evaluation example.
- **Subtopics/Skills:** working a small MLN-style example by hand (a couple of weighted formulas
  over a tiny world, comparing relative world probabilities conceptually, without implementing a
  full MLN inference engine); implementing fuzzy membership functions (e.g., triangular/trapezoidal)
  and fuzzy set operations in Python; evaluating a small fuzzy rule base (e.g., a toy
  temperature-to-fan-speed controller) by hand and in code.
- **Readings:** van Harmelen, Lifschitz & Porter (eds.), *Handbook of Knowledge Representation*,
  reasoning-under-uncertainty chapter (MLN and fuzzy logic overviews).
- **Software:** Python.

## Week 14 — Knowledge-Based Agents in Practice
- **Topics:** Pulling the semester together into a single knowledge-based agent architecture: a
  working memory (facts/frames), a rule base, and a query interface that can run forward chaining
  (derive everything it can, then answer queries by lookup) or backward chaining (derive only what
  is needed to answer a specific query, recursively); building a small Python-based
  forward/backward-chaining reasoner over a toy knowledge base that mixes plain facts, rules, and
  a shallow frame-style taxonomy (IS-A links feeding rule premises); querying the agent and tracing
  *why* it reached an answer (a simple proof/derivation trace), which matters for explaining a
  KR system's conclusions to a user.
- **Subtopics/Skills:** integrating the Week 5 rule engine and the Week 6 frame/inheritance code
  into one reasoner class with a single `ask(query)` method dispatching to forward or backward
  chaining; adding a derivation trace that records which rule/fact justified each derived fact;
  querying the integrated agent on a toy domain (e.g., a simple diagnostic or classification
  knowledge base) and inspecting the trace for a few queries.
- **Readings:** Brachman & Levesque Ch. 7 and Ch. 5 revisited (rules and frames, now as one
  integrated system); Russell & Norvig Ch. 7 (§7.5, knowledge-based agents, revisited at
  implementation depth).
- **Software:** Python.
- **Assignment 4 assigned** (temporal/probabilistic reasoning and KR practice, Weeks 11–14).

## Week 15 — Current Trends in Knowledge Representation
- **Topics:** Knowledge graphs as the modern, large-scale descendant of semantic networks —
  triple-store (subject-predicate-object) and property-graph representations, and real-world
  examples of the idea (e.g., large organizations' internal knowledge graphs, and public
  triple-store-backed knowledge bases) discussed conceptually as context for where this course's
  semantic-network/frame/DL ideas scale to in industry; neuro-symbolic AI as a conceptual survey of
  combining symbolic KR (the representations and inference procedures from this course) with
  modern machine-learning components (e.g., using a learned model to extract candidate facts/rules
  that a symbolic reasoner then checks and reasons over, or using symbolic constraints to guide a
  learned model) — discussed at a survey level, not implemented; evaluating KR systems —
  correctness (soundness/completeness relative to a formal semantics), scalability, and
  explainability (can the system justify its conclusions, as the Week 14 derivation trace did?) as
  the three standard axes.
- **Subtopics/Skills:** implementing a small knowledge-graph triple store (subject, predicate,
  object tuples) with simple path and pattern queries (e.g., "find all `X` such that `X
  worksFor Y` and `Y locatedIn Z`"); a short written/discussion exercise comparing the semantic
  network from Week 6 to the knowledge-graph representation built this week; a short discussion of
  one real or plausible neuro-symbolic system design for a toy problem.
- **Readings:** van Harmelen, Lifschitz & Porter (eds.), *Handbook of Knowledge Representation*,
  closing/applications chapter (used here as a bridge to current practice).
- **Software:** Python.

## Week 16 — Capstone Presentations; Course Review
- **Topics:** Student capstone project presentations; recap of the course map (logic foundations →
  rule-based/structured representation schemes → constraints and non-monotonic reasoning →
  planning → temporal and probabilistic reasoning → current trends); closing discussion on how
  these formalisms relate to and extend what *Introduction to Artificial Intelligence* covers only
  briefly, and where KR&R research is headed (knowledge graphs, neuro-symbolic AI).
- **Deliverable:** Capstone project final submission + presentation.

## Week 17 — Final Exam Week
- Comprehensive final exam, weighted toward Weeks 9–16 content (per Assessment Plan).

---

## Capstone Mini-Project (introduced Week 10, proposal Week 11, final Week 16)
Students (individually or in pairs) design and build a small **knowledge-based reasoning system
in Python that integrates at least two distinct formalisms from this course** end-to-end: problem
formulation → representation design → implementation → evaluation/demonstration → a short written
report and a 5–7 minute presentation. Example topics: a rule-based expert system over a narrow
domain (e.g., simple fault diagnosis or animal/plant identification) whose facts are organized as
a frame-based taxonomy with inheritance and defaults (integrating Weeks 5 and 6); a CSP-based
configuration or scheduling tool whose item types are organized under an ontology-style taxonomy
with inheritance of constraints (integrating Weeks 6/7 and 8); a small STRIPS planner whose
initial-state facts are derived by a non-monotonic default-reasoning step before planning begins
(integrating Weeks 9 and 10); or an integrated knowledge-based agent (Week 14 style) extended with
a Bayesian-network module for one uncertain sub-decision (integrating Weeks 5/6 and 12). The
capstone must be built on formalisms covered in this course; techniques beyond the syllabus
require instructor pre-approval and are not required for full credit.
