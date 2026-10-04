# Course Contents: Knowledge Representation and Reasoning (Graduate)

Detailed per-week breakdown of topics, subtopics, and resources. Companion to `course-plan.md`.
Each week lists: **Topics**, **Subtopics/Skills**, **Readings**, **Software/Libraries used**.

---

## Week 1 — Graduate KR&R Overview
- **Topics:** Course goals and expectations, including the research capstone; a rapid review,
  stated explicitly as assumed and not re-taught, of the undergraduate KR&R foundations this
  course builds on — propositional/FOL logic and resolution, unification, rule-based production
  systems, semantic networks/frames with non-monotonic inheritance, description-logic/ontology
  basics, CSP/AC-3, non-monotonic reasoning basics (CWA, default logic, circumscription),
  STRIPS/HTN planning, Allen's interval algebra, Bayesian-network inference by variable
  elimination, and a brief MLN/fuzzy-logic survey (also assumed is SAT/DPLL, owned by *Artificial
  Intelligence*, Graduate, Week 6); a map of the landscape of advanced KR research this course
  covers — modal/temporal logic, tableau-based automated theorem proving and DL complexity,
  Answer Set Programming, AGM belief revision, abstract argumentation, MLNs in depth, knowledge
  graphs/embeddings, multi-agent epistemic logic, ontology engineering practice, and
  explainability in symbolic reasoning — and how this course is scoped to avoid duplicating its
  siblings (the undergraduate KR&R course, *Artificial Intelligence* Graduate's SAT/DPLL and
  game-theoretic multi-agent content, and *Machine Learning* Graduate's CRF/structured-prediction
  angle).
- **Subtopics/Skills:** restating, from memory, the core algorithm or postulate set for each
  assumed undergraduate topic in one sentence (a diagnostic, not new instruction); mapping a given
  advanced-KR research question onto this course's week map and naming one sibling graduate course
  it is explicitly not about.
- **Readings:** Brachman & Levesque, review of Ch. 2–3, 5, 9–10 (propositional/FOL logic,
  frames/defaults, description logics, non-monotonic reasoning — at review pace only); course
  syllabus.
- **Software:** Python 3.10+, Jupyter/Colab (environment setup only).

## Week 2 — Modal Logic
- **Topics:** Possible-worlds (Kripke) semantics — a frame ⟨W, R⟩ of worlds and an accessibility
  relation, a model ⟨W, R, V⟩ adding a valuation V, and the satisfaction relation for the
  necessity (□) and possibility (◇) operators: M,w ⊨ □φ iff M,v ⊨ φ for every v with wRv; M,w ⊨ ◇φ
  iff M,v ⊨ φ for some v with wRv; the correspondence theory linking properties of R to modal
  axioms and systems — no constraint on R gives the minimal normal modal logic **K**; reflexivity
  validates **T** (□φ→φ); reflexivity + transitivity gives **S4** (adds □φ→□□φ); reflexivity +
  symmetry + transitivity (R an equivalence relation) gives **S5** (adds ◇φ→□◇φ) — stated and
  used conceptually, without a full completeness proof; the epistemic reading of modal logic —
  □ as "the agent knows," with S5 as the standard logic of knowledge because indistinguishability
  of worlds is naturally an equivalence relation.
