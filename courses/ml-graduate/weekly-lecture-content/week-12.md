# Week 12 — Lecture Content: Dimensionality Reduction Theory

## 1. PCA, Re-Derived Rigorously: the Variance-Maximization Formulation
Given centered data $x_1,\dots,x_m\in\mathbb{R}^d$ (mean subtracted) with empirical covariance
$\Sigma=\frac1m\sum_i x_ix_i^\top$, PCA seeks a unit vector $u$ ($\|u\|=1$) onto which projecting
the data retains maximum variance:
$$
\max_{\|u\|=1}\ \frac1m\sum_i (u^\top x_i)^2 \;=\; \max_{\|u\|=1}\ u^\top\Sigma u.
$$

## 2. The Lagrangian Argument — Optimal Directions Are Eigenvectors
Form the Lagrangian for the constrained maximization: $\mathcal{L}(u,\lambda) = u^\top\Sigma u -
\lambda(u^\top u-1)$. Setting the gradient to zero:
$$
\nabla_u\mathcal{L} = 2\Sigma u - 2\lambda u = 0 \;\;\Longrightarrow\;\; \Sigma u = \lambda u.
$$
So any stationary point $u$ is an **eigenvector** of $\Sigma$ with eigenvalue $\lambda$. Evaluating
the objective at such a point: $u^\top\Sigma u = u^\top(\lambda u) = \lambda\|u\|^2 = \lambda$. So
the objective value *equals* the eigenvalue — to **maximize** variance, choose $u$ to be the
eigenvector with the **largest** eigenvalue $\lambda_1$. This is the **first principal
component**.

## 3. Optimality for the Top-$k$ Subspace — Proof Sketch
For the top-$k$ projection (maximizing total retained variance among all rank-$k$ orthogonal
projections, i.e., choosing an orthonormal set $u_1,\dots,u_k$ to maximize
$\sum_{j=1}^k u_j^\top\Sigma u_j$), the **Courant–Fischer (Rayleigh-quotient) theorem**
characterizes this directly: for a symmetric $\Sigma$ with eigenvalues $\lambda_1\geq\cdots\geq
\lambda_d\geq0$,
$$
\max_{U\in\mathbb{R}^{d\times k},\, U^\top U=I_k}\ \mathrm{tr}(U^\top\Sigma U) \;=\; \sum_{j=1}^k \lambda_j,
$$
attained exactly by $U=[\,u_1,\dots,u_k\,]$, the top-$k$ eigenvectors. **Inductive sketch:**
having found $u_1$ (Section 2) as the global variance-maximizing direction, restrict the search to
the orthogonal complement of $u_1$ — within that subspace, the same Lagrangian argument (now
applied to $\Sigma$ restricted to that subspace) shows the next-best direction is the
largest-eigenvalue eigenvector of $\Sigma$ *within the complement*, which (since eigenvectors of a
symmetric matrix are mutually orthogonal) is exactly $u_2$, the second-largest eigenvector of the
original $\Sigma$. Repeating this deflation argument $k$ times shows the top-$k$ eigenvectors are
jointly optimal — not just a greedy approximation, but the true global maximizer of retained
variance among all rank-$k$ orthogonal projections.

## 4. Explained Variance
The **explained variance ratio** of the top $k$ components is $\sum_{j=1}^k\lambda_j /
\sum_{j=1}^d\lambda_j$ — a direct consequence of Section 3: $\mathrm{tr}(\Sigma)=\sum_j\lambda_j$
is the *total* variance in the data (trace is invariant to the choice of orthonormal basis), and
Section 3 shows the top-$k$ components capture exactly $\sum_{j=1}^k\lambda_j$ of it, the maximum
achievable by any rank-$k$ linear projection.

## 5. Kernel PCA
PCA is a *linear* method — it can only find linear subspaces. **Kernel PCA** performs PCA
implicitly in the RKHS $\mathcal{H}_k$ induced by a kernel $k$ (Week 7), capturing nonlinear
structure in the original space. Given a (not necessarily centered) kernel matrix $K_{ij}=
k(x_i,x_j)$, first **center** it in feature space:
$$
\widetilde K = K - \mathbf{1}_m K - K\mathbf{1}_m + \mathbf{1}_m K \mathbf{1}_m,\qquad (\mathbf{1}_m)_{ij}=\tfrac1m,
$$
(this is the kernelized equivalent of mean-subtracting the data before ordinary PCA). Eigendecompose
$\widetilde K = V\Lambda V^\top$; the projection of training point $x_i$ onto the $j$-th kernel
principal component is $\sqrt{\lambda_j}\,V_{ij}$, and a new point $x_\star$ projects via
$\sum_i \alpha_{ij}\, k(x_i,x_\star)$ with $\alpha_{\cdot j}=V_{\cdot j}/\sqrt{\lambda_j}$ — again
a finite kernel expansion, directly analogous to the representer theorem's form.

