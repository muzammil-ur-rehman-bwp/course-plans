# Week 11 — Lecture Content: Foundational and Philosophical Debates I

## 1. Why Treat These as Live Research Questions
The symbol grounding problem and the frame problem are often introduced in undergraduate AI
courses as brief historical curiosities from the 1960s–1990s. This course treats them instead as
**open, technically grounded questions** that resurface directly in the architectures studied in
Weeks 1–10: any system with learned, high-dimensional internal representations (rather than
hand-specified symbols) raises a modern version of symbol grounding; any agent that must
efficiently track and update world state across actions (the MDP state in Week 4, the joint
action space in Week 5) is implicitly answering some version of the frame problem through its
representation choices, whether or not its designers framed it that way.

## 2. The Symbol Grounding Problem
**Stevan Harnad (1990), "The Symbol Grounding Problem."** The problem, stated precisely: a
purely symbolic system manipulates tokens according to formal (syntactic) rules, where each
token's "meaning" is defined only by its relations to other tokens within the same system —
exactly as a dictionary defines each word using other words. Harnad's point is that this is
circular: if every symbol's meaning is cashed out only in terms of other symbols, nothing in the
system is ever connected to anything outside it, and it is unclear in what sense the system's
symbols are *about* the world at all, however internally coherent their manipulation. Harnad's
proposed response (not the only one in the literature, but the paper's own) is that symbols must
be **grounded** in non-symbolic, sensorimotor categorization processes that connect at least some
basic symbols directly to perceptual experience of the world, with higher-level symbols then
built compositionally on top of that grounded base.

**Why this is live, not historical.** A natural modern question: are the learned internal
representations of a system trained end-to-end on data (rather than hand-built symbolic
knowledge bases) "grounded" in Harnad's sense, or do they face the same circularity concern in a
different form — representations defined only by their statistical relationships to other
representations within the same training distribution, with no principled connection to anything
outside the data the system was trained on? This question does not have a settled answer, and
reasonable researchers disagree about whether learned representations trained on sufficiently
rich, multimodal data meaningfully address Harnad's concern, or merely relocate it.

## 3. The Frame Problem
**John McCarthy and Patrick Hayes (1969), "Some Philosophical Problems from the Standpoint of
Artificial Intelligence."** The problem, stated precisely in its original logical setting: when
an agent represents the effect of an action using logical axioms (e.g., "moving block A onto
block B makes A on top of B"), a naive representation requires separately stating, for every
action and every fact that action does *not* affect, an explicit "frame axiom" asserting that
fact's persistence — a combinatorially enormous, practically unworkable number of axioms (e.g.,
explicitly stating that moving block A does not change the color of block C, does not change the
weather, does not change block D's position, and so on, for every irrelevant fact). The frame
problem is the challenge of representing and reasoning about action effects **tractably**,
without this explicit enumeration of non-effects.

**Why this is live, not historical.** The frame problem is not merely a quirk of 1960s logical
formalisms — it is a specific instance of a general challenge any agent architecture faces:
representing world state compactly enough to update efficiently after an action, while still
correctly tracking everything that matters. Week 4's contrast between a contextual bandit
(context simply discarded and redrawn each round — there is no persistence to track) and a full
MDP (state persists and must be updated correctly after each action) is, at bottom, a direct
engineering response to exactly this problem: an MDP's transition model P(s'|s,a) is a *compact*
specification of what changes and (implicitly) what does not, avoiding the frame problem's
explosion by only specifying effects, not non-effects, for the state representation chosen.
Modern "world models" used by learned agents face the same underlying tension in a learned
rather than hand-specified form: a world model that must predict the consequences of actions
still has to implicitly solve some version of "what stays the same," and whether it does so
correctly outside its training distribution is an open empirical question, not one with a
textbook settled answer.

## 4. Structured Discussion Prompts
For each prompt, identify the specific design choice functioning as an implicit answer to symbol
grounding and/or the frame problem, and evaluate whether it is a satisfying answer or a practical
workaround that sidesteps the deeper question:
1. A classical STRIPS planner (graduate course, Week 7 there) represents state as a set of
   ground predicates and each action as an add-list/delete-list. Which problem does the
   add-list/delete-list formalism address, and how completely?
2. A reinforcement-learning agent's state representation is a raw, high-dimensional sensor
   vector, with no hand-specified predicates at all. Does this representation face a version of
   the frame problem? Does it face a version of symbol grounding? Are these the same question or
   different ones?

## 5. In-Class/Lab Exercise
See `lab-manuals/lab-11.md` for this week's structured critical-writing exercise: a short
position paper connecting one modern AI system design choice (of the student's choosing) to the
symbol grounding problem, the frame problem, or both, with a reasoned evaluation of how
satisfying that design choice's implicit answer actually is.