- **Subtopics/Skills:** evaluating a modal formula against a small, explicitly given Kripke model
  by hand; identifying, from a described accessibility relation's properties, which modal system
  (K, T, S4, S5) it validates; translating a simple knowledge statement ("agent a knows that if p
  then q") into a modal formula and evaluating it in a toy multi-world model.
- **Readings:** Fagin, Halpern, Moses & Vardi, Ch. 2 (Kripke semantics and the modal systems
  K–S5, introductory sections); Brachman & Levesque, as a point of contrast, does not cover modal
  logic in depth — this week is new material relative to the prerequisite course.
- **Software:** Python.

## Week 3 — Temporal Logic
- **Topics:** Why a single present-tense modality is too weak to reason about an evolving system;
  Linear Temporal Logic (LTL) over an execution trace (a sequence of states) — the **next**
  operator Xφ (true at step i iff φ holds at step i+1), **always** Gφ (φ holds at every step from
  i onward), **eventually** Fφ (φ holds at some step at or after i), and **until** φUψ (ψ holds at
  some step j ≥ i, and φ holds at every step from i up to but not including j); a brief,
  conceptual look at Computation Tree Logic (CTL) for branching time — path quantifiers **A**
  ("on all paths") and **E** ("on some path") combined with the temporal operators (e.g., AGφ,
  EFφ), reasoning over a branching computation tree rather than a single linear trace; applications
  in planning (expressing a goal as "eventually at a state satisfying G" or a safety constraint as
  "always avoid state B") and in model checking/verification (specifying and checking a system
  property such as "every request is eventually followed by a response").
- **Subtopics/Skills:** implementing an LTL evaluator over a finite execution trace and tracing
  it by hand on a small trace; translating an English planning/safety requirement into an LTL
  formula; explaining, at a conceptual level, why CTL's AGφ and EFφ are not interchangeable with
  LTL's Gφ and Fφ once branching is allowed.
- **Readings:** Fagin, Halpern, Moses & Vardi, Ch. 2 (temporal extensions, for the Kripke-semantics
  parallel to LTL/CTL's possible-worlds-over-time view); a standard model-checking reference
  (e.g., Clarke, Grumberg & Peled, *Model Checking*) for LTL/CTL syntax and semantics (external
  reference, not required purchase).
- **Software:** Python.

## Week 4 — Description Logics in Depth
- **Topics:** The tableau algorithm for deciding **ALC** concept satisfiability, worked through
  step by step — starting from a concept C, attempt to build a model by applying completion rules
  (⊓-rule: split a conjunction into both conjuncts on the same node; ⊔-rule: branch on a
  disjunction; ∃-rule: for ∃R.D on node x, create a new R-successor y and add D to y; ∀-rule: for
  ∀R.D on x and an existing R-successor y, add D to y) until either a **clash** is found (a node
  asserted both A and ¬A for some atomic concept A, meaning that branch fails) or no more rules
  apply (an open, clash-free branch is a model witnessing satisfiability); computational
  complexity of DL reasoning — ALC concept satisfiability is **PSPACE-complete** (stated and used
  as a real, standard result; the tableau algorithm's branching needs only polynomial *space* via
  careful reuse, which is what makes PSPACE-completeness, rather than something far worse, the
  right complexity class); how adding further constructors or unrestricted general TBoxes
  (as in more expressive DLs such as SHIQ) increases reasoning complexity further (e.g., to
  EXPTIME-complete), contrasted with OWL 2's tractable profiles — **OWL 2 EL** (based on EL++, no
  negation/disjunction, concept subsumption decidable in polynomial time; the profile used by
  large biomedical ontologies such as SNOMED CT), **OWL 2 QL** (based on DL-Lite, designed so that
  conjunctive-query answering can be rewritten into a relational-database query, with
  AC0/LogSpace data complexity), and **OWL 2 RL** (based on Description Logic Programs, reasoning
  implementable by a rule-based/forward-chaining engine in polynomial time) — each profile trades
  away some ALC expressiveness for a specific tractability guarantee.
- **Subtopics/Skills:** running the ALC tableau algorithm by hand on a small satisfiable concept
  and a small unsatisfiable (clashing) concept; implementing a tableau-based ALC satisfiability
  checker in Python for a small, fixed concept fragment (⊓, ¬, ∃R.C, ∀R.C) with a trace of rule
  applications and the clash (or open-branch model) found; stating the PSPACE-completeness result
  for ALC precisely and explaining, in one or two sentences, which OWL 2 profile would be chosen
  for a given application's expressiveness/tractability needs.
- **Readings:** Baader et al. (eds.), *The Description Logic Handbook*, Ch. 2 (the tableau
  algorithm, satisfiability of ALC) and Ch. 3 (complexity of DL reasoning); Brachman & Levesque
  Ch. 9 (description-logic basics, review only — this week goes well beyond it).
- **Software:** Python.
- **Assignment 1 assigned** (Weeks 1–4 content: modal logic, temporal logic, DL tableau and
  complexity).

## Week 5 — Automated Theorem Proving in Depth
- **Topics:** The sequent calculus, introduced conceptually — a sequent Γ ⊢ Δ asserts that the
  conjunction of formulas in Γ entails the disjunction of formulas in Δ; introduction rules build
  a connective on the right or left of a sequent from simpler sequents (e.g., **∧-right**: from
  Γ⊢Δ,φ and Γ⊢Δ,ψ infer Γ⊢Δ,φ∧ψ; **→-right**: from Γ,φ⊢Δ,ψ infer Γ⊢Δ,φ→ψ; **∀-right**: from
  Γ⊢Δ,φ[x/c] for a fresh constant c (the eigenvariable condition) infer Γ⊢Δ,∀x.φ); elimination
  (left) rules are the mirror image on the antecedent side; a derivation is a proof when every
  branch terminates in an axiom sequent (φ⊢φ); the tableau method for first-order satisfiability,
  building directly on the Week 4 DL-tableau idea but now for unrestricted FOL — ground and
  branching rules as before, plus **∀-rule** (a universal may be instantiated with any term,
  possibly needing several instantiations) and **∃-rule** (instantiate with a fresh Skolem
  constant, once), with a branch closing on a clash between a literal and its negation;
  resolution refinement strategies that extend the undergraduate course's basic (unrestricted)
  resolution — **set-of-support (SOS)**: partition the clause set into a "set of support" S
  (typically the negated goal/query) and the rest T; require every resolution step to use at least
  one parent from S or a descendant of one, which remains refutation-complete whenever T alone is
  satisfiable and prunes resolving within T, a major source of useless resolvents; **ordering
  strategies**: impose a term/literal ordering and only resolve on a clause's maximal literal(s)
  under that ordering, pruning symmetric/redundant derivations while preserving completeness.
- **Subtopics/Skills:** constructing a short sequent-calculus derivation by hand for a simple
  valid formula; building a first-order tableau by hand for a small unsatisfiable formula set,
  correctly applying the eigenvariable/fresh-constant conditions; implementing resolution with the
  set-of-support strategy in Python (reusing/extending the undergraduate course's resolution and
  unification code) and comparing the number of resolvents generated against unrestricted
  resolution on the same clause set.
- **Readings:** Baader et al. (eds.), *The Description Logic Handbook*, Ch. 2 (tableau methods,
  continued from Week 4, as the bridge to general FOL tableau); a standard automated-reasoning
  reference (e.g., Fitting, *First-Order Logic and Automated Theorem Proving*) for the sequent
  calculus and resolution refinements (external reference).
- **Software:** Python.

## Week 6 — Non-Monotonic Reasoning via Answer Set Programming
- **Topics:** The stable-model (answer-set) semantics for normal logic programs (Gelfond &
  Lifschitz): a program is a set of rules `h :- b_1,...,b_m, not c_1,...,not c_n` where `not` is
  **negation as failure**, not classical negation; given a candidate set of atoms M, the
  **Gelfond–Lifschitz reduct** P^M is formed by deleting every rule whose body contains `not c_i`
  for some `c_i ∈ M`, and deleting every remaining `not c_i` literal from the surviving rules'
  bodies (P^M is now a negation-free program with a unique least model); M is a **stable model**
  of P exactly when M equals the least model of P^M; ASP syntax — facts, rules, negation-as-
  failure, and **integrity constraints** (`:- body.`, a rule with an empty head that simply
  eliminates any candidate model satisfying its body); solving a small combinatorial problem —
  **graph coloring** — as an ASP program (a choice of color per vertex, expressed via the standard
  `1 {color(V,C) : color(C)} 1 :- vertex(V).` choice-rule idiom used by solvers such as `clingo`,
  together with a constraint `:- edge(V,W), color(V,C), color(W,C).` ruling out same-colored
  adjacent vertices); contrasting ASP's negation-as-failure and stable-model semantics with the
  undergraduate course's default logic and circumscription — both are non-monotonic, but ASP's
  semantics is defined model-theoretically over a fixed program via the reduct, giving it a direct
  computational realization (answer-set solvers) that default logic's extension-based semantics
  does not provide in the same way.
- **Subtopics/Skills:** computing the GL-reduct and checking stability of a candidate set by hand
  for a small program; implementing a brute-force stable-model checker in Python (enumerate
  candidate atom subsets, build the reduct, compute its least model via a simple fixpoint, check
  equality) and using it to solve a small graph-coloring instance; writing the equivalent
  `clingo`-syntax ASP program for the same instance as real-world context (not required to run).
- **Readings:** Gelfond, M. & Kahl, Y., *Knowledge Representation, Reasoning, and the Design of
  Intelligent Agents: The Answer-Set Programming Approach*, Ch. 1–4 (stable-model semantics, ASP
  syntax, representing a combinatorial problem as a program).
- **Software:** Python; `clingo` discussed conceptually as production ASP tooling (not required
  to install).
- **Quiz 1** (Week 2 content).

## Week 7 — Belief Revision and Update
- **Topics:** Rational belief change when a reasoner's belief set K must incorporate a new
  sentence φ — the **AGM postulates** (Alchourrón, Gärdenfors & Makinson) for belief **revision**
  K∗φ: (K∗1) K∗φ is a belief set (logically closed); (K∗2) φ ∈ K∗φ (success); (K∗3)
  K∗φ ⊆ K+φ (revision adds no more than plain logical expansion K+φ = Cn(K∪{φ}) would); (K∗4) if
  ¬φ ∉ K then K+φ ⊆ K∗φ (when φ does not contradict K, revision coincides with expansion); (K∗5)
  K∗φ = Cn({⊥}) (the absurd, everything-believing set) only if ⊨¬φ (φ is itself unsatisfiable);
  (K∗6) if φ and ψ are logically equivalent, K∗φ = K∗ψ; plus the supplementary postulates (K∗7)
  K∗(φ∧ψ) ⊆ (K∗φ)+ψ and (K∗8) if ¬ψ ∉ K∗φ then (K∗φ)+ψ ⊆ K∗(φ∧ψ); the distinction between
  **revision** (incorporating new information about a *static* world, which may force giving up
  some old beliefs that turn out to have been wrong) and **update** (reflecting that the world
  itself has *changed*, as formalized by the Katsuno–Mendelzon update postulates, which require
  minimal change *per possible world* of K rather than one globally minimal change) — the two are
  gotten wrong interchangeably but answer genuinely different questions.
- **Subtopics/Skills:** checking, for a small worked belief set and candidate revision outcome,
  whether each AGM postulate is satisfied; implementing a concrete AGM-satisfying revision
  operator — **Dalal revision** — representing a belief set K semantically as its set of
  satisfying propositional valuations (models), and defining K∗φ as the models of φ at minimum
  Hamming distance from the models of K; verifying on a small example that the Dalal operator
  satisfies the core AGM postulates (K∗1)–(K∗6); contrasting a revision scenario ("we learn the
  patient's test was mistaken") with an update scenario ("the patient's condition changed") on the
  same starting belief set and showing the two give different, both-correct answers.
- **Readings:** a standard belief-revision reference such as Gärdenfors, P., *Knowledge in Flux*,
  Ch. 3–4 (the AGM postulates, construction via epistemic entrenchment) (external reference, not
  required purchase); Fagin, Halpern, Moses & Vardi, as background on how belief sets relate to
  the epistemic-logic models used elsewhere in this course.
- **Software:** Python.
- **Quiz 2** (Week 4 content).

## Week 8 — Argumentation Frameworks; Midterm Review
- **Topics:** Dung's abstract **argumentation framework** AF = ⟨A, →⟩, a set of arguments A and
  an attack relation → ⊆ A×A, deliberately abstracting away each argument's internal structure to
  study acceptability purely in terms of attacks; a set S ⊆ A is **conflict-free** if no argument
  in S attacks another in S; an argument a is **acceptable with respect to** S (S *defends* a) if
  every attacker of a is itself attacked by some member of S; S is **admissible** if it is
  conflict-free and defends every one of its own members; the **grounded extension** is the
  least fixed point of the characteristic function F(S) = {a ∈ A : S defends a}, computed by
  iterating F from ∅ — it is unique and always exists; a **preferred extension** is a
  (⊆-)maximal admissible set — there may be several, and on a simple mutual-attack pair the
  grounded extension is empty while two distinct non-empty preferred extensions still exist;
  applications to
  reasoning with conflicting information (e.g., competing claims in a dispute, where arguments
  that survive in every/some preferred extension are the ones a rational agent can/may accept);
  review session for Weeks 1–7 ahead of the midterm.
- **Subtopics/Skills:** computing the grounded extension of a small AF by hand via fixpoint
  iteration of F; computing (or enumerating candidates for) the preferred extension(s) of a small
  AF, including a mutual-attack pair to see the grounded/preferred extensions diverge;
  implementing both computations in Python; practice problems for the midterm.
- **Readings:** Dung, P.M., "On the acceptability of arguments and its fundamental role in
  nonmonotonic reasoning, logic programming and n-person games," *Artificial Intelligence*, 1995
  (the original, foundational paper — assigned as the Week 13+ research-methods-style close
  reading this week, as an early, gentle example of reading a KR paper).
- **Software:** Python.
- **Quiz 3** (Week 6 content).
- **Assignment 2 assigned** (Weeks 5–8 content: sequent calculus/FOL tableau/resolution
  refinements, ASP, AGM belief revision/update, argumentation frameworks).

## Week 9 — Midterm Exam; Markov Logic Networks in Depth
- **Topics:** Midterm Exam (covers Weeks 1–8). Afterward: **Markov Logic Networks (MLNs)**,
  treated in genuine depth rather than the brief conceptual survey given in the undergraduate
  course — an MLN L is a set of pairs (F_i, w_i), each a first-order formula F_i with a real-
  valued weight w_i; given a finite domain of constants, **grounding** L produces a Markov
  network with one binary random variable per ground atom and one feature per *grounding* of each
  formula F_i; the induced **log-linear distribution** over possible worlds x (a truth assignment
  to every ground atom) is P(x) = (1/Z) · exp( Σ_i w_i · n_i(x) ), where n_i(x) counts how many
  groundings of F_i are satisfied in x, and Z = Σ_{x'} exp( Σ_i w_i · n_i(x') ) is the partition
  function normalizing over all possible worlds; a higher weight makes worlds that satisfy more
  groundings of that formula exponentially more probable without making any formula strictly
  required, and a formula with weight → ∞ behaves as a hard first-order constraint, recovering
  classical FOL as a limiting case; grounding and inference discussed conceptually at this scale —
  exact inference requires summing over an exponential number of worlds (tractable only for toy
  domains, as implemented in lab), while real-world MLN inference uses MAP (weighted-satisfiability
  style) or MCMC-based approximate marginal inference (described, not implemented).
- **Subtopics/Skills:** grounding a 2–3-constant, 2-formula toy MLN by hand (listing every ground
  atom and every formula grounding); implementing a brute-force Python MLN evaluator that
  enumerates all possible worlds over a small domain, computes each world's unnormalized score and
  the partition function Z, and reports the resulting probability distribution; varying one
  formula's weight and observing how the relative probabilities of worlds satisfying more or fewer
  of its groundings shift, including confirming that a very large weight concentrates probability
  on worlds that satisfy the formula in every grounding (recovering hard-constraint behavior).
- **Readings:** Richardson, M. & Domingos, P., "Markov Logic Networks," *Machine Learning*, 2006
  (the original MLN paper — the primary technical reference for this week's log-linear
  formulation).
- **Software:** Python.

## Week 10 — Knowledge Graphs
- **Topics:** Knowledge graphs as the large-scale, machine-queried descendant of semantic
  networks, represented as a set of **RDF triples** (subject, predicate, object) — e.g.,
  (Alice, worksFor, AcmeCorp); why a fixed symbolic graph alone is hard to search/generalize over
  at web scale, motivating **knowledge-graph embeddings**; **TransE** (Bordes et al., 2013), the
  foundational translation-based embedding model — each entity e and relation r is embedded as a
  vector in ℝ^d, and a valid triple (h, r, t) is modeled as a translation: **h + r ≈ t**; the
  scoring function f(h,r,t) = −‖h + r − t‖ (L1 or L2 norm; larger/less-negative score for triples
  closer to satisfying the translation, i.e. a plausible triple should have *small* ‖h+r−t‖);
  training by a margin-based ranking loss over a positive triple (h,r,t) and a **corrupted**
  negative triple (h′,r,t′) or (h,r,t′) (replace the head or tail with a random entity):
  L = Σ max(0, γ + d(h+r,t) − d(h′+r,t′)), where d(·) = ‖·‖ and γ > 0 is a margin, pushing positive
  triples' translation distance below negative triples' by at least the margin; **link
  prediction** as a reasoning task — given (h, r, ?), rank all candidate entities t by score and
  predict the top-ranked ones as the most plausible missing facts.
- **Subtopics/Skills:** encoding a small toy knowledge graph as RDF triples in Python; implementing
  a from-scratch TransE training loop with NumPy (random initialization, corrupted-triple
  sampling, margin-loss gradient steps) on the toy triple set; querying the trained embedding for
  link prediction on a held-out triple and inspecting whether the true tail entity ranks highly.
- **Readings:** Bordes, A., Usunier, N., Garcia-Durán, A., Weston, J. & Yakhnenko, O.,
  "Translating Embeddings for Modeling Multi-relational Data," *NeurIPS*, 2013 (the original
  TransE paper).
- **Software:** Python, NumPy.
- **Quiz 4** (Weeks 7–8 content).

## Week 11 — Reasoning over Knowledge Graphs
- **Topics:** **Rule mining** from graph data at a conceptual level — discovering closed-path
  (Horn) rules such as `worksFor(X,Y) ∧ locatedIn(Y,Z) ⇒ basedIn(X,Z)` by counting how often a
  candidate rule's body implies its head across the graph (**support**: how many times the rule
  fires correctly; **confidence**: support divided by how many times the body holds at all),
  mirroring the idea behind real systems such as AMIE, discussed conceptually; combining symbolic
  rules with learned embeddings — a grounded, non-hype survey of **neuro-symbolic reasoning over
  knowledge graphs**: using mined or hand-written rules to generate high-confidence candidate
  facts that a symbolic checker validates against the existing graph, using embedding-based
  scores (Week 10's TransE) to rank or filter candidates a purely symbolic rule miner would leave
  ambiguous, and being explicit about what each side contributes — rules give exact, explainable
  coverage where they apply but are brittle to noise and incompleteness; embeddings generalize
  smoothly and tolerate noise but give no explanation and can be wrong with high confidence.
- **Subtopics/Skills:** implementing a simple closed-path rule miner in Python over a toy
  knowledge graph (count support/confidence for a small set of candidate rule templates);
  combining the mined rules' candidate facts with the Week 10 TransE model's scores to rank
  candidate facts, and discussing, for a few examples, where the rule-based and embedding-based
  signals agree or disagree.
- **Readings:** Galárraga, L., Teflioudi, C., Hose, K. & Suchanek, F., "AMIE: Association Rule
  Mining under Incomplete Evidence in Ontological Knowledge Bases," *WWW*, 2013 (the reference
  for the rule-mining idea, read at a conceptual/survey level).
