### Principal Component Analysis (PCA): A Complete Explanation with Practical Examples

Principal Component Analysis (PCA) is one of the most important dimensionality-reduction techniques in machine learning. Since you already have knowledge of machine learning, deep learning, and AI algorithms, I'll explain it from both a mathematical and practical perspective.

I'll focus especially on your use case:

* You have vectors containing 256 features.

* You have two groups of vectors.

* You want to reduce the 256-dimensional vectors into 2D or 3D.

* You want to visualize the vectors and understand whether the two groups form separate clusters or overlap.

* You want to understand how PCA actually works rather than simply calling `PCA(n_components=2)` from scikit-learn.

Let's build the understanding step by step.

### 1\. What exactly is PCA?

Principal Component Analysis (PCA) is an unsupervised linear dimensionality-reduction algorithm that transforms high-dimensional data into a smaller number of new dimensions while preserving as much of the data's variance as possible.

Let's understand this statement.

Suppose you have a dataset containing 1,000 samples. Each sample is represented by a 256-dimensional vector.

Your dataset looks like this:

X =
\\begin{bmatrix}
x_{11} & x_{12} & \\cdots & x_{1,256}\\\\
x_{21} & x_{22} & \\cdots & x_{2,256}\\\\
\\vdots & \\vdots & \\ddots & \\vdots\\\\
x_{1000,1} & x_{1000,2} & \\cdots & x_{1000,256}
\\end{bmatrix}

Therefore:

X \\in \\mathbb{R}^{1000 \\times 256}

Here:

* 1,000 = number of samples.

* 256 = number of features per sample.

You cannot directly visualize this dataset because humans can easily visualize only 2D or 3D coordinates.

PCA transforms your dataset:

\\mathbb{R}^{1000 \\times 256}
\\longrightarrow
\\mathbb{R}^{1000 \\times 2}

Or:

\\mathbb{R}^{1000 \\times 256}
\\longrightarrow
\\mathbb{R}^{1000 \\times 3}

Now you can plot the samples on a 2D or 3D graph.

