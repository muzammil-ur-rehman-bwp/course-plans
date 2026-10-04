# Course Contents: Advanced Deep Learning (Post Graduate)

Detailed per-week breakdown of topics, subtopics, and resources. Companion to `course-plan.md`.
Each week lists: **Topics**, **Subtopics/Skills**, **Readings**, **Software/Libraries used**.

---

## Week 1 — Course Overview: The Frontier Deep-Learning Landscape
- **Topics:** How this course differs from *Deep Learning*, Graduate (that course derived the
  Transformer from scratch, advanced CNNs, self-supervised/contrastive learning, discrete-time
  DDPM diffusion, graph neural networks, deep RL, and large-scale training basics; this course
  assumes every one of those as settled background and studies several of the same underlying
  objects — diffusion chief among them — in much greater depth, plus genuinely new territory); a
  rapid, non-re-taught review checklist of the assumed graduate foundations; the landscape of
  frontier research/engineering topics this course covers (the SDE formulation of score-based
  diffusion, advanced diffusion techniques, Mixture-of-Experts, in-context learning mechanics,
  preference-based alignment engineering, efficient inference, neural architecture search,
  systems-view scaling laws, a frontier generative-modeling survey); explicit scoping against the
  three sibling postgraduate courses — *Advanced Artificial Neural Network* owns the theoretical
  "why do scaling laws hold" question, NTK/mean-field theory, implicit bias, grokking,
  statistical-physics approaches, PAC-Bayes, and mode connectivity (Week 11 here studies scaling
  laws from a systems/engineering angle instead); *Advanced Artificial Intelligence* owns
  regret/bandit theory, multi-agent RL, game theory/mechanism design, and AI safety/alignment as a
  research-agenda/philosophical topic (Weeks 6–7 here cover the concrete RLHF/DPO engineering
  instead); *Advanced Machine Learning* owns minimax bounds, high-dimensional statistics,
  full-information online convex optimization, nonparametric Bayesian methods, causal inference,
  fairness, and robust statistics, with no expected overlap; how to scope a research proposal,
  introduced early as the same four-part structure (problem statement, related-work survey,
  proposed approach, feasibility argument) used across the postgraduate sequence.
- **Subtopics/Skills:** self-assessing prerequisite fluency against the graduate-course checklist;
  setting up the course's PyTorch environment; reading a short frontier-landscape map; drafting a
  first research-interest paragraph.
- **Readings:** students re-skim their own graduate-course notes on DDPM diffusion, deep RL
  (policy gradients/actor-critic), and large-scale training basics (no new reading assigned); a
  short instructor-provided landscape map of this course's topics.
- **Software:** Python 3.10+, PyTorch, Matplotlib.

## Week 2 — Score-Based Generative Models: The SDE Formulation
- **Topics:** Recasting the graduate course's discrete-time DDPM forward process as the Euler-
  Maruyama discretization of a continuous-time **forward SDE**,
  $dx = f(x,t)\,dt + g(t)\,dw$, where $w$ is a standard Wiener process and $f,g$ are chosen so the
  marginal $p_t(x)$ smoothly interpolates from the data distribution ($t=0$) to a tractable prior,
  typically a standard Gaussian ($t=T$) — the Variance-Preserving (VP) SDE,
  $dx = -\tfrac12\beta(t)x\,dt + \sqrt{\beta(t)}\,dw$, is shown to recover DDPM's discrete
  $\sqrt{1-\beta_t}\,x_{t-1} + \sqrt{\beta_t}\,\epsilon$ update exactly in the small-step limit; the
  **reverse-time SDE** (Anderson's time-reversal result, stated and used, not re-derived in full
  measure-theoretic generality): running time backwards, the same diffusion process is described
  by $dx = \big[f(x,t) - g(t)^2 \nabla_x \log p_t(x)\big]dt + g(t)\,d\bar w$, so that **sampling**
  reduces to simulating this reverse SDE from $x_T \sim \mathcal N(0,I)$ down to $x_0 \sim p_0$,
  provided the **score function** $\nabla_x \log p_t(x)$ is known at every noise level $t$; score
  matching — since the true score is unknown, a neural network $s_\theta(x,t)$ is trained to
  approximate it by denoising score matching, using the same closed-form Gaussian marginal
  $p_t(x\mid x_0)$ the forward SDE admits, with the tractable objective
  $\mathbb E_{t,x_0,\epsilon}\big[\lambda(t)\lVert s_\theta(x_t,t) - \nabla_{x_t}\log p_t(x_t\mid x_0)\rVert^2\big]$;
  showing this score-matching objective is, up to a reweighting $\lambda(t)$ and a reparameterization
  $s_\theta = -\epsilon_\theta/\sigma_t$, exactly the graduate course's DDPM noise-prediction loss —
  **discrete-time DDPM is a specific discretization and reparameterization of this continuous-time
  score-based view, not a different model**; the probability-flow ODE (brief pointer) as a
  deterministic counterpart with the same marginals, previewing Week 12's fast-sampler discussion.
