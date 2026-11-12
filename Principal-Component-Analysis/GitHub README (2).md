# Principal Component Analysis (PCA): A Complete Explanation with Practical Examples

Principal Component Analysis (PCA) is one of the most important dimensionality-reduction techniques in machine learning. Since you already have knowledge of machine learning, deep learning, and AI algorithms, this guide explains PCA from both a mathematical and practical perspective.

---

### Key Focus Areas & Use Case

* You have vectors containing **256 features**.
* You have **two groups** of vectors.
* You want to reduce the 256-dimensional vectors into **2D or 3D**.
* You want to visualize the vectors and understand whether the two groups form separate clusters or overlap.
* You want to understand how PCA actually works rather than simply calling `PCA(n_components=2)` from scikit-learn.

Let's build the understanding step by step.

---

## 1. What Exactly is PCA?

**Principal Component Analysis (PCA)** is an unsupervised linear dimensionality-reduction algorithm that transforms high-dimensional data into a smaller number of new dimensions while preserving as much of the data's variance as possible.

Let's understand this statement.

Suppose you have a dataset containing 1,000 samples. Each sample is represented by a 256-dimensional vector.

Your dataset matrix $X$ looks like this:

$$X = \begin{bmatrix} x_{1,1} & x_{1,2} & \cdots & x_{1,256} \\ x_{2,1} & x_{2,2} & \cdots & x_{2,256} \\ \vdots & \vdots & \ddots & \vdots \\ x_{1000,1} & x_{1000,2} & \cdots & x_{1000,256} \end{bmatrix}$$

Therefore:

$$X \in \mathbb{R}^{1000 \times 256}$$

Here:
* $1000$ = number of samples ($N$).
* $256$ = number of features per sample ($D$).

You cannot directly visualize this dataset because humans can easily visualize only 2D or 3D coordinates.

PCA transforms your dataset:

$$\mathbb{R}^{1000 \times 256} \longrightarrow \mathbb{R}^{1000 \times 2}$$

Or:

$$\mathbb{R}^{1000 \times 256} \longrightarrow \mathbb{R}^{1000 \times 3}$$

Now you can plot the samples on a 2D or 3D graph.

