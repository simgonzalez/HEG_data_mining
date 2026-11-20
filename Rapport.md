---
title: HR analytics report
subtitle: Factors associated with employee departures
author:
  - Satcovschi Mihaela
  - Gonzalez Simon
date: HEG — Cours 62_62 Data Mining — 31 octobre 2026
---

# HR analytics report

## Factors associated with employee departures

### Executive summary

This report analyses 10,000 employee records to identify qualitative factors
associated with the target outcome `left`. The population is imbalanced:
7,619 employees (76.19%) stayed and 2,381 (23.81%) left.

Among the qualitative attributes, **salary category** shows the clearest and
most usable gradient in this univariate analysis. The observed departure rate
is 29.97% for employees in the low salary category, 20.09% for the medium
category and 6.08% for the high category. The low and medium groups are large
(4,885 and 4,325 employees), whereas the high group contains 790 employees
and 48 leavers, so its estimate should be interpreted with more caution.

**Department** is also informative, although less strongly than salary:
departure rates vary from 14.12% in RandD to 27.27% in accounting. The
department comparison can identify where retention deserves investigation, but
it does not by itself show that a department causes departures.

The quantitative analysis complements this picture: `satisfaction_level` is by
far the strongest quantitative attribute (55.6% of employees below 0.5 left,
against 9.9% of those at 0.5 or above), and `time_spend_company` adds
complementary information. Together they isolate three small regions containing
92% of all leavers, although the separation is non-linear and not perfect.


## 1. Scope and data quality

The objective is to support HR decision-making by describing which attributes
are most useful for distinguishing employees who stayed from those who left.
The analysis uses the supplied `HR_prediction-all.csv` dataset and the
qualitative exploration from the TP1-A notebook.

The dataset contains 10,000 instances and 11 attributes. There are no missing
values. The target variable is `left`: it is qualitative, with value `0`
meaning that the employee stayed and value `1` meaning that the employee left.

| Attribute | Type | Interpretation |
|---|---|---|
| `Id` | Quantitative | Record identifier |
| `satisfaction_level` | Quantitative | Satisfaction score |
| `last_evaluation` | Quantitative | Latest evaluation score |
| `number_project` | Quantitative | Number of projects |
| `average_montly_hours` | Quantitative | Average monthly hours |
| `time_spend_company` | Quantitative | Tenure in the company |
| `Work_accident` | Qualitative | Whether a work accident occurred |
| `left` | Qualitative | Target outcome |
| `promotion_last_5years` | Qualitative | Whether a promotion occurred |
| `department` | Qualitative | Department |
| `salary` | Qualitative | Low, medium or high salary category |

`Id` is excluded from interpretation because it only identifies a record and
has no employee meaning.

## 2. Qualitative exploratory analysis

For each qualitative attribute \(f\), the notebook computes its distribution
\(P(f)\) and the conditional distribution \(P(left\,|\,f)\). The comparison
below reports the three selected attributes with the clearest practical
differences: salary and department. The final part checks
whether one of the remaining qualitative attributes can reasonably be
described as having no effect.

### 2.1 Salary category

Salary provides the clearest qualitative gradient. The departure probability
is 0.2997 for the low category, 0.2009 for the medium category and 0.0608 for
the high category. The low category contains 48.85% of employees and 61.49% of
leavers; the high category contains 7.90% of employees and 2.02% of leavers.
The high category is smaller (790 employees and 48 leavers), but the ordering
is consistent across the low and medium categories, which together contain
most of the population.

\begin{center}
\begin{minipage}{0.82\textwidth}
\centering
\includegraphics[width=\textwidth]{figures/figure-8.png}

\textit{Figure 1: Conditional distribution of \(left\) by salary category}
\end{minipage}
\end{center}

### 2.2 Department

`department` is a useful nominal attribute because the departure rate is not
the same across departments. The table reports both the size of each group and
the proportion of employees who left:

| Department | Employees | \(P(department)\) | Leavers | \(P(left=1\mid department)\) |
|---|---:|---:|---:|---:|
| RandD | 524 | 5.24% | 74 | 14.12% |
| management | 420 | 4.20% | 65 | 15.48% |
| product_mng | 608 | 6.08% | 123 | 20.23% |
| IT | 825 | 8.25% | 182 | 22.06% |
| marketing | 551 | 5.51% | 128 | 23.23% |
| sales | 2,790 | 27.90% | 699 | 25.05% |
| support | 1,481 | 14.81% | 376 | 25.39% |
| technical | 1,781 | 17.81% | 459 | 25.77% |
| hr | 503 | 5.03% | 134 | 26.64% |
| accounting | 517 | 5.17% | 141 | 27.27% |

The overall departure rate is 23.81%. RandD and management are below this
baseline, while accounting, HR, technical and support are above it. The gap
between accounting and RandD is 13.15 percentage points, so department is a
meaningful secondary qualitative signal. It can identify where retention
deserves investigation, but it does not prove that the department itself
causes departures. Satisfaction and working hours are quantitative attributes
and belong to the TP1-B analysis; this TP1-A result alone cannot establish
which department has the best mood or the greatest workload strain.

\begin{center}
\begin{minipage}{0.82\textwidth}
\centering
\includegraphics[width=\textwidth]{figures/figure-7.png}

\textit{Figure 2: Conditional distribution of \(left\) by department}
\end{minipage}
\end{center}

### 2.3 attribute with no effect

`promotion_last_5years` is associated with the target, but the association is
concentrated in a very small group. Employees without a promotion represent
97.89% of the dataset and have a departure probability of 0.2420. Employees
with a promotion represent only 2.11% (211 employees and 12 leavers) and have
a departure probability of 0.0569. The relative difference is large (over four
times higher in the no-promotion group), but the no-promotion group is almost
the whole population. Flagging it as a departure risk therefore gives a 24.20%
rate against the 23.81% base rate. Only 0.50% of leavers had been promoted;
equivalently, 99.50% of leavers had no promotion, but so did 97.89% of all
employees. Promotion history is thus a weak discriminator despite the large
relative difference.

\begin{center}
\begin{minipage}{0.82\textwidth}
\centering
\includegraphics[width=\textwidth]{figures/figure-6.png}

\textit{Figure 3: Conditional distribution of \(left\) by promotion history}
\end{minipage}
\end{center}

## 3. Quantitative exploratory analysis

The TP1-B part uses the five quantitative attributes of the dataset
(`satisfaction_level`, `last_evaluation`, `number_project`,
`average_montly_hours` and `time_spend_company`). Their importance is measured
by the standardised gap between the class means,

$$
\mathrm{score}(f)=\frac{\left|\mu(f\mid left=0)-\mu(f\mid left=1)\right|}{\sigma(f)}.
$$

A high score means that the conditional means are far apart relative to the
spread of the attribute. The scores are:

| Attribute | Mean (stayed) | Mean (left) | Score |
|---|---:|---:|---:|
| `satisfaction_level` | 0.667 | 0.443 | **0.905** |
| `time_spend_company` | 3.374 | 3.878 | **0.345** |
| `average_montly_hours` | 198.77 | 206.83 | 0.162 |
| `number_project` | 3.785 | 3.843 | 0.047 |
| `last_evaluation` | 0.717 | 0.718 | 0.005 |

Following the assignment, only the two most important attributes are kept for
the visualizations: `satisfaction_level` and `time_spend_company`. The other
three have scores below 0.17 and almost identical conditional means.

### 3.1 `satisfaction_level`

\begin{center}
\begin{minipage}{0.82\textwidth}
\centering
\includegraphics[width=\textwidth]{screenshots/Psatisfaction_level.png}

\textit{Figure 4: Marginal distribution of \(satisfaction_level\) (left) and
conditional distributions given \(left\) (right)}
\end{minipage}
\end{center}

