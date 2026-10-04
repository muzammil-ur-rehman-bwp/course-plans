# Course Plan: Artificial Neural Network (Graduate)

## 1. Course Information

| Field | Detail |
|---|---|
| Course Title | Artificial Neural Network |
| Level | Graduate (MS Computer Science / Software Engineering / AI) |
| Credit Hours | 3 (2 hrs lecture + 1 lab/seminar session of 3 hrs/week) |
| Prerequisites | *Introduction to Artificial Neural Networks* or equivalent (the perceptron, the multi-layer perceptron, backpropagation derived and implemented by hand, basic optimizers (SGD/momentum/Adam) and basic regularization (L1/L2/dropout) at an applied, undergraduate level — assumed and **not** re-taught); linear algebra (vector/matrix calculus, eigenvalues); multivariate calculus; basic probability (random variables, expectation, variance) |
| Programming Language | Python 3.x |
| Core Libraries | NumPy (mandatory for from-scratch derivation/implementation work throughout), PyTorch (for experiments from Week 3 onward), Matplotlib |
| Duration | 16 teaching weeks (1 semester) + 1 exam week |
| Delivery Mode | Lecture + Lab/Seminar (concept lecture followed by a hands-on implementation or research-skills session) |

## 2. Course Description

This is a rigorous, research-oriented graduate treatment of **neural network theory and training
dynamics** — not an architectures survey and not a classical machine learning course. It assumes
students already know, at an applied level, what a perceptron and a multi-layer perceptron are,
how to derive and implement backpropagation by hand, and how basic optimizers and regularizers
behave (as covered in *Introduction to Artificial Neural Networks*), and it raises the study of
neural networks themselves to graduate rigor: the Universal Approximation Theorem and
depth-versus-width expressivity; automatic differentiation formalized as forward- and reverse-mode
graph traversal, with backpropagation understood as a special case; the theory of weight
initialization and signal propagation through deep networks; the theory of normalization
techniques (why Batch Normalization and Layer Normalization work — not just how to call them);
the geometry of neural network loss landscapes and the optimization theory behind modern adaptive
optimizers; generalization theory (the classical PAC/VC-dimension/Rademacher-complexity view and
the "double descent" phenomenon that complicates it); and a grounded, non-hype survey of current
theoretical research directions — the Neural Tangent Kernel, the Lottery Ticket Hypothesis and
pruning, and information-theoretic perspectives on what networks learn. A brief, theory-angled
unit touches why depth-easing tricks (skip connections) and attention mechanisms matter for
*expressivity and optimization*, explicitly without covering their architectures in depth.

**This course is deliberately scoped to avoid duplicating two sibling graduate courses built
alongside it.** It does **not** teach convolutional, recurrent, or Transformer architectures in
depth, nor generative model architectures (that is *Deep Learning*, Graduate — built after this
course and owning that architecture depth), and it does **not** teach classical statistical
machine learning algorithms such as SVMs, decision trees, or ensemble methods (that is *Machine
Learning*, Graduate). Where an architecture is relevant to a theoretical argument here — for
example, why a residual connection changes the optimization landscape — this course treats only
the theory angle and leaves architectural depth to the Deep Learning course.

## 3. Goals

- Prove and explain the Universal Approximation Theorem for single-hidden-layer networks, and
  reason rigorously about depth-versus-width expressivity tradeoffs.
- Formalize automatic differentiation as forward-mode and reverse-mode traversal of a
  computational graph, and situate backpropagation precisely within that formalism.
- Derive, from variance-preservation arguments, why naive weight initialization fails and why
  Xavier/Glorot and He initialization are the correct fixes for specific activation functions.
- Derive Batch Normalization from first principles and critically evaluate the competing
  explanations for why it works (internal covariate shift versus loss-landscape smoothing), and
  explain when Layer Normalization is preferred.
- Analyze the geometry of neural network loss landscapes (saddle points versus local minima in
  high dimensions) and the optimization theory of first- and second-order methods and modern
  adaptive optimizers, including their known failure modes.
- Critically compare the classical bias-variance framework against the double descent phenomenon,
  and explain modern regularization techniques (weight decay, dropout) through a Bayesian lens.
- Apply the PAC learning framework, VC dimension, and Rademacher complexity to reason about why
  classical generalization theory struggles to explain deep, overparameterized networks.
- Critique, at a technically grounded level, three current theoretical research directions — the
  Neural Tangent Kernel, the Lottery Ticket Hypothesis, and the information bottleneck idea —
  including their claims, evidence, and limitations.
- Read, critique, and present a theoretical machine learning research paper, and design,
  execute, and report an original small research-style capstone project.

## 4. Course Learning Outcomes (CLOs) — Mapped to Bloom's Taxonomy

