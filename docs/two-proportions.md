
# Comparing Two Proportions in R

#### Hypotheses

    **Null hypothesis:** the two proportions are equal.  
    **Alternative hypothesis:** the two proportions are not equal.

#### Assumptions

-   The data was gathered by **simple random sampling**.
-   The two samples are **independent**.
-   Both groups follow a **binomial distribution**.

### Entering the data in R

    For this R tutorial we will use the example from [Penn State’s
Introduction to Mathematical Statistics
course](https://online.stat.psu.edu/stat415/lesson/9/9.4). Assume that
800 adult Americans were polled on the question: “Should the federal tax
on cigarettes be raised to pay for health care reform?”. We can simply
input the total number of people polled in each group (n) and the
respective counts of yes answers (y) into R as follows:

``` r
n1 <- 605
n2 <- 195
y1 <- 351
y2 <- 41
```

#### Running the test & interpreting the results

    Running the test in R using `prop.test()` is very simple as it does
not require us to calculate the proportions before hand and only uses
one line of code:

``` r
prop.test(x = c(y1, y2), n = c(n1, n2))
```

    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(y1, y2) out of c(n1, n2)
    ## X-squared = 79.273, df = 1, p-value < 2.2e-16
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  0.2971087 0.4427091
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.5801653 0.2102564

    The p-value for our test is extremely small and well below 0.05, so
we can conclude that there is a statistically significant difference
between the two proportions. Specifically, the proportion of smokers who
answered ‘yes’ to the survey question is significantly lower than the
proportion of non-smokers who answered ‘yes’.

    *Note that the test statistic for our test is slightly different
than in the STAT 415 example. This is because `prop.test()` uses the
Yates’ continuity correction by default. By setting `correct = FALSE`
within prop.test() you can not apply the correction to result in the
test statistic calculated in the original example.*

### Full code block

``` r
n1 <- 605
n2 <- 195
y1 <- 351
y2 <- 41

prop.test(x = c(y1, y2), n = c(n1, n2))
```
