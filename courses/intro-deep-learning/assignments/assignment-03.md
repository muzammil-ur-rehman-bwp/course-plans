# Assignment 3 — Attention and Transformers (Weeks 8–9)

**Weight:** 5% of course grade (one of 4 assignments, 20% total) | **Assigned:** Week 9 | **Due:** Start of Week 11

## Instructions
Submit a single Jupyter notebook `assignment03.ipynb` answering all questions below. Show your
work (code + brief explanation) for each question.

## Questions
1. **(Attention by hand, 20 pts)** Given three encoder hidden states and one decoder query
   (provided as `assignment03_attention_values`), compute dot-product attention scores, apply
   softmax by hand, and compute the resulting context vector; verify with
   `dot_product_attention` from lecture.
2. **(Information bottleneck, 15 pts)** In 150–250 words, explain the information bottleneck
   problem in basic seq2seq models and how attention addresses it.
3. **(Self-attention, 25 pts)** Implement scaled dot-product self-attention on a provided 5-token
   sequence `assignment03_sequence`; verify your result against `nn.MultiheadAttention` with
   `num_heads=1`.
4. **(Multi-head attention and positional encoding, 25 pts)** Using `nn.MultiheadAttention` with
   4 heads on the same sequence, visualize the attention weights as a heatmap; then add sinusoidal
   positional encoding to the input and repeat, discussing how (if at all) the weights change.
5. **(Transformer architecture, 15 pts)** In 150–250 words, describe the overall encoder-decoder
   Transformer architecture at a high level, citing Vaswani et al.'s "Attention Is All You Need"
   (2017), and explain why positional encoding is necessary even though self-attention examines
   every token.

## Submission
Upload `assignment03.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