- **Subtopics/Skills:** deriving the Euler-Maruyama discretization of the VP-SDE and matching its
  coefficients to the graduate course's DDPM forward-process formula; implementing a score network
  on toy 2-D data (e.g., a Gaussian mixture) and training it with denoising score matching;
  implementing an Euler-Maruyama reverse-SDE sampler and visualizing generated samples against the
  true toy density; explicitly writing the DDPM-loss-to-score-matching-objective correspondence.
- **Readings:** Song et al., on the stochastic differential equation formulation of score-based
  generative modeling (the primary source for this week); Ho et al., on DDPM, re-read specifically
  for the discretization correspondence.
- **Software:** PyTorch.

## Week 3 — Advanced Diffusion Techniques
- **Topics:** **Classifier-free guidance**, derived: given a conditional score $s_\theta(x,t,c)$
  and an unconditional score $s_\theta(x,t,\varnothing)$ (the same network, trained with the
  condition randomly dropped during training), the guided score
  $\tilde s_\theta(x,t,c) = s_\theta(x,t,\varnothing) + w\big(s_\theta(x,t,c) - s_\theta(x,t,\varnothing)\big)$
  for guidance scale $w \geq 1$ is shown to correspond to sampling from a sharpened distribution
  proportional to $p_t(x\mid c)\, p_t(c\mid x)^{w-1}$ (obtained by substituting
  $\nabla_x \log p_t(c\mid x) = \nabla_x\log p_t(x\mid c) - \nabla_x \log p_t(x)$ into the implicit-
  classifier guidance formula and eliminating the need for a separately trained classifier); why
  $w>1$ trades sample diversity for condition-adherence and visual/semantic fidelity; **flow
  matching** introduced conceptually as an alternative continuous-time generative framework:
  rather than specifying a noising SDE and matching its score, flow matching directly regresses a
  neural velocity field $v_\theta(x,t)$ onto the velocity of a chosen (often simple, e.g. linear/
  optimal-transport) interpolation path between a noise sample and a data sample, so that
  integrating the learned ODE $dx/dt = v_\theta(x,t)$ transports noise to data; **continuous
  normalizing flows** as the general ODE-based generative-model family flow matching trains
  efficiently (contrasted with the graduate course's instantaneous change-of-variables training by
  maximum likelihood, which requires an expensive trace-of-Jacobian term that flow matching's
  regression objective avoids entirely); the conceptual relationship between flow matching and the
  Week 2 probability-flow ODE (both are deterministic, ODE-based samplers; flow matching allows a
  much broader family of interpolation paths, not only the ones induced by a diffusion SDE).
- **Subtopics/Skills:** deriving the classifier-free-guidance formula from the implicit-classifier
  substitution; implementing classifier-free guidance on the Week 2 toy 2-D score model (training
  with condition dropout, then sampling at several guidance scales $w$ and visualizing the
  diversity/fidelity tradeoff); implementing a minimal flow-matching training loop on 2-D synthetic
  data (linear interpolation paths, a velocity-regression loss) and an Euler ODE sampler, comparing
  its sample quality/training simplicity against the Week 2 score-based sampler on the same toy
  distribution.
- **Readings:** current, well-established work on classifier-free guidance, as covered in lecture
  (the original formulation is read for its derivation, not for an invented citation); current,
  well-established work introducing flow matching as a simulation-free training objective for
  continuous normalizing flows, as covered in lecture.
- **Software:** PyTorch.
- **Quiz 1** (Weeks 2–3: SDE score-based diffusion, classifier-free guidance, flow matching).

## Week 4 — Mixture-of-Experts and Sparse Architectures
- **Topics:** The Mixture-of-Experts (MoE) layer: replacing a single dense feed-forward block with
  $N$ expert sub-networks $E_1,\dots,E_N$ (each typically itself a feed-forward block) and a
  learned **gating/router** network $g(x)=\mathrm{softmax}(W_g x)$ that produces a distribution
  over experts for each input token; **sparse** (top-$k$) **routing** — rather than computing a
  dense weighted sum over all $N$ experts (which would cost as much as a single $N\times$-wider
  dense layer), only the top-$k$ (commonly $k=1$ or $k=2$) experts by gate score are actually
  evaluated per token, and the layer's output is
  $y = \sum_{i \in \mathrm{top\text{-}}k(g(x))} g(x)_i \, E_i(x)$; why this decouples **total
  parameter count** (which scales with $N$) from **per-example compute** (which scales only with
  $k$), letting a sparse model hold far more parameters than a dense model at matched inference
  FLOPs per token — the central efficiency argument for MoE; the **load-balancing** problem
  (conceptual): without an explicit incentive, the router tends to collapse onto a small subset of
  "popular" experts, under-training the rest and wasting capacity, motivating auxiliary
  load-balancing losses (e.g., penalizing the squared coefficient of variation of expert-assignment
  frequency across a batch) and noisy/jittered gating as practical mitigations; MoE's placement in
  a Transformer (typically replacing the feed-forward sublayer, leaving attention dense) and the
  resulting token-level routing granularity.
