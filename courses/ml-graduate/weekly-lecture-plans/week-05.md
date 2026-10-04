# Week 5 Lecture Plan — Machine Learning (Graduate)
## Topic: Concentration Inequalities

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall and prove Markov's and Chebyshev's inequalities. (*Remember, Apply*)
2. Derive Hoeffding's inequality in full, via Hoeffding's lemma and Chernoff bounding. (*Apply, Analyze*)
3. State McDiarmid's bounded-differences inequality and explain its role in the Weeks 3–4 bounds. (*Understand*)
4. Identify when applying Hoeffding's inequality would be invalid (non-i.i.d. or unbounded data). (*Analyze, Evaluate*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Markov & Chebyshev | Quick proofs, as building blocks |
| 0:15–0:30 | Hoeffding's lemma | MGF bound for a bounded, zero-mean variable |
| 0:30–1:00 | Hoeffding's inequality | Full Chernoff-bounding derivation, optimizing over $t$ |
| 1:00–1:10 | Break | — |
| 1:10–1:35 | McDiarmid's inequality | Bounded-differences condition; link to $\sup_h(L_D(h)-\widehat L_S(h))$ |
| 1:35–2:00 | Assumption failure case | Worked example: correlated sequence breaking Hoeffding's guarantee |

### Materials/Equipment
- Whiteboard for the full Chernoff-bounding derivation
- Jupyter notebook for the simulation (i.i.d. vs. correlated sequences)

### Formative Check (in-class)
Students re-derive the two-sided Hoeffding bound for $X_i\in[-1,1]$ instead of $[0,1]$ and report
the resulting constant in the exponent.

### Link to Lab/Assessment
Lab 5: simulate coin flips to verify Hoeffding's bound, then repeat with a correlated sequence to
see the bound's assumptions break down. **Quiz 2** (Weeks 3–4) administered.
