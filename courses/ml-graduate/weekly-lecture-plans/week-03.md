# Week 3 Lecture Plan — Machine Learning (Graduate)
## Topic: VC Dimension in Depth

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Define shattering and VC dimension precisely. (*Understand*)
2. Prove the VC dimension of intervals on the real line and of linear classifiers in $\mathbb{R}^d$. (*Apply, Analyze*)
3. State the VC generalization bound and explain the role of the Sauer–Shelah lemma. (*Understand, Analyze*)
4. Verify a VC-dimension claim by brute-force shattering search. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Why VC dimension | Motivation: finite-$|H|$ theory breaks for infinite classes |
| 0:15–0:35 | Shattering & definition | Formal definitions, worked toy shattering checks |
| 0:35–1:00 | Intervals example | Full proof: $\mathrm{VCdim}=2$ for intervals on $\mathbb{R}$ |
| 1:00–1:10 | Break | — |
| 1:10–1:45 | Linear classifiers example | Full proof via Radon's theorem: $\mathrm{VCdim}(\text{halfspaces in }\mathbb{R}^d)=d+1$ |
| 1:45–2:00 | VC bound & Sauer–Shelah | Statement of the bound; growth function collapses polynomially past $m=\mathrm{VCdim}$ |

### Materials/Equipment
- Whiteboard for the Radon's-theorem argument
- Jupyter notebook for brute-force shattering verification

### Formative Check (in-class)
Students identify, for 4 points at the corners of a square, the one labeling no line can realize,
and justify it via Radon's theorem in one sentence.

### Link to Lab/Assessment
Lab 3: brute-force shattering search confirming $\mathrm{VCdim}=3$ for linear classifiers in
$\mathbb{R}^2$ and $\mathrm{VCdim}=2$ for intervals. **Quiz 1** (Week 2 content) administered.
