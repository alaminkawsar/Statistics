# Principal Component Analysis (PCA) and Alternatives for 256-Dimensional Embeddings

A practical, mathematical, and implementation-focused guide to reducing,
visualizing, comparing, and evaluating high-dimensional vectors.

> **Primary use case:** You have one or more groups of 256-dimensional
> vectors and want to visualize their structure in 2D or 3D, compare
> groups, inspect clusters, and choose a dimensionality-reduction method
> without drawing misleading conclusions.

------------------------------------------------------------------------

## Table of contents

-   [1. What PCA is](#1-what-pca-is)
-   [2. Your 256-dimensional data](#2-your-256-dimensional-data)
-   [3. Intuition: variance and principal
    directions](#3-intuition-variance-and-principal-directions)
-   [4. PCA mathematics](#4-pca-mathematics)
-   [5. PCA algorithm, step by step](#5-pca-algorithm-step-by-step)
-   [6. Explained variance and choosing
    dimensions](#6-explained-variance-and-choosing-dimensions)
-   [7. Practical PCA with two groups of
    embeddings](#7-practical-pca-with-two-groups-of-embeddings)
-   [8. Two individual 256-D vectors are a special
    case](#8-two-individual-256-d-vectors-are-a-special-case)
-   [9. Preprocessing: centering, scaling, and
    normalization](#9-preprocessing-centering-scaling-and-normalization)
-   [10. PCA variants and computational
    considerations](#10-pca-variants-and-computational-considerations)
-   [11. Limitations of PCA](#11-limitations-of-pca)
-   [12. Alternatives to PCA](#12-alternatives-to-pca)
-   [13. How to choose an alternative](#13-how-to-choose-an-alternative)
-   [14. Comparing two groups
    correctly](#14-comparing-two-groups-correctly)
-   [15. Evaluation: do not trust a plot
    alone](#15-evaluation-do-not-trust-a-plot-alone)
-   [16. Common mistakes and how to avoid
    them](#16-common-mistakes-and-how-to-avoid-them)
-   [17. Reusable end-to-end Python
    pipeline](#17-reusable-end-to-end-python-pipeline)
-   [18. Reproducibility and
    deployment](#18-reproducibility-and-deployment)
-   [19. Decision checklist](#19-decision-checklist)
-   [20. Glossary](#20-glossary)
-   [21. References](#21-references)

------------------------------------------------------------------------

## 1. What PCA is

**Principal Component Analysis (PCA)** is an unsupervised, linear
transformation that represents data using new, mutually orthogonal
directions, ordered by how much variance they capture.

Suppose there are `N` observations, each represented by 256 features:

\[ X `\in `{=tex}`\mathbb{R}`{=tex}\^{N `\times 256`{=tex}} \]

PCA can transform them into `k` coordinates:

\[ Z `\in `{=tex}`\mathbb{R}`{=tex}\^{N `\times `{=tex}k},
`\quad `{=tex}k \< 256 \]

For visualization, commonly choose:

-   `k = 2` for a 2D scatter plot.
-   `k = 3` for a 3D scatter plot.

For example:

``` text
Original embeddings: (1000, 256)
PCA with 2 components: (1000, 2)
PCA with 3 components: (1000, 3)
```

PCA does not simply select two original features. It constructs two new
features, each a weighted combination of all 256 original features.

### What PCA is useful for

-   Visual exploration of high-dimensional data.
-   Removing linear redundancy among features.
-   Compressing data while retaining a chosen amount of variance.
-   Denoising, in some settings.
-   Producing compact inputs for downstream models.
-   Inspecting broad structure in embedding collections.

### What PCA does not guarantee

-   Separation of classes or groups.
-   Preservation of every pairwise distance.
-   Preservation of semantic similarity.
-   Discovery of nonlinear manifolds.
-   A faithful 2D picture of all 256-dimensional relationships.

A PCA plot is a projection, not a complete representation of the
original space.

------------------------------------------------------------------------

## 2. Your 256-dimensional data

The shape of the matrix matters.

### Case A: Many vectors, each 256-dimensional

For 500 embeddings:

``` python
X.shape == (500, 256)
```

There are 500 observations and 256 features. This is the normal PCA use
case.

### Case B: Two groups of vectors

For example:

``` python
A.shape == (100, 256)  # Group A
B.shape == (120, 256)  # Group B
```

Combine the groups:

``` python
X = np.vstack([A, B])
# X.shape == (220, 256)
```

Fit one PCA model to the combined observations, then color points
according to their group labels.

**Do not independently fit PCA to A and B if you intend to compare their
plotted coordinates.** Independently fitted models can have different
axes, rotations, and scales. Their coordinates would not share a common
projection system.

### Case C: Exactly two individual vectors

``` python
A.shape == (256,)
B.shape == (256,)
X = np.vstack([A, B])  # (2, 256)
```

After centering two observations, the matrix has rank at most one.
Consequently, there is at most one nonzero principal component. A
two-component PCA fit may return two coordinate columns, but the second
has no meaningful variance.

For only two vectors, use a similarity measure such as cosine
similarity, Euclidean distance, or the metric appropriate to the
embedding model. To learn a useful 2D PCA projection, collect a
representative set of many embeddings and fit PCA on that reference set.

------------------------------------------------------------------------

## 3. Intuition: variance and principal directions

Imagine points in a 2D plane forming a long, tilted cloud. The original
horizontal and vertical axes may not be the most informative axes for
describing the cloud.

PCA finds:

1.  **PC1:** the direction along which the projected observations have
    the greatest variance.
2.  **PC2:** a direction perpendicular to PC1 that captures the greatest
    remaining variance.
3.  Further components: mutually orthogonal directions, each capturing
    the greatest remaining variance under the orthogonality constraint.

In a 256-dimensional feature space, PCA finds up to 256 principal
directions, although the number of nonzero directions is limited by the
number of observations.

A principal component is a direction, not a particular sample and not
necessarily one original feature.

### Variance is not the same as usefulness

PCA ranks directions by variance. It does not know which variation
matters for a classification task, semantic distinction, or business
objective.

A low-variance direction may contain valuable class information.
Conversely, a high-variance direction may reflect nuisance variation,
such as lighting, speaker identity, document length, or acquisition
conditions.

This distinction is central when interpreting PCA plots.

------------------------------------------------------------------------

## 4. PCA mathematics

Let:

\[ X `\in `{=tex}`\mathbb{R}`{=tex}\^{N `\times `{=tex}D} \]

where `N` is the number of observations and `D = 256` is the feature
count.

### 4.1 Center each feature

Calculate the mean of each column:

\[ `\mu`{=tex}*j = `\frac{1}{N}`{=tex}`\sum`{=tex}*{i=1}\^{N} X\_{ij} \]

Subtract the feature mean:

\[ X_c = X - `\mathbf{1}`{=tex}`\mu`{=tex}\^T \]

The centered matrix has approximately zero mean in each feature.

PCA centers the data because it studies variation around the mean.
Scikit-learn's `PCA` centers features but does not automatically scale
each feature to unit variance.

### 4.2 Covariance matrix

The sample covariance matrix is:

\[ C = `\frac{1}{N-1}`{=tex}X_c\^T X_c \]

For 256 features:

\[ C `\in `{=tex}`\mathbb{R}`{=tex}\^{256 `\times 256`{=tex}} \]

The diagonal contains feature variances. Off-diagonal entries contain
pairwise covariances.

Positive covariance means two features tend to increase together;
negative covariance means one tends to increase as the other decreases.

### 4.3 Eigenvectors and eigenvalues

Solve:

\[ Cv_i = `\lambda`{=tex}\_i v_i \]

-   (v_i) is an eigenvector: a direction in feature space.
-   (`\lambda`{=tex}\_i) is its eigenvalue: the variance along that
    direction, under the sample covariance convention.

Sort eigenvalues from largest to smallest:

\[ `\lambda`{=tex}\_1 `\geq `{=tex}`\lambda`{=tex}\_2
`\geq `{=tex}`\cdots `{=tex}`\geq 0`{=tex} \]

The corresponding eigenvectors are the principal directions.

### 4.4 Select the first k directions

Construct:

\[ W_k = \[v_1, v_2, `\ldots`{=tex}, v_k\] \]

where:

\[ W_k `\in `{=tex}`\mathbb{R}`{=tex}\^{256 `\times `{=tex}k} \]

### 4.5 Project the observations

\[ Z = X_c W_k \]

The output has shape:

\[ Z `\in `{=tex}`\mathbb{R}`{=tex}\^{N `\times `{=tex}k} \]

For `k = 2`, each original vector becomes a 2D coordinate. For `k = 3`,
it becomes a 3D coordinate.

### 4.6 SVD formulation

Practical PCA implementations commonly use Singular Value Decomposition
(SVD):

\[ X_c = U`\Sigma `{=tex}V\^T \]

The rows of (V\^T) contain the principal directions. The reduced
coordinates are:

\[ Z = X_c V_k = U_k`\Sigma`{=tex}\_k \]

The covariance eigenvalues relate to singular values by:

\[ `\lambda`{=tex}\_i = `\frac{\sigma_i^2}{N-1}`{=tex} \]

SVD is often numerically preferable to explicitly forming a covariance
matrix, especially when the data is large or ill-conditioned.

------------------------------------------------------------------------

## 5. PCA algorithm, step by step

A conceptual implementation is:

1.  Arrange samples as rows and features as columns.
2.  Calculate the mean of every feature.
3.  Subtract the mean from each observation.
4.  Compute an eigendecomposition of the covariance matrix, or perform
    SVD on the centered data.
5.  Sort components by descending eigenvalue (or singular value).
6.  Keep the first `k` components.
7.  Project centered observations onto those directions.
8.  Inspect explained variance and validate whether the projection is
    useful for the task.

In production, use a tested numerical library rather than manually
implementing eigendecomposition unless you have a specific reason.

------------------------------------------------------------------------

## 6. Explained variance and choosing dimensions

The explained variance ratio for component `i` is:

\[ r_i = `\frac{\lambda_i}{\sum_j \lambda_j}`{=tex} \]

The cumulative explained variance through component `k` is:

\[ R_k = `\sum`{=tex}\_{i=1}\^{k} r_i \]

Example (illustrative numbers):

  Component     Explained variance   Cumulative
  ----------- -------------------- ------------
  PC1                          35%          35%
  PC2                          20%          55%
  PC3                          12%          67%
  PC4                           8%          75%
  PC5                           5%          80%

A 2D plot in this example retains 55% of the total variance. A
five-dimensional representation retains 80%.

There is no universal rule that 95% is required. The right number
depends on the goal:

-   **Visualization:** 2D or 3D is chosen for human inspection, even if
    variance retained is modest.
-   **Compression:** choose `k` based on a storage, speed, or
    reconstruction-error budget.
-   **Downstream learning:** select `k` using validation performance,
    not variance alone.
-   **Denoising:** choose a cutoff based on noise assumptions and
    validation.

### Scree plot and cumulative variance

``` python
import numpy as np
import matplotlib.pyplot as plt
from sklearn.decomposition import PCA

pca_full = PCA(svd_solver="full")
pca_full.fit(X)

ratios = pca_full.explained_variance_ratio_
cumulative = np.cumsum(ratios)

plt.figure(figsize=(8, 4))
plt.plot(np.arange(1, len(ratios) + 1), ratios, marker=".")
plt.xlabel("Principal component")
plt.ylabel("Explained variance ratio")
plt.title("PCA scree plot")
plt.grid(alpha=0.3)
plt.show()

plt.figure(figsize=(8, 4))
plt.plot(np.arange(1, len(cumulative) + 1), cumulative)
plt.axhline(0.95, linestyle="--", label="95% reference")
plt.xlabel("Number of components")
plt.ylabel("Cumulative explained variance")
plt.title("Cumulative explained variance")
plt.legend()
plt.grid(alpha=0.3)
plt.show()
```

------------------------------------------------------------------------

## 7. Practical PCA with two groups of embeddings

This is the main workflow for comparing two collections of
256-dimensional vectors.

Suppose:

``` python
A.shape == (100, 256)
B.shape == (120, 256)
```

The correct workflow is:

1.  Combine A and B.
2.  Create labels for coloring only.
3.  Fit one PCA model on the combined data (or on a designated
    reference/training set).
4.  Transform the combined data.
5.  Plot the projected points using their labels.

### 7.1 Complete 2D example

``` python
import numpy as np
import matplotlib.pyplot as plt
from sklearn.decomposition import PCA

# Replace these sample arrays with your real embeddings.
rng = np.random.default_rng(42)
A = rng.normal(size=(100, 256))
B = rng.normal(loc=0.5, size=(120, 256))

assert A.ndim == 2 and B.ndim == 2
assert A.shape[1] == B.shape[1] == 256

X = np.vstack([A, B])
labels = np.array(["Group A"] * len(A) + ["Group B"] * len(B))

pca = PCA(n_components=2, svd_solver="full")
Z = pca.fit_transform(X)

fig, ax = plt.subplots(figsize=(9, 7))

for group in np.unique(labels):
    mask = labels == group
    ax.scatter(
        Z[mask, 0],
        Z[mask, 1],
        label=group,
        alpha=0.7,
        s=35
    )

ax.set_xlabel("PC1")
ax.set_ylabel("PC2")
ax.set_title("PCA projection of 256-D embeddings")
ax.legend()
ax.grid(alpha=0.25)
plt.tight_layout()
plt.show()

print("Input shape:", X.shape)
print("Output shape:", Z.shape)
print("Explained variance per component:",
      pca.explained_variance_ratio_)
print("Total variance shown:",
      pca.explained_variance_ratio_.sum())
```

The synthetic data above is only for demonstrating the code. Its
clusters are not evidence about your real dataset.

### 7.2 3D visualization

``` python
import matplotlib.pyplot as plt
from sklearn.decomposition import PCA

pca3 = PCA(n_components=3)
Z3 = pca3.fit_transform(X)

fig = plt.figure(figsize=(10, 8))
ax = fig.add_subplot(111, projection="3d")

for group in np.unique(labels):
    mask = labels == group
    ax.scatter(
        Z3[mask, 0],
        Z3[mask, 1],
        Z3[mask, 2],
        label=group,
        alpha=0.7
    )

ax.set_xlabel("PC1")
ax.set_ylabel("PC2")
ax.set_zlabel("PC3")
ax.set_title("3D PCA projection")
ax.legend()
plt.show()

print("Variance captured by 3D:",
      pca3.explained_variance_ratio_.sum())
```

### 7.3 Reusable function

``` python
import numpy as np
import matplotlib.pyplot as plt
from sklearn.decomposition import PCA

def plot_two_embedding_groups(A, B, n_components=2):
    """
    A: array of shape (n_A, 256)
    B: array of shape (n_B, 256)
    n_components: 2 or 3
    """
    A = np.asarray(A)
    B = np.asarray(B)

    if A.ndim != 2 or B.ndim != 2:
        raise ValueError("A and B must be 2D arrays.")

    if A.shape[1] != 256 or B.shape[1] != 256:
        raise ValueError("Every input vector must have 256 features.")

    if n_components not in (2, 3):
        raise ValueError("n_components must be 2 or 3.")

    X = np.vstack([A, B])
    labels = np.array([0] * len(A) + [1] * len(B))

    if X.shape[0] <= n_components:
        raise ValueError("Need more observations than components.")

    pca = PCA(n_components=n_components)
    Z = pca.fit_transform(X)

    if n_components == 2:
        fig, ax = plt.subplots(figsize=(9, 7))
        for label, name in [(0, "Group A"), (1, "Group B")]:
            mask = labels == label
            ax.scatter(Z[mask, 0], Z[mask, 1],
                       label=name, alpha=0.7)
        ax.set_xlabel("PC1")
        ax.set_ylabel("PC2")
        ax.legend()
        ax.grid(alpha=0.25)
    else:
        fig = plt.figure(figsize=(9, 7))
        ax = fig.add_subplot(111, projection="3d")
        for label, name in [(0, "Group A"), (1, "Group B")]:
            mask = labels == label
            ax.scatter(Z[mask, 0], Z[mask, 1], Z[mask, 2],
                       label=name, alpha=0.7)
        ax.set_xlabel("PC1")
        ax.set_ylabel("PC2")
        ax.set_zlabel("PC3")
        ax.legend()

    ax.set_title(f"{n_components}D PCA of 256-D vectors")
    plt.tight_layout()
    plt.show()

    return Z, pca
```

### 7.4 Fit once, transform future embeddings

If you need to compare new embeddings with an existing plot, reuse the
fitted PCA model:

``` python
pca = PCA(n_components=2)
Z_reference = pca.fit_transform(X_reference)

# Later, new vectors must have the same 256 feature definitions/order.
Z_new = pca.transform(X_new)
```

Do not refit PCA separately for each batch if you need coordinates that
are comparable over time.

For an unbiased predictive evaluation, fit preprocessing and PCA on
training data only, then transform validation/test data with the fitted
objects. Fitting PCA on all data before splitting can leak information
about the test distribution.

------------------------------------------------------------------------

## 8. Two individual 256-D vectors are a special case

If you have exactly two observations:

``` python
A.shape == (256,)
B.shape == (256,)

X = np.vstack([A, B])  # (2, 256)
```

Centering two observations produces two opposite rows. The centered
matrix has rank at most one.

Thus, only one direction can have nonzero variance. A second PCA
coordinate is degenerate (zero variance, apart from numerical effects).

For two individual vectors, use direct comparison instead:

``` python
import numpy as np
from sklearn.metrics.pairwise import cosine_similarity

A = np.asarray(A).reshape(1, -1)
B = np.asarray(B).reshape(1, -1)

cos_sim = cosine_similarity(A, B)[0, 0]
euclidean = np.linalg.norm(A - B)

print("Cosine similarity:", cos_sim)
print("Euclidean distance:", euclidean)
```

Interpret similarity according to the embedding model and its intended
metric. Cosine similarity is not universally the correct metric for
every representation.

To visualize two points in a learned 2D space, fit PCA on a larger
representative reference collection, then transform both vectors using
that fitted model.

------------------------------------------------------------------------

## 9. Preprocessing: centering, scaling, and normalization

Preprocessing changes what PCA considers important. Choose it based on
the meaning of your features.

### 9.1 Centering

PCA centers each feature by subtracting its mean. Scikit-learn's `PCA`
does this automatically.

### 9.2 Standardization

`StandardScaler` standardizes each feature across observations:

\[ x'\_{ij} = `\frac{x_{ij} - \mu_j}{s_j}`{=tex} \]

``` python
from sklearn.preprocessing import StandardScaler
from sklearn.decomposition import PCA

scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

pca = PCA(n_components=2)
Z = pca.fit_transform(X_scaled)
```

Use this when feature units or scales differ and each feature should be
placed on a comparable scale.

**Caution:** Standardization can amplify low-variance or noisy features.
It is not automatically better for embeddings.

### 9.3 L2-normalizing each vector

L2 normalization makes each row have unit Euclidean norm:

\[ x' = `\frac{x}{\|x\|_2}`{=tex} \]

``` python
from sklearn.preprocessing import normalize

X_l2 = normalize(X, norm="l2")
Z = PCA(n_components=2).fit_transform(X_l2)
```

This can be appropriate when the embedding model is designed for
cosine-based comparison. It changes the geometry and the variance PCA
sees, so compare it with the unnormalized result.

### 9.4 What should embedding users try?

For learned 256-D embeddings, compare at least:

1.  Raw embeddings (PCA still centers features).
2.  L2-normalized embeddings followed by PCA, if cosine geometry is
    relevant.
3.  Standardized embeddings only when feature-wise scaling has a
    defensible rationale.

Do not blindly apply both standardization and L2 normalization. They
solve different problems and can change the representation
substantially.

### 9.5 Outliers

Extreme observations can dominate variance and rotate the principal
directions. Inspect outliers, verify data quality, and consider robust
preprocessing or robust PCA methods when justified. Do not remove
unusual observations merely because they make a plot look less
attractive.

------------------------------------------------------------------------

## 10. PCA variants and computational considerations

### 10.1 Full PCA

Computes an exact decomposition up to floating-point precision.

``` python
PCA(n_components=2, svd_solver="full")
```

Good for moderate datasets and reproducible reference calculations.

### 10.2 Randomized PCA

Uses randomized linear algebra to approximate leading components
efficiently for suitable large matrices.

``` python
PCA(
    n_components=50,
    svd_solver="randomized",
    random_state=42
)
```

Useful when only a relatively small number of leading components is
required.

### 10.3 Incremental PCA

Processes batches instead of requiring the entire dataset in memory.

``` python
from sklearn.decomposition import IncrementalPCA

ipca = IncrementalPCA(n_components=50, batch_size=512)

for batch in batches:
    ipca.partial_fit(batch)

Z_batches = [ipca.transform(batch) for batch in batches]
```

All batches must have the same feature representation and ordering.
Incremental fitting is useful for memory-constrained workflows, but
results can differ slightly from full PCA.

### 10.4 Kernel PCA

Kernel PCA can capture nonlinear relationships by applying a
kernel-defined feature mapping.

``` python
from sklearn.decomposition import KernelPCA

kpca = KernelPCA(
    n_components=2,
    kernel="rbf",
    gamma=0.01
)

Z = kpca.fit_transform(X)
```

Kernel choice and parameters matter. Kernel PCA can be expensive in
memory and computation because it uses pairwise relationships between
observations. In standard kernel PCA, out-of-sample transformation is
available through the fitted model when configured appropriately.

### 10.5 Sparse PCA

Sparse PCA seeks components with many zero loadings, which can improve
interpretability when only a subset of original features should
contribute.

``` python
from sklearn.decomposition import SparsePCA

spca = SparsePCA(n_components=2, random_state=42)
Z = spca.fit_transform(X)
```

Sparse PCA is not simply a faster PCA and does not have all the same
orthogonality and variance-optimality properties.

### 10.6 Truncated SVD

Truncated SVD extracts leading singular directions without explicitly
centering the input. It is widely used for sparse matrices such as
bag-of-words and TF-IDF.

``` python
from sklearn.decomposition import TruncatedSVD

svd = TruncatedSVD(n_components=2, random_state=42)
Z = svd.fit_transform(X_sparse)
```

For dense, centered numerical embeddings, ordinary PCA is often the
natural starting point. Truncated SVD on uncentered data is not exactly
the same operation as centered PCA.

------------------------------------------------------------------------

## 11. Limitations of PCA

Understanding these limitations prevents misleading conclusions.

### Limitation 1: PCA is linear

PCA projects data using linear combinations. If the observations lie on
a curved or nonlinear manifold, a 2D PCA plot may fail to reveal that
structure.

**Possible alternatives:** Kernel PCA, UMAP, t-SNE, or autoencoders.

### Limitation 2: PCA maximizes variance, not class separation

PCA does not use group labels. It can preserve high-variance nuisance
factors while discarding a low-variance direction that distinguishes
classes.

**Possible alternatives:** LDA, supervised metric learning, supervised
embeddings, or a classifier evaluated in the original feature space.

### Limitation 3: 2D projections lose information

Reducing 256 dimensions to two necessarily discards dimensions unless
the data has very low intrinsic rank.

A visible overlap in 2D does not prove that two groups overlap in 256D.
A visible separation does not prove that a classifier will generalize.

**Possible response:** inspect 3D, retain more dimensions for downstream
modeling, and calculate high-dimensional metrics.

### Limitation 4: Outliers can dominate

A few extreme points can influence the directions with the largest
variance.

**Possible response:** inspect data quality, use robust methods where
appropriate, and report any filtering decisions.

### Limitation 5: Scale affects the result

Features with large numerical scales can dominate covariance-based PCA.

**Possible response:** choose scaling based on feature semantics, not by
habit.

### Limitation 6: PCA may not preserve nearest neighbors

PCA optimizes total variance, not local neighborhoods or every pairwise
distance.

**Possible response:** compare neighborhood preservation,
trustworthiness, and distance behavior; consider UMAP or t-SNE for local
structure.

### Limitation 7: A plot can depend on the sample

Different sampled observations can yield different principal directions,
particularly when eigenvalues are close.

**Possible response:** use a fixed reference set, fixed random seeds for
approximate methods, and assess stability across samples.

------------------------------------------------------------------------

## 12. Alternatives to PCA

No dimensionality-reduction method bypasses every limitation. Each
method preserves a different aspect of the data and introduces its own
assumptions and failure modes.

### 12.1 t-SNE

**t-distributed Stochastic Neighbor Embedding** is a nonlinear technique
designed mainly to preserve local neighborhood relationships.

``` python
from sklearn.manifold import TSNE

tsne = TSNE(
    n_components=2,
    perplexity=30,
    init="pca",
    learning_rate="auto",
    max_iter=1000,
    random_state=42
)

Z_tsne = tsne.fit_transform(X)
```

Useful for: - Exploring local clusters. - Visualizing complex nonlinear
data. - Inspecting whether local neighborhoods contain similar
observations.

Limitations: - Results depend on perplexity, initialization, and other
settings. - Global distances between clusters are generally not
meaningful. - Cluster sizes and gaps can be visually misleading. -
Standard t-SNE is primarily a visualization method and does not provide
the same straightforward out-of-sample transform workflow as PCA. - Can
be computationally expensive for large datasets.

Use t-SNE to explore local structure, not to claim that distances
between far-apart clusters represent true global distances.

### 12.2 UMAP

**Uniform Manifold Approximation and Projection** is a nonlinear
manifold-learning method commonly used for embedding visualization.

Install:

``` bash
pip install umap-learn
```

Example:

``` python
import umap

reducer = umap.UMAP(
    n_components=2,
    n_neighbors=15,
    min_dist=0.1,
    metric="cosine",
    random_state=42
)

Z_umap = reducer.fit_transform(X)
```

Useful for: - Exploring local neighborhoods and some broader
structure. - Visualizing embedding collections. - Working with cosine
distance when appropriate for the representation. - Transforming new
observations with a fitted UMAP model.

Important parameters:

  -----------------------------------------------------------------------
  Parameter                           Effect
  ----------------------------------- -----------------------------------
  `n_neighbors`                       Controls the local-versus-broader
                                      neighborhood scale

  `min_dist`                          Controls how tightly points can
                                      pack in the low-dimensional
                                      embedding

  `n_components`                      Output dimensions

  `metric`                            Distance/similarity geometry in the
                                      original space

  `random_state`                      Reproducibility
  -----------------------------------------------------------------------

Limitations: - Results depend on parameters and sampling. - Apparent
cluster separation can be exaggerated. - Global distances and density
are not automatically preserved. - Different runs or settings can
produce different layouts.

For 256-D embeddings, cosine distance is worth testing when it matches
the embedding model's intended similarity measure. Do not assume it is
always best.

### 12.3 Linear Discriminant Analysis (LDA)

LDA is a supervised dimensionality-reduction method. Unlike PCA, it uses
class labels and seeks directions that separate classes relative to
within-class variation.

For `C` classes, the number of discriminant components is at most:

\[ `\min`{=tex}(D, C-1) \]

For two groups, LDA can produce at most one discriminant dimension. It
is therefore not a direct two-dimensional alternative for separating
exactly two labeled groups, although it can provide a useful 1D
separation score.

``` python
from sklearn.discriminant_analysis import LinearDiscriminantAnalysis

lda = LinearDiscriminantAnalysis(n_components=1)
Z_lda = lda.fit_transform(X, labels)
```

Use LDA when labels are available and the goal is supervised class
separation. Fit it only within the training data in predictive
evaluation. LDA's assumptions and regularization matter, especially when
features are numerous relative to observations.

### 12.4 Kernel PCA

Kernel PCA captures nonlinear structure through kernels such as RBF or
polynomial kernels.

Useful when: - Relationships are nonlinear. - You want a kernel-based
extension of PCA.

Limitations: - Kernel and hyperparameter selection can be difficult. -
Pairwise kernel matrices can require substantial memory. - It is not
guaranteed to improve visualization.

### 12.5 Autoencoders

An autoencoder is a neural network trained to reconstruct its input
through a lower-dimensional latent representation.

Architecture:

``` text
256-D input
    |
    v
Encoder neural network
    |
    v
2-D or 3-D latent vector
    |
    v
Decoder neural network
    |
    v
Reconstructed 256-D vector
```

A simple PyTorch example:

``` python
import torch
import torch.nn as nn

class Autoencoder(nn.Module):
    def __init__(self, input_dim=256, latent_dim=2):
        super().__init__()

        self.encoder = nn.Sequential(
            nn.Linear(input_dim, 128),
            nn.ReLU(),
            nn.Linear(128, 64),
            nn.ReLU(),
            nn.Linear(64, latent_dim)
        )

        self.decoder = nn.Sequential(
            nn.Linear(latent_dim, 64),
            nn.ReLU(),
            nn.Linear(64, 128),
            nn.ReLU(),
            nn.Linear(128, input_dim)
        )

    def forward(self, x):
        z = self.encoder(x)
        reconstruction = self.decoder(z)
        return reconstruction, z
```

Train using a reconstruction loss such as mean squared error when
appropriate:

``` python
model = Autoencoder(input_dim=256, latent_dim=2)
optimizer = torch.optim.Adam(model.parameters(), lr=1e-3)
criterion = nn.MSELoss()

# X_tensor has shape (number_of_samples, 256)
for epoch in range(100):
    model.train()
    optimizer.zero_grad()

    reconstructed, z = model(X_tensor)
    loss = criterion(reconstructed, X_tensor)

    loss.backward()
    optimizer.step()
```

Obtain the latent coordinates:

``` python
model.eval()
with torch.no_grad():
    _, Z_autoencoder = model(X_tensor)
```

Limitations: - Requires sufficient representative data. - Training can
be unstable or sensitive to architecture and optimization. -
Reconstruction quality does not guarantee class separation. - A 2D
bottleneck can be too restrictive. - A basic autoencoder does not
automatically preserve cosine similarity or local neighborhoods.

Variational autoencoders, denoising autoencoders, and contrastive
objectives are different approaches with different goals.

### 12.6 Other methods worth knowing

  -------------------------------------------------------------------------
  Method            Main idea         Useful situation  Important caveat
  ----------------- ----------------- ----------------- -------------------
  Isomap            Preserve          Manifold          Sensitive to
                    graph-based       exploration       neighborhood graph
                    geodesic                            
                    distances                           

  MDS               Preserve pairwise Distance-based    Pairwise distance
                    distances as much visualization     computation can be
                    as possible under                   expensive
                    an objective                        

  Random projection Project using a   Fast approximate  Not designed to
                    random matrix     distance          reveal clusters
                                      preservation      

  Factor Analysis   Model             Latent-variable   Makes
                    observations      interpretation    distributional
                    using latent                        assumptions
                    factors plus                        
                    noise                               

  NMF               Factorize         Nonnegative count Requires
                    nonnegative data  or intensity      nonnegative input
                    into nonnegative  features          
                    factors                             

  Supervised metric Learn a distance  Similarity and    Needs suitable
  learning          space using       retrieval tasks   supervision
                    labels or pairs                     

  Contrastive       Learn             Embedding         Requires carefully
  learning          representations   learning          designed
                    that bring                          positive/negative
                    related samples                     pairs
                    together                            
  -------------------------------------------------------------------------

------------------------------------------------------------------------

## 13. How to choose an alternative

Choose the method based on what you want to preserve.

  -----------------------------------------------------------------------
  Your goal               Good starting point     Compare against
  ----------------------- ----------------------- -----------------------
  Fast, interpretable     PCA                     Random projection
  linear baseline                                 

  Visualize nonlinear     UMAP                    t-SNE
  local clusters                                  

  Explore local           t-SNE                   UMAP
  neighborhoods                                   

  Separate known classes  LDA                     Supervised metric
  using labels                                    learning

  Learn a compact         Autoencoder             PCA
  nonlinear                                       
  representation                                  

  Preserve pairwise       MDS                     PCA, random projection
  distances                                       

  Work with sparse TF-IDF Truncated SVD           Sparse-compatible
  features                                        methods

  Compare embeddings by   Original-space cosine   PCA/UMAP plots as
  semantic similarity     similarity              visual supplements

  Reduce huge datasets in Incremental PCA         Randomized PCA
  batches                                         
  -----------------------------------------------------------------------

### A practical recommendation for 256-D embeddings

Start with three complementary analyses:

1.  **PCA:** a linear, interpretable baseline.
2.  **UMAP with a justified metric:** explore nonlinear neighborhood
    structure.
3.  **Original-space similarity and classification metrics:** check
    whether the apparent visual pattern is supported by the original
    256-D data.

Optionally add t-SNE as another exploratory visualization.

Do not select the method merely because its plot has the most visually
separated clusters. A visually attractive projection is not necessarily
the most faithful or predictive representation.

------------------------------------------------------------------------

## 14. Comparing two groups correctly

Suppose you have two collections:

``` python
A.shape == (n_A, 256)
B.shape == (n_B, 256)
```

### 14.1 Fit one shared projection

For a descriptive visualization of both groups:

``` python
X = np.vstack([A, B])
Z = PCA(n_components=2).fit_transform(X)
```

Then use labels to color the points.

### 14.2 Use a fixed reference projection for repeated comparisons

If Group A is a reference dataset and Group B arrives later:

``` python
pca = PCA(n_components=2)
Z_A = pca.fit_transform(A)
Z_B = pca.transform(B)
```

This answers how B maps into the coordinate system learned from A. If B
is expected to influence the reference geometry, define a representative
reference set and fit on that set instead.

### 14.3 Compare in the original 256-D space too

A 2D visualization is not a substitute for direct measurements.

Useful measures include:

**Cosine similarity**

``` python
from sklearn.metrics.pairwise import cosine_similarity

similarity_matrix = cosine_similarity(A, B)
print(similarity_matrix.shape)  # (n_A, n_B)
```

**Euclidean distances**

``` python
from sklearn.metrics import pairwise_distances

distance_matrix = pairwise_distances(A, B, metric="euclidean")
```

**Nearest-neighbor composition**

For each point, inspect the labels of its nearest neighbors in the
original space. This can reveal whether local neighborhoods mix the
groups.

**Classification evaluation**

If the goal is to determine whether groups are distinguishable, train an
appropriate classifier and evaluate on held-out data. Use stratified
splits where suitable, and avoid having near-duplicate or related
samples leak across train and test sets.

### 14.4 Visual overlap does not prove high-dimensional overlap

Two groups may overlap in a PCA plot because their separating
information lies in later components. Conversely, a projection can
visually exaggerate separation.

Always report the visualization together with original-space metrics and
the method's settings.

------------------------------------------------------------------------

## 15. Evaluation: do not trust a plot alone

Evaluate whether a projection preserves the property you care about.

### 15.1 Explained variance

For PCA, report the variance retained by the selected components.

``` python
print(pca.explained_variance_ratio_)
print(pca.explained_variance_ratio_.sum())
```

This measures variance retention, not class separation or semantic
fidelity.

### 15.2 Reconstruction error

For PCA, reconstruct an approximation:

``` python
Z = pca.transform(X)
X_reconstructed = pca.inverse_transform(Z)

mse = np.mean((X - X_reconstructed) ** 2)
print("Reconstruction MSE:", mse)
```

Lower reconstruction error generally means more information from the
original feature values is retained, but it does not guarantee
preservation of the relationships important to your task.

### 15.3 Trustworthiness

Trustworthiness measures whether nearby points in the low-dimensional
space were also nearby in the original space, penalizing false
neighbors.

``` python
from sklearn.manifold import trustworthiness

score = trustworthiness(
    X,
    Z,
    n_neighbors=10,
    metric="euclidean"
)

print("Trustworthiness:", score)
```

Choose the original-space metric based on the embedding semantics. For
cosine-oriented embeddings, evaluate a compatible distance metric where
supported.

### 15.4 Neighborhood preservation

Compare the original-space nearest neighbors with the projected-space
nearest neighbors. This is particularly useful when local similarity
matters more than total variance.

### 15.5 Stability

Repeat stochastic methods with different seeds or representative
samples. Check whether broad patterns persist. Do not interpret a single
layout as definitive.

### 15.6 Downstream performance

If the reduced vectors will feed a classifier or another model, compare
validation performance across:

-   Original 256-D vectors.
-   PCA with several component counts.
-   UMAP or autoencoder representations where a valid train/test
    transform workflow exists.

Use the same data splits and comparable evaluation protocols.

------------------------------------------------------------------------

## 16. Common mistakes and how to avoid them

1.  **Fitting PCA separately to each group:** use one shared fitted
    projection when comparing coordinates.
2.  **Treating PC1 and PC2 as original features:** they are learned
    combinations of the original features.
3.  **Assuming 2D retains all information:** it almost always loses
    information.
4.  **Assuming maximum variance means maximum class separation:** PCA is
    unsupervised.
5.  **Standardizing embeddings automatically:** inspect feature scales
    and embedding-model guidance first.
6.  **Comparing plots from separately fitted models:** coordinates may
    not be aligned.
7.  **Refitting on each new batch:** use the fitted model's
    `transform()` when coordinates must remain comparable.
8.  **Fitting preprocessing before a train/test split:** fit on training
    data only in predictive workflows.
9.  **Reading global distances from t-SNE or UMAP:** these layouts
    emphasize neighborhood structure and can distort global geometry.
10. **Using a plot as proof of model quality:** validate with
    original-space metrics and held-out evaluation.
11. **Using only two observations to learn a 2D PCA basis:** use a
    representative collection.
12. **Ignoring sample imbalance:** the larger group can contribute more
    to the covariance estimate. If equal group influence is required,
    design a justified sampling or weighting strategy and document it.

------------------------------------------------------------------------

## 17. Reusable end-to-end Python pipeline

This script accepts two groups of 256-D vectors, optionally
L2-normalizes them, fits PCA jointly, plots the result, and reports
useful diagnostics.

Install dependencies:

``` bash
pip install numpy matplotlib scikit-learn
```

Save as `pca_visualization.py`:

``` python
import numpy as np
import matplotlib.pyplot as plt

from sklearn.decomposition import PCA
from sklearn.preprocessing import normalize
from sklearn.metrics import pairwise_distances
from sklearn.manifold import trustworthiness


def visualize_groups(
    A,
    B,
    n_components=2,
    l2_normalize=False,
    random_state=42
):
    """
    Visualize two groups of 256-dimensional vectors.

    Parameters
    ----------
    A : array-like, shape (n_A, 256)
        First group.
    B : array-like, shape (n_B, 256)
        Second group.
    n_components : int
        2 or 3.
    l2_normalize : bool
        Whether to normalize each observation to unit L2 norm.
    random_state : int
        Random seed for PCA solvers that use randomness.

    Returns
    -------
    Z : ndarray, shape (n_A + n_B, n_components)
        Reduced coordinates.
    pca : fitted PCA object
    """
    A = np.asarray(A, dtype=float)
    B = np.asarray(B, dtype=float)

    if A.ndim != 2 or B.ndim != 2:
        raise ValueError("A and B must be 2D arrays.")

    if A.shape[1] != 256 or B.shape[1] != 256:
        raise ValueError("Every vector must have exactly 256 features.")

    if len(A) == 0 or len(B) == 0:
        raise ValueError("Both groups must contain observations.")

    if not np.isfinite(A).all() or not np.isfinite(B).all():
        raise ValueError("Input contains NaN or infinite values.")

    if n_components not in (2, 3):
        raise ValueError("n_components must be 2 or 3.")

    X = np.vstack([A, B])
    labels = np.array([0] * len(A) + [1] * len(B))

    if l2_normalize:
        X = normalize(X, norm="l2")

    pca = PCA(
        n_components=n_components,
        svd_solver="full"
    )

    Z = pca.fit_transform(X)

    print("Original shape:", X.shape)
    print("Reduced shape:", Z.shape)
    print("Explained variance ratio:",
          pca.explained_variance_ratio_)
    print("Total explained variance:",
          pca.explained_variance_ratio_.sum())

    if n_components == 2:
        fig, ax = plt.subplots(figsize=(9, 7))

        for label, name in [(0, "Group A"), (1, "Group B")]:
            mask = labels == label
            ax.scatter(
                Z[mask, 0],
                Z[mask, 1],
                label=name,
                alpha=0.7,
                s=30
            )

        ax.set_xlabel("PC1")
        ax.set_ylabel("PC2")

    else:
        fig = plt.figure(figsize=(10, 8))
        ax = fig.add_subplot(111, projection="3d")

        for label, name in [(0, "Group A"), (1, "Group B")]:
            mask = labels == label
            ax.scatter(
                Z[mask, 0],
                Z[mask, 1],
                Z[mask, 2],
                label=name,
                alpha=0.7
            )

        ax.set_xlabel("PC1")
        ax.set_ylabel("PC2")
        ax.set_zlabel("PC3")

    ax.set_title("PCA visualization of 256-D embeddings")
    ax.legend()
    plt.tight_layout()
    plt.show()

    if len(X) > 2:
        score = trustworthiness(
            X,
            Z,
            n_neighbors=min(10, (len(X) - 1) // 2)
        )
        print("Trustworthiness:", score)

    return Z, pca


if __name__ == "__main__":
    rng = np.random.default_rng(42)

    # Demonstration data only. Replace with actual embeddings.
    A = rng.normal(size=(100, 256))
    B = rng.normal(loc=0.5, size=(100, 256))

    Z, pca_model = visualize_groups(
        A,
        B,
        n_components=2,
        l2_normalize=False
    )
```

### Expected input formats

NumPy arrays:

``` python
A = np.load("group_a.npy")
B = np.load("group_b.npy")
```

Or from a PyTorch model:

``` python
embeddings = model.encode(texts)

# If embeddings is a PyTorch tensor:
embeddings = embeddings.detach().cpu().numpy()
```

Make sure all vectors use the same model, feature ordering, and
embedding-generation configuration.

------------------------------------------------------------------------

## 18. Reproducibility and deployment

For reliable experiments, record:

-   Embedding model name and version.
-   Number of observations in each group.
-   Feature dimension (256).
-   Preprocessing steps.
-   Distance metric used for comparisons.
-   Reduction algorithm and parameters.
-   Random seed, where applicable.
-   Explained variance for PCA.
-   Software/library versions.
-   Whether the projection was fitted on training, reference, or all
    exploratory observations.

For deployment, save the fitted preprocessing and projection pipeline:

``` python
import joblib

joblib.dump(pca, "pca_model.joblib")

# Later:
pca = joblib.load("pca_model.joblib")
new_coordinates = pca.transform(new_embeddings)
```

If scaling or normalization was fitted as a separate transformation,
save the complete pipeline rather than only PCA. For example:

``` python
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.decomposition import PCA

pipeline = make_pipeline(
    StandardScaler(),
    PCA(n_components=2)
)

Z = pipeline.fit_transform(X)
joblib.dump(pipeline, "embedding_projection.joblib")
```

Use this standardized pipeline only if standardization is appropriate
for your feature representation.

------------------------------------------------------------------------

## 19. Decision checklist

Before choosing a method, answer these questions:

-   [ ] Do I have many observations, each with 256 features?
-   [ ] Am I fitting one shared projection for groups I want to compare?
-   [ ] Is my goal visualization, compression, classification, or
    similarity search?
-   [ ] Does the embedding model recommend cosine similarity or another
    metric?
-   [ ] Have I compared raw and appropriately normalized embeddings?
-   [ ] Have I checked explained variance for PCA?
-   [ ] Have I checked neighborhood preservation or original-space
    similarity?
-   [ ] Have I avoided interpreting UMAP/t-SNE global distances
    literally?
-   [ ] Have I used training-only fitting for predictive evaluation?
-   [ ] Have I recorded the parameters and preprocessing for
    reproducibility?

### Suggested baseline experiment

For two groups of 256-D embeddings, run:

  -----------------------------------------------------------------------
  Experiment        Input             Method            Purpose
  ----------------- ----------------- ----------------- -----------------
  1                 Raw 256-D         PCA, 2D           Linear baseline

  2                 Raw 256-D         PCA, 3D           Inspect another
                                                        direction

  3                 L2-normalized     PCA, 2D           Test
                    256-D                               cosine-oriented
                                                        geometry

  4                 Appropriate       UMAP, 2D          Explore nonlinear
                    metric                              neighborhoods

  5                 Appropriate       t-SNE, 2D         Explore local
                    metric                              clusters

  6                 Original 256-D    Cosine similarity Validate actual
                                      / nearest         embedding
                                      neighbors         relationships

  7                 Original 256-D    Held-out          Test group
                                      classifier        separability
                                      evaluation        
  -----------------------------------------------------------------------

Do not declare a winner solely from visual appearance. Decide based on
the property you need to preserve.

------------------------------------------------------------------------

## 20. Glossary

  -----------------------------------------------------------------------
  Term                                Meaning
  ----------------------------------- -----------------------------------
  Observation/sample                  One vector or data point

  Feature                             One coordinate of an original
                                      vector

  Feature space                       The original 256-dimensional
                                      coordinate system

  Centering                           Subtracting each feature's mean

  Covariance                          How two features vary together

  Variance                            Spread of values along a direction

  Eigenvector                         A direction associated with a
                                      matrix transformation

  Eigenvalue                          The scale/variance associated with
                                      an eigenvector in PCA

  Principal component                 An ordered principal direction and
                                      its projected coordinate

  Projection                          Mapping an observation onto
                                      selected directions

  Explained variance ratio            Fraction of total variance captured
                                      by a component

  Latent representation               A lower-dimensional representation
                                      learned or computed from data

  Manifold                            A potentially lower-dimensional
                                      structure embedded in a
                                      higher-dimensional space

  Embedding                           A numeric vector representing an
                                      item, often learned by a model

  Neighborhood preservation           How well nearby observations remain
                                      neighbors after reduction

  Out-of-sample transform             Applying a fitted reduction model
                                      to new observations
  -----------------------------------------------------------------------

------------------------------------------------------------------------

## 21. References

1.  [Scikit-learn: PCA
    API](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html)
2.  [Scikit-learn: Decomposition and PCA user
    guide](https://scikit-learn.org/stable/modules/decomposition.html#pca)
3.  [Scikit-learn: t-SNE
    API](https://scikit-learn.org/stable/modules/generated/sklearn.manifold.TSNE.html)
4.  [UMAP documentation](https://umap-learn.readthedocs.io/en/latest/)
5.  [Scikit-learn: Linear Discriminant
    Analysis](https://scikit-learn.org/stable/modules/lda_qda.html)
6.  [Scikit-learn: Kernel PCA
    API](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.KernelPCA.html)
7.  [Scikit-learn: Incremental PCA
    API](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.IncrementalPCA.html)
8.  [Scikit-learn: Trustworthiness
    API](https://scikit-learn.org/stable/modules/generated/sklearn.manifold.trustworthiness.html)

------------------------------------------------------------------------

## Final recommendation

For two groups of 256-dimensional deep-learning embeddings, begin with a
shared 2D PCA projection, inspect its explained variance, and compare it
with a 2D UMAP visualization using a distance metric justified by your
embedding model. Also compare cosine similarities and nearest neighbors
in the original 256-dimensional space.

PCA, UMAP, and t-SNE are tools for different purposes---not
interchangeable ways to reveal a single objectively correct picture. The
best method is the one that preserves the structure relevant to your
actual task and is validated with appropriate measurements.
