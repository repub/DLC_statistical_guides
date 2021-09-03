
# One-way Analysis of Variance (ANOVA)

### [ANOVA hypotheses](https://online.stat.psu.edu/stat500/lesson/10/10.2)

     **Null hypothesis:** the means of each group are equal.  
     **Alternative hypothesis:** at least one of the group means is not
equal to the others.

### [ANOVA assumptions](https://online.stat.psu.edu/stat500/lesson/10/10.2/10.2.1)

-   The observations are **independent**
-   Each factor is approximately **normally distributed**
-   The distributions the factors have **equal variances**

### Entering the data in R

Typically, we would load our data into R from a previously generated
file, but for this example we can manually input the data into a data
frame with the following code.

``` r
lab_precise <- data.frame(tar_mg = c(10.21, 10.25, 10.24, 9.80, 9.77, 9.73,
                                     11.32, 11.20, 11.40, 10.50, 10.68, 10.90,
                                     11.60, 11.90, 11.80, 12.30, 12.20, 12.20),
                          Brand = c(rep("Brand A", 6),
                                    rep("Brand B", 6),
                                    rep("Brand C", 6)))
```

### Fitting an one-way ANOVA in R

Now that we have our data, we can fit an ANOVA model using the `aov()`
function in R with the following syntax:

-   A formula for our model with the response on the left and the factor
    names to the right of a `~`
-   Specification of the dataset to be used

``` r
aov.fit <- aov(tar_mg ~ Brand, data = lab_precise)
```

### Interpreting the results

To summarize the results of the ANOVA model we can use the `summary()`
or alternatively `anova()` function with the fitted model as the only
argument.

``` r
summary(aov.fit)
```

    ##             Df Sum Sq Mean Sq F value   Pr(>F)    
    ## Brand        2 12.000   6.000   65.46 3.89e-08 ***
    ## Residuals   15  1.375   0.092                     
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

From the summary of the ANOVA model we see that our factor, *Brand*, has
a p-value well below our threshold of 0.05. This tells us that the mean
tar content for at least one of the brands of cigarettes is different
from the others. To determine which of the means are different from one
another we follow up the ANOVA with a post-hoc test, in this case we are
using a Tukey’s honestly significant difference test (Tukey HSD) with
the `TukeyHSD()` function.

``` r
TukeyHSD(aov.fit)
```

    ##   Tukey multiple comparisons of means
    ##     95% family-wise confidence level
    ## 
    ## Fit: aov(formula = tar_mg ~ Brand, data = lab_precise)
    ## 
    ## $Brand
    ##                 diff       lwr      upr     p adj
    ## Brand B-Brand A    1 0.5459914 1.454009 0.0001132
    ## Brand C-Brand A    2 1.5459914 2.454009 0.0000000
    ## Brand C-Brand B    1 0.5459914 1.454009 0.0001132

From the Tukey HSD post-hoc test we see that the p-values for each
comparison are below 0.05, meaning that each brand of cigarettes is
significantly different from one another.

### Full code block

``` r
# Enter the data
lab_precise <- data.frame(tar_mg = c(10.21, 10.25, 10.24, 9.80, 9.77, 9.73,
                                     11.32, 11.20, 11.40, 10.50, 10.68, 10.90,
                                     11.60, 11.90, 11.80, 12.30, 12.20, 12.20),
                          Brand = c(rep("Brand A", 6),
                                    rep("Brand B", 6),
                                    rep("Brand C", 6)))

# Fit a one-way ANOVA model
aov.fit <- aov(tar_mg ~ Brand, data = lab_precise)

# Summarize the results of the ANOVA fit
summary(aov.fit)

# Perform and summarize a Tukey HSD post-hoc test
TukeyHSD(aov.fit)
```