- **Software:** Python, NumPy.

## Week 12 — Multi-Agent Epistemic Reasoning
- **Topics:** Extending single-agent modal epistemic logic (Week 2) to groups of agents — for a
  group G, **"everyone in G knows" (E_G φ)** is Λ_{i∈G} K_iφ, modeled by the union R_E = ⋃_{i∈G}
  R_i of the agents' individual accessibility relations; **common knowledge C_Gφ** holds at a
  world w iff φ holds at every world reachable from w by a finite chain of individual-agent
  accessibility steps (R_i for any i ∈ G) — equivalently, C_Gφ is the infinite conjunction
  E_Gφ ∧ E_G E_Gφ ∧ ⋯, and semantically corresponds to the transitive closure of R_E; **distributed
  knowledge D_Gφ** ("what the group would know if members pooled their individual information")
  is modeled by the *intersection* R_D = ⋂_{i∈G} R_i — pooling information only ever shrinks the
  set of worlds the group jointly cannot distinguish, so D_Gφ can hold even when no individual
  agent knows φ; the **muddy children puzzle** as a standard worked illustration (Fagin, Halpern,
  Moses & Vardi): n children, k ≥ 1 of whom have a muddy forehead; each child sees every other
  child's forehead but not their own; the father truthfully and *publicly* announces "at least one
  of you has a muddy forehead" (creating common knowledge of this fact where before it may have
  been only individually/distributedly known), then repeatedly asks "does any of you know whether
  you are muddy?"; by induction on k, every child answers "no" for the first k−1 rounds, and on
  round k every muddy child simultaneously realizes they are muddy and answers "yes" — the public
  announcement is doing real epistemic work even though every child could already *see* who
  (else) was muddy; connections to multi-agent systems, explicitly contrasted with *Artificial
  Intelligence* Graduate's game-theoretic multi-agent treatment — this week's lens is what agents
  *know and believe* and how public communication changes it, not strategic payoffs or
  equilibrium behavior.
