# Week 2 Lecture Plan — Advanced Deep Learning (Post Graduate)
## Topic: Score-Based Generative Models — The SDE Formulation

**Duration:** 2 hours lecture + 3 hour research seminar/lab

### Learning Objectives (Bloom's Level)
1. Write the forward variance-preserving SDE and derive its Euler-Maruyama discretization,
   matching it to the graduate course's DDPM forward update. (*Apply, Analyze*)
2. State the reverse-time SDE and explain why sampling reduces to knowing the score function at
   every noise level. (*Understand, Apply*)
3. Derive the denoising score-matching objective and show its exact correspondence to the DDPM
   noise-prediction loss. (*Analyze*)
4. Implement a toy score network and an Euler-Maruyama reverse-SDE sampler on 2-D synthetic data.
   (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap + motivation | DDPM as a discrete Markov chain (graduate course recap, one slide); why a continuous-time view is more general and reveals more tools |
| 0:15–0:45 | The forward SDE | Variance-preserving SDE; Euler-Maruyama discretization; matching coefficients to DDPM's $\sqrt{1-\beta_t}x_{t-1}+\sqrt{\beta_t}\epsilon$ |
| 0:45–1:15 | The reverse-time SDE | Anderson's time-reversal result (stated, used); the score function's role; why sampling = simulating backwards |
| 1:15–1:45 | Score matching | The closed-form Gaussian marginal; the denoising score-matching objective; the DDPM-loss correspondence, derived on the board |
| 1:45–2:00 | Synthesis + preview | Probability-flow ODE pointer (→ Week 12); recap table |

### Materials/Equipment
- Slides: "Score-Based Generative Models: The SDE View"
- Whiteboard for the Euler-Maruyama/DDPM coefficient-matching derivation
- Live-coding environment (Jupyter, PyTorch)

### Formative Check (in-class)
Given the VP-SDE's drift/diffusion coefficients $f(x,t)=-\tfrac12\beta(t)x$, $g(t)=\sqrt{\beta(t)}$,
write the one-step Euler-Maruyama update and show it reduces to the DDPM forward step when
$\beta(t)\,\Delta t \to \beta_t$.

### Link to Lab/Assessment
Lab 2: implement a toy score network trained by denoising score matching, and an Euler-Maruyama
reverse-SDE sampler, on 2-D synthetic data (see `lab-manuals/lab-02.md`).
