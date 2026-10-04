# Week 9 Lecture Plan — Machine Learning (Graduate)
## Topic: Midterm Exam; Bayesian Machine Learning I

**Duration:** Midterm (1 hour) + 1 hour lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Demonstrate mastery of Weeks 1–8 content on the midterm exam. (*Remember–Analyze*)
2. Derive the Bayesian linear regression posterior in closed form. (*Apply, Analyze*)
3. Explain ridge regression as MAP estimation under a Gaussian prior. (*Understand, Analyze*)
4. Derive the posterior predictive distribution and its two variance sources. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–1:00 | **Midterm Exam** | Covers Weeks 1–8 |
| 1:00–1:10 | Break | — |
| 1:10–1:35 | Bayesian linear regression setup | Prior, likelihood, Bayes' rule |
| 1:35–2:00 | Posterior derivation | Completing the square; closed-form mean/covariance; ridge-as-MAP |

### Materials/Equipment
- Midterm exam booklet
- Whiteboard for the posterior derivation

### Formative Check (in-class)
Students state, from the derivation, what $\mu_N$ and $\Sigma_N$ reduce to as $\tau^2\to\infty$
and $\tau^2\to0$.

### Link to Lab/Assessment
Lab 9: implement Bayesian linear regression from scratch; verify the MAP estimate matches
`sklearn.linear_model.Ridge` for the corresponding $\lambda$. **Capstone project introduced;
proposal guidelines distributed.**