- **Subtopics/Skills:** constructing the Kripke model for a small muddy-children instance (worlds
  = subsets of "who is muddy," one accessibility relation per child relating worlds that agree on
  every bit except possibly that child's own); implementing a simulation in Python that repeatedly
  eliminates worlds inconsistent with each public "no" announcement (a simple public-announcement-
  logic update) and determines, for given n and k, the exact round at which the muddy children
  announce "yes"; explaining, for a small G, the difference between E_Gφ, C_Gφ, and D_Gφ on one
  worked example where all three differ.
- **Readings:** Fagin, R., Halpern, J., Moses, Y. & Vardi, M., *Reasoning About Knowledge*,
  Ch. 2 (common and distributed knowledge) and Ch. 6 (the muddy-children puzzle and public
  announcements) — the canonical textbook treatment this week follows.
- **Software:** Python.
- **Quiz 5** (Weeks 10–11 content).
- **Assignment 3 assigned** (Weeks 9–12 content: MLNs, knowledge graphs/embeddings, KG
  reasoning, multi-agent epistemic logic).

## Week 13 — Ontology Engineering in Practice
- **Topics:** Methodologies for building and maintaining ontologies — an iterative development
  cycle (specification of scope and competency questions, conceptualization, formalization,
  implementation, evaluation, and maintenance as requirements evolve), and why competency
  questions (concrete questions the ontology must be able to answer) drive good scoping decisions;
  **ontology alignment/matching** at a conceptual level — proposing correspondences (equivalence,
  subsumption) between concepts of two independently built ontologies using lexical/string-based
  similarity (e.g., normalized label similarity), structural similarity (how a concept's position
  in its hierarchy compares), and instance-based similarity (shared individuals), and why
  alignment is hard in practice (synonymy, differing granularity, differing modeling choices for
  the "same" real-world concept); real DL reasoner tooling, now discussed as practice rather than
  abstract context — **Pellet** and **HermiT** as tableau-based OWL DL reasoners implementing the
  Week 4 algorithm (and its extensions) at production scale, and **Protégé** as the standard
  ontology-editing environment that calls such a reasoner to check consistency and classify a
  growing ontology as it is built.
- **Subtopics/Skills:** writing a short set of competency questions for a toy domain ontology and
  checking, for a sketched class hierarchy, whether it can answer each one; implementing a toy
  lexical ontology matcher in Python (e.g., normalized edit distance or token-Jaccard similarity
  on class labels) over two small, independently named class lists, and manually auditing which
  proposed correspondences are correct, incorrect, or ambiguous; describing, for a given modeling
  decision, what Pellet/HermiT would need to check (consistency, classification, instance
  checking) and how Protégé would surface a detected inconsistency to the ontology engineer.
- **Readings:** Baader et al. (eds.), *The Description Logic Handbook*, Ch. 20 (ontology
  engineering and tooling, including reasoner-backed editors); Noy, N. & McGuinness, D.,
  "Ontology Development 101: A Guide to Creating Your First Ontology," Stanford KSL Technical
  Report, 2001 (a standard, widely used practitioner guide to the development methodology).
- **Software:** Python; Protégé, Pellet, and HermiT discussed conceptually (not required to
  install).
- **Quiz 6** (Weeks 12–13 content).
- **Paper Critique & Presentation assignment assigned** (student selects a real KR paper — e.g.,
  from the KR conference, an IJCAI/AAAI KR track, or the journal *Artificial Intelligence* —
  from a suggested-topics list covering any formalism in this course).

## Week 14 — Explainability and Reasoning
- **Topics:** Explanation generation in rule-based and logic-based systems — representing a
  derived conclusion's justification as a **proof tree** (each internal node a rule application,
  its children the facts/sub-conclusions that satisfied that rule's premises, down to leaf facts
  stated outright in the knowledge base), extending the undergraduate course's single-step
  derivation trace (Week 14 there) to a full recursive justification structure, plus a "why not"
  explanation (identifying which specific premise of which rule failed to block a query from being
  derived); contrasting the **inherent explainability of symbolic reasoning** — a proof tree is an
  exact, complete, human-checkable record of *why* a conclusion followed, because the inference
  steps themselves are the explanation — with **black-box machine-learning explainability**,
  where post-hoc methods (e.g., feature-perturbation/attribution techniques) only *approximate* a
  model's true decision boundary locally and are not guaranteed to be faithful to what the model
  actually computed; this comparison is kept brief and grounded — it is a contrast, not a tutorial
  on any specific ML explainability method, which belongs to the machine-learning-family courses.
