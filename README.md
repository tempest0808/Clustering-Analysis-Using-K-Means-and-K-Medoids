# MSCS_634_Lab_3 — Clustering with K-Means and K-Medoids

## Purpose

This lab explores unsupervised clustering on the **Wine dataset** from
`sklearn.datasets`. The goals are to:

- Load, explore, and standardize the Wine dataset (13 numeric features, 3
  classes, 178 samples).
- Implement and train a **K-Means** clustering model with k = 3.
- Implement and train a **K-Medoids** clustering model with k = 3.
- Evaluate both models with the **Silhouette Score** (internal cluster
  cohesion/separation) and the **Adjusted Rand Index (ARI)** (agreement
  with the true wine cultivar labels).
- Visualize both sets of clusters side by side using a 2-D PCA projection,
  with centroids/medoids marked, and compare the two algorithms.

## Key Insights and Observations

- After z-score standardization, **K-Means (k=3)** achieved a Silhouette
  Score of **≈0.28** and an Adjusted Rand Index of **≈0.90** — very close
  agreement with the true wine classes.
- **K-Medoids (k=3)**, implemented from scratch using the PAM algorithm,
  achieved a Silhouette Score of **≈0.27** and an ARI of **≈0.74**.
- **K-Means slightly outperformed K-Medoids** on this dataset. This makes
  sense: the Wine features are continuous, standardized, and reasonably
  free of extreme outliers, which is exactly the setting where K-Means
  (mean-based centroids, Euclidean distance) tends to do well.
- Visually, both algorithms recover the same broad three-cluster structure
  in PCA space, but K-Means' centroids sit at the true geometric center of
  each cluster, while K-Medoids' medoids are always actual data points and
  land slightly off-center, closer to the densest part of each group.
- **When to prefer which:** K-Means is a good default for clean, numeric,
  roughly globular data where speed matters. K-Medoids is preferable when
  the data has outliers/noise (medoids are far more robust than means),
  when a non-Euclidean distance/dissimilarity measure is needed, or when
  interpretability of the "representative" point per cluster matters,
  since each medoid is a real observation rather than an abstract average.

## Challenges Faced and Decisions Made

- **No `scikit-learn-extra` available:** The standard `KMedoids` class
  (from `scikit-learn-extra`) could not be installed in this environment
  (no internet access). To meet the lab requirement, K-Medoids was
  implemented **from scratch** using the classic PAM (Partitioning Around
  Medoids) alternating-optimization approach.
- **Poor local optima with naive random initialization:** An initial,
  purely-random medoid initialization produced a much lower ARI (~0.34) for
  K-Medoids than expected. This was resolved by switching to a
  **k-means++-style probabilistic initialization** and running **10 random
  restarts**, keeping the solution with the lowest total within-cluster
  distance — which brought K-Medoids' ARI up to a much more reasonable
  ~0.74 and made the K-Means vs. K-Medoids comparison fair.
- **Visualizing 13-dimensional data:** Since the Wine dataset has 13
  features, **PCA** was used to project both the data points and the
  cluster centroids/medoids down to 2 components purely for plotting
  (the clustering itself was performed on the full standardized
  13-dimensional data, not the PCA projection).

## Files in This Repository

| File | Description |
|---|---|
| `MSCS_634_Lab_3.ipynb` | Jupyter Notebook with all code, outputs, and analysis |
| `README.md` | This file |

## How to Run

1. Clone this repository.
2. Install dependencies: `pip install numpy pandas matplotlib scikit-learn scipy`
3. Open `MSCS_634_Lab_3.ipynb` in Jupyter Notebook / JupyterLab and run all cells.