| CLO | Statement | Bloom's Level(s) |
|---|---|---|
| CLO1 | Recall the assumed undergraduate ANN foundation (perceptron, MLP, backpropagation, basic optimization/regularization) at graduate rigor and map the theoretical research landscape this course covers. | Remember, Understand |
| CLO2 | State and prove (or closely follow a proof sketch of) the Universal Approximation Theorem, and analyze depth-versus-width expressivity tradeoffs. | Understand, Analyze |
| CLO3 | Formalize forward- and reverse-mode automatic differentiation over a computational graph, implement a minimal reverse-mode autodiff engine, and derive backpropagation as its special case. | Apply, Analyze |
| CLO4 | Derive Xavier/Glorot and He initialization from variance-preservation arguments and analyze signal propagation (vanishing/exploding activations and gradients) through deep networks. | Apply, Analyze |
| CLO5 | Derive Batch Normalization's forward and backward computation from scratch, and critically evaluate competing theoretical explanations for why normalization improves training. | Apply, Evaluate |
| CLO6 | Analyze the geometry of neural network loss landscapes (saddle points, Hessian eigenstructure) and critique the practicality of second-order methods versus modern adaptive optimizers, including their documented convergence failure cases. | Analyze, Evaluate |
| CLO7 | Compare the classical bias-variance tradeoff against the double descent phenomenon in overparameterized networks, and justify regularization choices (weight decay, dropout) using both classical and Bayesian arguments. | Analyze, Evaluate |
| CLO8 | Apply the PAC learning framework, VC dimension, and Rademacher complexity to classical hypothesis classes, and critically analyze why these classical bounds are insufficient to explain deep network generalization. | Apply, Analyze, Evaluate |
| CLO9 | Critique, with technical grounding, the Neural Tangent Kernel framework, the Lottery Ticket Hypothesis, and information-bottleneck perspectives on neural network training, correctly stating what each result does and does not establish. | Analyze, Evaluate |
| CLO10 | Critique a theoretical machine learning research paper's claims and methodology, and design, execute, and present an original research-style capstone: literature review, a reproduced or extended experiment/derivation, a written paper, and a conference-style talk. | Analyze, Evaluate, Create |

### Bloom's Taxonomy progression across the semester

| Phase | Weeks | Dominant Bloom's Levels | Focus |
|---|---|---|---|
| Graduate Foundations & Expressivity | 1–3 | Remember, Understand, Analyze | Research landscape; Universal Approximation; automatic differentiation formalized |
| Initialization, Normalization & Optimization Theory | 4–8 | Apply, Analyze, Evaluate | Variance-preserving initialization; BatchNorm/LayerNorm theory; loss-landscape geometry; adaptive optimizers; regularization as Bayesian averaging |
| Generalization Theory | 9–11 | Apply, Analyze, Evaluate | PAC learning, VC dimension, Rademacher complexity, double descent; expressivity of depth and attention |
| Modern Theory, Research Methods & Capstone | 12–16 | Analyze, Evaluate, Create | Neural Tangent Kernel; Lottery Ticket Hypothesis; information bottleneck; paper critique; capstone research project |

## 5. Weekly Topic Overview (16 Weeks)

| Week | Topic | Bloom's Focus |
|---|---|---|
| 1 | Graduate ANN theory overview: course goals, rapid review of assumed prerequisites (perceptron→MLP→backprop), the theoretical landscape of neural network research | Remember, Understand |
| 2 | Universal Approximation: the theorem for single-hidden-layer networks (statement, proof sketch), depth-vs-width expressivity tradeoffs | Understand, Analyze |
| 3 | Automatic differentiation in depth: computational graphs, forward-mode vs. reverse-mode AD, backpropagation as a special case of reverse-mode AD | Apply, Analyze |
| 4 | Initialization theory: signal propagation, why naive initialization fails, deriving Xavier/Glorot and He initialization | Apply, Analyze |
| 5 | Normalization theory: deriving Batch Normalization, internal-covariate-shift vs. loss-landscape-smoothing explanations, Layer Normalization | Apply, Evaluate |
| 6 | Optimization landscape theory I: saddle points vs. local minima in high dimensions, Newton's method and Gauss-Newton, why second-order methods are impractical at scale | Analyze, Evaluate |
| 7 | Optimization landscape theory II: Adam's convergence subtleties and failure cases, natural gradient descent (conceptual), learning-rate warmup theory | Analyze, Evaluate |
| 8 | Regularization theory: bias-variance vs. double descent, weight decay, dropout as approximate Bayesian model averaging; midterm review | Analyze, Evaluate |
| 9 | **Midterm Exam** + Generalization theory I: PAC learning framework, VC dimension, why classical bounds struggle with deep learning | Remember–Analyze |
| 10 | Generalization theory II: Rademacher complexity, margin-based arguments, the random-label-fitting phenomenon | Analyze, Evaluate |
| 11 | Expressivity and depth: why skip connections ease optimization (theory angle only), attention mechanisms from an expressivity viewpoint (brief) | Analyze |
| 12 | The Neural Tangent Kernel: infinite-width networks as kernel regression, what this reveals and its limits | Analyze, Evaluate |
| 13 | The Lottery Ticket Hypothesis and pruning: sparse winning-ticket subnetworks, model compression/distillation basics | Analyze, Evaluate |
| 14 | Information-theoretic perspectives: the information bottleneck idea (conceptual, debated research area) | Analyze, Evaluate |
| 15 | Research methods and project work session: reading/critiquing theoretical ML papers, reproducibility, capstone work session | Analyze, Evaluate, Create |
| 16 | Capstone research presentations + course review | Evaluate, Create |
| 17 | Final Exam Week | — |

