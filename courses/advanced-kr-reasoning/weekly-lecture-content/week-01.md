# Week 1 — Lecture Content: Postgraduate Overview

## 1. What This Course Assumes (Stated, Not Re-Taught)
This course assumes the graduate *Knowledge Representation and Reasoning* course in full. As a
diagnostic (not new instruction), each assumed topic's core result in one sentence:
- **Modal logic:** a Kripke model ⟨W,R,V⟩ with `M,w ⊨ □φ iff M,v ⊨ φ for every v with wRv`; the
  correspondence between R's properties and the systems K/S4/S5.
- **Temporal logic (LTL):** X, G, F, U operators over an execution trace; a brief look at CTL's
  path quantifiers A/E.
- **The ALC tableau method:** completion rules (⊓, ⊔, ∃, ∀) searching for a clash-free model;
  ALC concept satisfiability is PSPACE-complete.
- **Answer Set Programming:** the Gelfond–Lifschitz reduct and stable-model semantics for normal
  logic programs with negation-as-failure.
- **AGM belief revision:** the eight AGM postulates (K∗1–K∗8) and Dalal's model-based revision
  operator.
- **Dung's abstract argumentation framework:** AF = ⟨A,→⟩; conflict-freeness, admissibility, the
  grounded extension (least fixed point of the characteristic function), preferred extensions.
- **Markov Logic Networks:** weighted first-order formulas inducing a log-linear distribution
  `P(x) = (1/Z)·exp(Σᵢ wᵢ nᵢ(x))` over possible worlds.
- **Knowledge graphs:** RDF triples; TransE's translation model `h + r ≈ t` and margin-based
  ranking loss for link prediction.
- **Multi-agent epistemic logic:** everyone-knows E_G, common knowledge C_G (transitive closure of
  the union of accessibility relations), distributed knowledge D_G (intersection); the
  muddy-children puzzle.
- **Ontology engineering:** competency questions, alignment, reasoner-backed tooling (Protégé +
  Pellet/HermiT).
- **Symbolic explainability:** proof trees as exact, complete derivation records, contrasted with
  approximate, post-hoc ML explanation.

## 2. The Landscape of Open Problems This Course Covers
Four pillars, each pushing past one of the above into genuinely new territory:
1. **Expressive logics beyond ALC** — higher-order logic/type theory (Week 2), many-valued and
   paraconsistent logics (Week 3), and SROIQ (Week 4), which extends ALC with role hierarchies,
   complex role inclusions, number restrictions, and nominals.
2. **Structured and quantitative non-classical reasoning** — ASPIC+'s structured arguments and
   rebutting/undercutting attacks (Week 5), building concrete structure Dung's AF deliberately
   abstracts away; and the distribution semantics for probabilistic logic programs (Week 6),
   approaching probabilistic reasoning from a different angle than MLNs' log-linear model.
3. **Neuro-symbolic integration and formal verification as research frontiers** — differentiable/
   fuzzy logic (Week 7) and neural theorem proving/embedding-constraint hybrids (Week 8) as an
   unsettled research area; model checking (Week 9), a rigorous decision procedure extending the
   graduate course's LTL introduction.
4. **Multi-agent, explanatory, and methodological topics** — belief merging across multiple,
   equally-standing agents (Week 10, extending single-agent AGM); justification/explanation
   research for expressive-DL entailments (Week 11); ontology evolution and versioning (Week 12);
   and a survey of currently open problems (Week 14).

## 3. Course Scope Boundaries
This course does **not** repeat the graduate course's foundations listed in §1 — they are
recalled only as background when directly relevant (e.g., "recall the ALC tableau rules" in Week
4). It also does not duplicate the sibling postgraduate courses: *Advanced Artificial
Intelligence* owns regret theory, algorithmic game theory, and AI safety/alignment/
interpretability; *Advanced Artificial Neural Network*, *Advanced Machine Learning*, and
*Advanced Deep Learning* own deep architectures, training, and statistical/algorithmic learning
theory. Where one of those topics is relevant to an argument here (e.g., gradient descent in Week
7), it receives at most a one-sentence pointer.

## 4. Scoping a Research Proposal — Early Preview
The capstone (full treatment: Week 13 onward) is a **research proposal**, not a completed
project, with four required components: (1) a precise, **falsifiable problem statement** — a
knowledgeable reader should be able to state, from the statement alone, what evidence would
resolve it; (2) a **related-work survey** of 5+ papers, each accurately represented and related to
the others, motivating the stated gap; (3) a **proposed novel approach or extension** — the
student's own formulation; (4) either **preliminary results** or a **rigorous feasibility
argument** (what must be true for the approach to work, the single most likely failure mode named
honestly, and a reasoned case the risk is not disqualifying). This is previewed now, in Week 1,
specifically so students can begin noticing, across the whole semester, which topics spark a
genuine research question worth pursuing — not to produce a committed topic yet.

## 5. In-Class/Lab Exercise
For three randomly assigned graduate-course topics from §1, state the core result in one sentence
each from memory (no notes), then for one assigned advanced-KR research question (e.g., "can a
rule-based system be trained end-to-end with gradient descent?"), map it onto this course's week
schedule and name one sibling course (graduate KR&R, or a postgraduate sibling) it is explicitly
not about. Conclude with a one-paragraph sketch of a tentative personal research direction of
interest, to be revisited and refined across the semester (not graded as a commitment).
