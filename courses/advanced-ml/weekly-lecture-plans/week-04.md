# Week 4 Lecture Plan — Advanced Machine Learning (Post Graduate)
## Topic: High-Dimensional Statistics II — Random Matrix Theory Basics

**Duration:** 2 hours lecture + 3 hour research seminar/lab

### Learning Objectives (Bloom's Level)
1. State the Marchenko–Pastur law and explain its conceptual derivation. (*Understand*)
2. Explain why sample-covariance eigenstructure is distorted when $p/n$ is not small. (*Understand, Analyze*)
3. Simulate the empirical spectral distribution of a sample covariance matrix and confirm
   convergence to the Marchenko–Pastur density. (*Apply, Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Motivation | Why $p\approx n$ breaks classical fixed-$p$, $n\to\infty$ covariance theory |
| 0:15–0:45 | The Marchenko–Pastur law | Statement; support interval $[(1-\sqrt\gamma)^2,(1+\sqrt\gamma)^2]$; density formula |
| 0:45–1:10 | Conceptual derivation sketch | Why eigenvalue spreading occurs even when the true covariance is the identity |
| 1:10–1:40 | Consequences for PCA | Top sample eigenvalues overstate true variance in the $p\approx n$ regime |
| 1:40–2:00 | Boundary cases | $\gamma\to 0$ (classical regime recovered); $\gamma\to 1$ (near-singularity) |

### Materials/Equipment
- Slides: "Random Matrix Theory Basics: The Marchenko–Pastur Law"
- Whiteboard for the support-interval and density-formula walkthrough
- Live-coding environment (Jupyter)

### Formative Check (in-class)
For $p=500, n=1000$ ($\gamma=0.5$), compute the Marchenko–Pastur support interval and explain why
a data analyst who fits a 500-feature model on 1000 identity-covariance samples should expect the
largest sample eigenvalue to be noticeably larger than 1.

### Link to Lab/Assessment
Lab 4: simulate the empirical spectral distribution of a sample covariance matrix across several
$\gamma$ values and overlay the Marchenko–Pastur density (see `lab-manuals/lab-04.md`).
