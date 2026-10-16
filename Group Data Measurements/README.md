# Statistical Measures for Grouped Data

This README explains how to calculate the mean, median, mode, quartiles, deciles, percentiles, range, mean deviation, variance, standard deviation, and coefficient of variation for grouped frequency distributions.

The formulas use GitHub-supported LaTeX math syntax. Display equations are enclosed in `$$ ... $$`, while inline equations use `$ ... $`.

## Table of Contents

- [1. Notation](#1-notation)
- [2. Mean](#2-mean)
  - [2.1 Direct Method](#21-direct-method)
  - [2.2 Assumed Mean Method](#22-assumed-mean-method)
  - [2.3 Step-Deviation Method](#23-step-deviation-method)
- [3. Median](#3-median)
- [4. Mode](#4-mode)
- [5. Quartiles](#5-quartiles)
- [6. Deciles and Percentiles](#6-deciles-and-percentiles)
- [7. Range](#7-range)
- [8. Mean Deviation](#8-mean-deviation)
- [9. Variance](#9-variance)
- [10. Standard Deviation](#10-standard-deviation)
- [11. Coefficient of Variation](#11-coefficient-of-variation)
- [12. Worked Example](#12-worked-example)
- [13. Formula Reference](#13-formula-reference)
- [14. GitHub Markdown Tips](#14-github-markdown-tips)

---

## 1. Notation

| Symbol | Meaning |
|---|---|
| $f$ | Frequency of a class |
| $N$ | Total frequency, $N = \sum f$ |
| $x$ | Class midpoint |
| $L$ | Lower class boundary of the relevant class |
| $U$ | Upper class boundary |
| $h$ | Class width |
| $A$ | Assumed mean |
| $d$ | Deviation from the assumed mean, $d = x - A$ |
| $u$ | Step deviation, $u = (x-A)/h$ |
| $CF$ | Cumulative frequency before the relevant class |
| $f_m$ | Frequency of the modal class |
| $f_1$ | Frequency of the class before the modal class |
| $f_2$ | Frequency of the class after the modal class |
| $\bar{x}$ | Arithmetic mean |
| $M$ | Median |
| $Mo$ | Mode |
| $\sigma^2$ | Population variance |
| $\sigma$ | Population standard deviation |
| $s^2$ | Sample variance |
| $s$ | Sample standard deviation |

The class midpoint is:

$$
x = \frac{L+U}{2}
$$

The class width is:

$$
h = U-L
$$

For continuous grouped data, use class boundaries. If class intervals are written as inclusive integer ranges, convert them to continuous boundaries when appropriate.

---

## 2. Mean

The arithmetic mean is the average of the observations. For grouped data, class midpoints represent the observations in each class.

### 2.1 Direct Method

Formula:

$$
\boxed{\bar{x} = \frac{\sum fx}{N}}
$$

Steps:

1. Calculate the midpoint of each class.
2. Multiply each midpoint by its frequency to obtain $fx$.
3. Add the products to calculate $\sum fx$.
4. Divide by the total frequency $N$.

### 2.2 Assumed Mean Method

Formula:

$$
\boxed{\bar{x} = A + \frac{\sum fd}{N}}
$$

where:

$$
d = x-A
$$

Here, $A$ is a convenient class midpoint chosen as the assumed mean.

Steps:

1. Choose an assumed mean $A$.
2. Calculate $d=x-A$ for each class.
3. Calculate $fd$ for each class.
4. Substitute the values into the formula.

### 2.3 Step-Deviation Method

Formula:

$$
\boxed{\bar{x} = A + h\left(\frac{\sum fu}{N}\right)}
$$

where:

$$
u = \frac{x-A}{h}
$$

This method is especially convenient when the class widths are equal.

Steps:

1. Choose an assumed mean $A$.
2. Identify the common class width $h$.
3. Calculate $u=(x-A)/h$ for each class.
4. Calculate and sum $fu$.
5. Substitute the values into the formula.

### Example: Mean by the Direct Method

| Class interval | Frequency ($f$) | Midpoint ($x$) | $fx$ |
|---|---:|---:|---:|
| 0–10 | 2 | 5 | 10 |
| 10–20 | 3 | 15 | 45 |
| 20–30 | 5 | 25 | 125 |
| **Total** | **10** | | **180** |

Therefore:

$$
N = \sum f = 10
$$

$$
\sum fx = 180
$$

$$
\bar{x} = \frac{180}{10} = \boxed{18}
$$

All three methods give the same mean when applied correctly.

---

## 3. Median

The median divides an ordered distribution into two equal parts. For grouped continuous data, it is estimated by interpolation.

Formula:

$$
\boxed{M = L + \left(\frac{N/2-CF}{f_m}\right)h}
$$

where:

- $L$ is the lower boundary of the median class.
- $N$ is the total frequency.
- $CF$ is the cumulative frequency before the median class.
- $f_m$ is the frequency of the median class.
- $h$ is the width of the median class.

Steps:

1. Calculate the total frequency $N$.
2. Calculate $N/2$.
3. Construct the cumulative frequency column.
4. Identify the first class whose cumulative frequency is at least $N/2$.
5. Identify $L$, $CF$, $f_m$, and $h$ for that class.
6. Substitute the values into the formula.

### Example

| Class interval | Frequency ($f$) | Cumulative frequency |
|---|---:|---:|
| 0–10 | 2 | 2 |
| 10–20 | 3 | 5 |
| 20–30 | 5 | 10 |

Here, $N=10$ and $N/2=5$. Using the usual grouped-median convention, the median is at the boundary between the second and third classes:

$$
M = 10 + \left(\frac{5-2}{3}\right)10
$$

$$
\boxed{M=20}
$$

The grouped median is an estimate because the original observations within each class are not known.

---

## 4. Mode

The mode is the value that occurs most frequently. In grouped data, the class with the highest frequency is called the **modal class**.

For grouped continuous data, the estimated mode is:

$$
\boxed{Mo = L + \left(\frac{f_m-f_1}{2f_m-f_1-f_2}\right)h}
$$

where:

- $L$ is the lower boundary of the modal class.
- $f_m$ is the frequency of the modal class.
- $f_1$ is the frequency of the class immediately before the modal class.
- $f_2$ is the frequency of the class immediately after the modal class.
- $h$ is the width of the modal class.

### Example

| Class interval | Frequency |
|---|---:|
| 0–10 | 2 |
| 10–20 | 3 |
| 20–30 | 5 |
| 30–40 | 4 |

The modal class is 20–30 because it has the highest frequency.

Thus, $L=20$, $f_m=5$, $f_1=3$, $f_2=4$, and $h=10$.

$$
Mo = 20 + \left(\frac{5-3}{2(5)-3-4}\right)10
$$

$$
Mo = 20 + \frac{2}{3}\times 10
$$

$$
\boxed{Mo \approx 26.67}
$$

This is an estimate; it need not be an actual observed value.

---

## 5. Quartiles

Quartiles divide an ordered distribution into four parts:

- $Q_1$: first quartile, corresponding to the 25th percentile.
- $Q_2$: second quartile, equal to the median.
- $Q_3$: third quartile, corresponding to the 75th percentile.

For grouped continuous data, the general formula is:

$$
\boxed{Q_k = L + \left(\frac{kN/4-CF}{f_q}\right)h}
$$

where $k=1,2,3$ and $f_q$ is the frequency of the relevant quartile class.

### First Quartile

$$
\boxed{Q_1 = L + \left(\frac{N/4-CF}{f_q}\right)h}
$$

### Second Quartile

$$
\boxed{Q_2 = L + \left(\frac{N/2-CF}{f_q}\right)h}
$$

### Third Quartile

$$
\boxed{Q_3 = L + \left(\frac{3N/4-CF}{f_q}\right)h}
$$

### Interquartile Range

The interquartile range measures the spread of the middle 50% of the distribution.

$$
\boxed{IQR = Q_3-Q_1}
$$

### Quartile Deviation

Also called the semi-interquartile range:

$$
\boxed{QD = \frac{Q_3-Q_1}{2}}
$$

---

## 6. Deciles and Percentiles

Deciles divide a distribution into ten parts. Percentiles divide it into one hundred parts.

### Deciles

The formula for the $k$th decile is:

$$
\boxed{D_k = L + \left(\frac{kN/10-CF}{f_d}\right)h}
$$

where $k=1,2,\ldots,9$ and $f_d$ is the frequency of the relevant decile class.

### Percentiles

The formula for the $k$th percentile is:

$$
\boxed{P_k = L + \left(\frac{kN/100-CF}{f_p}\right)h}
$$

where $k=1,2,\ldots,99$ and $f_p$ is the frequency of the relevant percentile class.

In both formulas, $L$ is the lower boundary of the relevant class, $CF$ is the cumulative frequency before that class, and $h$ is the class width.

---

## 7. Range

The range is the difference between the largest and smallest values.

For raw data:

$$
\boxed{R = X_{\max}-X_{\min}}
$$

For grouped data, an approximate range is:

$$
\boxed{R \approx U_{\text{last}}-L_{\text{first}}}
$$

The grouped-data range is approximate because the exact minimum and maximum observations are usually unknown.

---

## 8. Mean Deviation

Mean deviation measures the average absolute distance from a central value.

### Mean Deviation About the Mean

$$
\boxed{MD_{\bar{x}} = \frac{\sum f|x-\bar{x}|}{N}}
$$

### Mean Deviation About the Median

$$
\boxed{MD_M = \frac{\sum f|x-M|}{N}}
$$

Here, $x$ is the class midpoint, $f$ is the class frequency, $\u0000bar{x}$ is the mean, $M$ is the median, and $N$ is the total frequency.

For grouped data, these calculations use class midpoints as representative values.

---

## 9. Variance

Variance measures the average squared deviation from the mean.

### Population Variance

Use this formula when the data represent the entire population:

$$
\boxed{\sigma^2 = \frac{\sum f(x-\bar{x})^2}{N}}
$$

An equivalent computational formula is:

$$
\boxed{\sigma^2 = \frac{\sum fx^2}{N}-\bar{x}^{\,2}}
$$

where:

$$
\bar{x} = \frac{\sum fx}{N}
$$

### Sample Variance

For a sample, the usual sample-variance formula is:

$$
\boxed{s^2 = \frac{\sum f(x-\bar{x})^2}{N-1}}
$$

This formula assumes that the grouped observations are represented by their class midpoints and that the usual sample-variance correction is appropriate.

---

## 10. Standard Deviation

Standard deviation is the positive square root of variance.

### Population Standard Deviation

$$
\boxed{\sigma = \sqrt{\frac{\sum f(x-\bar{x})^2}{N}}}
$$

Alternatively:

$$
\boxed{\sigma = \sqrt{\frac{\sum fx^2}{N}-\bar{x}^{\,2}}}
$$

### Sample Standard Deviation

$$
\boxed{s = \sqrt{\frac{\sum f(x-\bar{x})^2}{N-1}}}
$$

The sample formula uses the usual correction for estimating population variance from a sample.

---

## 11. Coefficient of Variation

The coefficient of variation (CV) expresses the standard deviation as a percentage of the mean.

For population data:

$$
\boxed{CV = \frac{\sigma}{\bar{x}}\times 100\%}
$$

For sample data:

$$
\boxed{CV = \frac{s}{\bar{x}}\times 100\%}
$$

A lower CV indicates less variation relative to the mean, provided the mean is positive and the comparison is meaningful.

---

## 12. Worked Example

Consider the following grouped frequency distribution.

| Class interval | Frequency ($f$) | Midpoint ($x$) | $fx$ | $fx^2$ |
|---|---:|---:|---:|---:|
| 0–10 | 2 | 5 | 10 | 50 |
| 10–20 | 3 | 15 | 45 | 675 |
| 20–30 | 5 | 25 | 125 | 3,125 |
| **Total** | **10** | | **180** | **3,850** |

### Mean

$$
\bar{x} = \frac{\sum fx}{N} = \frac{180}{10}
$$

$$
\boxed{\bar{x}=18}
$$

### Population Variance

$$
\sigma^2 = \frac{\sum fx^2}{N}-\bar{x}^{\,2}
$$

$$
\sigma^2 = \frac{3850}{10}-18^2
$$

$$
\sigma^2 = 385-324
$$

$$
\boxed{\sigma^2=61}
$$

### Population Standard Deviation

$$
\sigma = \sqrt{61}
$$

$$
\boxed{\sigma\approx 7.81}
$$

These variance and standard-deviation results are estimates based on class midpoints.

---

## 13. Formula Reference

| Measurement | Formula |
|---|---|
| Mean — direct method | `x̄ = Σfx / N` |
| Mean — assumed mean | `x̄ = A + Σfd / N` |
| Mean — step-deviation | `x̄ = A + h(Σfu / N)` |
| Median | `M = L + ((N/2 − CF) / fₘ)h` |
| Mode | `Mo = L + ((fₘ − f₁) / (2fₘ − f₁ − f₂))h` |
| First quartile | `Q₁ = L + ((N/4 − CF) / f_q)h` |
| Third quartile | `Q₃ = L + ((3N/4 − CF) / f_q)h` |
| Interquartile range | `IQR = Q₃ − Q₁` |
| Quartile deviation | `QD = (Q₃ − Q₁) / 2` |
| Range | `R = X_max − X_min` |
| Mean deviation about mean | `MD = Σf\|x − x̄\| / N` |
| Population variance | `σ² = Σf(x − x̄)² / N` |
| Sample variance | `s² = Σf(x − x̄)² / (N − 1)` |
| Population standard deviation | `σ = √(Σf(x − x̄)² / N)` |
| Coefficient of variation | `CV = (σ / x̄) × 100%` |

---

## 14. GitHub Markdown Tips

GitHub supports LaTeX math in Markdown files.

- Use `$$ ... $$` for display equations.
- Use `$ ... $` for inline equations.
- Use `\frac{a}{b}` for fractions.
- Use `\sum` for summation.
- Use `\sqrt{x}` for square roots.
- Use `\bar{x}` for the mean symbol.
- Use `\sigma^2` for population variance.
- Use `\sigma` for population standard deviation.
- Save this file as `README.md` in the root directory of your repository.

For reliable rendering, place display equations on separate lines with a blank line before and after each equation. Other Markdown viewers may need their own math-rendering support.

## Important Notes

1. Grouped-data formulas for the median, mode, quartiles, deciles, and percentiles generally provide estimates based on interpolation.
2. Variance and standard deviation calculated from class midpoints are also estimates of the spread in the original data.
3. Use the population formulas when describing the entire population and sample formulas when estimating population variability from a sample.
4. Different textbooks may use slightly different conventions for locating quantiles in grouped data. Follow the convention required by your course or examination.
