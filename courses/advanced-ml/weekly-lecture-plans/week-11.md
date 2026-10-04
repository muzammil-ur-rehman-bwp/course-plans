# Week 11 Lecture Plan — Advanced Machine Learning (Post Graduate)
## Topic: Algorithmic Fairness

**Duration:** 2 hours lecture + 3 hour research seminar/lab

### Learning Objectives (Bloom's Level)
1. State the formal definitions of demographic parity, equalized odds, and calibration.
   (*Understand*)
2. Derive the calibration/equalized-odds impossibility result via the PPV-monotonicity argument.
   (*Analyze*)
3. Compute fairness metrics on simulated data with differing base rates and empirically confirm
   the impossibility result. (*Apply, Analyze, Evaluate*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | Formal criteria | Demographic parity, equalized odds, calibration — precise definitions |
| 0:20–0:50 | The PPV identity | $\mathrm{PPV}=pTPR/(pTPR+(1-p)FPR)$; monotonicity in base rate $p$ |
| 0:50–1:25 | The impossibility result | Why equal PPV + equal TPR/FPR forces equal base rates (or a degenerate classifier) |
| 1:25–1:50 | Practical implications | Fairness-criterion selection as value-laden, not purely technical |
| 1:50–2:00 | Synthesis | What the impossibility result does and does not say |

### Materials/Equipment
- Slides: "Formal Fairness Criteria and the Impossibility Result"
- Whiteboard for the PPV-monotonicity derivation
- Live-coding environment (Jupyter)

### Formative Check (in-class)
Given two groups with base rates 0.1 and 0.4 and a classifier with TPR=0.8, FPR=0.2 in both
groups, compute PPV for each group and explain why they differ despite equal TPR/FPR.

### Link to Lab/Assessment
Lab 11: compute fairness metrics on simulated data and empirically demonstrate the impossibility
result (see `lab-manuals/lab-11.md`).
