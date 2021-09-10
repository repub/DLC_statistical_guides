
# Logistic (Logit) Regression

### Load the data

    Here, we will use a dataset provided in [STAT 501 Regression
Methods](https://online.stat.psu.edu/stat501/lesson/15/15.1) that looks
at factors that may predict Leukemia remission. To do so, we can use the
`read.table()` command with the pathway to where we put our file.
Because of how the file is encoded, we will need to also add the
`fileEncoding = "UCS-2LE"` and `header = TRUE` options to properly load
the dataset into R.

``` r
remiss <- read.table("dat/leukemia_remission.txt",
                     fileEncoding = "UCS-2LE", header = TRUE)
```

### Fit a binary logistic regression model

Now that we have our dataset we can simply fit a logistic regression
model with the `glm()` function where we set up our model formula with
the response variable, *REMISS*, to the left and the predictor variables
to the right of a *\~*. Instead of writing out all of our predictor
variables we can instead choose to use a “.” which will call add all of
the variables to the model. Finally, after filling out the `data =`
argument we need to set `family = binomial` to fit a logistic regression
model, otherwise `glm()` would default to fitting a linear model.

``` r
remiss.fit <- glm(REMISS ~ ., data = remiss, family = binomial)
```

### Interpret the results

Like any other model fit by the `glm()` function we can use the
`summary()` command to see a summary of the results of the fit.

``` r
summary(remiss.fit)
```

    ## 
    ## Call:
    ## glm(formula = REMISS ~ ., family = binomial, data = remiss)
    ## 
    ## Deviance Residuals: 
    ##      Min        1Q    Median        3Q       Max  
    ## -1.95404  -0.66259  -0.02516   0.78184   1.57465  
    ## 
    ## Coefficients:
    ##               Estimate Std. Error z value Pr(>|z|)
    ## (Intercept)   64.25808   74.96480   0.857    0.391
    ## CELL          30.83006   52.13520   0.591    0.554
    ## SMEAR         24.68632   61.52601   0.401    0.688
    ## INFIL        -24.97447   65.28088  -0.383    0.702
    ## LI             4.36045    2.65798   1.641    0.101
    ## BLAST         -0.01153    2.26634  -0.005    0.996
    ## TEMP        -100.17340   77.75289  -1.288    0.198
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 34.372  on 26  degrees of freedom
    ## Residual deviance: 21.594  on 20  degrees of freedom
    ## AIC: 35.594
    ## 
    ## Number of Fisher Scoring iterations: 8

From the p-values for the Walds tests on the estimated coefficients are
listed in the `Pr(>|z|)` column, from where we can see that none of the
variables are significant predictors in this model. Unlike in the
Minitab output provided in the STAT 510 notes, the base `summary()`
function in R does not provide variance inflation factors (VIFs). While
we could use a package to calculate the VIFs for this model, we will
instead go ahead and refit our model with only one predictor.

### Refit a simpler model

Instead of writing a new formula in `glm()` as above, the `update()`
function provides a simpler way to refitting models. To keep variables
in the updated model we use the “.” and to add or remove variables we
can use “-” and “+” or "\*" for interactions.

``` r
remiss.fit2 <- update(remiss.fit, .~LI)

summary(remiss.fit2)
```

    ## 
    ## Call:
    ## glm(formula = REMISS ~ LI, family = binomial, data = remiss)
    ## 
    ## Deviance Residuals: 
    ##     Min       1Q   Median       3Q      Max  
    ## -1.9448  -0.6465  -0.4947   0.6571   1.6971  
    ## 
    ## Coefficients:
    ##             Estimate Std. Error z value Pr(>|z|)   
    ## (Intercept)   -3.777      1.379  -2.740  0.00615 **
    ## LI             2.897      1.187   2.441  0.01464 * 
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 34.372  on 26  degrees of freedom
    ## Residual deviance: 26.073  on 25  degrees of freedom
    ## AIC: 30.073
    ## 
    ## Number of Fisher Scoring iterations: 4

In the summary from our refit model, we see that when *LI* is the only
predictor its estimated coefficient is statistically significant with a
p-value below 0.05. Keep in mind that the interpretation of the
coefficient is different from linear regression. Specifically for our
example, we could conclude that for every 1 unit increase in the *LI*
variable the log odds of predicting *REMISS* increases by 2.897. This
type of interpretation will not be very meaningful for many
applications, but the important information to take away here is that
*LI* is a significant predictor of *REMISS* for which they share some
positive relationship.

### Full code block

``` r
# Load the data
remiss <- read.table("dat/leukemia_remission.txt",
                     fileEncoding = "UCS-2LE", header = TRUE)

# Fit a binary logistic regression model including all the variables
remiss.fit <- glm(REMISS ~ ., data = remiss, family = binomial)

# Summarize the results of the full model
summary(remiss.fit)

# Fit a new logistic regression model with only the Leukemia Index (LI) variable
remiss.fit2 <- glm(REMISS ~ LI, data = remiss, family = binomial)

# Summarize the results of the simple model
summary(remiss.fit2)
```
