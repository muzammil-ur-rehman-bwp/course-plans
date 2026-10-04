# Week 12 Lecture Plan — Advanced Machine Learning (Post Graduate)
## Topic: Robust Statistics

**Duration:** 2 hours lecture + 3 hour research seminar/lab

### Learning Objectives (Bloom's Level)
1. Explain the sample mean's breakdown point and motivate robust estimation. (*Understand*)
2. Derive the median-of-means error bound via the group-concentration-plus-majority-vote
   argument. (*Apply, Analyze*)
3. Implement trimmed-mean and median-of-means estimators and empirically compare them to the
   sample mean under heavy tails and adversarial contamination. (*Apply, Analyze, Evaluate*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Motivation | The sample mean's breakdown point $1/n\to0$; why heavy tails/contamination matter |
| 0:15–0:35 | Trimmed mean | Definition; breakdown point $\epsilon$ |
| 0:35–1:15 | Median-of-means | Construction; Chebyshev-per-group argument; Chernoff/majority-vote boost |
| 1:15–1:40 | Adversarial contamination | Bounded-error guarantees vs. the sample mean's unbounded error |
| 1:40–2:00 | Connection to adversarial ML | Conceptual link: contamination robustness and adversarial-example robustness |

### Materials/Equipment
- Slides: "Robust Mean Estimation: Median-of-Means and Trimmed Mean"
- Whiteboard for the median-of-means error-bound derivation
- Live-coding environment (Jupyter)

### Formative Check (in-class)
Explain why a single arbitrarily large outlier can make the sample mean take any value, but
affects at most one group mean in a $k$-group median-of-means construction.

### Link to Lab/Assessment
Lab 12: implement median-of-means and trimmed-mean estimators and compare their error to the
sample mean under heavy-tailed and adversarially contaminated data (see `lab-manuals/lab-12.md`).
