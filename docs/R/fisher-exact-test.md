
# Fisher’s Exact Test

### Fisher’s exact test hypotheses

    **Null hypothesis:** The proportions of the two variables are the
equal.
<center>
<i>H<sub>0</sub>: p<sub>1</sub> = p<sub>2</sub></i>
</center>

    **Null hypothesis:** The proportions of the two variables are not
equal.

### [Fisher’s exact test assumptions](https://online.stat.psu.edu/stat504/lesson/3/3.3)

-   The total counts are **fixed and not random**.
-   **Small sample size**.
-   The samples and observations are **independent** of each other.
-   The observations are **mutually exclusive** and can be classified
    into only one category.
-   The **expected counts is &lt; 5 for** at least some of the
    observations.

### Entering the data in R

``` r
x <- matrix(data = c(3, 1, 1, 3), nrow = 2)
```

### Running the test & interpreting the results

    We can now run the test using the `fisher.test` function, which like
the proportions test is very simple and does not require us to calculate
the proportions before hand and only uses one line of code.

``` r
fisher.test(x, alternative = "greater")
```

    ## 
    ##  Fisher's Exact Test for Count Data
    ## 
    ## data:  x
    ## p-value = 0.2429
    ## alternative hypothesis: true odds ratio is greater than 1
    ## 95 percent confidence interval:
    ##  0.3135693       Inf
    ## sample estimates:
    ## odds ratio 
    ##   6.408309

    From the results of our test, we can see that our p-value of 0.2429
is greater than 0.05, indicating that there is not sufficient evidence
to reject our null hypothesis and we can conclude that by taste alone
the lady is not able to tell whether tea or milk was poured first.

### Full code block

``` r
# Enter the data
x <- matrix(data = c(3, 1, 1, 3), nrow = 2)

# Run a Fisher's exact test
fisher.test(x, alternative = "greater")
```