## 6. Assessment Plan

| Component | Weight | Notes |
|---|---|---|
| Lab Work (weekly) | 15% | Graded notebooks, submitted weekly (Labs 1–15) |
| Assignments (3 problem sets) | 15% | Tied to Weeks 3, 7, 10 |
| Quizzes (6, best 5 counted) | 10% | Short, in-class/online, 15 min each |
| Paper Critique & Presentation | 10% | Week 15 research-methods assignment; critique of a theoretical ML paper + in-class presentation |
| Midterm Exam | 15% | Week 9, covers Weeks 1–8 |
| Research Capstone | 25% | Literature review + reproduced/extended experiment or derivation + paper + presentation (proposal Wk 8–9, work session Wk 15, presentation Wk 16) |
| Final Exam | 10% | Comprehensive, emphasis on Weeks 9–15 |

**Rationale for the graduate weighting.** As in a research-oriented graduate course, weight shifts
away from high-stakes closed-book exams (Midterm + Final total 25%) and toward sustained,
research-style work: a dedicated Paper Critique & Presentation component (10%) and a heavy
Research Capstone (25%) that requires a genuine literature review and an experiment or derivation,
not just an implementation exercise. Labs are weighted at 15% rather than an undergraduate course's
20%, reflecting that graduate labs are shorter, more targeted theory-verification exercises,
assuming strong baseline NumPy/PyTorch fluency from the prerequisite course.

## 7. Grading Policy

Standard letter grading per institutional policy (e.g., A ≥ 85, B ≥ 70, C ≥ 55, D ≥ 40, F < 40;
adjust to institution). Late submissions: −10% per day up to 3 days, then not accepted unless
documented emergency. Capstone milestones (proposal, draft, final submission) have fixed deadlines
because of the downstream presentation schedule; late capstone milestones are handled case-by-case
with the instructor.

## 8. Tools & Software

- Python 3.10+, pip/conda
- Jupyter Notebook / Google Colab
- NumPy (mandatory for from-scratch derivation and implementation work throughout — autodiff
  engines, initialization experiments, BatchNorm, optimizers, VC-dimension/Rademacher demos)
- PyTorch (for larger-scale experiments from Week 3 onward: NTK computation, pruning, capstone
  work), Matplotlib
- Git/GitHub for lab, assignment, and capstone submission

## 9. Reference Textbooks

- Goodfellow, I., Bengio, Y., & Courville, A. — *Deep Learning*. MIT Press. (primary reference for
  the theory chapters: optimization, regularization, and representation learning theory).
- Nielsen, M. — *Neural Networks and Deep Learning* (free online book, neuralnetworksanddeeplearning.com)
  — used for its rigorous treatment of why deep networks are hard to train (vanishing/exploding
  gradients) as a bridge from the prerequisite course into this course's initialization and
  optimization theory units.
- Reading and critiquing current theoretical machine learning papers from venues such as NeurIPS
  and ICML is standard graduate practice and is treated as a graded skill in this course from Week
  11 onward (Neural Tangent Kernel, Lottery Ticket Hypothesis, information bottleneck units) and
  especially in the Week 15 research-methods unit and the capstone. Where a specific named result
  is discussed — for example, Jacot et al.'s Neural Tangent Kernel framework, or Frankle & Carbin's
  Lottery Ticket Hypothesis — it is attributed to its authors by name; students are expected to
  locate and read the primary paper themselves as part of the corresponding week's and the
  capstone's work, rather than rely on a secondary citation given here.
- Official NumPy and PyTorch documentation.

## 10. Academic Integrity

Labs and assignments are individual unless stated otherwise. The research capstone may be done in
pairs with clearly attributed contributions. Any use of another author's ideas, text, code, or
results — including figures or results from a paper being critiqued or reproduced — must be
properly cited; uncredited reuse of a paper's text or another student's code (including uncredited
AI-generated code or text submitted as original work) is handled per institutional academic
integrity policy. Reproducing a published experiment or derivation is expected and encouraged for
the capstone; presenting someone else's reported results as one's own experimental findings is
not.
