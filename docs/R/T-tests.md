
# T-tests

  When we are interested in testing t

### T-test for one mean

``` r
x <- c(161, 108, 148, 107, 104, 124, 152, 152)

t.test(x, mu = 120, alternative = "greater")
```

    ## 
    ##  One Sample t-test
    ## 
    ## data:  x
    ## t = 1.4295, df = 7, p-value = 0.09796
    ## alternative hypothesis: true mean is greater than 120
    ## 95 percent confidence interval:
    ##  116.0963      Inf
    ## sample estimates:
    ## mean of x 
    ##       132

### Two-sample t-test

``` r
dinopis <- c(12.9, 10.2, 7.4, 7, 10.5, 11.9, 7.1, 9.9, 14.4, 11.3)
menneus <- c(10.2, 6.9, 10.9, 11, 10.1, 5.3, 7.5, 10.3, 9.2, 8.8)

t.test(dinopis, menneus)
```

    ## 
    ##  Welch Two Sample t-test
    ## 
    ## data:  dinopis and menneus
    ## t = 1.2453, df = 16.74, p-value = 0.2302
    ## alternative hypothesis: true difference in means is not equal to 0
    ## 95 percent confidence interval:
    ##  -0.8633815  3.3433815
    ## sample estimates:
    ## mean of x mean of y 
    ##     10.26      9.02

``` r
t.test(dinopis, menneus, var.equal = TRUE)
```

    ## 
    ##  Two Sample t-test
    ## 
    ## data:  dinopis and menneus
    ## t = 1.2453, df = 18, p-value = 0.229
    ## alternative hypothesis: true difference in means is not equal to 0
    ## 95 percent confidence interval:
    ##  -0.8520327  3.3320327
    ## sample estimates:
    ## mean of x mean of y 
    ##     10.26      9.02

### Paired t-test

``` r
Lab_1 <- c(258, 259, 256, 274, 268, 253, 270, 260, 258, 262)
Lab_2 <- c(273, 277, 289, 279, 273, 278, 266, 283, 271, 267)

t.test(Lab_1, Lab_2, paired = TRUE)
```

    ## 
    ##  Paired t-test
    ## 
    ## data:  Lab_1 and Lab_2
    ## t = -3.8646, df = 9, p-value = 0.00382
    ## alternative hypothesis: true difference in means is not equal to 0
    ## 95 percent confidence interval:
    ##  -21.877869  -5.722131
    ## sample estimates:
    ## mean of the differences 
    ##                   -13.8

``` r
PSU_Lab <- c(258, 259, 256, 274, 268, 253, 270, 260, 258, 262)

t.test(PSU_Lab, mu = 260, alternative = "greater")
```

    ## 
    ##  One Sample t-test
    ## 
    ## data:  PSU_Lab
    ## t = 0.8479, df = 9, p-value = 0.2092
    ## alternative hypothesis: true mean is greater than 260
    ## 95 percent confidence interval:
    ##  257.9085      Inf
    ## sample estimates:
    ## mean of x 
    ##     261.8

``` r
Wisc_Lab <- c(273, 277, 289, 279, 273, 278, 266, 283, 271, 267)

t.test(PSU_Lab, Wisc_Lab)
```

    ## 
    ##  Welch Two Sample t-test
    ## 
    ## data:  PSU_Lab and Wisc_Lab
    ## t = -4.4643, df = 17.942, p-value = 0.0003019
    ## alternative hypothesis: true difference in means is not equal to 0
    ## 95 percent confidence interval:
    ##  -20.295882  -7.304118
    ## sample estimates:
    ## mean of x mean of y 
    ##     261.8     275.6

``` r
PSU_FollowUp <- c(262, 258, 267, 281, 259, 255, 264, 261, 258, 265)

t.test(PSU_Lab, PSU_FollowUp, paired = TRUE)
```

    ## 
    ##  Paired t-test
    ## 
    ## data:  PSU_Lab and PSU_FollowUp
    ## t = -0.65336, df = 9, p-value = 0.5299
    ## alternative hypothesis: true difference in means is not equal to 0
    ## 95 percent confidence interval:
    ##  -5.354822  2.954822
    ## sample estimates:
    ## mean of the differences 
    ##                    -1.2