- **Subtopics/Skills:** implementing a top-$k$ MoE layer from scratch in PyTorch (a router linear
  layer, a top-$k$ gate selection, and a batched dispatch to $N$ small expert feed-forward
  networks) and verifying its per-token FLOP count matches $k$ dense experts' worth of compute
  regardless of $N$; implementing a simple load-balancing auxiliary loss and empirically showing
  it flattens an otherwise skewed expert-usage histogram on a toy classification task; comparing a
  small dense feed-forward network against an MoE layer with matched per-token FLOPs but a larger
  total parameter count on a toy task.
- **Readings:** Shazeer et al., on Mixture-of-Experts / outrageously large neural networks — the
  primary source for this week's gating/routing formulation and load-balancing discussion.
- **Software:** PyTorch.
- **Assignment 1 assigned** (SDE-based diffusion, advanced diffusion techniques, Mixture-of-
  Experts — covering Weeks 2–4).

## Week 5 — In-Context Learning Mechanics
- **Topics:** The empirical phenomenon of **in-context learning (ICL)**: a frozen, pretrained
  decoder-only Transformer, given a prompt containing a handful of input-output examples of a new
  task followed by a fresh query, often produces a correct or near-correct answer to that query —
  with **no gradient update to any weight** — purely from the examples placed in its context
  window; why this is scientifically surprising relative to the graduate course's standard
  pretrain-then-fine-tune picture, since no parameter is being adapted to the new task at all; a
  careful, evidence-graded survey of current mechanistic hypotheses — **induction heads**,
  presented as a comparatively well-supported, falsifiable mechanistic finding (a specific
  two-attention-head circuit, identified via direct weight/activation analysis in small
  Transformers, that implements an approximate "complete the pattern" operation: having seen token
  B follow token A earlier in the context, an induction head attends back to the token that
  followed the most recent prior occurrence of the current token and copies it forward — a
  concrete algorithmic mechanism, not merely a correlational observation, and one with ablation
  evidence connecting its presence to few-shot ICL capability); the **implicit-gradient-descent
  analogy**, presented explicitly as more speculative theory (the claim that a Transformer's
  forward pass over in-context examples can, under restrictive assumptions, be shown mathematically
  equivalent to one or more steps of gradient descent on an implicit per-task loss, most cleanly
  demonstrated for linear-attention/linear-regression toy settings) — this week states precisely
  what the toy-setting results do and do not establish about real, nonlinear, large-scale
  Transformers, and why extrapolating the toy equivalence to claim "real ICL literally is gradient
  descent" outruns the current evidence.
