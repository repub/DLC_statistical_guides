# One-way Analysis of Variance (ANOVA)

### [ANOVA hypotheses](https://online.stat.psu.edu/stat500/lesson/10/10.2)

&nbsp;&nbsp;&nbsp;&nbsp;**Null hypothesis:** the means of each group are equal.  
&nbsp;&nbsp;&nbsp;&nbsp;**Alternative hypothesis:** at least one of the group means is not equal to the others.

### [ANOVA assumptions](https://online.stat.psu.edu/stat500/lesson/10/10.2/10.2.1)

- The observations are **independent**.
- Each factor is approximately **normally distributed**.
- The distributions the factors have **equal variances**.

### Entering the data in R

&nbsp;&nbsp;&nbsp;&nbsp;For this tutorial we will be using a simple dataset from the [PSU STAT 500 Applied Statistics course](https://online.stat.psu.edu/stat500/lesson/10/10.1). Typically, we would load our data into R from a previously generated file, however for this example we will manually input the data into a data frame with the following code.


Using the `data.frame()` function we create a data frame with two columns, one labeled *tar_mg* with the tar content from each sample and the second *Brand* which indicates which cigarette brand each measurement came from.  Note that the data frame is currently in long format which is preferred in many R functions compared to the wide format as shown in the [PSU STAT 500 example](https://online.stat.psu.edu/stat500/lesson/10/10.1).

### Fitting an one-way ANOVA in SPSS


### Interpreting the results