## 6. Nonlinear Manifold Learning — Conceptual Survey
PCA (and kernel PCA with a fixed, global kernel) can still be limited when data lies on a
*curved*, low-dimensional manifold embedded nonlinearly in high-dimensional space (e.g., a "Swiss
roll"). Two influential ideas that go further:
- **Isomap:** build a neighborhood graph over the data, estimate pairwise **geodesic** distances
  (shortest paths along the manifold, approximated by shortest paths on the neighborhood graph)
  instead of straight-line Euclidean distances, then apply classical multidimensional scaling to
  find a low-dimensional embedding that best preserves those geodesic distances — directly
  targeting the manifold's intrinsic geometry rather than its ambient linear structure.
- **t-SNE:** models pairwise similarities in the *original* space as probabilities (based on local
  neighborhoods, with a Gaussian kernel whose bandwidth adapts per-point to match a target local
  "perplexity"), and finds a low-dimensional embedding whose pairwise similarities — modeled with a
  **heavy-tailed** (Student-t) distribution to avoid "crowding" points together — match those
  probabilities as closely as possible (minimizing a KL divergence between the two similarity
  distributions). t-SNE is excellent for visualization (preserving local neighborhood structure)
  but, unlike PCA, its embedding does not have a simple linear out-of-sample extension and its
  global distances are not meaningfully interpretable.

## 7. Code: PCA via Eigendecomposition vs. scikit-learn, and Kernel PCA
```python
import numpy as np
from sklearn.decomposition import PCA, KernelPCA

rng = np.random.default_rng(0)
X = rng.normal(size=(200, 5)) @ rng.normal(size=(5, 5))   # correlated features
X_centered = X - X.mean(axis=0)

cov = (X_centered.T @ X_centered) / X_centered.shape[0]
eigvals, eigvecs = np.linalg.eigh(cov)                    # ascending order
order = np.argsort(eigvals)[::-1]
eigvals, eigvecs = eigvals[order], eigvecs[:, order]

k = 2
scratch_proj = X_centered @ eigvecs[:, :k]

skl_pca = PCA(n_components=k).fit(X)
skl_proj = skl_pca.transform(X)

# sign of eigenvectors is arbitrary; compare absolute column-wise correlation instead of raw values
for j in range(k):
    corr = np.corrcoef(scratch_proj[:, j], skl_proj[:, j])[0, 1]
    print(f"component {j}: |correlation| between from-scratch and sklearn projections = {abs(corr):.6f}")

print("explained variance ratio (from-scratch):", np.round(eigvals[:k] / eigvals.sum(), 4))
print("explained variance ratio (sklearn):      ", np.round(skl_pca.explained_variance_ratio_, 4))

# Kernel PCA on a nonlinearly separable (concentric circles) dataset
theta = rng.uniform(0, 2 * np.pi, 100)
inner = np.c_[np.cos(theta), np.sin(theta)]
outer = np.c_[2 * np.cos(theta), 2 * np.sin(theta)]
X_circles = np.vstack([inner, outer])

kpca = KernelPCA(n_components=2, kernel="rbf", gamma=1.0)
X_kpca = kpca.fit_transform(X_circles)
print("Kernel PCA separates the two circles along component 0:",
      np.sign(X_kpca[:100, 0]).std() == 0 or np.sign(X_kpca[100:, 0]).std() == 0)
```
Expect near-perfect (`|correlation| ~ 1.0`) agreement between the from-scratch and `sklearn`
projections (up to an arbitrary sign flip per component) and matching explained-variance ratios;
expect kernel PCA's leading component to separate the two concentric circles in a way linear PCA
cannot (linear PCA on this data finds no single linear direction separating inner from outer).

## 8. In-Class Exercise
Using Section 3's eigenvalue characterization, explain why the explained-variance ratio is always
non-increasing as you compute it for $k=1,2,\dots,d$ components, and why it must reach exactly $1$
at $k=d$.