- **Subtopics/Skills:** implementing a forward-chaining rule engine (reusing the undergraduate
  course's rule representation) augmented to build and print a full proof tree for a derived fact;
  implementing a "why not" explanation that, for a query that fails to derive, reports the first
  rule whose premises could not all be satisfied and which specific premise was missing; writing a
  short, precise paragraph contrasting proof-tree explanation with a feature-attribution-style ML
  explanation on the same toy decision, stating exactly what guarantee each one does and does not
  provide.
- **Readings:** Brachman & Levesque, Ch. 7 (rules and derivation, as the foundation this week's
  proof trees extend); a current survey on explainable AI (e.g., a KR-venue or AIJ paper on
  explanation in symbolic/hybrid systems), selected by the instructor each offering, read at a
  survey level for the ML-contrast section.
- **Software:** Python.

## Week 15 — Research Methods and Project Work Session
- **Topics:** How to read a KR research paper efficiently (abstract → results/examples → formal
  definitions → related work → full read) and how to critique one — is the formalism's claimed
  property (soundness, completeness, a complexity bound) actually established by the paper's
  argument, are the worked examples representative or cherry-picked, how does the proposed
  formalism/algorithm relate to the alternatives covered in this course; reproducibility concerns
  specific to KR research (is the proposed logic's semantics fully and unambiguously specified,
  are complexity claims proved or only conjectured, is example code/encoding available);
  structured, instructor-guided capstone work time — finalizing the literature review, designing
  and running the small reproduced/extended experiment, and drafting the written paper; written
  Paper Critique due and in-class presentations during this week's seminar sessions.
- **Subtopics/Skills:** critiquing a short KR paper excerpt as a structured in-class exercise
  (identifying the claimed formal result, whether the paper's argument actually establishes it,
  and how the formalism compares to one covered in this course); giving and receiving structured,
  specific feedback on a capstone work-in-progress or a Paper Critique presentation.
- **Readings:** none assigned beyond each student's own capstone and Paper Critique materials.
- **Software:** whatever each capstone project requires (see individual proposals).
- **Deliverable:** Paper Critique written submission + in-class presentation (see
  `assignments/paper-critique-and-presentation.md`); capstone written-paper draft due.

## Week 16 — Capstone Research Presentations; Course Review
- **Topics:** Student capstone research presentations (conference-talk format: problem, related
  work, method/experiment, results, limitations, Q&A); recap of the course map (modal/temporal
  logic → DL tableau/complexity → sequent calculus/FOL tableau/resolution refinements → ASP → AGM
  belief revision → argumentation → MLNs → knowledge graphs/embeddings → neuro-symbolic KG
  reasoning → multi-agent epistemic logic → ontology engineering → explainability); closing
  discussion connecting this course's advanced formalisms back to the undergraduate KR&R
  foundations they build on, and forward to where KR&R research is headed (large-scale knowledge
  graphs, neuro-symbolic integration, and explainable symbolic/hybrid reasoning).
- **Deliverable:** Capstone final paper submission + conference-style presentation.

## Week 17 — Final Exam Week
- Comprehensive final exam, weighted toward Weeks 9–15 content (per Assessment Plan).

---

## Research Capstone (topic selection Weeks 7–8, proposal due end of Week 8, work session Week
15, presentations Week 16)
Students (individually or in pairs) choose an advanced KR subtopic covered in this course and
complete a research-style project with four required components: (1) a **literature review** of
3–5 relevant papers summarizing the state of the art and the specific gap or question the project
addresses; (2) a **small reproduced or extended experiment or implementation** — e.g., reproducing
a core result from one of the reviewed papers at small scale, or extending/varying it in a focused
way (a new ALC tableau optimization, a different ASP encoding of a combinatorial problem, an AGM-
style revision operator compared against Dalal revision, a different argumentation semantics
compared against grounded/preferred, a larger-domain MLN grounding experiment, a TransE variant or
a different corruption strategy, a muddy-children-style public-announcement puzzle variant); (3) a
**short written paper** (introduction, related work, method, results, limitations, in a
conference-short-paper style); and (4) a **conference-style presentation** in Week 16. The project
must build on formalisms covered in this course; topics requiring deep neural-network, classical-
ML, or deep-learning content (owned by the sibling graduate courses) require instructor
pre-approval and must still center on this course's own techniques. See
`assignments/capstone-proposal-guidelines.md` and `assignments/capstone-rubric.md`.
