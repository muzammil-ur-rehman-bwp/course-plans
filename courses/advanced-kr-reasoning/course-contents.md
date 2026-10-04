# Course Contents: Advanced Knowledge Representation and Reasoning (Post Graduate)

Detailed per-week breakdown of topics, subtopics, and resources. Companion to `course-plan.md`.
Each week lists: **Topics**, **Subtopics/Skills**, **Readings**, **Software/Libraries used**.

---

## Week 1 — Postgraduate Overview
- **Topics:** Course goals and expectations, including the research-proposal capstone; a rapid
  review, stated explicitly as assumed and not re-taught, of the graduate KR&R foundations this
  course builds on — modal/temporal logic (Kripke semantics, K/S4/S5, LTL), the ALC tableau
  method and DL complexity, Answer Set Programming's stable-model semantics, the AGM postulates,
  Dung's abstract argumentation framework, Markov Logic Networks' log-linear formulation,
  RDF/TransE knowledge-graph embeddings, multi-agent epistemic logic (common/distributed
  knowledge), ontology engineering practice, and symbolic explainability (proof trees); a map of
  the landscape of open problems in advanced KR research this course covers — higher-order
  logic/type theory, many-valued/paraconsistent logics, SROIQ and OWL 2 DL, structured
  argumentation (ASPIC+), probabilistic logic programming (distribution semantics), neuro-symbolic
  integration, formal verification/model checking, multi-agent belief merging, explanation
  research for expressive ontologies, and ontology evolution; how this course is scoped to avoid
  duplicating the graduate course and the sibling postgraduate courses (Advanced AI's regret
  theory/game theory/alignment, Advanced ANN/ML/DL's architectures and learning theory); an early
  introduction to how to scope a research proposal (problem statement, related-work survey,
  proposed approach, feasibility argument) — full treatment in Week 13, previewed now so students
  can begin thinking about a capstone direction immediately.
- **Subtopics/Skills:** restating, from memory, the core formalism or result for each assumed
  graduate-level topic in one sentence (a diagnostic, not new instruction); mapping a given
  advanced-KR research question onto this course's week map and naming one sibling course
  (graduate KR&R or a postgraduate sibling) it is explicitly not about; sketching a one-paragraph
  tentative research direction of personal interest, to be refined across the semester.
- **Readings:** Baader et al. (eds.), *The Description Logic Handbook*, review of Ch. 2–3 (ALC
  tableau and DL complexity, at review pace only, as the direct jumping-off point for Week 4);
  course syllabus.
- **Software:** Python 3.10+, Jupyter/Colab (environment setup only).

## Week 2 — Higher-Order Logic and Type Theory
- **Topics:** Why first-order logic has real expressive limits — FOL can quantify over
  individuals but not over predicates or relations themselves, so a genuinely second-order
  statement such as "every property that holds of the empty set and is preserved by successor
  holds of every natural number" (induction, stated as a single sentence rather than a
  schema) cannot be expressed in FOL; **second-order and higher-order logic** introduced as
  quantification over predicates, functions, and predicates-of-predicates; the **simply-typed
  lambda calculus** as a foundation for higher-order representation — a type system built from
  base types (e.g., `e` for entities, `t` for truth values) and function types `σ → τ`, terms
  formed by abstraction (`λx:σ. M`) and application (`M N`), and **β-reduction**
  ((λx. M) N → M[x:=N]) as the computational engine identifying a function applied to an argument
  with its result; why this matters for KR — a typed lambda term can represent a relation as a
  first-class object that itself can be passed as an argument or quantified over (e.g.,
  `λR. λx. R x x` denotes "the property of being reflexive under R," a legitimate higher-order
  object); connections to how modern KR/NLP systems use typed representations (e.g., Montague-
  style compositional semantics builds sentence meaning by function application of typed lambda
  terms, and typed feature structures in some ontology formalisms use the same type-discipline
  idea to prevent ill-formed compositions) — surveyed at a conceptual level, not implemented as a
  full system.
- **Subtopics/Skills:** stating, precisely, one property FOL cannot express that second-order
  logic can, and explaining why; implementing a small simply-typed lambda calculus interpreter in
  Python (type-checking a term against a simple type signature, and performing β-reduction to
  normal form) over a handful of toy terms; translating a short natural-language relational
  statement into a typed lambda term and reducing an application of it by hand and in code.
- **Readings:** a standard higher-order logic / type theory introduction (e.g., the relevant
  chapters of Barendregt's survey material on the lambda calculus, or a type-theory textbook's
  introductory chapters on simple types) — external reference, conceptual depth only; this
  course does not require a full type-theory textbook purchase.
- **Software:** Python.

