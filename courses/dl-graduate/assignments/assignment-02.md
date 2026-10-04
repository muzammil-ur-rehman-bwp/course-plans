# Assignment 2 — Self-Supervised Learning, Generative Models, and Graph Neural Networks (Weeks 5–9)

**Weight:** 5% of course grade (one of 3 problem-set assignments, 15% total) | **Assigned:** Week
9 | **Due:** Start of Week 11

## Instructions
Submit a single Jupyter notebook `assignment02.ipynb` answering all questions below. Show your
work (derivations and/or code) for each question.

## Questions
1. **(InfoNCE derivation, 15 pts)** Derive the InfoNCE loss from the positive-vs-negatives
   $(K{+}1)$-way classification framing, starting from the categorical cross-entropy loss; state
   explicitly what the "logit" for each candidate is in this framing.
2. **(SimCLR implementation, 20 pts)** Implement a SimCLR-style pretraining step (two augmented
   views, shared encoder, projection head, InfoNCE loss) on a small image dataset; report the
   linear-probe accuracy after pretraining versus a random-encoder baseline.
3. **(Normalizing flow vs. diffusion, 20 pts)** In 200–300 words, compare normalizing flows and
   diffusion models on: (a) what is learned (an invertible map vs. a noise-prediction network),
   (b) how density/likelihood is computed or estimated, and (c) sampling cost.
4. **(Diffusion implementation, 20 pts)** Implement the simplified DDPM-style training loss and
   sampling loop on a 2-D synthetic dataset (e.g., a ring or spiral shape); report a scatter plot
   of generated samples against the true distribution.
5. **(GCN layer and GAT comparison, 25 pts)** Implement a GCN layer and a GAT layer from scratch;
   train both for node classification on the same small graph/split; report both models' accuracy
   and discuss, with reference to the learned GAT attention weights, when attention-weighted
   aggregation should help relative to GCN's fixed degree-normalized aggregation.

## Submission
Upload `assignment02.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