![Principal Component Analysis Graphical Explanation](https://images.openai.com/static-rsc-4/-lwT7YVv-vT1T1SOkp-MEg6y-a2k83xSzlJNtXrdkpudcau9Qk8bnKFnKCGrRYM9oyCF5had3G4uwMgq01n0BWLj3J9dJafigH63qZrYNuFhXw_PEnYfRTdZTN0Se8rTcOfP0J_iyhgPM9_kn5hTjq4i9yMUyndrOIfNa9VyiY0KK7Seu6JzxS9Hkq5hn_lV?purpose=fullsize)

**Central Question:** *How does PCA decide which two or three dimensions should be preserved?*

That's the core idea behind PCA.

---

## 2. Understand the Intuition Behind PCA

Imagine that you have two-dimensional data points where each point has two features:
* **X-axis:** Height
* **Y-axis:** Weight

| Person | Height | Weight |
| :--- | :--- | :--- |
| **A** | 150 | 45 |
| **B** | 155 | 48 |
| **C** | 160 | 55 |
| **D** | 165 | 60 |
| **E** | 170 | 65 |
| **F** | 175 | 70 |

Notice something interesting: as height increases, weight generally increases. Therefore, the data points are not randomly distributed—they follow a particular direction.

![How many principal components should I keep?](https://images.openai.com/static-rsc-4/AbMpya0PJwmXWt2k59aWZjt5JSpODKV5m-qss53x424PmXatz7U7yVpirPKVbYX4gn9Ya6WxmXZMUupFD2DeYmqbg0pwWa51Ht5D2D7J58CkejpaTxW_2c_4OUHfWigvZfN_T3FHWQChP2a1fd_K3i0Iw5p3P7JRpgcRjqu7xVQ9J7DyqtJGA2tVMNgc5iVE?purpose=fullsize)

1. PCA tries to discover the direction along which the data varies the most. This direction is called the **First Principal Component ($\text{PC}_1$)**.
2. Then PCA finds another direction that:
   * Is **perpendicular (orthogonal)** to the first principal component.
   * Captures as much of the **remaining variance** as possible.
   This is called the **Second Principal Component ($\text{PC}_2$)**.

In a 2D dataset:
* $\text{PC}_1$ captures the maximum possible variance.
* $\text{PC}_2$ captures the maximum remaining variance.

Since the original data has only two dimensions, PCA can represent it using $\text{PC}_1$ and $\text{PC}_2$ without losing information.
* If you retain only $\text{PC}_1$, you reduce the data from 2D to 1D.
* If you retain $\text{PC}_1$ and $\text{PC}_2$, you retain both dimensions.

---

## 3. What Does Variance Mean in PCA?

This is probably the most important concept to understand.

**Variance** measures how spread out the data is.

Consider two features:
* **Feature A:** $[10, 11, 12, 13, 14]$
* **Feature B:** $[10, 50, 100, 150, 200]$

Feature B has much greater variance than Feature A.

PCA tries to find combinations of features that have the **maximum variance**.

**Why?**
Because directions with greater variance often contain more information about how the observations differ.

For example, suppose your 256-dimensional vectors contain correlated information:
* Feature 1 represents one characteristic.
* Feature 2 represents a very similar characteristic.
* Feature 3 represents another characteristic.
* And so on.

There may be redundancy across features. PCA discovers new combinations of the original features that capture the largest variations.

> **Important:** High variance does not always mean useful information. PCA doesn't know which variations are meaningful for your particular machine-learning task.

---

## 4. What is a Principal Component Mathematically?

Suppose your original data has 3 features:

$$X = [x_1, x_2, x_3]$$

PCA creates new features using linear combinations of the original features:

$$\begin{aligned} \text{PC}_1 &= 0.5x_1 + 0.6x_2 + 0.4x_3 \\ \text{PC}_2 &= 0.7x_1 - 0.7x_2 + 0.1x_3 \\ \text{PC}_3 &= 0.2x_1 + 0.1x_2 - 0.9x_3 \end{aligned}$$

These are new axes in your feature space.

Notice that PCA doesn't simply select a subset of original features. Instead, it creates new features from combinations of all original features.

Each principal component is represented by a vector of weights (eigenvector):

$$v_1 = \begin{bmatrix} 0.5 \\ 0.6 \\ 0.4 \end{bmatrix}$$

This vector describes the direction of $\text{PC}_1$.

For 256-dimensional input vectors, each principal component will have 256 weights:

$$v_1 \in \mathbb{R}^{256}, \quad v_2 \in \mathbb{R}^{256}$$

To reduce your data to 2D, PCA finds two such directions.

---

## 5. How Does PCA Actually Work? Step by Step

Let's understand the step-by-step algorithm.

Suppose $X \in \mathbb{R}^{N \times D}$, where:
* $N$ = number of samples.
* $D$ = number of dimensions (for your case, $D = 256$).

---

### Step 1: Center the Data

First, calculate the mean of every feature across all samples:

$$\mu_j = \frac{1}{N} \sum_{i=1}^{N} X_{ij}$$

Then subtract the mean vector from every sample:

$$X_{\text{centered}} = X - \mu$$

**Numerical Example:**

Original data $X$:
$$X = \begin{bmatrix} 2 & 4 \\ 4 & 6 \\ 6 & 8 \end{bmatrix}$$

Mean $\mu$:
$$\mu = [4, 6]$$

Centered data $X_c$:
$$X_c = \begin{bmatrix} -2 & -2 \\ 0 & 0 \\ 2 & 2 \end{bmatrix}$$

**Why do we center the data?**
Because PCA focuses on the variation around the mean rather than the absolute location of the data points.

![Principal Component Analysis Data Centering](https://images.openai.com/static-rsc-4/o6h8RHXB54l6fZisBdC8AvmGsZNSEH4eSx9DQ-prOr5otRwWTkcy6YFOvZjMhLwRl15JgHkR7AvrWw7B7MEJVGLzndvYd58RMOjlkwXKOaok8Fc6tOjnA0HIzT9IfA60v1xKfBpLoXaPvLdfR9KW2tusy1STpfl9a636tUzWzlPdL4Xz3LVBSCljxzhEGBhe?purpose=fullsize)

---

### Step 2: Calculate the Covariance Matrix

The covariance matrix tells us how pairs of features vary together:

$$C = \frac{1}{N-1} X_c^T X_c$$

For our centered matrix $X_c$:

$$C = \begin{bmatrix} 4 & 4 \\ 4 & 4 \end{bmatrix}$$

Matrix breakdown:

$$C = \begin{bmatrix} \operatorname{Var}(x_1) & \operatorname{Cov}(x_1, x_2) \\ \operatorname{Cov}(x_2, x_1) & \operatorname{Var}(x_2) \end{bmatrix}$$

* Diagonal elements represent **variance**.
* Off-diagonal elements represent **covariance**.

Here, both features have the same variance, and their covariance is positive (meaning they increase together).

For 256 features:

$$C \in \mathbb{R}^{256 \times 256}$$

---

### Step 3: Calculate Eigenvalues and Eigenvectors

This is the mathematical core of PCA. We calculate the eigenvectors and eigenvalues of the covariance matrix:

$$C v = \lambda v$$

Where:
* $C$ = covariance matrix.
* $v$ = eigenvector (direction).
* $\lambda$ = eigenvalue (magnitude / variance).

**What does this mean?**
* An **eigenvector** represents a direction in feature space.
* Its corresponding **eigenvalue** tells us how much variance exists along that direction.

For example, suppose PCA produces:

| Component | Eigenvalue ($\lambda$) |
| :--- | :--- |
| **PC1** | 10 |
| **PC2** | 5 |
| **PC3** | 2 |
| **PC4** | 1 |

* $\text{PC}_1$ captures the greatest variance.
* $\text{PC}_2$ captures the second greatest variance, and so on.

PCA sorts the eigenvectors according to their eigenvalues in descending order.

![Eigenvalues and Eigenvectors Explanation](https://images.openai.com/static-rsc-4/KdRtdpzuRm4GIdLSTwRZHPDZx7yr-va975AEhZPGy18Htiks3s04eIBWAPUJSYmQge-P3RwIl7mVMSZo0uz7iKhWIZOVoiyGu08IQWlVgBHli9vRw9w9WWGOdBE1LHWAa3_XJMkDjodA8C49eAY_uDx-2-yb1IXT0LWPl7lKymipLYdCukc2Iu5BPYleoIhD?purpose=fullsize)

---

### Step 4: Select the Top Components

Suppose you have 256 original features. PCA calculates 256 eigenvalues and corresponding eigenvectors.

To reduce dimensionality to 2D, select the two eigenvectors corresponding to the largest eigenvalues:

$$v_1, v_2$$

Construct a projection matrix $W$:

$$W = \begin{bmatrix} \mid & \mid \\ v_1 & v_2 \\ \mid & \mid \end{bmatrix}$$

Its shape is:

$$W \in \mathbb{R}^{256 \times 2}$$

For 3D visualization:

$$W \in \mathbb{R}^{256 \times 3}$$

---

### Step 5: Project the Original Data

Multiply the centered data matrix by the projection matrix:

$$Z = X_c W$$

**For 2D:**
$$(N \times 256) \times (256 \times 2) = (N \times 2)$$

Your original 256-dimensional data matrix becomes 2-dimensional.

**For 3D:**
$$(N \times 256) \times (256 \times 3) = (N \times 3)$$

Now you have low-dimensional coordinates ready to plot.

---

## 6. A Complete Python Example for 256-Dimensional Vectors

Suppose you have two groups:
* **Group A:** 100 vectors, each of size 256 ($A \in \mathbb{R}^{100 \times 256}$)
* **Group B:** 100 vectors, each of size 256 ($B \in \mathbb{R}^{100 \times 256}$)

---

### CRITICAL: Combine the Two Groups Before Applying PCA

Do **NOT** independently apply PCA to Group A and Group B.

**Why?**
PCA learns a coordinate system based on data variance. If you fit PCA independently to each group, each group will be mapped onto a different coordinate system, rendering their 2D/3D coordinates non-comparable.

Instead, concatenate both groups:

$$X = \begin{bmatrix} A \\ B \end{bmatrix} \in \mathbb{R}^{200 \times 256}$$

Fit PCA on this combined dataset $X$.

![Combining Datasets for Clustering and Dimensionality Reduction](https://images.openai.com/static-rsc-4/y6iPvo6ii_MYbz6uD9zcsvcLji4zndrKyb_b-icas2DESfNO3zak3Jt3sL9dsOQrLGrpNaPM5YyA6JiGhv1216TcSec0UafLmB5lGMs1d1K2f3s5gKFX4ymofcrIxGhbI-4oK569x_a1qPI20XwN-DdWtLp7jqWlbVROKJK2QEqg2olTtahplAde3kNFbfvA?purpose=fullsize)

---

### Python Implementation: 256D to 2D

```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn.decomposition import PCA

# Generate example data (Replace with your actual 256D vectors)
# Group A: 100 samples, 256 dimensions
A = np.random.randn(100, 256)

# Group B: 100 samples, 256 dimensions (with offset to show clustering)
B = np.random.randn(100, 256) + 1.5

# Combine both groups
X = np.vstack([A, B])

# Create class labels (0 for Group A, 1 for Group B)
labels = np.array([0] * len(A) + [1] * len(B))

# Apply PCA
pca = PCA(n_components=2)
X_2d = pca.fit_transform(X)

# Separate the projected points
A_2d = X_2d[labels == 0]
B_2d = X_2d[labels == 1]

# Visualization
plt.figure(figsize=(10, 7))
plt.scatter(A_2d[:, 0], A_2d[:, 1], label="Group A", alpha=0.7)
plt.scatter(B_2d[:, 0], B_2d[:, 1], label="Group B", alpha=0.7)

plt.xlabel("Principal Component 1")
plt.ylabel("Principal Component 2")
plt.title("PCA Visualization of 256D Vectors")
plt.legend()
plt.grid(alpha=0.3)
plt.show()

# Print variance statistics
print("Explained variance ratio per component:", pca.explained_variance_ratio_)
print("Total variance preserved:", pca.explained_variance_ratio_.sum())
```

![PCA Scatter Plot Output Example](https://images.openai.com/static-rsc-4/_kylIpXo1tF6bGEWLv3FrpYCbohL4j_dbwrrOEdbhEPD6kJao6bVhMTMPGBVSMXfppmLPCCoVNVI0k-qIYqolqYCbqm1PgSoTjp2NrCLFEE4-8HpUGU3TWT6Ibn50UgfAZbE49AXT-DEoGW3bbid1baixbZRNlSaXF3l0K7YXzGTXDlmE085y1FHo4N2IFGD?purpose=fullsize)

---

## 7. How to Visualize Actual 256-Dimensional Embeddings

If you have embeddings generated by a deep-learning model (e.g., Sentence-BERT, OpenAI, etc.):

```python
import matplotlib.pyplot as plt
from sklearn.decomposition import PCA

# Assume embeddings shape: (500, 256) and labels shape: (500,)
pca = PCA(n_components=2)
reduced = pca.fit_transform(embeddings)

plt.figure(figsize=(10, 7))

for group in set(labels):
    indices = (labels == group)
    plt.scatter(
        reduced[indices, 0],
        reduced[indices, 1],
        label=f"Group {group}",
        alpha=0.7
    )

plt.xlabel("PC1")
plt.ylabel("PC2")
plt.title("256D Embeddings Visualized Using PCA")
plt.legend()
plt.grid(alpha=0.3)
plt.show()

print("Variance explained ratio:", pca.explained_variance_ratio_)
```

---

## 8. What If You Have Only TWO Vectors of Size 256?

Suppose you have only two vectors:

$$A \in \mathbb{R}^{256}, \quad B \in \mathbb{R}^{256}$$

**Critical Issue:**
Two observations are **not enough** to learn a meaningful 2D PCA subspace for 256-dimensional space. After centering two points, the rank of the matrix is at most 1. Consequently, PCA will capture at most one non-zero principal component, making both points lie along a single line in 2D space.

```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn.decomposition import PCA

A = np.random.randn(256)
B = np.random.randn(256)
X = np.vstack([A, B])

pca = PCA(n_components=2)
X_2d = pca.fit_transform(X)

plt.scatter(X_2d[:, 0], X_2d[:, 1])
plt.text(X_2d[0, 0], X_2d[0, 1], "Vector A")
plt.text(X_2d[1, 0], X_2d[1, 1], "Vector B")
plt.xlabel("PC1")
plt.ylabel("PC2")
plt.title("Two 256D Vectors Projected Using PCA")
plt.show()
```

### Alternative Recommendation for 2 Vectors: Cosine Similarity

Instead of PCA, compute **Cosine Similarity**:

$$\operatorname{cosine}(A, B) = \frac{A \cdot B}{\|A\| \|B\|}$$

Cosine similarity provides an exact scalar measure of directional similarity between two high-dimensional vectors.

---

## 9. How to Decide How Many Principal Components to Retain?

PCA provides the **Explained Variance Ratio** to evaluate how much variance is preserved.

| Component | Explained Variance | Cumulative Variance |
| :--- | :--- | :--- |
| **PC1** | 35% | 35% |
| **PC2** | 20% | 55% |
| **PC3** | 12% | 67% |
| **PC4** | 8% | 75% |
| **PC5** | 5% | 80% |

* Retaining 2 components preserves: $35\% + 20\% = 55\%$ of total variance.
* Retaining 5 components preserves: $35\% + 20\% + 12\% + 8\% + 5\% = 80\%$ of total variance.

### Cumulative Variance Plot in Python

```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn.decomposition import PCA

pca = PCA().fit(X)
cumulative_variance = np.cumsum(pca.explained_variance_ratio_)

plt.figure(figsize=(9, 5))
plt.plot(range(1, len(cumulative_variance) + 1), cumulative_variance, marker='.')
plt.axhline(y=0.95, color='r', linestyle='--', label='95% Preserved Variance threshold')
plt.xlabel("Number of Components")
plt.ylabel("Cumulative Explained Variance")
plt.title("PCA Cumulative Explained Variance Plot")
plt.legend()
plt.grid()
plt.show()
```

To automatically retain a specific threshold of total variance (e.g., 95%):

```python
pca = PCA(n_components=0.95, svd_solver="full")
X_reduced = pca.fit_transform(X)
```

---

## 10. How to Visualize Vectors in 3D?

Simply set `n_components=3` and use Matplotlib's 3D projection tool:

```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn.decomposition import PCA

A = np.random.randn(100, 256)
B = np.random.randn(100, 256) + 1.5
X = np.vstack([A, B])
labels = np.array([0] * len(A) + [1] * len(B))

pca = PCA(n_components=3)
X_3d = pca.fit_transform(X)

fig = plt.figure(figsize=(10, 8))
ax = fig.add_subplot(111, projection='3d')

ax.scatter(X_3d[labels == 0, 0], X_3d[labels == 0, 1], X_3d[labels == 0, 2], label="Group A", alpha=0.7)
ax.scatter(X_3d[labels == 1, 0], X_3d[labels == 1, 1], X_3d[labels == 1, 2], label="Group B", alpha=0.7)

ax.set_xlabel("PC1")
ax.set_ylabel("PC2")
ax.set_zlabel("PC3")
ax.set_title("3D PCA Projection")
ax.legend()
plt.show()

print("Explained variance ratio:", pca.explained_variance_ratio_)
print("Total variance:", pca.explained_variance_ratio_.sum())
```

![3D PCA Visualization Diagram](https://images.openai.com/static-rsc-4/-iucxTsvu7uwD7Yy-rh9RBN_Wc1V0U9El6br3WW-cSf_9lwtzLjGbxtEYdXS4oLvuSD1vCAVCxYCskw_qyWDm4V8iy7et054EHY9Fqq30pYfjvdvhntXDqJ0kI9B80GyhPEnie4RaNvxexULQGNczpb_RckpxdKISoJzF4puKC_jRKSRSThJeDdok5O9KgRT?purpose=fullsize)

---

## 11. Important Considerations for Groups with Different Distributions

Suppose Group A consists of positive sentence embeddings and Group B consists of negative sentence embeddings, each of length 256.

### Workflow Pipeline

```
Group A (100 x 256)  \
                      ---> Combine Matrix X (200 x 256) ---> Fit PCA (2D) ---> Plot (200 x 2)
Group B (100 x 256)  /
```

![Group Embedding Workflow](https://images.openai.com/static-rsc-4/eZVwCgxz4gqFlXm0SnGkz2DQhGLPj1HZSFo8tBSuKIqE_jzletpGDoRlQZUoFAt4Ndt-beFbh90kOqVBhEwv0U-WyWMw_FxBm2DsVh5Uskdj6bdeOfxSfQQumQYVFM9FmiDtIEMNgf_oj9X8rsOxHaazPHR53SnW0_SMNpAxuWpu493p1UDJSOa3-7slJhVZ?purpose=fullsize)

### Critical Insight: PCA Does Not Know Group Labels

PCA is strictly **unsupervised**. When executing `pca.fit_transform(X)`, PCA operates without class labels and strictly maximizes total feature variance.

> **Key Limitation:** PCA does not explicitly maximize class separation. If two groups differ primarily along low-variance directions, PCA may discard those directions during 2D mapping, causing clusters to overlap in visualization despite being linearly separable in 256D space.

---

## 12. Normalization Before Applying PCA

Two distinct preprocessing steps are often confused:

### A. Feature Standardization (`StandardScaler`)
$$x'_{ij} = \frac{x_{ij} - \mu_j}{\sigma_j}$$
Standardizes every feature independently across samples to zero mean and unit variance.

### B. $L_2$ Normalization (`normalize`)
$$x' = \frac{x}{\|x\|_2}$$
Normalizes each vector to unit length ($L_2$ norm = 1). Common when evaluating cosine distance.

| Preprocessing Step | Primary Action |
| :--- | :--- |
| **`StandardScaler`** | Normalizes individual features across all samples |
| **`L2 normalization`** | Normalizes sample vectors across their feature dimensions |
| **`PCA`** | Identifies directions of maximum variance |

**Recommendations:**
* For raw neural net embeddings: Run PCA directly first.
* If comparing embeddings using Cosine Similarity: Try $L_2$ normalization before PCA.

---

## 13. Limitations & Alternatives to PCA

| Limitation | Description |
| :--- | :--- |
| **Linear Transformation** | Unable to capture non-linear manifold structures. |
| **Variance-Based** | Preserves spread, not necessarily class separation. |
| **Information Loss** | Reducing 256D to 2D removes dimensions. |
| **Outliers** | Highly sensitive to extreme outlier observations. |
| **Unsupervised** | Ignores class label structures. |

### Comparative Techniques

![PCA Method](https://images.openai.com/static-rsc-4/tbzPlfhh42ONvrdkKOaMJtMggu0c74dT6PNCyqKN3m-2jmiXYZ59LZRMgd1YMXPuPTVeqdI1b6e0LO9z7KMxn4p8Th9lpV7Vrvi9RLvulF_suQyVBW8wTiuSqD6zoNkmp8vwyt8SZ4Xaw2h3lMJCJ-iRTrShGvg3Cv-eVU5NurSmnTxcV7c_qFoiB54RworN?purpose=fullsize)
* **PCA (Linear):** Fast, global baseline for initial visual analysis.

![t-SNE Method](https://images.openai.com/static-rsc-4/ygMWRIiGpl4PwwZw3Z-4JjqyukGIGnnBFrpZGgQHaOoWkyoSTCkeomjjzIitbIDqJJ1HaltYIp3rHJFF2t_2C5yf_JlDDjrJjjfiFokvo133G3qpCeCCghEZO_mOxi989PD4F3SRaZo8qO_7cdXmMsjyzOyduMIRe1Shdn_H5SArdxwjkhYq430jBoFD6Ixz?purpose=fullsize)
* **t-SNE (Non-linear):** Ideal for preserving local neighborhood clusters.

![UMAP Method](https://images.openai.com/static-rsc-4/s_24R6j_c8M9TSxDJpZL8yhcEr66g9hq6mexsmFxNORU7KQAv_g3bFgCVJcdwtZVFs_kc1XgbjEOaj0ZXtAgUVJbE0nQ103TAvs6flkDGNSzYWABUIl-n9tzEEJv6c-cJJNUeRqORl1lFcy87Z3KLou9XjsRFawgAJS30DbjzTf2FkHWRkA5jLpQs9nNLbIp?purpose=fullsize)
* **UMAP (Non-linear):** Balances local cluster structure with global distances.

---

## 14. Sample Size vs. Feature Dimensions (e.g., 56 Vectors)

Distinguish between sample counts and dimensions:
* $X \in \mathbb{R}^{56 \times 256}$: 56 samples, 256 dimensions each.
  * Matrix rank is at most $55$; at most $55$ non-zero principal components exist.
* $X \in \mathbb{R}^{N \times 56}$: $N$ samples, 56 features each.

---

## 15. Applying PCA to Out-of-Sample / Future Embeddings

When processing new incoming embeddings after training:

```python
# Train initial model
pca = PCA(n_components=2)
X_2d = pca.fit_transform(X_train)

# For new incoming vectors, call transform() ONLY:
new_embedding = model.encode(["New sentence vector"])
new_2d = pca.transform(new_embedding)
```

> **Do not re-fit PCA** on new embeddings, as doing so would alter the coordinate reference axes.

---

## 16. Mathematical Implementation Detail: Singular Value Decomposition (SVD)

In scikit-learn and practical compute frameworks, PCA uses **Singular Value Decomposition (SVD)** on centered matrix $X_c$:

$$X_c = U \Sigma V^T$$

Where:
* $X_c$ = centered data matrix.
* $U$ = left singular vectors ($N \times N$).
* $\Sigma$ = singular values ($N \times D$).
* $V^T$ = right singular vectors ($D \times D$).

The projected lower-dimensional representation $Z$ using the top $k$ components is:

$$Z = X_c V_k$$

Where $V_k$ contains the first $k$ columns of $V$ (right singular vectors).

---

## 17. Reusable Modular Visualization Function

```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn.decomposition import PCA

def visualize_embeddings(A, B, n_components=2):
    """
    Combines two embedding groups A & B, performs PCA, and generates a visual plot.
    """
    assert A.shape[1] == B.shape[1], "Dimensions of A and B must match!"

    X = np.vstack([A, B])
    pca = PCA(n_components=n_components)
    X_reduced = pca.fit_transform(X)

    A_reduced = X_reduced[:len(A)]
    B_reduced = X_reduced[len(A):]

    print(f"Original matrix shape: {X.shape}")
    print(f"Reduced matrix shape:  {X_reduced.shape}")
    print(f"Explained variance ratio: {pca.explained_variance_ratio_}")
    print(f"Total variance preserved: {pca.explained_variance_ratio_.sum():.4f}")

    if n_components == 2:
        plt.figure(figsize=(10, 7))
        plt.scatter(A_reduced[:, 0], A_reduced[:, 1], label="Group A", alpha=0.7)
        plt.scatter(B_reduced[:, 0], B_reduced[:, 1], label="Group B", alpha=0.7)
        plt.xlabel("PC1")
        plt.ylabel("PC2")
        plt.title("2D PCA Embedding Projection")
        plt.legend()
        plt.grid(alpha=0.3)
        plt.show()

    elif n_components == 3:
        fig = plt.figure(figsize=(10, 8))
        ax = fig.add_subplot(111, projection='3d')
        ax.scatter(A_reduced[:, 0], A_reduced[:, 1], A_reduced[:, 2], label="Group A", alpha=0.7)
        ax.scatter(B_reduced[:, 0], B_reduced[:, 1], B_reduced[:, 2], label="Group B", alpha=0.7)
        ax.set_xlabel("PC1")
        ax.set_ylabel("PC2")
        ax.set_zlabel("PC3")
        ax.set_title("3D PCA Embedding Projection")
        ax.legend()
        plt.show()

    return X_reduced, pca

# Example Usage:
# A_vecs = np.random.randn(100, 256)
# B_vecs = np.random.randn(100, 256) + 1.2
# reduced_data, model = visualize_embeddings(A_vecs, B_vecs, n_components=2)
```

---

## 18. Quick Reference Summary

| Concept | Definition |
| :--- | :--- |
| **PCA** | Unsupervised linear dimensionality-reduction algorithm |
| **Variance** | Spread of data across feature axes |
| **PC1** | Axis capturing maximum dataset variance |
| **PC2** | Orthogonal axis capturing maximum remaining variance |
| **Eigenvector** | Direction vector of a principal component |
| **Eigenvalue** | Amount of variance along an eigenvector |
| **Explained Variance Ratio** | Percentage of overall variance preserved by components |
| **`fit_transform()`** | Fits PCA coordinate system & projects input data |
| **`transform()`** | Projects new data onto an existing fitted PCA reference system |

---

## Further Reading & Resources

* [Scikit-Learn PCA API Documentation](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html)
* [Scikit-Learn User Guide on Dimensionality Reduction](https://scikit-learn.org/stable/modules/decomposition.html)