## Week 3 — Many-Valued and Paraconsistent Logics
- **Topics:** Why classical two-valued logic is sometimes the wrong tool — a knowledge base with
  genuinely incomplete or genuinely contradictory information needs a semantics that does not
  collapse "unknown" into "false" or let one contradiction prove everything; **Kleene's
  three-valued logic** (strong Kleene, K3) with truth values {T, F, U} (true, false, unknown) and
  truth tables defined so that a connective's value is U exactly when the classical value cannot
  be determined regardless of how U resolves (e.g., `U ∧ F = F` because F alone already forces
  falsity, but `U ∧ T = U`; `U ∨ T = T`; `¬U = U`); **Łukasiewicz's three-valued logic** (Ł3),
  which agrees with Kleene on conjunction/disjunction/negation but gives **implication** a
  different, non-classical table — Łukasiewicz implication is designed so that `U → U = T` (not U
  as Kleene's material-implication-style reading would give), reflecting a "degree of truth
  preservation" reading rather than Kleene's "determinacy" reading; the two logics are contrasted
  directly on this single differing case, since it is the cleanest way to see they encode
  different philosophical commitments about partial truth; **paraconsistent logics** — systems
  designed to tolerate contradictions (both `φ` and `¬φ` holding) without **explosion** (the
  classical result that `{φ, ¬φ} ⊨ ψ` for every ψ); the key move is rejecting or restricting the
  classical inference rule *ex contradictione sequitur quodlibet*, typically by using a
  **paraconsistent negation** under which `φ ∧ ¬φ` does not semantically force every other
  formula to be true (e.g., a four-valued Belnap/Dunn-style logic with values {T, F, Both,
  Neither}, where "Both" is a genuine, non-explosive truth value distinct from classical
  contradiction); applications to reasoning over information sources that genuinely disagree
  (e.g., merging two sensor feeds or two conflicting expert knowledge bases) without the entire
  system becoming useless the moment one conflict is found.
- **Subtopics/Skills:** implementing a Python truth-table evaluator for Kleene's K3 and
  Łukasiewicz's Ł3 over formulas built from ∧, ∨, ¬, → and comparing their outputs on a formula
  set chosen specifically to expose the implication difference; implementing a small
  Belnap/Dunn four-valued evaluator and demonstrating, on a toy knowledge base with one direct
  contradiction, that a query unrelated to the contradiction is not trivially derivable (no
  explosion), contrasted explicitly with what classical logic would conclude from the same
  premises.
- **Readings:** a standard many-valued-logic survey (e.g., the relevant sections of Priest's work
  on paraconsistent logic, or a many-valued-logics handbook chapter) for Kleene/Łukasiewicz truth
  tables and the Belnap/Dunn four-valued system — external reference, conceptual depth.
- **Software:** Python.
- **Quiz 1** (Week 2 content).

## Week 4 — Advanced Description Logics: SROIQ
- **Topics:** **SROIQ**, the expressive description logic underlying **OWL 2 DL**, introduced as
  a direct extension of the graduate course's **ALC** — recall ALC's constructors (⊓, ⊔, ¬, ∃R.C,
  ∀R.C) and its tableau completion rules before adding: **role hierarchies** (R ⊑ S: every
  R-related pair is also S-related, e.g. `hasMother ⊑ hasParent`); **complex role inclusion
  axioms** (role chains such as R∘S ⊑ T, e.g. `hasParent ∘ hasParent ⊑ hasGrandparent`, with the
  **regularity condition** on the set of role inclusion axioms required for decidability — the
  axioms must not create certain cyclic dependency patterns, stated here as a real, named
  restriction rather than derived in full); **number restrictions** (≥n R.C and ≤n R.C, "at least/
  at most n R-successors satisfying C," generalizing ALC's lack of cardinality constructs, together
  with **qualified** number restrictions which constrain the successors' type C, not just their
  count); **nominals** (O: a concept `{a}` naming a single, specific individual, letting an
  individual be used inside a concept expression — e.g. `hasCapital.{Paris}` — which is what
  lets SROIQ express "exactly this one named thing" rather than only general class structure);
  **role characteristics** — reflexive/irreflexive, symmetric/asymmetric, transitive, and
  disjoint roles, each a further named axiom kind; extending the ALC tableau method to this richer
  language — the ⊓/⊔/∃/∀ rules carry over, and a **≥-rule** (for ≥n R.C on x, if x does not yet
  have n R-successors satisfying C, create them, pairwise-distinct) and a **≤-rule** (for ≤n R.C
  on x, if x has more than n R-successors satisfying C, *merge* two of them — requiring an
  equality/inequality bookkeeping step not needed in ALC) are sketched as the key new completion
  rules, with role-hierarchy propagation (applying ∀R.C along any sub-role of R, not just R
  itself) and nominal-driven merging noted as additional bookkeeping; the resulting
  complexity/decidability picture, stated accurately — SROIQ concept satisfiability is
  **N2ExpTime-complete** (double-exponential time), a genuine jump from ALC's PSPACE-completeness
  and from simpler expressive DLs such as SHIQ (EXPTIME-complete), the price paid for nominals and
  complex role inclusions; SROIQ is still **decidable** provided the regularity condition on role
  inclusions is respected (dropping it can lose decidability entirely) — contrasted with OWL 2's
  tractable profiles (EL, QL, RL, owned by the graduate course's Week 4) which deliberately give
  up SROIQ's expressiveness for polynomial-time or better reasoning.
- **Subtopics/Skills:** tracing, by hand, how a ≥-rule and a ≤-rule application would proceed on
  a small SROIQ-style concept with a qualified number restriction, including the merge step the
  ≤-rule requires; implementing a from-scratch Python satisfiability checker for a small, fixed
  SROIQ-like fragment (ALC's constructors plus unqualified number restrictions and a simple role
  hierarchy) that applies role-hierarchy propagation correctly and reports a clash or an open
  model; stating the N2ExpTime-completeness result for SROIQ precisely, and explaining in 2–3
  sentences why nominals and complex role inclusions are the specific constructs responsible for
  the complexity jump beyond ALC/SHIQ.
- **Readings:** Baader et al. (eds.), *The Description Logic Handbook* — the chapters covering
  expressive description logics and the SROIQ family (role hierarchies, number restrictions,
  nominals) and the complexity results for expressive DLs, read as the direct continuation of the
  graduate course's Week 4 ALC chapter.
- **Software:** Python.
- **Assignment 1 assigned** (Weeks 1–4 content: higher-order logic/type theory, many-valued and
  paraconsistent logics, SROIQ tableau extension and complexity).

## Week 5 — Structured Argumentation: ASPIC+
- **Topics:** The **ASPIC+** framework for *structured* argumentation, contrasted directly with
  the graduate course's Dung abstract argumentation framework, where arguments are opaque nodes
  with no internal structure — ASPIC+ instead builds a concrete **argumentation system**
  from: a knowledge base of **premises** (some ordinary, some axiomatic/unassailable); **strict
  inference rules** (`φ1,...,φn → φ`, classical-logic-like: if the premises hold, the conclusion
  is beyond question); and **defeasible inference rules** (`φ1,...,φn ⇒ φ`, holding only
  presumptively — "normally, if φ1,...,φn then φ"); an **argument** is then built recursively as
  a tree: a premise is a (trivial) argument for itself, and applying a rule to arguments for its
  premises yields a larger argument for the rule's conclusion, with the argument's **top rule**
  being the last rule applied; two distinct **attack** types on an argument B (both only possible
  on a defeasible element, never on a strict one) — a **rebutting attack**: argument A rebuts B on
  a sub-argument B′ of B if A's conclusion contradicts B′'s conclusion, and B′'s top rule is
  defeasible (rebuttal targets a *conclusion*); an **undercutting attack**: argument A undercuts B
  on a sub-argument B′ of B if A's conclusion specifically denies the applicability of B′'s last
  (defeasible) rule — formally, by concluding a special proposition naming that rule's
  non-applicability (undercutting targets a *rule's licence to fire*, not any conclusion it or
  any other argument reaches); a third attack type, **undermining** (attacking an ordinary,
  non-axiomatic premise directly), is introduced briefly as the premise-level counterpart of
  rebuttal; how a **preference ordering** over defeasible rules/arguments determines which attacks
  actually **defeat** (succeed against) their target, after which Dung-style semantics (grounded/
  preferred extensions, owned by the graduate course) can be applied on top of the resulting
  structured attack-and-defeat graph — ASPIC+ is explicitly a layer that *produces* a Dung
  framework from structured ingredients, not a replacement for Dung's acceptability semantics.
- **Subtopics/Skills:** building a small ASPIC+ argumentation system by hand from a toy set of
  strict rules, defeasible rules, and premises, and constructing the arguments it licenses as
  explicit trees; for a given pair of arguments, correctly classifying an attack between them as
  rebutting, undercutting, or undermining, with the sub-argument/rule being targeted stated
  explicitly; implementing a Python ASPIC+ argument-construction-and-attack calculator that, given
  rules and premises, enumerates arguments up to a bounded depth, builds the attack relation
  (tagged by type), and hands the resulting graph to a reused graduate-course grounded-extension
  computation.
- **Readings:** Modgil, S. & Prakken, H. — the ASPIC+ structured-argumentation papers (the
  primary source for the rules/premises/attack-type framework); Prakken, H. & Vreeswijk, G. — the
  survey literature on logics for structured argumentation, for the rebutting/undercutting/
  undermining distinction's broader context.
- **Software:** Python.

## Week 6 — Probabilistic Logic Programming
- **Topics:** The **distribution semantics** for probabilistic logic programs, as popularized by
  ProbLog — a program consists of a set of **probabilistic facts**, each written `p :: fact`
  (meaning `fact` is true with probability p, independently of every other probabilistic fact),
  together with ordinary definite-clause rules built on top of them; a **total choice** is a
  selection, for every probabilistic fact, of whether it is included or excluded, and each total
  choice induces a unique ordinary logic program (and hence a unique least model, exactly as in
  the graduate course's ASP groundwork minus any `not`); the probability of a total choice is the
  product of p for every included fact and (1−p) for every excluded one (independence is a
  defining assumption of the semantics, not an approximation); the probability of a query atom q
  is then the sum, over every total choice whose induced least model entails q, of that total
  choice's probability — `P(q) = Σ_{total choices θ : q ∈ LM(P_θ)} P(θ)`; this is formally a
  **discrete mixture over possible worlds** in the same spirit as the graduate course's MLN
  log-linear distribution over worlds, but structurally different — an MLN assigns a world's
  (unnormalized) probability from weighted formula satisfaction counts and needs a partition
  function Z, while the distribution semantics assigns a world's probability directly as a
  product of independent fact-level probabilities, with no partition function required, and rules
  are hard logical consequences rather than weighted soft constraints; inference, discussed
  conceptually at this scale — brute-force enumeration of all 2^k total choices (k = number of
  probabilistic facts) is the teaching-scale approach used in lab, while production systems such
  as ProbLog compile the relevant Boolean formula (which total choices entail q) to a **Binary
  Decision Diagram (BDD)** to compute the probability in time manageable for realistic k by
  exploiting shared sub-structure, avoiding explicit enumeration (described, not implemented).
- **Subtopics/Skills:** computing, by hand, the probability of a simple query in a 2–3-fact
  probabilistic logic program by enumerating total choices; implementing a brute-force Python
  distribution-semantics evaluator (enumerate all total choices, build each induced program's
  least model via simple fixpoint iteration reused from the graduate course's ASP lab, sum the
  probabilities of choices entailing the query) on a small program with probabilistic facts and
  rules; explicitly contrasting, in a short written paragraph, the distribution semantics'
  independent-fact mixture-over-worlds view against the graduate course's MLN log-linear view on
  the same toy domain recast in both formalisms.
- **Readings:** De Raedt, L., Kimmig, A., and co-authors — the probabilistic logic programming /
  ProbLog literature (the primary source for the distribution semantics and its relation to
  Sato's original distribution-semantics formulation for logic programs).
- **Software:** Python; `problog` discussed conceptually as production probabilistic-logic-
  programming tooling (not required to install — all graded code is a from-scratch brute-force
  evaluator).
- **Quiz 2** (Weeks 3–4 content).

## Week 7 — Neuro-Symbolic Integration I: Differentiable and Fuzzy Logic
- **Topics:** Why making logic **differentiable** matters — a classical Boolean formula's
  truth value is a step function of its inputs (no useful gradient), which makes it impossible to
  learn the *weights/parameters* of a symbolic rule system by gradient descent the way a neural
  network's weights are learned; **fuzzy/differentiable relaxations** replace each Boolean
  connective with a smooth, real-valued ([0,1]-valued) function that specializes to the Boolean
  case at the extremes 0/1 — a **t-norm** generalizes conjunction (any function T: [0,1]²→[0,1]
  that is commutative, associative, monotonic, and has 1 as identity); the **product t-norm**
  `T(a,b) = a·b` is the most common differentiable choice (smooth, exact gradient everywhere), with
  the **Gödel (minimum) t-norm** `T(a,b) = min(a,b)` and the **Łukasiewicz t-norm**
  `T(a,b) = max(0, a+b−1)` as the other two standard choices, each with a different gradient
  behavior (min has a zero gradient on the non-minimal argument almost everywhere, which is a
  real practical liability for learning — the product t-norm is usually preferred specifically
  because every input continues to receive gradient signal); the corresponding **t-conorm**
  generalizes disjunction via De Morgan duality (`S(a,b) = a+b−a·b` for the product/probabilistic-
  sum pairing); **negation** is standardly relaxed as `¬a = 1−a`; a soft implication can be built
  from these (e.g., `a→b` relaxed as `S(¬a, b)` under a chosen t-conorm, or via the Łukasiewicz
  implication `min(1, 1−a+b)`); given a set of soft logical constraints, a **loss function**
  `L = Σ (1 − soft-truth-value of constraint)` (or `-log` of the soft truth value, for a
  probabilistic reading) can be minimized by ordinary gradient descent, so that the parameters
  feeding into the fuzzy truth values of atomic propositions (e.g., produced by a neural network's
  output layer) are nudged toward satisfying the symbolic constraints — this is the core mechanism
  by which systems in this research family (e.g., Logic Tensor Networks and related frameworks,
  surveyed conceptually) inject symbolic domain knowledge into a trainable, gradient-based model.
- **Subtopics/Skills:** implementing, in NumPy or plain Python, the product, Gödel, and
  Łukasiewicz t-norms/t-conorms and the standard negation, and evaluating a small formula under
  each to see where they agree and disagree; implementing a differentiable soft-logic loss in
  PyTorch (or NumPy with manually coded gradients) for a toy constraint (e.g., a soft version of
  `∀x. P(x) → Q(x)` over a small batch of learnable or given truth-degree vectors) and running
  gradient descent to show the constraint's soft satisfaction increasing over training steps;
  explaining, in 3–5 sentences, specifically why the product t-norm's gradient behavior makes it
  the more practical choice for learning compared to the Gödel t-norm's near-everywhere-zero
  gradient on the non-minimal input.
- **Readings:** current KR/neuro-symbolic literature on differentiable/fuzzy logic for learning
  (e.g., the Logic Tensor Networks line of work and related t-norm-based neuro-symbolic papers
  from recent KR, IJCAI, or AAAI proceedings), selected by the instructor each offering as this
  area moves quickly.
- **Software:** Python, NumPy, PyTorch (or NumPy with manual gradients as a fallback).
- **Quiz 3** (Weeks 5–6 content).

## Week 8 — Neuro-Symbolic Integration II; Midterm Review
- **Topics:** A conceptual survey of **neural theorem proving** — embedding-based approaches
  (e.g., the Neural Theorem Prover line of work) that replace a symbolic unification step in a
  backward-chaining proof search with a *soft, differentiable* unification score between
  embedded symbols (two atoms "unify" to a degree given by the similarity of their embeddings,
  rather than exactly or not at all), letting the system learn to generalize a rule-based proof
  procedure over facts it was not explicitly given, at the cost of exactness and interpretability
  — surveyed at the level of what problem it solves and why it is hard (proof search over a soft,
  continuous unification space is far larger and noisier than exact symbolic search), not
  re-derived in full; combining **knowledge-graph embeddings** (the graduate course's TransE) with
  **explicit logical constraints** — a grounded, non-hype pattern in which embedding-based link
  prediction proposes candidate facts, and explicit hard or soft logical rules (Week 7's soft
  logic, or ordinary symbolic rules) filter, re-rank, or veto candidates that violate known
  constraints (e.g., a learned rule that a person cannot be their own parent used to veto a
  TransE-proposed triple), in the same spirit as the graduate course's Week 11 rule-mining-plus-
  embedding survey but now explicitly through the lens of this week's differentiable-constraint
  machinery rather than independent rule mining; being explicit about what neuro-symbolic
  integration contributes and does not — it is a genuine, still-unsettled research frontier for
  combining the generalization/noise-tolerance of learned representations with the exactness/
  explainability of symbolic constraints, not a solved problem; midterm review session covering
  Weeks 1–8 (higher-order logic/type theory, many-valued/paraconsistent logics, SROIQ, ASPIC+,
  probabilistic logic programming, differentiable/fuzzy logic).
- **Subtopics/Skills:** explaining, in a short written paragraph, how soft unification in neural
  theorem proving differs from the graduate course's exact unification in resolution/tableau
  methods, and what is gained and lost by the relaxation; implementing a small Python pipeline
  that takes a toy TransE-style embedding's top-ranked candidate facts (reusing the graduate
  course's Week 10 TransE lab) and filters them against a small set of hand-written hard
  constraints (e.g., type constraints, a no-self-loop constraint on an irreflexive relation),
  reporting which candidates survive; practice problems spanning Weeks 1–8 for the midterm.
- **Readings:** current neuro-symbolic-integration literature on neural theorem proving and
  embedding-plus-constraint hybrids (recent KR, IJCAI, AAAI, or NeurIPS papers), selected by the
  instructor each offering.
- **Software:** Python, NumPy.

## Week 9 — Midterm Exam; Formal Verification for Knowledge-Based Systems
- **Topics:** Midterm Exam (qualifying-exam style, covers Weeks 1–8). Afterward: **model
  checking**, built rigorously on the graduate course's LTL introduction — the **model-checking
  problem**: given a finite **Kripke structure** M = ⟨S, S₀, R, L⟩ (a finite set of states S, a
  set of initial states S₀ ⊆ S, a total transition relation R ⊆ S×S, and a labeling function L
  assigning each state the set of atomic propositions true there) and a temporal-logic formula φ
  (here, LTL), decide whether **M ⊨ φ** — every infinite path through M starting from an initial
  state satisfies φ; the standard **automata-theoretic approach**, stated precisely at a
  conceptual level — translate ¬φ into a **Büchi automaton** A_¬φ accepting exactly the infinite
  words violating φ, compute the product M ⊗ A_¬φ, and check whether this product has a reachable
  **accepting cycle** (a cycle through an accepting state, reachable from an initial state); M ⊨ φ
  exactly when no such cycle exists — i.e., when M, running in lockstep with A_¬φ, can never
  satisfy A_¬φ's acceptance condition forever, so no execution of M actually violates φ; LTL model
  checking is **PSPACE-complete** in the size of the formula (stated and used as a real, standard
  result — the Büchi automaton for φ can be exponentially larger than φ, but reachability/cycle
  detection in the product is checked without building the whole automaton explicitly, keeping
  space polynomial) and polynomial in the size of the model M (the real practical bottleneck in
  industrial model checking is the model's state-space size, addressed by techniques such as
  symbolic/BDD-based model checking, described conceptually, not implemented); applying this to
  checking whether a **knowledge base or agent specification** satisfies a property — e.g.,
  modeling an agent's belief-update protocol or a simple multi-agent system (connecting back to
  the graduate course's epistemic logic) as a finite Kripke structure over its possible
  information states, and checking an LTL safety property ("the agent never simultaneously
  believes p and ¬p") or liveness property ("every query is eventually answered").
- **Subtopics/Skills:** stating the model-checking problem M ⊨ φ precisely for a given finite
  Kripke structure and LTL formula; implementing, in Python, a small explicit-state LTL model
  checker over a finite state graph for a useful fragment of LTL (at minimum G, F, X, U, and
  Boolean connectives) via cycle detection in the state graph restricted to states consistent with
  the relevant sub-formulas (a simplified, teaching-scale stand-in for the full automata-theoretic
  product construction, clearly flagged as such) and using it to check a safety and a liveness
  property on a small example system; tracing, by hand, why a specific counterexample path
  (a reachable cycle violating the property) demonstrates M ⊭ φ.
- **Readings:** a standard model-checking reference (e.g., Clarke, Grumberg & Peled, *Model
  Checking*, or Baier & Katoen, *Principles of Model Checking*) for the automata-theoretic
  approach and PSPACE-completeness result — external reference, building directly on the
  graduate course's Week 3 LTL introduction and pointer to the same model-checking literature.
- **Software:** Python.

## Week 10 — Multi-Agent Belief Merging
- **Topics:** Extending the graduate course's single-agent **AGM belief revision** to **multiple
  agents**, each with their own belief set (or more generally, their own total preorder over
  possible worlds expressing degrees of plausibility), who must combine their individually
  consistent but possibly mutually **conflicting** belief sets into one merged result; **belief
  merging operators**, introduced at the semantic (model-based) level in the spirit of Dalal
  revision — represent each agent's belief set K_i by its set of models, and define a **distance-
  based merging operator** that selects the models minimizing an aggregate distance (e.g., summed
  or maximum Hamming distance, mirroring Dalal's per-world distance from the graduate course) to
  the profile of individual belief sets, subject to any hard integrity constraints IC the merged
  result must satisfy; the standard **merging postulates** a rational operator should satisfy
  (stated conceptually, in the spirit of the Konieczny–Pino Pérez postulates that generalize AGM
  to the multi-source case) — e.g., **(IC0)** the merged result entails the integrity constraints;
  **(IC1)** if the individual belief sets are jointly consistent (and consistent with IC), merging
  is equivalent to simple conjunction; **(IC2)** merging commutes with reordering the agents (the
  result should not depend on an arbitrary ordering of the inputs); **(IC3)** a form of logical
  equivalence invariance (merging gives logically equivalent results on logically equivalent
  inputs), paralleling AGM's own structure but now over a *profile* of belief sets rather than one
  belief set and one new sentence; the key conceptual difference from single-agent AGM revision —
  AGM revision has one privileged belief set receiving one new, trusted input, while merging has
  several belief sets of (in the base case) *equal standing*, none privileged, so the operator
  must adjudicate between peers rather than simply incorporate new information into an existing
  structure.
- **Subtopics/Skills:** computing, by hand, the result of a small distance-based merging operator
  on 2–3 toy agent belief sets (each given as a small propositional formula) under a stated
  integrity constraint, by enumerating valuations and their aggregate distances; implementing a
  Python distance-based belief-merging evaluator (reusing the graduate course's Dalal-revision
  model-enumeration machinery, generalized to several input belief sets and an aggregate-distance
  objective) and checking, on a worked example, that the result satisfies a couple of the stated
  merging postulates; contrasting, in a short written paragraph, a merging scenario (several peer
  agents' pre-existing, equally trusted beliefs about a static fact are combined) against an AGM
  revision scenario (one agent's existing beliefs are revised by one new, trusted input) on
  structurally similar toy examples, to show precisely where the two formalisms diverge.
- **Readings:** the belief-merging literature extending AGM to multiple sources (e.g., the
  Konieczny & Pino Pérez line of work on logic-based merging operators and postulates), read at a
  conceptual/survey level as this course's primary technical reference for the week.
- **Software:** Python.
- **Quiz 4** (Weeks 7–9 content).
- **Assignment 2 assigned** (Weeks 5–8 content: ASPIC+ structured argumentation, probabilistic
  logic programming/distribution semantics, differentiable/fuzzy logic, neuro-symbolic
  integration).

## Week 11 — Explanation and Justification Research
- **Topics:** Why "why does my ontology entail this?" is itself a genuine, actively researched KR
  problem, not a solved implementation detail — for an expressive DL such as SROIQ, a single
  entailment can follow from a large, non-obvious combination of axioms interacting through role
  hierarchies, number restrictions, and nominals (Week 4), so simply re-running the tableau
  algorithm and reading off its trace does not by itself give a human-usable explanation;
  **justification-based explanation**: a **justification** for an entailment α, given an ontology
  O, is a **minimal** subset J ⊆ O such that J ⊨ α — minimality matters because O itself trivially
  entails α, so an explanation is useful only to the extent it is as small as possible while still
  sufficient, and in general an entailment can have **several distinct justifications**, each a
  genuinely different reason the entailment holds; computing justifications is itself
  computationally hard research territory — naively, finding *all* justifications requires, in
  the worst case, checking exponentially many subsets for entailment and minimality, so real
  systems use targeted techniques (e.g., black-box methods that repeatedly call an existing DL
  reasoner as a sub-procedure to test candidate subsets, versus glass-box methods that instrument
  the tableau algorithm itself to track which axioms were actually used on the branch that
  produced the entailment) — surveyed conceptually as an open systems/algorithms research
  question, not resolved in this course; **proof-tree explanation** as the complementary idea
  from rule-based reasoning (the graduate course's Week 14), extended here to note that a single
  "proof tree" is a weaker notion than "the set of all justifications" once a logic allows multiple
  independent derivations of the same conclusion — a full explanation-research treatment must
  decide whether it wants *one* understandable derivation, *all* minimal reasons, or some
  practical compromise (e.g., the single smallest justification, or a bounded number of distinct
  ones), and this design choice is itself an open, actively debated question in the explanation-
  for-ontologies literature.
- **Subtopics/Skills:** constructing, by hand, two distinct justifications for a single small toy
  SROIQ-style entailment (hand-crafted so that two genuinely different axiom subsets each suffice)
  and verifying minimality by checking that removing any one axiom from each breaks the
  entailment; implementing a small black-box Python justification finder for a toy ontology
  fragment (repeatedly test candidate axiom subsets, using the Week 4 from-scratch satisfiability/
  entailment checker as the sub-procedure, and prune non-minimal supersets of already-found
  justifications) and running it on a toy entailment with more than one justification; writing a
  short, precise paragraph stating why "the set of all justifications" and "a single proof tree"
  are different explanation notions and when each is the more appropriate deliverable.
- **Readings:** Baader et al. (eds.), *The Description Logic Handbook* — the chapters on
  explanation and justification for DL entailments, continuing this course's anchor text; current
  KR-venue papers on ontology explanation/justification-finding algorithms, selected by the
  instructor as this remains an active research area.
- **Software:** Python.
- **Quiz 5** (Weeks 10 content).

## Week 12 — Ontology Evolution and Versioning
- **Topics:** A grounded survey of the real engineering and research challenges in handling
  **ontology change over time** — an ontology, once deployed, is rarely static: new domain
  knowledge requires adding, removing, or restructuring classes, properties, and axioms, and any
  such change can silently alter what the ontology entails, potentially **breaking dependent
  reasoning** (a downstream application, query, or integrated ontology that relied on a now-
  changed entailment); the core research/engineering problems, surveyed honestly rather than
  presented as solved — **change detection and diffing** (given two ontology versions, identifying
  exactly which axioms were added/removed/modified is non-trivial once logically-equivalent-but-
  syntactically-different restatements are possible); **impact/consequence analysis** (determining
  which entailments are gained or lost between versions — related to, but distinct from, Week 11's
  justification problem, since here the question is "what changed," not "why does this one
  entailment hold"); **backward compatibility and versioning policies** (should a new version
  guarantee it still entails everything the old version did — a strict backward-compatible
  extension — or is some entailment loss an accepted, intentional correction, and how should
  dependent systems be notified which kind of change occurred); **modularity** as a partial
  mitigation (structuring a large ontology into smaller, more independently evolvable modules so
  that a local change has a more predictable, bounded effect on global entailments) — itself an
  incomplete solution, since module boundaries can still interact in non-obvious ways; this week
  is explicitly a grounded survey of open and partially-solved engineering/research problems, not
  a single clean algorithm to implement, mirroring how the field itself treats this topic.
- **Subtopics/Skills:** given two small, hand-written versions of a toy ontology, manually
  identifying which axioms were added/removed and which entailments were gained or lost as a
  result (a miniature impact analysis); implementing a simple Python ontology-diff tool over two
  small sets of axioms (set difference plus a basic "entailment changed" check reusing an earlier
  week's entailment checker on a small fixed query set) and reporting which queries' answers
  changed between versions; writing a short, precise paragraph arguing for or against treating a
  given hypothetical ontology change as backward-compatible, citing specifically which entailments
  are preserved and which are lost.
- **Readings:** Baader et al. (eds.), *The Description Logic Handbook* — the ontology-engineering
  and tooling chapters (continuing the graduate course's Week 13 reference), supplemented by
  current KR-venue papers on ontology evolution/versioning, selected by the instructor as a
  genuinely active research area.
- **Software:** Python.
- **Quiz 6** (Weeks 11–12 content).

## Week 13 — Research Methods for Advanced KR Research
- **Topics:** How to read a frontier KR paper efficiently at the postgraduate level (abstract →
  claimed formal results/examples → full formal definitions and proofs → related work → full
  read) and how to critique one at research depth — is the paper's claimed property (soundness,
  completeness, decidability, a complexity bound) actually established by its argument or only
  asserted; are the worked examples representative of the paper's claimed general case or
  cherry-picked; how does the proposed formalism or algorithm relate to, extend, or conflict with
  the alternatives covered across this course (SROIQ, ASPIC+, the distribution semantics,
  differentiable logic, model checking, belief merging); reproducibility concerns specific to KR
  research (is the proposed logic's semantics fully and unambiguously specified; are complexity
  claims proved or only conjectured; is example code or a reference encoding available); applying
  this framework to formulate a genuine, falsifiable **research problem statement** — the Week 1
  preview now given full treatment: a problem statement names a precise gap a knowledgeable reader
  could, from the statement alone, state what evidence would resolve; standards for a proper
  **related-work survey** (5+ papers, each accurately represented by what it actually claims and
  found, related to each other, not summarized in isolation, and used to motivate the stated gap);
  structured, instructor-guided capstone work time — refining the problem statement via peer
  critique and annotating 5+ candidate related-work papers.
- **Subtopics/Skills:** critiquing a short frontier-KR paper excerpt as a structured in-class
  exercise (identifying the claimed formal result, whether the paper's argument actually
  establishes it, and how the formalism compares to one covered in this course); drafting a
  candidate research problem statement and subjecting it to the precision test in structured peer
  review, revising it until a peer can state, from the statement alone, what evidence would
  resolve it; annotating at least 5 candidate related-work papers for the capstone with each
  paper's precise claim and finding.
- **Readings:** none assigned beyond each student's own capstone-direction papers and the Paper
  Critique assignment's chosen paper.
- **Software:** whatever each capstone direction requires (see individual problem statements).
- **Paper Critique & Presentation assignment assigned** (student selects a real, current KR paper
  — e.g., from the KR conference, an IJCAI/AAAI KR track, or the journal *Artificial
  Intelligence* — from a suggested-topics list covering any formalism in this course).

## Week 14 — Current Open Problems Survey
- **Topics:** A grounded survey, explicitly flagged as covering a **fast-moving research area**
  (not settled textbook material), of 2–3 currently active, genuinely unsolved questions in KR
  research relevant to this course's topics — selected and refreshed by the instructor each
  offering from current KR/IJCAI/AAAI/journal-*Artificial-Intelligence* venues, but representative
  examples include: scaling expressive-DL reasoning and justification-finding (Weeks 4 and 11) to
  realistically large ontologies without losing explainability; principled integration of
  probabilistic (Week 6) and paraconsistent/many-valued (Week 3) semantics into a single coherent
  formalism for reasoning under both uncertainty and inconsistency at once; and closing the gap
  between neuro-symbolic systems' (Weeks 7–8) empirical success and any rigorous guarantee that
  the learned model actually respects the symbolic constraints it was trained toward, rather than
  merely approximating them on the training distribution; for each topic surveyed, the specific
  technical obstacle blocking progress is stated precisely (not just "it's hard"), and at least
  one currently proposed partial approach and its known limitation is named.
- **Subtopics/Skills:** for a given open problem, stating the specific technical obstacle
  precisely (not a vague restatement of the problem) and naming one concrete partial approach and
  its limitation; connecting at least one surveyed open problem to the student's own capstone
  direction, explicitly, as a check on whether the capstone's problem statement is appropriately
  scoped (neither already fully solved nor requiring a solution to a problem this fundamental).
- **Readings:** current papers from KR, IJCAI, AAAI, or the journal *Artificial Intelligence*
  covering the specific open problems selected for this offering — refreshed by the instructor
  each time the course runs, since a fixed reading list here would quickly date.
- **Software:** none required.

## Week 15 — Research Proposal Work Session
- **Topics:** Structured, instructor-guided time for drafting and refining the capstone research
  proposal — finalizing the problem statement (re-applying the Week 13 precision test after the
  Week 14 scoping check), completing the 5+ paper related-work survey and its synthesis, drafting
  the proposed novel approach or extension, and constructing either a small pilot/preliminary
  result or a rigorous feasibility argument (what must be true for the approach to work, the
  single most likely failure mode named honestly, and a reasoned case the risk does not make the
  proposal obviously doomed); **peer feedback on proposal drafts** via a structured, thesis-
  committee-style workshop — each draft receives direct, specific critique on all four required
  components from peers role-playing a proposal committee, mirroring the Week 16 defense format
  students will face.
- **Subtopics/Skills:** giving and receiving structured, specific, thesis-committee-style feedback
  on a capstone proposal draft; revising a problem statement, survey synthesis, proposed approach,
  or feasibility argument in direct response to specific peer critique; producing a written
  revision plan responding to the workshop's feedback.
- **Readings:** none assigned beyond each student's own capstone materials.
- **Software:** whatever each capstone proposal requires (see individual proposals).
- **Deliverable:** draft research proposal due, workshopped in-session; written revision plan
  produced in response (see `assignments/capstone-proposal-guidelines.md`).

## Week 16 — Capstone Research Proposal Presentations; Course Review
- **Topics:** Student capstone research-proposal presentations and oral defenses
  (qualifying-exam/thesis-proposal-defense format: problem statement, related work, proposed
  approach, feasibility argument or preliminary results, anticipated risks, committee-style Q&A —
  see `presentations/capstone-presentation-template.md`); recap of the course map (higher-order
  logic/type theory → many-valued/paraconsistent logics → SROIQ → ASPIC+ structured argumentation
  → probabilistic logic programming → differentiable/fuzzy logic → neural theorem proving/KG-
  embedding hybrids → model checking → multi-agent belief merging → DL explanation/justification
  → ontology evolution → research methods → open problems); closing discussion connecting this
  course's frontier formalisms back to the graduate course's foundations they build on, and
  forward to where KR research is actually headed as of this offering (expressive-reasoning
  scalability, uncertainty-plus-inconsistency integration, and verified neuro-symbolic systems —
  the three Week 14 threads, revisited as the course's closing word on an unfinished research
  landscape, not a tidy conclusion).
- **Deliverable:** final written research proposal submission + qualifying-exam-style oral
  defense.

## Week 17 — Final Exam Week
- Comprehensive final exam, weighted toward Weeks 9–14 content (per Assessment Plan).

---

## Research Proposal Capstone (problem-statement check-in Week 8, draft proposal due Week 15,
## final proposal + oral defense Week 16)
Students formulate an original research question connected to one of this course's four pillars
(expressive logics — HOL/type theory, many-valued/paraconsistent logics, SROIQ; structured/
quantitative non-classical reasoning — ASPIC+, probabilistic logic programming; neuro-symbolic
integration and formal verification; or multi-agent/explanatory topics — belief merging,
explanation research, ontology evolution), or to adjacent territory with instructor approval, and
complete a research-proposal-style project with four required components: (1) a precise,
**falsifiable problem statement**; (2) a **related-work survey of 5+ papers** that accurately
represents each paper's claim and finding, relates the papers to each other, and motivates the
stated gap; (3) a **proposed novel approach or extension** — the student's own formulation,
distinguishable from simply restating a single surveyed paper's contribution; and (4) either
**preliminary results** from a small implemented pilot, or — where a full pilot is infeasible
within the semester — a **rigorous feasibility argument** naming what must be true for the
approach to work and the single most likely failure mode, honestly assessed. This capstone is
evaluated as a thesis-proposal committee would evaluate it (see
`assignments/capstone-rubric.md`), not as a completed project — unlike the graduate course's
capstone (a literature review plus a reproduced/extended experiment, judged on execution), the
object being judged here is the soundness of a **plan** for research not yet completed. Topics
requiring deep neural-architecture, statistical-ML, deep-learning, or the sibling postgraduate
*Advanced Artificial Intelligence* course's regret-theoretic/game-theoretic/alignment content
require instructor pre-approval and must still center on this course's own KR techniques. See
`assignments/capstone-proposal-guidelines.md` and `assignments/capstone-rubric.md`.
