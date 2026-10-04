# Presentation: Module 3 — Graph Neural Networks & Deep RL (Weeks 8–11)

**Format:** Slide deck outline for lecture delivery.

1. **Title slide** — Module 3: Graph Neural Networks & Deep RL
2. **Representing graphs** — adjacency, node/edge features, permutation invariance
3. **Message passing** — the aggregate-and-update framework
4. **The GCN layer** — spectral motivation (brief); the normalized-adjacency spatial form
5. **Midterm review** — Weeks 1–8 topic map
6. **GraphSAGE** — sampled-neighborhood aggregation; concatenation with self-representation
7. **Graph Attention Networks** — learned, softmax-normalized per-neighbor attention
8. **GNN applications** — molecule property prediction; recommendation systems
9. **From tabular to deep RL** — why a $Q$-table fails at scale (not re-deriving Bellman/tabular
   Q-learning, assumed from *Artificial Intelligence*, Graduate)
10. **Deep Q-Networks** — the DQN loss; experience replay; target networks
11. **Why naive function approximation can diverge** — correlated data, moving targets,
    generalization
12. **Policy gradients & REINFORCE, derived** — the log-derivative trick; the estimator
13. **Baselines & actor-critic** — variance reduction; the advantage $A(s,a)=G_t-V(s)$

**Speaker notes:** slide 11 (why naive function approximation can diverge) is this module's
conceptual anchor for the RL half — it is the first time in the course students see a training
*instability* argument as rigorous as the identity-mapping argument from Module 1's ResNet
slide; connect the two explicitly.
