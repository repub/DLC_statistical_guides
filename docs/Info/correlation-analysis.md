
# Pearson’s Correlation Coefficient

Pearson’s correlation coefficient is a measure that quantifies the
strength and direction of the linear relationship between two continuous
variables. It is denoted as `r`. The coefficient can range from -1 to 1,
where -1 indicates a perfect negative linear relationship, 1 indicates a
perfect positive inear relationship, and 0 indicates no linear
relationship.

## Hypotheses for Pearson’s correlation:

$$H_0 : \text{There is no linear correlation between the two variables }(\rho=0).$$
$$H_a : \text{There is no linear correlation between the two variables }(\rho≠0).$$

## Assumptions for Pearson’s correlation

- Both variables are continuous and measured on an interval or ratio
  scale.
- There is a linear relationship between the variables.
- No outliers significantly affet the correlation.
- Data for each variable is normally distributed.
- A linear relationship is expected between the two variables.

# Spearman’s Rank Correlation ($\rho$)

Spearman’s rank correlation is a non-parametric test that measures the
strength and direction of association between two ranked or ordinal
variables. Like Pearson’s, the coefficient ranges from -1 to 1.

## Hypotheses

$$H_0 : \text{There is no monotonic relationship between the two variables }(\rho=0).$$
$$H_a : \text{There is a monotonic relationship between the two variables }(\rho≠0).$$

## Assumptions

- Both variables are ordinal, interval, or ratio.
- There is a monotonic relationship between the variables.
- No tied ranks or tied ranks are dealt with appropriately (e.g.,
  averaging ranks).

If the calculated p-value from testing the rank correlation is less than
the chosen statistical threshold (typically α = 0.05), the null
hypothesis may be rejected to conclude that there is a statistically
significant monotonic relationship between the two variables.

Both Pearson’s and Spearman’s correlations are valuable tools for
understanding relationships between variables. While Pearson’s requires
a stricter set of assumptions and focuses on linear relationships,
Spearman’s can be used even when normality is violated and also measures
monotonic relationships. The choice between the two often depends on the
nature of your data and the assumptions you are willing to make.
