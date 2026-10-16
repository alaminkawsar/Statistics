Measures of Central Tendency and Dispersion for Grouped Data

This README explains the formulas and methods used to calculate statistical measurements from grouped frequency distributions.

Table of Contents

- "1. Basic Notation" (#1-basic-notation)
- "2. Mean" (#2-mean)
  - "2.1 Direct Method" (#21-direct-method)
  - "2.2 Assumed Mean Method" (#22-assumed-mean-method)
  - "2.3 Step-Deviation Method" (#23-step-deviation-method)
- "3. Median" (#3-median)
- "4. Mode" (#4-mode)
- "5. Quartiles" (#5-quartiles)
- "6. Deciles and Percentiles" (#6-deciles-and-percentiles)
- "7. Range" (#7-range)
- "8. Mean Deviation" (#8-mean-deviation)
- "9. Variance" (#9-variance)
- "10. Standard Deviation" (#10-standard-deviation)
- "11. Coefficient of Variation" (#11-coefficient-of-variation)
- "12. Worked Example" (#12-worked-example)
- "13. Quick Reference" (#13-quick-reference)

---

1. Basic Notation

The following symbols are commonly used in grouped-data statistics.

Symbol| Meaning
$x$| Class midpoint
$f$| Frequency
$N$| Total frequency
$L$| Lower class boundary
$U$| Upper class boundary
$h$| Class width
$A$| Assumed mean
$d$| Deviation from assumed mean
$u$| Step deviation
$CF$| Cumulative frequency before the relevant class
$f_m$| Frequency of the modal class
$\bar{x}$| Arithmetic mean
$M$| Median
$Mo$| Mode

The total frequency is:

$$
N=\sum f
$$

For a continuous grouped frequency distribution, the class midpoint is:

$$
x=\frac{L+U}{2}
$$

The class width is:

$$
h=U-L
$$

Use class boundaries rather than class limits when necessary, especially when converting inclusive integer class intervals into continuous intervals.

---

2. Mean

The arithmetic mean represents the average value of a distribution.

2.1 Direct Method

Formula:

$$
\boxed{\bar{x}=\frac{\sum fx}{\sum f}}
$$

Since $N=\sum f$, this can also be written as:

$$
\boxed{\bar{x}=\frac{\sum fx}{N}}
$$

Steps:

1. Calculate the midpoint of every class.
2. Multiply each midpoint by its frequency.
3. Add all the products to obtain $\sum fx$.
4. Divide by the total frequency $N$.

2.2 Assumed Mean Method

Formula:

$$
\boxed{\bar{x}=A+\frac{\sum fd}{N}}
$$

Where:

$$
d=x-A
$$

Here, $A$ is the assumed mean, usually selected as a convenient class midpoint.

Steps:

1. Choose an assumed mean $A$.
2. Calculate the deviation $d=x-A$ for each class.
3. Calculate $fd$ for every class.
4. Substitute the values into the formula.

2.3 Step-Deviation Method

Formula:

$$
\boxed{\bar{x}=A+h\frac{\sum fu}{N}}
$$

Where:

$$
u=\frac{x-A}{h}
$$

This method is particularly convenient when all classes have the same width.

Steps:

1. Select an assumed mean $A$.
2. Determine the common class width $h$.
3. Calculate $u=(x-A)/h$.
4. Calculate $fu$ and add the products.
5. Apply the formula.

Example: Calculating the Mean

Consider the following frequency distribution.

Class interval| Frequency ($f$)| Midpoint ($x$)| $fx$
0–10| 2| 5| 10
10–20| 3| 15| 45
20–30| 5| 25| 125
Total| 10| | 180

Therefore:

$$
N=2+3+5=10
$$

$$
\sum fx=10+45+125=180
$$

The mean is:

$$
\bar{x}=\frac{180}{10}
$$

$$
\boxed{\bar{x}=18}
$$

All three methods produce the same mean when applied correctly.

---

3. Median

The median is the value that divides an ordered distribution into two equal parts.

For grouped continuous data, the median is estimated using the following formula:

$$
\boxed{M=L+\left(\frac{\frac{N}{2}-CF}{f_m}\right)h}
$$

Where:

- $L$ = lower boundary of the median class
- $N$ = total frequency
- $CF$ = cumulative frequency before the median class
- $f_m$ = frequency of the median class
- $h$ = width of the median class

Steps to Calculate the Median

1. Calculate the total frequency $N$.
2. Find $N/2$.
3. Construct the cumulative frequency column.
4. Identify the first class whose cumulative frequency is at least $N/2$. This is the median class.
5. Identify $L$, $CF$, $f_m$, and $h$.
6. Substitute these values into the median formula.

Example

Class interval| Frequency ($f$)| Cumulative frequency ($CF$)
0–10| 2| 2
10–20| 3| 5
20–30| 5| 10

Here:

$$
N=10,\qquad \frac{N}{2}=5
$$

Using the usual median-class convention, the median is at the boundary between the second and third classes. The grouped median estimate is:

$$
M=10+\left(\frac{5-2}{3}\right)10
$$

$$
\boxed{M=20}
$$

The grouped-data median formula assumes a uniform distribution within the median class, so the result is an estimate when only grouped frequencies are available.

---

4. Mode

The mode is the value that occurs most frequently.

For grouped continuous data, the estimated mode is calculated using:

$$
\boxed{Mo=L+\left(\frac{f_m-f_1}{2f_m-f_1-f_2}\right)h}
$$

Where:

- $L$ = lower boundary of the modal class
- $f_m$ = frequency of the modal class
- $f_1$ = frequency of the class immediately before the modal class
- $f_2$ = frequency of the class immediately after the modal class
- $h$ = width of the modal class

The modal class is the class with the highest frequency.

Example

Class interval| Frequency
0–10| 2
10–20| 3
20–30| 5
30–40| 4

The modal class is 20–30 because it has the highest frequency.

Therefore:

$$
L=20,\quad f_m=5,\quad f_1=3,\quad f_2=4,\quad h=10
$$

Substituting:

$$
Mo=20+\left(\frac{5-3}{2(5)-3-4}\right)10
$$

$$
Mo=20+\frac{2}{3}\times10
$$

$$
\boxed{Mo\approx26.67}
$$

This is an estimated mode, not necessarily an exact observation from the original data.

---

5. Quartiles

Quartiles divide an ordered distribution into four parts.

- $Q_1$ is the first quartile, or 25th percentile.
- $Q_2$ is the second quartile, equal to the median.
- $Q_3$ is the third quartile, or 75th percentile.

For grouped continuous data, the general quartile formula is:

$$
\boxed{Q_k=L+\left(\frac{\frac{kN}{4}-CF}{f_q}\right)h}
$$

Where $k=1,2,3$, and:

- $L$ = lower boundary of the relevant quartile class
- $N$ = total frequency
- $CF$ = cumulative frequency before the quartile class
- $f_q$ = frequency of the quartile class
- $h$ = class width

First Quartile

$$
\boxed{Q_1=L+\left(\frac{\frac{N}{4}-CF}{f_q}\right)h}
$$

Second Quartile

$$
\boxed{Q_2=L+\left(\frac{\frac{N}{2}-CF}{f_q}\right)h}
$$

Third Quartile

$$
\boxed{Q_3=L+\left(\frac{\frac{3N}{4}-CF}{f_q}\right)h}
$$

Interquartile Range

The interquartile range measures the spread of the middle 50% of a distribution.

$$
\boxed{IQR=Q_3-Q_1}
$$

Quartile Deviation

Also called the semi-interquartile range:

$$
\boxed{QD=\frac{Q_3-Q_1}{2}}
$$

---

6. Deciles and Percentiles

Deciles divide a distribution into ten parts, while percentiles divide it into one hundred parts.

Deciles

The formula for the $k$th decile is:

$$
\boxed{D_k=L+\left(\frac{\frac{kN}{10}-CF}{f_d}\right)h}
$$

Where $k=1,2,\ldots,9$, and $f_d$ is the frequency of the relevant decile class.

Percentiles

The formula for the $k$th percentile is:

$$
\boxed{P_k=L+\left(\frac{\frac{kN}{100}-CF}{f_p}\right)h}
$$

Where $k=1,2,\ldots,99$, and $f_p$ is the frequency of the relevant percentile class.

In both formulas, $L$ is the lower boundary of the relevant class, $CF$ is the cumulative frequency before that class, and $h$ is its width.

---

7. Range

The range is the difference between the largest and smallest values.

For raw data:

$$
\boxed{R=X_{\max}-X_{\min}}
$$

For grouped data, an approximate range can be calculated using the upper boundary of the last class and the lower boundary of the first class:

$$
\boxed{R\approx U_{\text{last}}-L_{\text{first}}}
$$

The grouped-data range is approximate because the exact minimum and maximum observations are generally unknown.

---

8. Mean Deviation

Mean deviation measures the average absolute distance of observations from a central value.

Mean Deviation About the Mean

$$
\boxed{MD_{\bar{x}}=\frac{\sum f|x-\bar{x}|}{N}}
$$

Mean Deviation About the Median

$$
\boxed{MD_M=\frac{\sum f|x-M|}{N}}
$$

Where:

- $x$ = class midpoint
- $f$ = class frequency
- $\bar{x}$ = mean
- $M$ = median
- $N$ = total frequency

For grouped data, class midpoints are used as representative observations.

---

9. Variance

Variance measures the average squared deviation from the mean.

Population Variance

When the grouped distribution represents the entire population:

$$
\boxed{\sigma^2=\frac{\sum f(x-\bar{x})^2}{N}}
$$

An equivalent computational formula is:

$$
\boxed{\sigma^2=\frac{\sum fx^2}{N}-\bar{x}^{,2}}
$$

Where:

$$
\bar{x}=\frac{\sum fx}{N}
$$

Sample Variance

When the grouped data represent a sample and the usual sample-variance correction is required:

$$
\boxed{s^2=\frac{\sum f(x-\bar{x})^2}{N-1}}
$$

The formula assumes the grouped observations are represented by their class midpoints.

---

10. Standard Deviation

Standard deviation is the positive square root of the variance.

Population Standard Deviation

$$
\boxed{\sigma=\sqrt{\frac{\sum f(x-\bar{x})^2}{N}}}
$$

Alternatively:

$$
\boxed{\sigma=\sqrt{\frac{\sum fx^2}{N}-\bar{x}^{,2}}}
$$

Sample Standard Deviation

$$
\boxed{s=\sqrt{\frac{\sum f(x-\bar{x})^2}{N-1}}}
$$

The sample formula applies when the usual sample-variance correction is appropriate.

---

11. Coefficient of Variation

The coefficient of variation (CV) measures relative variability as a percentage of the mean.

For population data:

$$
\boxed{CV=\frac{\sigma}{\bar{x}}\times100%}
$$

For sample data:

$$
\boxed{CV=\frac{s}{\bar{x}}\times100%}
$$

A smaller CV indicates less variability relative to the mean, provided the mean is positive and comparisons are meaningful.

---

12. Worked Example

Consider the following grouped frequency distribution.

Class interval| Frequency ($f$)| Midpoint ($x$)| $fx$| $fx^2$
0–10| 2| 5| 10| 50
10–20| 3| 15| 45| 675
20–30| 5| 25| 125| 3,125
Total| 10| | 180| 3,850

Mean

$$
\bar{x}=\frac{\sum fx}{N}=\frac{180}{10}
$$

$$
\boxed{\bar{x}=18}
$$

Population Variance

$$
\sigma^2=\frac{\sum fx^2}{N}-\bar{x}^{,2}
$$

$$
\sigma^2=\frac{3850}{10}-18^2
$$

$$
\sigma^2=385-324
$$

$$
\boxed{\sigma^2=61}
$$

Population Standard Deviation

$$
\sigma=\sqrt{61}
$$

$$
\boxed{\sigma\approx7.81}
$$

These calculations use class midpoints as representative observations. Therefore, the variance and standard deviation are estimates based on the grouped distribution.

---

13. Quick Reference

Measurement| Formula
Mean — direct method| $\displaystyle \bar{x}=\frac{\sum fx}{N}$
Mean — assumed mean| $\displaystyle \bar{x}=A+\frac{\sum fd}{N}$
Mean — step-deviation| $\displaystyle \bar{x}=A+h\frac{\sum fu}{N}$
Median| $\displaystyle M=L+\frac{N/2-CF}{f_m}h$
Mode| $\displaystyle Mo=L+\frac{f_m-f_1}{2f_m-f_1-f_2}h$
First quartile| $\displaystyle Q_1=L+\frac{N/4-CF}{f_q}h$
Third quartile| $\displaystyle Q_3=L+\frac{3N/4-CF}{f_q}h$
Interquartile range| $\displaystyle IQR=Q_3-Q_1$
Quartile deviation| $\displaystyle QD=\frac{Q_3-Q_1}{2}$
Range| $\displaystyle R=X_{\max}-X_{\min}$
Mean deviation about mean| $\displaystyle MD_{\bar{x}}=\frac{\sum f
Population variance| $\displaystyle \sigma^2=\frac{\sum f(x-\bar{x})^2}{N}$
Sample variance| $\displaystyle s^2=\frac{\sum f(x-\bar{x})^2}{N-1}$
Population standard deviation| $\displaystyle \sigma=\sqrt{\frac{\sum f(x-\bar{x})^2}{N}}$
Coefficient of variation| $\displaystyle CV=\frac{\sigma}{\bar{x}}\times100%$

Important: For grouped continuous data, the median, mode, quartiles, deciles, and percentiles are generally estimated by interpolation. The formulas assume the observations are distributed approximately uniformly within the relevant class. Variance and standard deviation also use class midpoints as representative values.

GitHub Markdown Notes

- Use "$$ ... $$" for display equations.
- Use "$ ... $" for inline equations.
- Use "\frac{a}{b}" for fractions.
- Use "\sum" for summation.
- Use "\sqrt{x}" for square roots.
- Use "\bar{x}" for the mean symbol.
- Use "\sigma^2" for population variance.
- Use "\sigma" for population standard deviation.
- Save the file with a ".md" extension, such as "README.md".

For the best results, keep each display equation on its own lines with a blank line before and after it. GitHub renders LaTeX math in Markdown files, but other Markdown viewers may require different math-rendering settings.
