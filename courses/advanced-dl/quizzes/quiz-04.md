# Quiz 4 — Quantization, Distillation, and Speculative Decoding (Week 10)

**Duration:** 15 minutes | **Format:** 5 short-answer/code questions | **Weight:** part of
Quizzes (10%, best 5 of 6 counted)

1. Write the affine post-training quantization mapping (quantize and dequantize), and state why
   an outlier value in a tensor degrades PTQ's precision for every other value in that tensor.
   (2 pts)
2. Explain, in 2–3 sentences, what the straight-through estimator does in quantization-aware
   training and why it is needed. (2 pts)
3. Write the knowledge-distillation loss and explain the role of the temperature $T$ and the
   $T^2$ rescaling factor. (2 pts)
4. State the speculative-decoding acceptance probability formula, and state precisely what
   happens to a rejected position's token. (2 pts)
5. A colleague claims: "Speculative decoding produces slightly lower-quality text than running
   the target model alone, as the price of its speedup." Identify precisely what is wrong with
   this claim. (2 pts)

**Answer key available to instructors in the course LMS gradebook (not distributed to students
before the quiz).**
