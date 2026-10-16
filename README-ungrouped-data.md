# Statistical Measures for Ungrouped Data

This README contains formulas for common statistical measures calculated from **ungrouped (raw) data**. Ungrouped data lists individual observations rather than grouping them into class intervals and frequencies.

The equations use GitHub-supported LaTeX math syntax: `$$ ... $$` for display equations and `$ ... $` for inline math.

## Table of Contents

- [1. Notation](#1-notation)
- [2. Arithmetic Mean](#2-arithmetic-mean)
- [3. Median](#3-median)
- [4. Mode](#4-mode)
- [5. Quartiles](#5-quartiles)
- [6. Deciles and Percentiles](#6-deciles-and-percentiles)
- [7. Range](#7-range)
- [8. Mean Absolute Deviation](#8-mean-absolute-deviation)
- [9. Variance](#9-variance)
- [10. Standard Deviation](#10-standard-deviation)
- [11. Coefficient of Variation](#11-coefficient-of-variation)
- [12. Interquartile Range and Quartile Deviation](#12-interquartile-range-and-quartile-deviation)
- [13. Formula Reference](#13-formula-reference)

---

## 1. Notation

| Symbol | Meaning |
|---|---|
| $x_i$ | The $i$th observation |
| $n$ | Number of observations |
| $\bar{x}$ | Arithmetic mean |
| $M$ | Median |
| $Mo$ | Mode |
| $X_{\min}$ | Smallest observation |
| $X_{\max}$ | Largest observation |
| $\sigma^2$ | Population variance |
| $\sigma$ | Population standard deviation |
| $s^2$ | Sample variance |
| $s$ | Sample standard deviation |

When calculating the median, quartiles, deciles, or percentiles, first arrange the observations in ascending order.

---

## 2. Arithmetic Mean

The arithmetic mean is the sum of all observations divided by the number of observations.

$$
\boxed{\bar{x}=\frac{\sum_{i=1}^{n}x_i}{n}}
$$

Here, $x_i$ represents each observation and $n$ is the total number of observations.

### Example

For the data $2, 4, 6, 8, 10$:

$$
\bar{x}=\frac{2+4+6+8+10}{5}
$$

$$
\boxed{\bar{x}=6}
$$

---

## 3. Median

The median is the middle value after the data have been arranged in ascending order.

Let $x_{(i)}$ denote the $i$th observation in the sorted data.

### When the Number of Observations Is Odd

If $n$ is odd:

$$
\boxed{M=x_{\left(\frac{n+1}{2}\right)}}
$$

### When the Number of Observations Is Even

If $n$ is even:

$$
\boxed{M=\frac{x_{(n/2)}+x_{(n/2+1)}}{2}}
$$

### Example

For the data $3, 5, 7, 9, 11$, there are five observations:

$$
M=x_{(5+1)/2}=x_{(3)}
$$

$$
\boxed{M=7}
$$

For the data $3, 5, 7, 9$, there are four observations:

$$
M=\frac{x_{(2)}+x_{(3)}}{2}
=\frac{5+7}{2}
$$

$$
\boxed{M=6}
$$

---

## 4. Mode

The mode is the observation that occurs most frequently.

There is no single arithmetic formula required to calculate the mode. Count the occurrences of each value and identify the value or values with the highest frequency.

- **Unimodal:** One value occurs most frequently.
- **Bimodal:** Two values tie for the highest frequency.
- **Multimodal:** More than two values tie for the highest frequency.
- **No mode:** Every value occurs equally often, so no value is more frequent than the others.

### Example

For the data:

$$
2,\ 3,\ 3,\ 4,\ 5
$$

The value $3$ occurs most often.

$$
\boxed{Mo=3}
$$

---

## 5. Quartiles

Quartiles divide ordered data into four parts. The exact calculation can vary by textbook or software because several quartile-position conventions are in use.

One common method uses the median of the lower and upper halves of the sorted data:

- $Q_1$: median of the lower half.
- $Q_2$: median of the full data set.
- $Q_3$: median of the upper half.

For odd $n$, this particular method excludes the overall median from the lower and upper halves.

For a position-based convention often used in introductory courses, the quartile positions are written as:

$$
\boxed{\text{Position of }Q_1=\frac{n+1}{4}}
$$

$$
\boxed{\text{Position of }Q_2=\frac{n+1}{2}}
$$

$$
\boxed{\text{Position of }Q_3=\frac{3(n+1)}{4}}
$$

If a position is not an integer, the quartile is estimated by interpolating between adjacent ordered observations. Follow the convention specified by your course.

---

## 6. Deciles and Percentiles

Deciles divide ordered data into ten parts. Percentiles divide ordered data into one hundred parts.

Using a common introductory position convention:

### Deciles

The position of the $k$th decile is:

$$
\boxed{\text{Position of }D_k=\frac{k(n+1)}{10}}
$$

where $k=1,2,\ldots,9$.

### Percentiles

The position of the $k$th percentile is:

$$
\boxed{\text{Position of }P_k=\frac{k(n+1)}{100}}
$$

where $k=1,2,\ldots,99$.

If the calculated position is not an integer, use the interpolation rule required by your textbook or chosen statistical method. Different conventions may give slightly different results.

---

## 7. Range

The range measures the difference between the largest and smallest observations.

$$
\boxed{R=X_{\max}-X_{\min}}
$$

### Example

For the data $4, 7, 9, 12, 15$:

$$
R=15-4
$$

$$
\boxed{R=11}
$$

---

## 8. Mean Absolute Deviation

Mean absolute deviation (MAD) is the average absolute distance of the observations from a chosen central value.

### About the Mean

$$
\boxed{MAD_{\bar{x}}=\frac{\sum_{i=1}^{n}|x_i-\bar{x}|}{n}}
$$

### About the Median

$$
\boxed{MAD_M=\frac{\sum_{i=1}^{n}|x_i-M|}{n}}
$$

Here, $x_i$ is each observation, $\bar{x}$ is the mean, and $M$ is the median.

---

## 9. Variance

Variance measures the average squared deviation from the mean.

### Population Variance

Use this formula when the data include the entire population:

$$
\boxed{\sigma^2=\frac{\sum_{i=1}^{N}(x_i-\mu)^2}{N}}
$$

where $\mu$ is the population mean and $N$ is the population size.

An equivalent formula is:

$$
\boxed{\sigma^2=\frac{\sum_{i=1}^{N}x_i^2}{N}-\mu^2}
$$

### Sample Variance

Use this formula for the usual unbiased estimate of population variance from a sample:

$$
\boxed{s^2=\frac{\sum_{i=1}^{n}(x_i-\bar{x})^2}{n-1}}
$$

An equivalent computational formula is:

$$
\boxed{s^2=\frac{\sum_{i=1}^{n}x_i^2-n\bar{x}^{\,2}}{n-1}}
$$

---

## 10. Standard Deviation

Standard deviation is the positive square root of variance.

### Population Standard Deviation

$$
\boxed{\sigma=\sqrt{\frac{\sum_{i=1}^{N}(x_i-\mu)^2}{N}}}
$$

### Sample Standard Deviation

$$
\boxed{s=\sqrt{\frac{\sum_{i=1}^{n}(x_i-\bar{x})^2}{n-1}}}
$$

Standard deviation is expressed in the same units as the original observations.

---

## 11. Coefficient of Variation

The coefficient of variation (CV) expresses the standard deviation as a percentage of the mean.

### Population

$$
\boxed{CV=\frac{\sigma}{\mu}\times100\%}
$$

### Sample

$$
\boxed{CV=\frac{s}{\bar{x}}\times100\%}
$$

The coefficient of variation is most useful when the mean is positive and comparisons of relative variability are meaningful. It is undefined when the mean in the denominator is zero.

---

## 12. Interquartile Range and Quartile Deviation

The interquartile range measures the spread of the middle 50% of the data.

$$
\boxed{IQR=Q_3-Q_1}
$$

The quartile deviation, also called the semi-interquartile range, is:

$$
\boxed{QD=\frac{Q_3-Q_1}{2}}
$$

---

## 13. Formula Reference

| Measurement | Formula |
|---|---|
| Arithmetic mean | `x̄ = Σxᵢ / n` |
| Median, odd $n$ | `M = x₍ₙ₊₁₎/₂` (the observation at position $(n+1)/2$) |
| Median, even $n$ | `M = [x₍ₙ/₂₎ + x₍ₙ/₂₊₁₎] / 2` |
| Mode | Value with the highest frequency |
| First quartile position | `(n + 1) / 4` |
| Second quartile position | `(n + 1) / 2` |
| Third quartile position | `3(n + 1) / 4` |
| $k$th decile position | `k(n + 1) / 10` |
| $k$th percentile position | `k(n + 1) / 100` |
| Range | `R = Xₘₐₓ − Xₘᵢₙ` |
| Mean absolute deviation about mean | `MAD = Σ|xᵢ − x̄| / n` |
| Population variance | `σ² = Σ(xᵢ − μ)² / N` |
| Sample variance | `s² = Σ(xᵢ − x̄)² / (n − 1)` |
| Population standard deviation | `σ = √[Σ(xᵢ − μ)² / N]` |
| Sample standard deviation | `s = √[Σ(xᵢ − x̄)² / (n − 1)]` |
| Population coefficient of variation | `CV = (σ / μ) × 100%` |
| Interquartile range | `IQR = Q₃ − Q₁` |
| Quartile deviation | `QD = (Q₃ − Q₁) / 2` |

---

## GitHub Markdown Notes

- Use `$$ ... $$` for display equations.
- Use `$ ... $` for inline equations.
- Put display equations on separate lines, with blank lines before and after them.
- Save this file as `README.md` in the root directory of your GitHub repository.
- Quartile, decile, and percentile calculations depend on the selected position convention. Use the convention required by your course or software.
