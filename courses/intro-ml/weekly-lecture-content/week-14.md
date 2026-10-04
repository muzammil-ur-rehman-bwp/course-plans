# Week 14 — Lecture Content: Recommender Systems

## 1. Content-Based Filtering
Content-based filtering recommends items similar to ones a user already liked, based on **item
feature vectors** (e.g., a movie's genre tags, a product's category/description embedding).
Similarity between two feature vectors `a` and `b` is commonly measured with **cosine
similarity**:
```
cos_sim(a, b) = (a . b) / (||a|| * ||b||)
```
which ranges from -1 to 1 (or 0 to 1 for non-negative feature vectors), measuring the angle
between vectors regardless of their magnitude.

```python
from sklearn.metrics.pairwise import cosine_similarity
import numpy as np

# item_features: one row per item, one column per content feature (e.g., genre one-hot vector)
sim_matrix = cosine_similarity(item_features)   # shape (n_items, n_items)

def recommend_similar_items(item_idx, sim_matrix, top_n=5):
    scores = sim_matrix[item_idx]
    similar_idx = np.argsort(scores)[::-1]
    similar_idx = similar_idx[similar_idx != item_idx]   # exclude the item itself
    return similar_idx[:top_n]
```
Content-based filtering works well even for brand-new items with no ratings yet (no
"cold-start" problem for items), but is limited to features the system designer thought to
encode, and tends to over-recommend very similar items.

## 2. The User-Item Ratings Matrix
Collaborative filtering instead uses a **user-item ratings matrix** `R`, where `R[u, i]` is user
`u`'s rating of item `i` (often very sparse — most users rate only a small fraction of items).
Ratings may be **explicit** (a star rating) or **implicit** (a click, purchase, or watch-time,
treated as a positive signal).

## 3. User-Based Collaborative Filtering
Find users similar to the target user (by cosine similarity, or Pearson correlation, over their
rating vectors), then predict a rating for an unseen item as a similarity-weighted average of
those similar users' ratings for that item:
```
predicted_rating(u, i) = sum_{v in neighbors(u)} sim(u, v) * R[v, i] / sum_{v in neighbors(u)} |sim(u, v)|
```
```python
from sklearn.neighbors import NearestNeighbors

# R: users x items ratings matrix (missing entries filled with 0 or user-mean, per design choice)
model = NearestNeighbors(metric="cosine", n_neighbors=5)
model.fit(R)
distances, neighbor_idx = model.kneighbors(R[target_user_idx].reshape(1, -1))
```

## 4. Item-Based Collaborative Filtering
Symmetric to the user-based approach, but computes similarity **between items** based on how
users rated them (i.e., over `R`'s columns), then predicts a user's rating for item `i` as a
similarity-weighted average of their own ratings for similar items. Item-based filtering is
often preferred in practice because item-item similarities tend to be more stable over time than
user-user similarities (users' tastes and available neighbors shift faster than relationships
between items).

## 5. A Simple Worked Example
Combining both ideas: a small movie-ratings scenario where (a) a content-based recommender
suggests movies in the same genre as ones a user rated highly, and (b) an item-based
collaborative recommender suggests movies that users with similar rating patterns also rated
highly — illustrating how a real system (e.g., hybrid recommenders) often blends both signals,
using content-based recommendations to cover new items and collaborative signals to capture
patterns content features alone would miss.

```python
# Minimal worked numeric example: 4 users x 5 movies ratings matrix
import pandas as pd

R = pd.DataFrame(
    [[5, 4, 0, 1, 0],
     [4, 5, 0, 1, 0],
     [1, 1, 5, 4, 0],
     [0, 0, 4, 5, 5]],
    index=["Alice", "Bob", "Carol", "Dave"],
    columns=["M1", "M2", "M3", "M4", "M5"],
)
item_sim = cosine_similarity(R.T.values)   # item-item similarity from the ratings matrix
```

## 6. In-Class Exercise
Using two rows of a small ratings matrix, compute cosine similarity between two users by hand,
then predict a plausible rating for a held-out item for one of them using the other as the sole
neighbor.