- **Subtopics/Skills:** precisely restating the ICL phenomenon and why "no weight update" is the
  operative, surprising claim; working through a concrete toy trace of an induction-head circuit
  on a short repeated token sequence (by hand or in a minimal PyTorch attention-pattern
  visualization) and identifying the two-head mechanism; reproducing the linear-attention/linear-
  regression toy argument for the implicit-gradient-descent analogy at the level of its stated
  assumptions, and writing a structured critique distinguishing what is empirically demonstrated
  (induction heads' existence and correlation with ICL ability) from what remains a toy-setting
  analogy not yet established at scale (the general implicit-optimization claim).
- **Readings:** Brown et al., on GPT-3's few-shot in-context learning, for the empirical
  phenomenon; current mechanistic-interpretability research identifying and characterizing
  induction heads; current, more speculative work relating in-context learning to implicit
  gradient descent in restricted (e.g., linear-attention) settings — all read with explicit
  attention to each paper's actual evidentiary strength.
- **Software:** PyTorch (for the attention-pattern visualization exercise).
- **Quiz 2** (Weeks 4–5: Mixture-of-Experts, in-context learning mechanics).

## Week 6 — Preference-Based Alignment I: The Bradley-Terry Model and the RLHF Pipeline
- **Topics:** Formalizing pairwise human preference data: given two model outputs $y_1,y_2$ for
  the same prompt $x$, a human labels which is preferred; the **Bradley-Terry model** posits a
  latent scalar "quality" $r(x,y)$ for each output such that
  $P(y_1 \succ y_2 \mid x) = \sigma\big(r(x,y_1)-r(x,y_2)\big)$, where $\sigma$ is the logistic
  function — derived from the odds-ratio form $P(y_1\succ y_2)/P(y_2\succ y_1)=e^{r(x,y_1)-r(x,y_2)}$,
  the standard pairwise-comparison model also used in rating systems outside ML; fitting $r_\phi$
  (a **reward model**) by maximum likelihood on a dataset of labeled pairs reduces to minimizing the
  binary cross-entropy/logistic loss
  $-\mathbb E_{(x,y_w,y_l)}\big[\log \sigma(r_\phi(x,y_w) - r_\phi(x,y_l))\big]$ where $y_w,y_l$ are
  the preferred/dispreferred outputs; the **RLHF pipeline** structure, stage by stage — (1)
  supervised fine-tuning (SFT) of a pretrained language model on a dataset of high-quality
  demonstrations, producing a reference policy $\pi_{\mathrm{ref}}$; (2) reward-model training, as
  derived above, on preference pairs typically sampled from $\pi_{\mathrm{ref}}$ or a close
  variant; (3) RL fine-tuning of a policy $\pi_\theta$ (initialized from $\pi_{\mathrm{ref}}$)
  against the trained reward model, with a KL penalty to $\pi_{\mathrm{ref}}$ added to the reward to
  prevent the policy from drifting into reward-model blind spots (reward over-optimization/"reward
  hacking" against the learned proxy) — the full stage-3 objective
  $\max_\theta \mathbb E_{x,y\sim\pi_\theta}\big[r_\phi(x,y)\big] - \beta\, \mathbb{E}_x\,
  D_{\mathrm{KL}}\!\big(\pi_\theta(\cdot\mid x)\,\|\,\pi_{\mathrm{ref}}(\cdot\mid x)\big)$, set up
  here algebraically and optimized procedurally in Week 7.
- **Subtopics/Skills:** deriving the Bradley-Terry pairwise-probability formula from the odds-ratio
  assumption; implementing a small reward model (a shared encoder plus a scalar head) and training
  it via the Bradley-Terry logistic loss on synthetic preference pairs with a known, injected
  ground-truth reward, and verifying the fitted reward model's induced ranking correlates strongly
  with the ground truth; stating the three-stage RLHF pipeline precisely and writing out the
  stage-3 KL-regularized objective, explaining in one paragraph why the KL term is necessary (what
  goes wrong, concretely, if $\beta=0$).
- **Readings:** Ouyang et al., on InstructGPT and the RLHF pipeline — the primary source for this
  week's pipeline structure and KL-regularized objective.
- **Software:** PyTorch.

## Week 7 — Preference-Based Alignment II: PPO for RLHF and Direct Preference Optimization
- **Topics:** Optimizing the Week 6 stage-3 objective with **PPO**, conceptually, built explicitly
  on the graduate course's policy-gradient/actor-critic foundation (not re-derived here): the
  policy-gradient estimator is applied to the KL-regularized reward
  $\hat r(x,y) = r_\phi(x,y) - \beta\log\frac{\pi_\theta(y\mid x)}{\pi_{\mathrm{ref}}(y\mid x)}$ in
  place of the plain environment reward, with a learned value-function baseline (as in
  actor-critic) and PPO's clipped surrogate objective
  $L^{\mathrm{CLIP}}(\theta) = \mathbb E\big[\min(\rho_t \hat A_t,\ \mathrm{clip}(\rho_t,1-\epsilon,1+\epsilon)\hat A_t)\big]$
  (where $\rho_t = \pi_\theta/\pi_{\theta_{\mathrm{old}}}$ is the policy-ratio and $\hat A_t$ the
  estimated advantage) substituted for the raw policy-gradient update to bound how far a single
  update step moves the policy, improving stability; why RLHF's PPO stage is, in practice,
  engineering-heavy and unstable (four interacting networks — policy, value, reward model,
  reference policy — and sensitivity to the KL coefficient $\beta$); **Direct Preference
  Optimization (DPO)**, derived step by step as a reparameterization that sidesteps training an
  explicit reward model or running RL at all: starting from the stage-3 KL-regularized objective,
  its optimal policy has the closed form
  $\pi^*(y\mid x) \propto \pi_{\mathrm{ref}}(y\mid x)\exp\!\big(\tfrac{1}{\beta} r(x,y)\big)$
  (a standard KL-regularized-reward-maximization result); inverting this relation gives the reward
  implied by any policy, $r(x,y) = \beta\log\frac{\pi_\theta(y\mid x)}{\pi_{\mathrm{ref}}(y\mid x)} + \beta\log Z(x)$
  for a prompt-dependent (and hence, in a Bradley-Terry *difference*, cancelling) partition function
  $Z(x)$; substituting this implied reward directly into the Bradley-Terry loss from Week 6 — in
  place of a separately parameterized $r_\phi$ — yields the **DPO loss**,
  $\mathcal L_{\mathrm{DPO}}(\theta) = -\mathbb E_{(x,y_w,y_l)}\Big[\log\sigma\Big(\beta\log\tfrac{\pi_\theta(y_w\mid x)}{\pi_{\mathrm{ref}}(y_w\mid x)} - \beta\log\tfrac{\pi_\theta(y_l\mid x)}{\pi_{\mathrm{ref}}(y_l\mid x)}\Big)\Big]$,
  a single supervised-style classification loss directly on policy log-probabilities, with no
  reward model, no sampling from the policy during training, and no RL optimizer at all; why this
  reparameterization is correct (the $Z(x)$ terms cancel exactly in the Bradley-Terry log-odds
  difference) and what DPO gives up relative to full RLHF (it optimizes the same objective only to
  the extent the Bradley-Terry model and the closed-form optimal-policy derivation hold exactly;
  it cannot easily incorporate reward signals that are not naturally pairwise-preference-shaped).
- **Subtopics/Skills:** stating the PPO clipped objective and explaining each term's role in
  bounding policy-update step size, explicitly citing the graduate course's actor-critic baseline
  rather than re-deriving it; carrying out the full DPO derivation by hand (the closed-form optimal
  policy, the implied-reward inversion, the substitution into Bradley-Terry, the $Z(x)$
  cancellation) and stating precisely which step would break if the Bradley-Terry assumption did
  not hold; implementing the DPO loss in PyTorch and training a small policy against a frozen
  reference policy on synthetic preference pairs, verifying the fitted policy's implied reward
  ranking matches the injected ground truth, and comparing training stability/simplicity
  qualitatively against the Week 6 reward-model-plus-RL pipeline.
- **Readings:** Rafailov et al., on Direct Preference Optimization — the primary source for this
  week's derivation; course notes building explicitly on the graduate course's policy-gradient/
  actor-critic chapter for the PPO component.
- **Software:** PyTorch.
- **Quiz 3** (Weeks 6–7: Bradley-Terry, the RLHF pipeline, PPO for RLHF, DPO).

## Week 8 — Efficient Inference I: Quantization and Knowledge Distillation; Midterm Review
- **Topics:** **Quantization**: representing a trained network's weights (and, optionally,
  activations) in a lower-precision numeric format (e.g., INT8 in place of FP32/FP16) to reduce
  memory footprint and increase inference throughput; **post-training quantization (PTQ)** —
  quantizing an already-trained model's weights via a calibration pass that estimates a suitable
  per-tensor or per-channel scale/zero-point (the affine mapping
  $x_{\mathrm{int}} = \mathrm{round}(x/s) + z$, dequantized as $\hat x = s(x_{\mathrm{int}}-z)$),
  with no retraining; the resulting precision/accuracy tradeoff — PTQ is cheap but can degrade
  accuracy, especially at very low bit-widths or on outlier-heavy weight distributions; a
  conceptual look at **quantization-aware training (QAT)** — simulating quantization's rounding
  error during training itself (via a straight-through-estimator gradient through the
  non-differentiable round operation) so the network learns weights that are robust to the
  precision loss it will face at inference, typically recovering most of PTQ's accuracy gap at the
  cost of a retraining pass; **knowledge distillation**: the teacher-student framework, in which a
  smaller student network is trained not only on hard labels but to match a larger, already-trained
  teacher network's output distribution, using a distillation loss
  $\mathcal L = (1-\alpha)\,\mathcal L_{\mathrm{CE}}(y,\sigma(z_s)) + \alpha\, T^2\, D_{\mathrm{KL}}\big(\sigma(z_t/T)\,\|\,\sigma(z_s/T)\big)$
  (teacher/student logits $z_t,z_s$, a softmax temperature $T>1$ that softens both distributions to
  reveal more of the teacher's relative confidence across non-top classes, and the $T^2$ rescaling
  that keeps gradient magnitudes comparable across temperatures); why a softened teacher
  distribution carries more usable signal ("dark knowledge") than hard labels alone; midterm review
  for Weeks 1–8.
- **Subtopics/Skills:** implementing PTQ's affine quantize/dequantize mapping from scratch on a
  small trained network's weights and measuring the resulting accuracy drop at several bit-widths;
  implementing a straight-through-estimator-based QAT training loop and comparing its accuracy
  recovery against plain PTQ at the same bit-width; implementing the knowledge-distillation loss
  (teacher logits precomputed, student trained with the combined CE + temperature-scaled KL loss)
  and comparing a distilled student's accuracy against the same architecture trained from hard
  labels alone; review exercises spanning Weeks 1–8.
- **Readings:** current, well-established work on post-training and quantization-aware training
  for neural networks, as covered in lecture; Hinton et al., on knowledge distillation — the
  primary source for this week's distillation loss.
- **Software:** PyTorch.
- **Capstone problem-statement check-in.**

## Week 9 — Midterm Exam; Efficient Inference II: Speculative Decoding
- **Topics:** Midterm Exam (covers Weeks 1–8, qualifying-exam style — emphasis on deriving and
  critiquing, not just stating, each week's central result). Afterward: **speculative decoding** —
  autoregressive generation from a large target model is sequential and latency-bound because each
  new token requires a full forward pass; speculative decoding uses a small, fast **draft model**
  to propose a short block of $k$ candidate next tokens, then runs the large target model **once**,
  in parallel, over the whole proposed block to compute the target model's own probabilities for
  every position in it; an exact, un-biasing acceptance/rejection rule (modified rejection sampling)
  accepts each draft token $\hat x_i$ with probability $\min\!\big(1,\, p_{\mathrm{target}}(\hat
  x_i)/p_{\mathrm{draft}}(\hat x_i)\big)$ in sequence, stopping at the first rejection and, at the
  rejection point, resampling from the residual distribution
  $(p_{\mathrm{target}} - p_{\mathrm{draft}})_+$ renormalized — the key correctness argument,
  verified step by step, that this procedure's output token distribution is **exactly** the target
  model's own distribution, token for token, never an approximation, even though most tokens are
  "generated" by the cheap draft model; why this yields a wall-clock speedup with **zero** change
  to output quality whenever the draft model agrees with the target model reasonably often (the
  expected number of accepted tokens per large-model forward pass exceeds 1, amortizing the large
  model's per-call cost over multiple tokens); **KV-cache management**, conceptual — the key/value
  cache that lets autoregressive decoding avoid recomputing attention over the whole prefix at each
  step, and the practical bookkeeping speculative decoding requires to roll the cache back to the
  first rejected position when a draft token is rejected.
- **Subtopics/Skills:** stating and verifying the speculative-decoding acceptance probability
  formula's correctness property (that the marginal distribution of the accepted/resampled token
  equals the target model's distribution exactly) on a small discrete toy example with
  hand-computable probabilities; implementing a toy speculative-decoding simulation in PyTorch —
  two small toy "language models" (e.g., tiny categorical distributions over a short vocabulary at
  each position) standing in for the draft/target models, implementing the accept/reject/resample
  loop, and empirically confirming the resulting token distribution matches the target model's
  distribution over many trials while measuring the average number of tokens accepted per target
  forward pass; describing, precisely, what must be rolled back in the KV cache on a rejection.
- **Readings:** current, well-established work introducing speculative decoding for autoregressive
  language model inference, as covered in lecture.
- **Software:** PyTorch.
- **Assignment 2 assigned** (preference-based alignment, efficient inference — covering Weeks
  6–9).

## Week 10 — Neural Architecture Search
- **Topics:** The NAS problem formalized via its three components — a **search space** (the set of
  candidate architectures, e.g., a cell-based space of operation choices and connection patterns, or
  a macro space of per-layer width/depth choices), a **search strategy** (the algorithm that
  proposes candidate architectures to evaluate), and a **performance estimation strategy** (how a
  candidate's quality is scored, since training every candidate to full convergence is usually
  prohibitively expensive); **RL-based search** (conceptual) — treating architecture generation as
  a sequential decision process, with a controller (e.g., an RNN) that samples an architecture
  description and is trained via a policy-gradient method (the graduate course's REINFORCE,
  referenced not re-derived) using the sampled architecture's validation performance as the reward;
  **evolutionary search** (conceptual) — maintaining a population of candidate architectures,
  applying mutation (small random edits to the architecture encoding) and selection (keeping
  better-performing candidates) across generations; **differentiable NAS** (conceptual,
  DARTS-style) — relaxing the discrete choice of operation at each position into a continuous,
  softmax-weighted mixture over all candidate operations, making the architecture itself a
  differentiable function of continuous "architecture weights" that can be optimized jointly with
  network weights via gradient descent, then discretized by taking the arg-max operation at the
  end of search; why NAS's own search cost is itself a major practical constraint — early RL-based
  NAS required thousands of GPU-days to find a single architecture, motivating every later
  development (weight sharing across candidates, differentiable relaxation, cheap proxy
  performance estimators) as a direct response to this cost problem, not an independent
  improvement; the resulting efficiency-accuracy tradeoff in choosing a search strategy and a
  performance-estimation strategy.
- **Subtopics/Skills:** precisely stating the search-space/search-strategy/performance-estimation
  decomposition for a given NAS method description; implementing a small evolutionary search over a
  toy, tightly bounded architecture search space (e.g., choosing layer widths and activation
  functions for a small MLP) with mutation and truncation selection, and tracking best-found
  validation accuracy across generations; implementing a minimal differentiable-NAS-style relaxation
  (a softmax mixture over 2–3 candidate operations at a single point in a small network, with
  learnable architecture-weight logits trained jointly with the network's ordinary weights) and
  showing the architecture weights concentrate on one operation over training; estimating, from
  reported search costs in the literature versus the toy experiments just run, why search-cost
  reduction has been NAS's central practical research thread.
- **Readings:** current, well-established work on RL-based NAS, evolutionary NAS, and
  differentiable NAS (DARTS-style relaxation), as covered in lecture.
- **Software:** PyTorch.
- **Quiz 4** (Weeks 8–9: quantization, distillation, speculative decoding).

## Week 11 — Scaling Laws from a Systems/Engineering Perspective
- **Topics:** **This week is explicitly distinct from the sibling *Advanced Artificial Neural
  Network* course's Week 8, which asks why power-law scaling holds at all** (data-manifold,
  random-feature/kernel-theoretic explanations); this week instead takes the empirical power-law
  relationships (model size, data size, compute → loss) as a given engineering fact and asks how a
  real training run should actually allocate a fixed compute budget — **compute-optimal training
  in practice**: given a fixed compute budget $C \approx 6\,N\,D$ (model parameters $N$, training
  tokens $D$, the standard training-FLOPs approximation already available from the graduate
  course's large-scale-training material), the compute-optimal allocation (broadly attributed to
  Hoffmann et al.'s empirical refinement) grows $N$ and $D$ together at comparable rates as $C$
  grows, rather than growing $N$ alone — the direct practical consequence for a systems engineer:
  a model trained well past its data-optimal point for its size is wasting compute relative to a
  smaller model trained on proportionally more data at the same total cost, a concrete,
  actionable allocation decision rather than a theoretical curiosity; real engineering challenges of
  training at scale beyond the graduate course's Week 12 material — **checkpointing** (periodically
  saving full model/optimizer state so a long run can resume after a failure without restarting
  from scratch, and the practical tradeoff between checkpoint frequency, storage cost, and lost
  compute on failure) and **fault tolerance** (conceptual: at the node-count and duration of modern
  large-scale training runs, hardware failures are a near-certainty over the run's lifetime, not an
  edge case, so production training systems are engineered around elastic/fault-tolerant
  restart rather than an assumption of uninterrupted execution) — presented as the systems reality
  that compute-optimal allocation math has to survive in practice.
- **Subtopics/Skills:** deriving/using the $C\approx 6ND$ training-FLOPs approximation to compute,
  for a fixed compute budget, the compute-optimal $(N,D)$ allocation implied by a provided fitted
  scaling-exponent relationship, and contrasting it with a naive "just scale up $N$" allocation at
  the same total compute; computing, for a toy training-run specification (model size, checkpoint
  size, checkpoint interval, assumed failure rate), the expected compute lost to failures with and
  without checkpointing, and the optimal checkpoint interval that minimizes total expected lost
  compute; writing a short, precise statement of how this week's compute-allocation-in-practice
  question differs from the sibling course's "why do power laws hold" theoretical question.
- **Readings:** Hoffmann et al.'s compute-optimal scaling refinement, re-read here strictly for its
  practical compute-allocation implications (already introduced from a theoretical angle in the
  sibling *Advanced Artificial Neural Network* course); the graduate course's own Week 12
  large-scale-training material, extended here with checkpointing/fault-tolerance engineering
  practice as covered in lecture.
- **Software:** PyTorch, NumPy, Matplotlib (for the compute-allocation and checkpoint-interval
  exercises).

## Week 12 — Current Frontier Generative-Modeling Survey (Grounded, Fast-Moving)
- **Topics:** **This week is explicitly flagged as covering a fast-moving area** — a grounded,
  non-hype survey of current research on making diffusion-style generative models sample
  efficiently: **fast samplers and distillation of diffusion models**, surveyed conceptually —
  consistency-model-style ideas (training a model to map any point on a diffusion trajectory
  directly, in one or few steps, to the trajectory's clean endpoint, rather than requiring the
  many small steps the Week 2 reverse-SDE/probability-flow-ODE sampler needs), and progressive/step-
  distillation approaches (distilling a many-step sampler into a model that reproduces its output
  in far fewer steps, directly reusing Week 8's teacher-student distillation framing in the sampling-
  trajectory setting rather than the classification setting); the current landscape of sampling-
  efficiency research, presented as a landscape of competing tradeoffs (few-step samplers typically
  trade some sample quality or diversity for large latency reductions, and the precise Pareto
  frontier is an actively moving target this course does not pretend to fix permanently); honest
  framing throughout: this survey names the *kind* of technique and its mechanism precisely, while
  flagging that specific state-of-the-art numbers and the best current method will likely have
  changed by the time this course is offered again.
- **Subtopics/Skills:** precisely stating the consistency-model mechanism's training objective at
  the level of "map any trajectory point to the same clean endpoint" and explaining why this lets
  sampling collapse from many reverse-SDE steps to one or few; stating the progressive/step-
  distillation idea as a direct application of Week 8's teacher-student framing to a sampling
  trajectory rather than a classification output, and identifying precisely what plays the role of
  "teacher" and "student" in that setting; for one current fast-sampler technique (instructor-
  selected, refreshed each offering), stating what quality/latency tradeoff it reports and what
  about that report should be read with appropriate caution (small-scale evaluation, a single
  benchmark, a specific guidance-scale regime, etc.).
- **Readings:** current, well-established work on consistency-model-style and progressive/step-
  distillation approaches to fast diffusion sampling, selected by the instructor and refreshed each
  offering (no fixed citation list, as this is explicitly a fast-moving area; see
  `course-plan.md` §9).
- **Software:** none required beyond optional extension of the Week 2 toy sampler (critical-
  reading-and-survey-focused week; see `lab-manuals/lab-12.md`).
- **Quiz 5** (Weeks 10–11: neural architecture search, systems-view scaling laws).

## Week 13 — Research Methods for Applied Deep Learning Research
- **Topics:** How to read and critique a systems-and-methods deep learning paper efficiently and
  critically (identifying the precise engineering claim, e.g. a latency/throughput/accuracy number
  under a stated hardware and batch-size configuration; the baselines it is compared against and
  whether they are current and fairly tuned; whether an ablation isolates which specific design
  choice drives the reported improvement; and what is and is not reproducible from the paper as
  written); **benchmark culture and reproducibility challenges specific to large-scale DL research**
  — leaderboard-chasing and benchmark saturation/contamination; the gap between a benchmark number
  and real deployment behavior; compute cost itself as a barrier to independent reproduction of a
  claimed result (a paper's central result may require resources far beyond most readers' access,
  making independent verification rare in practice); hyperparameter and hardware sensitivity (a
  reported speedup or accuracy gain that depends on an under-disclosed batch size, precision
  setting, or hardware generation); structured in-class capstone work time: finalizing topic choice
  and literature search.
- **Subtopics/Skills:** critiquing a short systems-and-methods DL paper excerpt as a structured
  in-class exercise, identifying claim, evidence, baseline/ablation fairness, and a reproducibility
  concern; beginning the capstone literature search (3–5 candidate papers) and drafting a one-
  paragraph, falsifiable problem statement for the student's own candidate capstone topic.
- **Readings:** instructor-provided guidance handouts on reading/critiquing DL systems-and-methods
  papers; students begin selecting a paper from a suggested list for the Paper Critique &
  Presentation assignment.
- **Software:** none (methods/discussion week).
- **Paper Critique & Presentation assignment assigned** (due Week 15).

## Week 14 — Current Open Problems Survey
- **Topics:** A grounded survey of 2–3 currently active, unsolved engineering/research questions
  relevant to this course's topics, **explicitly flagged as a fast-moving area** where the specific
  set of open questions is refreshed each offering rather than fixed permanently by this syllabus —
  illustrative examples of the *kind* of open question surveyed (instructor-selected and refreshed):
  whether Mixture-of-Experts load-balancing can be solved with a principled mechanism rather than
  an auxiliary loss tuned per-model; how far the induction-heads/implicit-gradient-descent picture
  of in-context learning generalizes to the full range of tasks large pretrained models exhibit ICL
  on, and whether a single mechanistic account can cover all of them; whether DPO-style direct
  preference optimization's reliance on the exact Bradley-Terry/closed-form-optimal-policy
  derivation breaks down in practice when preference data violates the Bradley-Terry assumption,
  and what a principled fix looks like; whether speculative decoding's acceptance-rate gains can be
  pushed further by jointly training draft and target models rather than treating the draft model
  as a fixed, independently-trained component.
- **Subtopics/Skills:** for each surveyed open question, precisely stating what would count as
  progress or resolution, and what the strongest current partial answer actually establishes versus
  what it is sometimes informally taken to establish; relating at least one surveyed open question
  to the student's own emerging capstone interest.
- **Readings:** instructor-curated current papers/preprints representing the frontier of each
  surveyed open question (refreshed each offering).
- **Software:** none required this week (critical-writing/discussion week).
- **Quiz 6** (Weeks 12–13: frontier generative-modeling survey, research-methods standards).

## Week 15 — Research Proposal Work Session
- **Topics:** Structured, instructor-guided time for drafting and refining the capstone research
  proposal: finalizing the problem statement, completing the 5+ paper related-work survey,
  developing the proposed novel approach, and building the feasibility argument or preliminary
  result; a thesis-committee-style structured peer-review workshop on complete proposal drafts,
  applying the Week 13 precision test and survey-synthesis standard to a peer's draft and producing
  specific, actionable feedback; producing a written revision plan in response to feedback
  received.
- **Subtopics/Skills:** giving and receiving structured, specific peer feedback on a problem
  statement's precision, a survey's synthesis quality, a proposed approach's novelty, and a
  feasibility argument's honesty about risk; revising a proposal draft in direct response to
  specific, itemized feedback.
- **Readings:** none assigned; working session on students' own capstone materials.
- **Software:** whatever each proposal's feasibility argument or preliminary result requires.
- **Deliverable:** capstone written-proposal draft due; peer-feedback worksheet submitted; Paper
  Critique & Presentation due.

## Week 16 — Capstone Research Proposal Presentations; Course Review
- **Topics:** Student capstone research-proposal presentations, delivered and defended in a
  qualifying-exam/thesis-proposal-defense format (problem statement, related work, proposed
  approach, feasibility argument or preliminary results, anticipated risks, committee-style Q&A);
  recap of the course map (SDE score-based diffusion → classifier-free guidance/flow matching →
  Mixture-of-Experts → in-context learning mechanics → Bradley-Terry/RLHF → PPO/DPO → quantization/
  distillation → speculative decoding → neural architecture search → systems-view scaling laws →
  frontier generative survey → research methods → open problems); discussion of where this
  course's frontier-engineering foundation connects into the sibling postgraduate courses
  (*Advanced Artificial Neural Network*, *Advanced Artificial Intelligence*, *Advanced Machine
  Learning*) and into current deep learning research venues such as NeurIPS, ICML, and ICLR.
- **Deliverable:** Final written research proposal + oral defense.

## Week 17 — Final Exam Week
- Comprehensive final exam, weighted toward Weeks 9–14 content (per Assessment Plan).

---

## Research Proposal Capstone (introduced Week 1 overview; problem-statement check-in Week 8;
## research-methods workshop Week 13; draft due Week 15; final proposal + defense Week 16)
This capstone is a **research proposal**, not a completed project — a PhD-qualifying-exam-style
deliverable evaluated the way a thesis-proposal committee evaluates a candidate's proposal before
the dissertation work has happened. Each student formulates an original, falsifiable research
question connected to one of this course's topics (SDE-based score generative modeling and
advanced diffusion techniques; Mixture-of-Experts and sparse architectures; in-context learning
mechanics; preference-based alignment engineering; efficient inference; neural architecture
search; systems-view scaling and large-scale training engineering; frontier generative-modeling
sampling efficiency) or to adjacent territory with instructor approval, conducts a related-work
survey of 5+ papers that synthesizes rather than lists, proposes a novel approach or extension of
their own formulation, and provides either preliminary results from a small pilot or a rigorous
feasibility argument (what must be true, the main technical risk named honestly, and why that risk
is judged manageable). Results — including negative or partial ones — must be reported honestly
and analyzed soundly; see `assignments/capstone-rubric.md` and
`assignments/capstone-proposal-guidelines.md`.
