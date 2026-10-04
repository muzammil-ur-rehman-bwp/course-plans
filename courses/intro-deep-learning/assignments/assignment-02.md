# Assignment 2 — Sequence Models (Weeks 6–7)

**Weight:** 5% of course grade (one of 4 assignments, 20% total) | **Assigned:** Week 7 | **Due:** Start of Week 9

## Instructions
Submit a single Jupyter notebook `assignment02.ipynb` answering all questions below. Show your
work (code + brief explanation) for each question.

## Questions
1. **(LSTM gate equations, 20 pts)** Given numeric weight matrices/biases and an input/previous
   state (provided as `assignment02_lstm_values`), compute $f_t, i_t, o_t, \tilde{c}_t, c_t, h_t$
   by hand (showing each intermediate value), then verify against `nn.LSTMCell`.
2. **(Vanishing gradients in RNNs, 15 pts)** In 150–250 words, explain why vanilla RNNs struggle
   with long sequences, and explain specifically how the LSTM's cell-state update mitigates this
   compared to a vanilla RNN's hidden-state update.
3. **(LSTM vs. GRU, 25 pts)** Build and train both an `nn.LSTM`-based and an `nn.GRU`-based model
   on a provided sequence dataset `assignment02_sequences`; compare final performance and
   parameter count.
4. **(Sequence-to-sequence, 30 pts)** Build and train a small LSTM-based encoder-decoder model
   (text generation or time-series forecasting, your choice) using teacher forcing; generate
   output autoregressively for at least 3 test examples and discuss any divergence observed,
   connecting it to exposure bias.
5. **(Reflection, 10 pts)** In 4–6 sentences, compare the LSTM and GRU architecturally (gates,
   parameter count) and state one scenario where you would prefer each.

## Submission
Upload `assignment02.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
