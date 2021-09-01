
# Comparing Two Proportions

#### Hypotheses

    *Null hypothesis:* the two proportions are equal.  
    *Alternative hypothesis:* the two proportions are not equal.

#### Assumptions

#### Running the test

For this R tutorial we will use the example from [Penn State’s
Introduction to Mathematical Statistics
course](https://online.stat.psu.edu/stat415/lesson/9/9.4). Assume that
800 adult Americans were polled on the question: “Should the federal tax
on cigarettes be raised to pay for health care reform?”.

``` r
n1 <- 605
n2 <- 195
y1 <- 351
y2 <- 41

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