![Principal Component Analysis Graphical Explanation at Maria Kring blog](https://images.openai.com/static-rsc-4/-lwT7YVv-vT1T1SOkp-MEg6y-a2k83xSzlJNtXrdkpudcau9Qk8bnKFnKCGrRYM9oyCF5had3G4uwMgq01n0BWLj3J9dJafigH63qZrYNuFhXw_PEnYfRTdZTN0Se8rTcOfP0J_iyhgPM9_kn5hTjq4i9yMUyndrOIfNa9VyiY0KK7Seu6JzxS9Hkq5hn_lV?purpose=fullsize)

But the important question is:

How does PCA decide which two or three dimensions should be preserved?

That's the central idea behind PCA.

### 2\. Understand the intuition behind PCA

Imagine that you have two-dimensional data points.

Each point has two features:

* X-axis: Height

* Y-axis: Weight

For example:

| Person | Height | Weight |
| ------ | ------ | ------ |
| A      | 150    | 45     |
| B      | 155    | 48     |
| C      | 160    | 55     |
| D      | 165    | 60     |
| E      | 170    | 65     |
| F      | 175    | 70     |

Notice something interesting.

As height increases, weight generally increases.

Therefore, the data points are not randomly distributed. They follow a particular direction.

![How many principal components should I keep? Part 1: common approaches](https://images.openai.com/static-rsc-4/AbMpya0PJwmXWt2k59aWZjt5JSpODKV5m-qss53x424PmXatz7U7yVpirPKVbYX4gn9Ya6WxmXZMUupFD2DeYmqbg0pwWa51Ht5D2D7J58CkejpaTxW_2c_4OUHfWigvZfN_T3FHWQChP2a1fd_K3i0Iw5p3P7JRpgcRjqu7xVQ9J7DyqtJGA2tVMNgc5iVE?purpose=fullsize)

PCA tries to discover the direction along which the data varies the most.

This direction is called the First Principal Component (PC1).

Then PCA finds another direction that:

* Is perpendicular to the first principal component.

* Captures as much of the remaining variance as possible.

This is called the Second Principal Component (PC2).

In a 2D dataset:

* PC1 captures the maximum possible variance.

* PC2 captures the maximum remaining variance.

Since the original data has only two dimensions, PCA can represent it using PC1 and PC2 without losing information.

If you retain only PC1, you reduce the data from 2D to 1D.

If you retain PC1 and PC2, you retain both dimensions.

### 3\. What does variance mean in PCA?

This is probably the most important concept to understand.

Variance measures how spread out the data is.

Consider two features:

* Feature A: \[10, 11, 12, 13, 14\]

* Feature B: \[10, 50, 100, 150, 200\]

Feature B has much greater variance than Feature A.

PCA tries to find combinations of features that have the maximum variance.

Why?

Because directions with greater variance often contain more information about how the observations differ.

For example, suppose your 256-dimensional vectors contain correlated information.

Maybe:

* Feature 1 represents one characteristic.

* Feature 2 represents a very similar characteristic.

* Feature 3 represents another characteristic.

* And so on.

There may be redundancy across features.

PCA discovers new combinations of the original features that capture the largest variations.

Important: High variance does not always mean useful information. PCA doesn't know which variations are meaningful for your particular machine-learning task.

### 4\. What is a principal component mathematically?

Suppose your original data has 3 features:

X = \[x_1,x_2,x_3\]

PCA creates new features using linear combinations of the original features.

For example:

PC_1 = 0.5x_1+0.6x_2+0.4x_3PC_2 = 0.7x_1-0.7x_2+0.1x_3PC_3 = 0.2x_1+0.1x_2-0.9x_3

These are new axes in your feature space.

Notice that PCA doesn't simply select the original features.

Instead, it creates new features from combinations of the original features.

Each principal component is represented by a vector of weights.

For example:

v_1 =
\\begin{bmatrix}
0.5\\\\
0.6\\\\
0.4
\\end{bmatrix}

This vector describes the direction of PC1.

For 256-dimensional input vectors, each principal component will have 256 weights.

For example:

v_1 \\in \\mathbb{R}^{256}

And similarly:

v_2 \\in \\mathbb{R}^{256}

To reduce your data to 2D, PCA finds two such directions.

### 5\. How does PCA actually work? Step by step

Let's understand the actual algorithm.

Suppose:

X \\in \\mathbb{R}^{N \\times D}

Where:

* = number of samples.

* = number of dimensions.

For your case:

D=256

PCA performs the following operations.

### Step 1: Center the data

First, calculate the mean of every feature.

\\mu_j = \\frac{1}{N}\\sum_{i=1}^{N}X_{ij}

Then subtract the mean from every sample:

X_{centered}=X-\\mu

For example:

Original data:

X=
\\begin{bmatrix}
2 & 4\\\\
4 & 6\\\\
6 & 8
\\end{bmatrix}

Mean:

\\mu=\[4,6\]

Centered data:

X_c=
\\begin{bmatrix}
-2 & -2\\\\
0 & 0\\\\
2 & 2
\\end{bmatrix}

Why do we do this?

Because PCA focuses on the variation around the mean, rather than the absolute location of the data.

![Principal Component Analysis - Part 2 - Cybernetist](https://images.openai.com/static-rsc-4/o6h8RHXB54l6fZisBdC8AvmGsZNSEH4eSx9DQ-prOr5otRwWTkcy6YFOvZjMhLwRl15JgHkR7AvrWw7B7MEJVGLzndvYd58RMOjlkwXKOaok8Fc6tOjnA0HIzT9IfA60v1xKfBpLoXaPvLdfR9KW2tusy1STpfl9a636tUzWzlPdL4Xz3LVBSCljxzhEGBhe?purpose=fullsize)

### Step 2: Calculate the covariance matrix

The covariance matrix tells us how features vary together.

C=\\frac{1}{N-1}X_c^T X_c

For our example:

C=
\\begin{bmatrix}
4 & 4\\\\
4 & 4
\\end{bmatrix}

What does this matrix tell us?

C=
\\begin{bmatrix}
Var(x_1) & Cov(x_1,x_2)\\\\
Cov(x_2,x_1) & Var(x_2)
\\end{bmatrix}

* Diagonal elements represent variance.

* Off-diagonal elements represent covariance.

Here, both features have the same variance, and their covariance is positive.

This means that they increase together.

For 256 features:

C \\in \\mathbb{R}^{256 \\times 256}

### Step 3: Calculate eigenvalues and eigenvectors

This is the mathematical heart of PCA.

We calculate the eigenvectors and eigenvalues of the covariance matrix.

The mathematical equation is:

Cv=\\lambda v

Where:

* = covariance matrix.

* = eigenvector.

* = eigenvalue.

What does this mean?

An eigenvector represents a direction in the feature space.

Its corresponding eigenvalue tells us how much variance exists along that direction.

For example, suppose PCA produces:

| Component | Eigenvalue |
| --------- | ---------- |
| PC1       | 10         |
| PC2       | 5          |
| PC3       | 2          |
| PC4       | 1          |

PC1 captures the greatest variance.

PC2 captures the second greatest variance.

PC3 captures the third greatest variance.

And so on.

PCA sorts the eigenvectors according to their eigenvalues, from largest to smallest.

![Medium](https://images.openai.com/static-rsc-4/KdRtdpzuRm4GIdLSTwRZHPDZx7yr-va975AEhZPGy18Htiks3s04eIBWAPUJSYmQge-P3RwIl7mVMSZo0uz7iKhWIZOVoiyGu08IQWlVgBHli9vRw9w9WWGOdBE1LHWAa3_XJMkDjodA8C49eAY_uDx-2-yb1IXT0LWPl7lKymipLYdCukc2Iu5BPYleoIhD?purpose=fullsize)

### Step 4: Select the top components

Suppose you have 256 original features.

PCA calculates 256 eigenvalues and corresponding eigenvectors (assuming the full feature-space decomposition).

You want to reduce the dimensionality to 2.

Therefore, select:

v_1,v_2

These are the two eigenvectors corresponding to the largest eigenvalues.

Create a projection matrix:

W=
\\begin{bmatrix}
| & |\\\\
v_1 & v_2\\\\
| & |
\\end{bmatrix}

Its shape is:

W \\in \\mathbb{R}^{256 \\times 2}

For 3D:

W \\in \\mathbb{R}^{256 \\times 3}

### Step 5: Project the original data

Now comes the actual dimensionality reduction.

Multiply the centered data by the projection matrix:

Z=X_cW

For 2D:

(N\\times256)(256\\times2)
=
(N\\times2)

Your original 256-dimensional data becomes 2-dimensional.

For 3D:

(N\\times256)(256\\times3)
=
(N\\times3)

That's it!

You now have coordinates that you can visualize.

### 6\. A complete Python example for your 256-dimensional vectors

Now let's implement exactly your use case.

Suppose you have two groups:

* Group A: 100 vectors, each containing 256 dimensions.

* Group B: 100 vectors, each containing 256 dimensions.

Your matrices are:

A\\in\\mathbb{R}^{100\\times256}B\\in\\mathbb{R}^{100\\times256}

You want to visualize both groups together.

### Important: Combine the two groups before applying PCA

This is extremely important.

You should NOT independently apply PCA to Group A and Group B.

Why?

Because PCA learns a coordinate system from the data. If you independently fit PCA to each group, they will have different principal-component directions.

Consequently, their coordinates won't be directly comparable.

Instead, combine both groups:

X = \\begin{bmatrix} A\\\\B \\end{bmatrix}

Therefore:

X\\in\\mathbb{R}^{200\\times256}

Now fit PCA on this combined dataset.

![Fast and explainable clustering with CLASSIX - File Exchange - MATLAB Central](https://images.openai.com/static-rsc-4/y6iPvo6ii_MYbz6uD9zcsvcLji4zndrKyb_b-icas2DESfNO3zak3Jt3sL9dsOQrLGrpNaPM5YyA6JiGhv1216TcSec0UafLmB5lGMs1d1K2f3s5gKFX4ymofcrIxGhbI-4oK569x_a1qPI20XwN-DdWtLp7jqWlbVROKJK2QEqg2olTtahplAde3kNFbfvA?purpose=fullsize)

### Python implementation: 256D to 2D

Here is a complete working example.

```python

import numpy as np
import matplotlib.pyplot as plt
from sklearn.decomposition import PCA

# Example data:
# Replace these with your actual 256-dimensional vectors.

# Group A: 100 samples, 256 dimensions
A = np.random.randn(100, 256)

# Group B: 100 samples, 256 dimensions
B = np.random.randn(100, 256) + 1.5

# Combine both groups
X = np.vstack([A, B])

# Create labels
labels = np.array(
    [0] * len(A) + [1] * len(B)
)

# Apply PCA
pca = PCA(n_components=2)

X_2d = pca.fit_transform(X)

# Separate the projected points
A_2d = X_2d[labels == 0]
B_2d = X_2d[labels == 1]

# Visualize
plt.figure(figsize=(10, 7))

plt.scatter(
    A_2d[:, 0],
    A_2d[:, 1],
    label="Group A",
    alpha=0.7
)

plt.scatter(
    B_2d[:, 0],
    B_2d[:, 1],
    label="Group B",
    alpha=0.7
)

plt.xlabel("Principal Component 1")
plt.ylabel("Principal Component 2")

plt.title("PCA Visualization of 256D Vectors")

plt.legend()
plt.grid(alpha=0.3)
plt.show()

# Explained variance
print("Explained variance ratio:",
      pca.explained_variance_ratio_)

print("Total variance preserved:",
      pca.explained_variance_ratio_.sum())
```

Let's understand the output.

![Conjugal Futures: A Machine Learning Approach to Predicting Divorces | by André Silva | Medium](https://images.openai.com/static-rsc-4/_kylIpXo1tF6bGEWLv3FrpYCbohL4j_dbwrrOEdbhEPD6kJao6bVhMTMPGBVSMXfppmLPCCoVNVI0k-qIYqolqYCbqm1PgSoTjp2NrCLFEE4-8HpUGU3TWT6Ibn50UgfAZbE49AXT-DEoGW3bbid1baixbZRNlSaXF3l0K7YXzGTXDlmE085y1FHo4N2IFGD?purpose=fullsize)

You will get:

* Blue points: Group A.

* Orange points: Group B.

* X-axis: First principal component.

* Y-axis: Second principal component.

If the groups are well separated, you will see two relatively distinct clusters.

If they overlap, you will see points from both groups occupying similar regions.

One important clarification: The example uses randomly generated data, so the exact shape of your real clusters will depend on your actual embeddings.

### 7\. How do you visualize your actual 256-dimensional embeddings?

Let's say you already have embeddings generated by a deep learning model.

For example:

```python
embeddings = model.encode(sentences)
```

Suppose:

```python
embeddings.shape
# (500, 256)
```

And you have labels:

```python
labels.shape
# (500,)
```

You can directly use:

```python

from sklearn.decomposition import PCA
import matplotlib.pyplot as plt

# embeddings: (500, 256)
# labels: (500,)

pca = PCA(n_components=2)

reduced = pca.fit_transform(embeddings)

plt.figure(figsize=(10, 7))

for group in set(labels):
    indices = labels == group

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
plt.show()

print("Variance explained:",
      pca.explained_variance_ratio_)
```

This is the basic workflow you will probably use most often.

### 8\. What if you have only TWO vectors, each of size 256?

This is a slightly different situation, and it's important to understand.

Suppose:

A\\in\\mathbb{R}^{256}B\\in\\mathbb{R}^{256}

You have only two individual vectors.

You want to reduce both into 2D and visualize their relationship.

Here is the critical issue:

Two observations are not enough to learn a meaningful 2D PCA representation of 256-dimensional data.

After centering two observations, the centered matrix has a rank of at most 1.

That means PCA can capture at most one nonzero principal component.

You can still project both vectors, but the resulting points will lie along one line. You won't get a meaningful two-dimensional representation of the underlying embedding space.

For example:

```python

import numpy as np
import matplotlib.pyplot as plt
from sklearn.decomposition import PCA

# Two 256-dimensional vectors
A = np.random.randn(256)
B = np.random.randn(256)

# Stack into a matrix
X = np.vstack([A, B])

# PCA
pca = PCA(n_components=2)

X_2d = pca.fit_transform(X)

print(X_2d)
print(pca.explained_variance_ratio_)

plt.scatter(X_2d[:, 0], X_2d[:, 1])

plt.text(X_2d[0, 0], X_2d[0, 1], "Vector A")
plt.text(X_2d[1, 0], X_2d[1, 1], "Vector B")

plt.xlabel("PC1")
plt.ylabel("PC2")
plt.title("Two 256D Vectors Projected Using PCA")

plt.show()
```

For comparing just two embeddings, I would actually recommend cosine similarity as well:

\\operatorname{cosine}(A,B)=
\\frac{A\\cdot B}{\\|A\\|\\|B\\|}

Cosine similarity gives you a numerical measure of how similar their directions are.

It is often useful for comparing deep-learning embeddings.

### 9\. How do you decide how many principal components to retain?

Suppose you have 256-dimensional vectors.

You could choose:

* 2 components for visualization.

* 3 components for 3D visualization.

* 50 components for dimensionality reduction.

* 100 components for dimensionality reduction.

* A larger number if you want to preserve more variance.

But how do you know how many components are enough?

PCA provides something called Explained Variance Ratio.

For example:

| Component | Explained variance |
| --------- | ------------------ |
| PC1       | 35%                |
| PC2       | 20%                |
| PC3       | 12%                |
| PC4       | 8%                 |
| PC5       | 5%                 |
| Remaining | 20%                |

If you retain two components:

35\\%+20\\%=55\\%

That means your 2D representation preserves 55% of the total variance in the original data.

If you retain five components:

35+20+12+8+5=80\\%

You preserve 80% of the variance.

You can calculate this using:

```python
print(pca.explained_variance_ratio_)
```

For 2D:

```python
print(sum(pca.explained_variance_ratio_))
```

For a more detailed analysis, use a cumulative explained variance plot:

```python

import numpy as np
import matplotlib.pyplot as plt
from sklearn.decomposition import PCA

pca = PCA()
pca.fit(X)

cumulative_variance = np.cumsum(
    pca.explained_variance_ratio_
)

plt.figure(figsize=(9, 5))

plt.plot(
    range(1, len(cumulative_variance) + 1),
    cumulative_variance,
    marker="."
)

plt.axhline(
    y=0.95,
    linestyle="--",
    label="95% variance"
)

plt.xlabel("Number of Components")
plt.ylabel("Cumulative Explained Variance")
plt.title("PCA Explained Variance")

plt.legend()
plt.grid()
plt.show()
```

For example, if the cumulative variance reaches 95% at 120 components, you can use:

```python
pca = PCA(n_components=0.95, svd_solver="full")
X_reduced = pca.fit_transform(X)
```

This automatically selects enough components to retain at least 95% of the variance.

Remember that 2D visualization and preserving 95% of variance are two different goals. You don't need to preserve 95% just to create a visualization.

### 10\. How to visualize your vectors in 3D?

The process is exactly the same as 2D PCA.

You only change:

```python
PCA(n_components=2)
```

to:

```python
PCA(n_components=3)
```

Here is a complete 3D example:

```python

import numpy as np
import matplotlib.pyplot as plt

from sklearn.decomposition import PCA

# Two groups of 256-dimensional embeddings
A = np.random.randn(100, 256)
B = np.random.randn(100, 256) + 1.5

# Combine before PCA
X = np.vstack([A, B])

labels = np.array(
    [0] * len(A) + [1] * len(B)
)

# Reduce 256D to 3D
pca = PCA(n_components=3)

X_3d = pca.fit_transform(X)

# Create 3D plot
fig = plt.figure(figsize=(10, 8))

ax = fig.add_subplot(111, projection="3d")

ax.scatter(
    X_3d[labels == 0, 0],
    X_3d[labels == 0, 1],
    X_3d[labels == 0, 2],
    label="Group A",
    alpha=0.7
)

ax.scatter(
    X_3d[labels == 1, 0],
    X_3d[labels == 1, 1],
    X_3d[labels == 1, 2],
    label="Group B",
    alpha=0.7
)

ax.set_xlabel("PC1")
ax.set_ylabel("PC2")
ax.set_zlabel("PC3")

ax.set_title("3D PCA Visualization")

ax.legend()

plt.show()

print("Explained variance:",
      pca.explained_variance_ratio_)

print("Total variance:",
      pca.explained_variance_ratio_.sum())
```

![Document - Investment System (fixed TOC + print)](https://images.openai.com/static-rsc-4/-iucxTsvu7uwD7Yy-rh9RBN_Wc1V0U9El6br3WW-cSf_9lwtzLjGbxtEYdXS4oLvuSD1vCAVCxYCskw_qyWDm4V8iy7et054EHY9Fqq30pYfjvdvhntXDqJ0kI9B80GyhPEnie4RaNvxexULQGNczpb_RckpxdKISoJzF4puKC_jRKSRSThJeDdok5O9KgRT?purpose=fullsize)

You can rotate the 3D plot to inspect the distribution of your vectors from different angles.

### 11\. Very important: What if your two groups have different distributions?

Let's consider a practical example involving deep-learning embeddings.

Suppose you have:

* Group A: Embeddings generated from positive sentences.

* Group B: Embeddings generated from negative sentences.

Each sentence is represented by a 256-dimensional embedding.

Your objective is to understand whether the embeddings generated from the two groups have different distributions.

Your workflow should be:

Group A

### 100 × 256

Positive sentence embeddings

Group B

### 100 × 256

Negative sentence embeddings

Combine the embeddings

Combined dataset

### 200 × 256

Fit PCA with 2 components

Reduced dataset

### 200 × 2

Plot using Matplotlib

![GC×GC-TOFMS結合化學計量學，解析傳統型半干黃酒的揮發性物質 - 每日頭條](https://images.openai.com/static-rsc-4/eZVwCgxz4gqFlXm0SnGkz2DQhGLPj1HZSFo8tBSuKIqE_jzletpGDoRlQZUoFAt4Ndt-beFbh90kOqVBhEwv0U-WyWMw_FxBm2DsVh5Uskdj6bdeOfxSfQQumQYVFM9FmiDtIEMNgf_oj9X8rsOxHaazPHR53SnW0_SMNpAxuWpu493p1UDJSOa3-7slJhVZ?purpose=fullsize)

This approach ensures both groups are projected onto the same coordinate system.

### A subtle but important point: PCA does not know your group labels

PCA is an unsupervised algorithm.

When you execute:

```python
pca.fit_transform(X)
```

PCA does not know which vectors belong to Group A or Group B.

It only looks at the feature values and tries to find directions that maximize variance.

You use the labels afterward to assign different colors to the projected points.

This means something important:

PCA does not necessarily maximize the separation between your two groups.

For example, your two groups might be very different in some low-variance direction. PCA could discard that direction when retaining only two components.

Therefore, two clusters might overlap in a PCA visualization even though they are distinguishable in the original 256-dimensional space.

This is one of the most important limitations of PCA.

### 12\. Should you normalize your 256-dimensional vectors before PCA?

This is particularly important for deep-learning embeddings.

There are two different operations that people often confuse.

A. Feature standardization

```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()

X_scaled = scaler.fit_transform(X)
```

This standardizes every feature independently:

x'_{ij}=\\frac{x_{ij}-\\mu_j}{\\sigma_j}

This makes each feature have approximately zero mean and unit variance.

However, it changes the relative importance of the original features.

B. L2 normalization

```python
from sklearn.preprocessing import normalize

X_normalized = normalize(X, norm="l2")
```

This normalizes each vector to unit length:

x'=\\frac{x}{\\|x\\|_2}

This is commonly useful for embeddings when you care about cosine similarity.

These operations are not equivalent.

| Operation        | What it does                               |
| ---------------- | ------------------------------------------ |
| StandardScaler   | Normalizes each feature across samples     |
| L2 normalization | Normalizes each sample across its features |
| PCA              | Finds directions of maximum variance       |

My recommendation:

If your 256-dimensional vectors are already deep-learning embeddings, start by applying PCA directly to the embeddings without `StandardScaler`.

If your embedding comparison is based on cosine similarity and the vectors are not already normalized, also experiment with L2 normalization before PCA.

Compare the resulting visualizations rather than assuming one preprocessing method is always correct.

Scikit-learn's PCA centers the data but does not automatically standardize each feature. scikit-learn 1.9.1 documentation +1

### 13\. What are the limitations of PCA?

PCA is powerful, but you should understand its limitations.

| Limitation            | Explanation                                                                                  |
| --------------------- | -------------------------------------------------------------------------------------------- |
| Linear transformation | PCA cannot directly discover nonlinear structures.                                           |
| Variance-based        | It preserves variance, not necessarily class separation.                                     |
| Information loss      | Reducing 256D to 2D discards information.                                                    |
| Sensitive to outliers | Extreme observations can influence principal components.                                     |
| Unsupervised          | It does not use class labels when learning components.                                       |
| Interpretation        | Principal components may combine many original features, making them difficult to interpret. |

There are also alternatives worth knowing.

![Mastering PCA for Dimensionality Reduction in Data Science | Medium](https://images.openai.com/static-rsc-4/tbzPlfhh42ONvrdkKOaMJtMggu0c74dT6PNCyqKN3m-2jmiXYZ59LZRMgd1YMXPuPTVeqdI1b6e0LO9z7KMxn4p8Th9lpV7Vrvi9RLvulF_suQyVBW8wTiuSqD6zoNkmp8vwyt8SZ4Xaw2h3lMJCJ-iRTrShGvg3Cv-eVU5NurSmnTxcV7c_qFoiB54RworN?purpose=fullsize)

PCA

Linear

Good baseline for dimensionality reduction and visualization.

![t-SNE | RoboSathi](https://images.openai.com/static-rsc-4/ygMWRIiGpl4PwwZw3Z-4JjqyukGIGnnBFrpZGgQHaOoWkyoSTCkeomjjzIitbIDqJJ1HaltYIp3rHJFF2t_2C5yf_JlDDjrJjjfiFokvo133G3qpCeCCghEZO_mOxi989PD4F3SRaZo8qO_7cdXmMsjyzOyduMIRe1Shdn_H5SArdxwjkhYq430jBoFD6Ixz?purpose=fullsize)

t-SNE

Nonlinear

Useful for visualizing local neighborhood structures and clusters.

![GAN-Based Anomaly Detection Tailored for Classifiers](https://images.openai.com/static-rsc-4/s_24R6j_c8M9TSxDJpZL8yhcEr66g9hq6mexsmFxNORU7KQAv_g3bFgCVJcdwtZVFs_kc1XgbjEOaj0ZXtAgUVJbE0nQ103TAvs6flkDGNSzYWABUIl-n9tzEEJv6c-cJJNUeRqORl1lFcy87Z3KLou9XjsRFawgAJS30DbjzTf2FkHWRkA5jLpQs9nNLbIp?purpose=fullsize)

UMAP

Nonlinear

Useful for exploring local and broader data structure.

For your embedding visualization, I would start with PCA, then compare it with UMAP or t-SNE if PCA doesn't reveal the structure you're looking for.

One caution: t-SNE and UMAP plots can make clusters look more separated than they really are. Distances between clusters and apparent cluster sizes should not automatically be interpreted as reliable global distances or densities.

### 14\. What if you have 56 vectors instead of 200?

There is another important distinction:

* 56-dimensional vectors means each vector has 56 features.

* 56 vectors means you have 56 observations.

For example, suppose you have:

X\\in\\mathbb{R}^{56\\times256}

This means 56 vectors, each containing 256 features.

You can apply PCA:

```python
pca = PCA(n_components=2)
X_2d = pca.fit_transform(X)
```

Your output will have:

X_{2d}\\in\\mathbb{R}^{56\\times2}

However, because you have only 56 observations, the centered data has a maximum rank of 55. Therefore, you can have at most 55 nonzero principal components.

If instead your data is:

X\\in\\mathbb{R}^{N\\times56}

Then each vector contains 56 features, and PCA can reduce those 56 features to 2 or 3 dimensions.

Both situations are supported.

### 15\. What happens when you get new embeddings later?

Suppose you train PCA on 1,000 embeddings.

You obtain:

```python
pca = PCA(n_components=2)

X_2d = pca.fit_transform(X)
```

Later, your deep-learning model generates another 256-dimensional embedding.

You should not fit PCA again.

Instead:

```python
new_embedding = model.encode(["New sentence"])

new_embedding_2d = pca.transform(new_embedding)
```

Why?

Because you want to use the same learned coordinate system.

If you fit PCA again, the new projection directions could be different.

This is especially important when building applications that continuously generate embeddings.

If you have a train/test split for a machine-learning experiment, fit PCA only on the training data, then transform the test data using the same PCA object. This avoids information leakage.

### 16\. One more important mathematical detail: PCA uses SVD

Earlier, I explained PCA using eigenvalues and eigenvectors of the covariance matrix.

That is mathematically correct.

However, in practical implementations, PCA is commonly computed using Singular Value Decomposition (SVD).

The decomposition is:

X_c=U\\Sigma V^T

Where:

* = centered input matrix.

* = left singular vectors.

* = singular values.

* = right singular vectors.

The principal components are obtained from the right singular vectors.

The reduced representation is:

Z=X_cV_k

Where contains the first principal directions.

This is how scikit-learn implements PCA internally for its SVD-based solvers. scikit-learn 1.9.1 documentation +1

You don't need to calculate the covariance matrix manually when using scikit-learn.

### 17\. My recommended workflow for your actual project

Since your main goal is to visualize the relationship between two groups of 256-dimensional embeddings, I recommend the following workflow.

Recommended experimental pipeline

* Collect embeddings

    Group A:

    Group B:

* Combine both groups

    X=\\operatorname{vstack}(A,B)

* Apply PCA

    Start with 2 components and then experiment with 3.

* Visualize

    Plot Group A and Group B using different colors.

* Evaluate

    Examine explained variance, overlap, and cluster structure.

* Compare

    Experiment with UMAP and t-SNE if you need nonlinear visualizations.

Here is a compact version you can reuse with your actual data:

```python

import numpy as np
import matplotlib.pyplot as plt

from sklearn.decomposition import PCA

def visualize_embeddings(A, B, n_components=2):

    # A and B must have the same feature dimension
    assert A.shape[1] == B.shape[1]

    # Combine both groups
    X = np.vstack([A, B])

    # Fit PCA on combined data
    pca = PCA(n_components=n_components)

    X_reduced = pca.fit_transform(X)

    # Split the projected data
    A_reduced = X_reduced[:len(A)]
    B_reduced = X_reduced[len(A):]

    print("Original shape:", X.shape)
    print("Reduced shape:", X_reduced.shape)

    print(
        "Explained variance ratio:",
        pca.explained_variance_ratio_
    )

    print(
        "Total explained variance:",
        pca.explained_variance_ratio_.sum()
    )

    if n_components == 2:

        plt.figure(figsize=(10, 7))

        plt.scatter(
            A_reduced[:, 0],
            A_reduced[:, 1],
            label="Group A",
            alpha=0.7
        )

        plt.scatter(
            B_reduced[:, 0],
            B_reduced[:, 1],
            label="Group B",
            alpha=0.7
        )

        plt.xlabel("PC1")
        plt.ylabel("PC2")
        plt.title("PCA Visualization")
        plt.legend()
        plt.grid(alpha=0.3)

        plt.show()

    elif n_components == 3:

        fig = plt.figure(figsize=(10, 8))
        ax = fig.add_subplot(111, projection="3d")

        ax.scatter(
            A_reduced[:, 0],
            A_reduced[:, 1],
            A_reduced[:, 2],
            label="Group A"
        )

        ax.scatter(
            B_reduced[:, 0],
            B_reduced[:, 1],
            B_reduced[:, 2],
            label="Group B"
        )

        ax.set_xlabel("PC1")
        ax.set_ylabel("PC2")
        ax.set_zlabel("PC3")

        ax.legend()
        plt.show()

    return X_reduced, pca

# Example:
# A.shape = (100, 256)
# B.shape = (100, 256)

reduced, pca_model = visualize_embeddings(
    A,
    B,
    n_components=2
)
```

### 18\. Final understanding: What should you remember?

Let's summarize the most important concepts.

| Concept                  | Meaning                                                 |
| ------------------------ | ------------------------------------------------------- |
| PCA                      | Dimensionality-reduction algorithm                      |
| Variance                 | Measures data spread                                    |
| PC1                      | Direction with maximum variance                         |
| PC2                      | Orthogonal direction with maximum remaining variance    |
| Eigenvector              | Direction of a principal component                      |
| Eigenvalue               | Variance along that direction                           |
| Explained variance ratio | Percentage of total variance captured                   |
| Projection               | Mapping original vectors into the new coordinate system |
| fit_transform()          | Learns PCA and reduces the input data                   |
| transform()              | Reduces new data using an already-fitted PCA            |

The three most important things for your particular application:

* If you have two groups of 256-dimensional vectors, combine them before fitting PCA.

* Use `PCA(n_components=2)` for 2D visualization or `PCA(n_components=3)` for 3D visualization.

* Remember that PCA preserves maximum variance, not maximum separation between your groups.

### Further learning resources

* [Scikit-learn: PCA API documentation](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html?highlight=fit_transform&utm_source=chatgpt.com) — implementation, parameters and attributes.

* [Scikit-learn: PCA User Guide](https://sklearn.org/stable/modules/decomposition.html?utm_source=chatgpt.com) — mathematical background and practical examples.

One question that would help me tailor the next explanation: Are your 256-dimensional vectors generated by a deep-learning model (such as BERT, Sentence-BERT, or another embedding model), or are they numerical feature vectors from a traditional machine-learning dataset?

That distinction matters because the best preprocessing and visualization strategy can be different.