The marginal distribution (left) is fairly flat between 0.4 and 1.0, so it says
little about departure on its own. The conditional distributions (right) are
very different. Among employees who stayed, 82% have a score of at least 0.5
and the mean is 0.667. Among leavers, the mean drops to 0.443 and the mass is
concentrated in three groups: very unsatisfied employees below 0.15
(24.6% of leavers), employees around 0.35--0.45 (42.0%) and a group of
apparently satisfied employees between 0.7 and 0.9 (25.0%). In total, 71.0% of
leavers have a satisfaction below 0.5, against 17.7% of the employees who
stayed. Equivalently, 55.6% of employees below 0.5 left, against 9.9% of those
at 0.5 or above.

This is a good discriminating attribute: its importance score (0.905) is about
2.6 times the score of the second attribute, and low satisfaction is a strong
indicator of departure. It is not sufficient on its own, because the group of
satisfied leavers largely overlaps with the employees who stayed, so a single
threshold cannot identify all departures.

### 3.2 `time_spend_company`

\begin{center}
\begin{minipage}{0.82\textwidth}
\centering
\includegraphics[width=\textwidth]{screenshots/Ptimespent_company.png}

\textit{Figure 5: Marginal distribution of \(time_spend_company\) (left) and
conditional distributions given \(left\) (right)}
\end{minipage}
\end{center}

The marginal distribution (left) is skewed to the right: most employees spent
between 2 and 4 years in the company, with a tail up to 10 years. The
conditional distributions (right) overlap, but only in certain ranges.
Employees who stayed cover the whole range; they represent 28% of the stayers
at 2 years, against 1.5% of the leavers, and all 374 employees with 7, 8 or 10
years of tenure stayed. Leavers are concentrated between 3 and 6 years:
44.6% of them have 3 years of tenure, 24.5% have 4 years and 23.4% have 5
years. The departure rate is not monotonic: 1.7% at 2 years, 24.6% at 3 years,
35.0% at 4 years, **56.6% at 5 years**, 29.8% at 6 years and 0% from 7 years.

This attribute is only a partial discriminator. Its score (0.345) is much
lower than the one of `satisfaction_level`, and the two class means differ by
only half a year (3.37 against 3.88). Taken alone it cannot separate the
classes, but it brings complementary information: 2 years or 7 years and more
are almost risk-free, while 4--5 years are the most at risk. Because this
shape is non-monotonic, a score based only on the means probably understates
its usefulness.

### 3.3 Joint visualization

\begin{center}
\begin{minipage}{0.82\textwidth}
\centering
\includegraphics[width=\textwidth]{screenshots/scatter_plot.png}

\textit{Figure 6: Scatter plot of \(time_spend_company\) against
\(satisfaction_level\), with one colour per value of \(left\)}
\end{minipage}
\end{center}

Plotting the two attributes together reveals a structure that neither
distribution shows alone. The employees who left (in red) form three compact
areas, while the employees who stayed (in blue) cover almost everything else:

| Area | Satisfaction | Tenure | Employees | Of which left |
|---|---|---:|---:|---:|
| A: unsatisfied, experienced | below 0.12 | 4--5 years | 574 | 574 (100%) |
| B: moderately unsatisfied, 3 years | 0.36--0.46 | 3 years | 1,130 | 1,022 (90.4%) |
| C: satisfied, long tenure | 0.72--0.92 | 5--6 years | 750 | 595 (79.3%) |

These three areas contain 2,191 of the 2,381 leavers (92%), while outside them
only 190 of 7,809 employees left (2.4%). The joint view also explains the weak
points of the individual attributes: the satisfied leavers of area C are
separated from the stayers by their tenure (5--6 years), and the 3-year tenure,
shared by both classes, splits into a leaver group with satisfaction between
0.36 and 0.46 and a much larger stayer group above 0.5.

The joint visualization therefore separates the classes much better than either
attribute alone, but not perfectly. The separation is non-linear: the red
areas are small islands inside the blue cloud, so rules by region (as a
decision tree could learn) are more appropriate than a single threshold. The
areas are also not pure: area C still contains about 20% of stayers, and 190
leavers (8%) lie outside the three areas.

### 3.4 Conclusion of the quantitative analysis

