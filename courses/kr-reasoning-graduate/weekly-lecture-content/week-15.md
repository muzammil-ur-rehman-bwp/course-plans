# Week 15 — Lecture Content: Research Methods and Project Work Session

## 1. How to Read a KR Research Paper Efficiently
A standard efficient order for a first pass: **abstract** (what is claimed) → **results/worked
examples** (what the paper actually shows, concretely) → **formal definitions** (the precise
semantics/algorithm the results depend on) → **related work** (how it positions itself against
alternatives) → **full read** (only once the shape of the contribution is clear). This order
front-loads exactly the information needed to decide whether a full read is worth the time.

## 2. A Structured Critique Framework for KR Papers
Adapting a general research-critique framework to this course's subject matter, ask explicitly:
1. **Claimed result.** State the paper's central formal claim precisely — a new semantics, a
   complexity bound, a soundness/completeness result, an algorithm with a stated guarantee.
2. **Does the argument actually establish it?** Is the claimed property proved, or only
   conjectured/argued informally where a proof (or at least a proof sketch) would be expected at
   this venue?
3. **Are the worked examples representative?** A single cherry-picked example that happens to
   avoid a formalism's hard cases is weaker evidence than one that stresses it.
4. **How does it compare to alternatives covered in this course?** E.g., a paper proposing a new
   non-monotonic formalism should be read against this course's default logic, circumscription,
   and ASP — does it genuinely improve on them, and at what cost?
5. **Reproducibility, KR-specific.** Is the proposed logic's syntax and semantics fully and
   unambiguously specified (so another researcher could re-derive the same results)? Are
   complexity claims proved or only asserted? Is any implementation/encoding available?

## 3. Reproducibility Concerns Specific to KR Research
Unlike an empirical ML paper (where reproducibility concerns center on data/seeds/hyperparameters
— see *Artificial Intelligence* Graduate's Week 13 for that treatment), a KR paper's
reproducibility hinges on **semantic precision**: can a reader reconstruct, from the paper alone,
exactly which models/worlds/extensions the formalism admits, without needing to guess an
unstated convention? A paper that states "our logic generalizes modal logic" without giving the
exact accessibility-relation or satisfaction-clause changes is a reproducibility red flag by this
course's standard, exactly the standard this course has modeled all semester (Weeks 2, 3, 4, 6,
7, 8, 9, 12 each gave an exact formal definition before any claim about it).

## 4. In-Class Critique Exercise
Apply the §2 framework to a short excerpt (instructor-provided, drawn from a KR-venue paper or a
historically important excerpt such as Dung's 1995 paper, already read in Week 8): identify the
claimed result, assess whether the argument given actually establishes it, and name which
formalism from this course it most directly compares to.

## 5. Structured Capstone Work Session
Guided work time for: finalizing the literature review (3–5 papers, each summarized as problem/
method/result, ending in an explicitly stated gap or question); designing and running the small
reproduced/extended experiment; drafting the written paper (introduction, related work, method,
results, limitations). Students/pairs also give a short practice run of their capstone talk and
receive structured peer feedback — the same specificity standard as §2: concrete, actionable
feedback on the problem statement's clarity, the experiment's soundness, and the honesty of the
limitations discussion, not generic praise or generic criticism.

## 6. Deliverables This Week
- **Paper Critique & Presentation** (written critique + in-class presentation) — see
  `assignments/paper-critique-and-presentation.md`.
- **Capstone written-paper draft** due, incorporating this session's feedback before Week 16's
  final presentation.