`satisfaction_level` is the strongest quantitative indicator of departure, and
`time_spend_company` adds complementary information. Neither separates the two
classes perfectly alone, but their joint view isolates 92% of leavers in three
small regions. This remains a descriptive result: it does not establish a
causal interpretation, and a classifier would be needed to turn these regions
into predictions.

## 4. Probability calculations and Bayes' theorem

The following example uses \(f=promotion\_last\_5years\) and
\(y=left\). The joint distribution estimated from the data is:

| \(f\) | \(P(f,y=0)\) | \(P(f,y=1)\) |
|---|---:|---:|
| \(f=0\) | 0.7420 | 0.2369 |
| \(f=1\) | 0.0199 | 0.0012 |

The joint table is useful because each cell represents a proportion of all
employees. To obtain the probability of a target value, we add the cells that
belong to that target value. For example, all employees who left are in the
column \(left=1\). Therefore:

$$
P(left=y)=\sum_{f} P(f,left=y).
$$

In practical terms, \(P(left=1)\) is obtained by adding the two cells in the
column \(left=1\). This is called a **marginal probability**: we ignore the
promotion value and keep only the target value. We can do the same thing for
promotion:

$$
P(f)=\sum_{y} P(f,left=y).
$$

The sums give \(P(left=0)=0.7619\), \(P(left=1)=0.2381\),
\(P(f=0)=0.9789\) and \(P(f=1)=0.0211\). In other words, the sum changes
the detailed table into a simpler distribution for one variable. This is the
same operation that a data-mining program performs when it counts the rows
belonging to each category and divides by the total number of rows.

To calculate \(P(left\mid f)\), we keep only the employees having a given
promotion value and divide by the total probability of that promotion value:

$$
P(left=y\,|\,f)=\frac{P(f,left=y)}{P(f)}.
$$

For example, among employees without a promotion (\(f=0\)), the probability
of leaving is the cell \(P(f=0,left=1)\) divided by the total probability of
having \(f=0\). This gives 0.2420. It is the value used by the analysis to
estimate how often this type of employee left in the observed data.

Bayes' theorem gives another way to calculate the same probability. It is
useful in data mining because it allows us to start with a known class
probability and update it after observing an attribute:

$$
P(left=1\,|\,f=0)
= \frac{P(f=0\,|\,left=1)\,P(left=1)}{P(f=0)}
=\frac{0.994960101\times0.2381}{0.9789}
\approx 0.2420.
$$

The interpretation is straightforward: \(P(left=1)=0.2381\) is the initial
probability of departure before considering promotion. The observation
\(f=0\) updates this probability; after the update, it becomes
\(P(left=1\,|\,f=0)=0.2420\). The update is small: the prior probability of
0.2381 becomes 0.2420, an increase of less than half a percentage point. This
confirms that promotion history, despite its large relative difference,
carries little information for distinguishing leavers. Thus, Bayes' theorem is
a mathematical rule that a classification method can use to estimate the most
likely class of a new employee from the attributes that are known.

## 5. Conclusions for HR

The qualitative analysis identifies **salary category** as the most promising
qualitative signal for further investigation: departures are substantially
more frequent among low- and medium-salary employees, and those groups contain
most of the population. **Promotion history**
is associated with departure, but it is too rare to be a useful screening
variable: almost all employees had no promotion, and the observed rate for
that group is barely above the base rate. `department`
shows a meaningful secondary spread: accounting and HR have the highest
observed departure rates, while RandD and management have the lowest. The department by itself is not a good indication of wheteher an employee will leave, but it can help identify where retention deserves investigation. Maybe departments with high departure rates have a more stressful work environment or less satisfying work, but this cannot be concluded from the qualitative analysis alone. Further investigation is needed to understand the underlying causes of departures in different departments.

The quantitative analysis strengthens this conclusion: **satisfaction level**
is the main quantitative signal of departure, and tenure is useful mainly in
combination with it. Their joint plot isolates three at-risk regions covering
92% of leavers, but the separation is non-linear and leaves some employees
misclassified, so these descriptive findings should be confirmed with a
predictive model before being used for HR decisions.
