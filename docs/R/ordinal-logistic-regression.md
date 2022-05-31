
# Ordinal Logistic Regression (OLR)

 For this tutorial we will be using a [modified version]() of the *Red
Wine Quaity* data set originally published by [Paulo <i>et al.</i>
2009](https://www.sciencedirect.com/science/article/pii/S0167923609001377?casa_token=FA46cZUBbKYAAAAA:-dW3CVVFbG_qTvY-73Bzhob89GojMasXIjpPX3-Hv37eV9kEZRXzuG8Ijns3JBMvewTLYww9).
The response variable, *quality*, is an ordinal variable that scales the
quality of red wines from 0 (worst) to 10 (best) to be predicted by 11
wine attributes:

-   *fixed acidity* - the acids involved with wine that are fixed
    (nonvolatile, or do not evaporate readily).
-   *volatile acidity* - the amount of volatile acid in wine, which at
    too high of levels can lead to an unpleasant, vinegar taste.
-   *citric acid* - when found in small quantities, citric acid can add
    ‘freshness’ and flavor to wines.
-   *residual sugar* - the amount of sugar remaining after fermentation
    stops; it’s rare to find wines with less than 1 gram/liter and wines
    are considered sweeter as sugar increases.
-   *chlorides* - the amount of salt in the wine.
-   *free sulfur dioxide* - the free form of SO<sub>2</sub> exists in
    equilibrium between molecular SO<sub>2</sub> (as a dissolved gas)
    and bisulfite ion; SO<sub>2</sub> inhibits microbial growth and the
    oxidation of wine.
-   *total sulfur dioxide* - amount of free and bound forms of
    S0<sub>2</sub>; in low concentrations, SO<sub>2</sub> is mostly
    undetectable in wine, but at free SO<sub>2</sub> concentrations over
    50 ppm, SO<sub>2</sub> becomes evident in the nose and taste of
    wine.
-   *density* - the density of water is close to that of water depending
    on the percent alcohol and sugar content.
-   *pH* - describes how acidic or basic a wine is on a scale from 0
    (very acidic) to 14 (very basic); most wines are between 3-4 on the
    pH scale.
-   *sulphates* - a wine additive that contributes to SO<sub>2</sub>
    levels.
-   *alcohol* - the relative amount of ethanol present in the wine.

 First, we can load the data set and print its structure using the
`str()` function to get an idea of how the data is formatted.

``` r
wine <- read.csv("dat/winequality-red.csv")

str(wine)
```

    ## 'data.frame':    1599 obs. of  12 variables:
    ##  $ fixed.acidity       : num  7.4 7.8 7.8 11.2 7.4 7.4 7.9 7.3 7.8 7.5 ...
    ##  $ volatile.acidity    : num  0.7 0.88 0.76 0.28 0.7 0.66 0.6 0.65 0.58 0.5 ...
    ##  $ citric.acid         : num  0 0 0.04 0.56 0 0 0.06 0 0.02 0.36 ...
    ##  $ residual.sugar      : num  1.9 2.6 2.3 1.9 1.9 1.8 1.6 1.2 2 6.1 ...
    ##  $ chlorides           : num  0.076 0.098 0.092 0.075 0.076 0.075 0.069 0.065 0.073 0.071 ...
    ##  $ free.sulfur.dioxide : num  11 25 15 17 11 13 15 15 9 17 ...
    ##  $ total.sulfur.dioxide: num  34 67 54 60 34 40 59 21 18 102 ...
    ##  $ density             : num  0.998 0.997 0.997 0.998 0.998 ...
    ##  $ pH                  : num  3.51 3.2 3.26 3.16 3.51 3.51 3.3 3.39 3.36 3.35 ...
    ##  $ sulphates           : num  0.56 0.68 0.65 0.58 0.56 0.56 0.46 0.47 0.57 0.8 ...
    ##  $ alcohol             : num  9.4 9.8 9.8 9.8 9.4 9.4 9.4 10 9.5 10.5 ...
    ##  $ quality             : int  5 5 5 6 5 5 5 7 7 5 ...

 From the output we can see that each of the predictor variables are
coded as numeric, which is what we want for this data. However, the
*quality* variable is currently coded as an integer. We should change
this variable into an ordered factor using the `ordered()` function. If
we wanted to set the order of the levels manually we can use the
`levels` option, however since *quality* is given numerically the
function will automatically order the levels from lowest to highest for
us. We can see this by again using the `str()` function on just the
*quality* variable after setting it to an ordered factor.

``` r
wine$quality <- ordered(wine$quality)

wine$alcohol <- ifelse(wine$alcohol < 9, "<9%",
                       ifelse(9 <= wine$alcohol & wine$alcohol < 11, "9-11%",
                              ifelse(11 <= wine$alcohol & wine$alcohol < 13, "11-13%", ">13%")))
                       
wine$alcohol <- ordered(wine$alcohol,
                        levels = c("<9%", "9-11%","11-13%", ">13%"))
str(wine$quality)
```

    ##  Ord.factor w/ 6 levels "3"<"4"<"5"<"6"<..: 3 3 3 4 3 3 3 5 5 3 ...

 Now the *quality* variable is an ordered factor (`Ord.factor`) with 6
levels in order from 3 to 8 (although the quality scale is from 0 to 10
we only have observations from 3 to 8 on that scale).

 Now that our data are in the correct format we can fit an OLR model.
First, we will want to load the `MASS` library which provides functions
for fitting and assessing OLR models. With the `MASS` library loaded we
can fit an OLR model with the `polr()` function. Like other GLMs, we
write the model formula with the response variable on the left
(*quality*) and predictor variables (*.* to include all 11 wine
attributes) on the right of a `~`. We should also set the `Hess` option
to `TRUE` to return the observed information matrix for when we assess
the model using the `summary()` function.

``` r
library(MASS)

fit <- polr(quality ~ .,
            data = wine,
            Hess = TRUE)

summary(fit)
```

    ## Call:
    ## polr(formula = quality ~ ., data = wine, Hess = TRUE)
    ## 
    ## Coefficients:
    ##                           Value Std. Error   t value
    ## fixed.acidity           0.39418   0.050664    7.7803
    ## volatile.acidity       -3.23253   0.395345   -8.1765
    ## citric.acid            -0.64690   0.457262   -1.4147
    ## residual.sugar          0.22463   0.038402    5.8495
    ## chlorides              -5.71084   1.332156   -4.2869
    ## free.sulfur.dioxide     0.01190   0.006758    1.7605
    ## total.sulfur.dioxide   -0.01169   0.002338   -5.0030
    ## density              -357.09515   0.957203 -373.0610
    ## pH                      1.00302   0.487706    2.0566
    ## sulphates               3.41598   0.356290    9.5876
    ## alcohol.L               1.63454   0.603658    2.7077
    ## alcohol.Q              -0.64608   0.447965   -1.4423
    ## alcohol.C              -0.19297   0.214935   -0.8978
    ## 
    ## Intercepts:
    ##     Value     Std. Error t value  
    ## 3|4 -354.9856    0.9879  -359.3215
    ## 4|5 -353.0736    0.9858  -358.1713
    ## 5|6 -349.4379    0.9844  -354.9763
    ## 6|7 -346.6390    0.9864  -351.4283
    ## 7|8 -343.6681    1.0109  -339.9552
    ## 
    ## Residual Deviance: 3129.439 
    ## AIC: 3165.439

 Notice that in the summary statistics for the OLR model we are given
the coefficients (`Value`), standard errors (`Std. Error`) and
t-statstics (`t value`) for each variable and for the intercepts between
each ordinal level. To get p-values we will need to do our own
approximations from the t statistics. We can do this in R through the
following steps:

First we can make an object with all of the coefficients, standard
errors, and t-statistics by using `coef()` on the summary statistics.

``` r
## Get table of coefficients
coef_table <- coef(summary(fit))
```

Next, we estimate the p-values for each coefficient using the `pnorm()`
function, which approximates the p-values from the t-statistics using
the normal distribution function. With this function, we want to take
the absolute values for the t-statistics with the `abs()` function, set
the lower tail to false so that the probabilities are P\[X &gt; x\], and
finally multiply the result by 2 for a two-tailed test.

``` r
## Estimate and print p-values
p <- pnorm(abs(coef_table[, "t value"]), lower.tail = FALSE) * 2
```

Finally, we round the p-values with `round()` for simplicity and add
them to the table with `cbind()`, then print the table.

``` r
coef_table <- cbind(coef_table, "p value" = round(p, 4))

coef_table
```

    ##                              Value  Std. Error      t value p value
    ## fixed.acidity           0.39418169 0.050664373    7.7802539  0.0000
    ## volatile.acidity       -3.23253164 0.395345251   -8.1764777  0.0000
    ## citric.acid            -0.64689541 0.457261617   -1.4147162  0.1572
    ## residual.sugar          0.22463301 0.038401889    5.8495301  0.0000
    ## chlorides              -5.71083622 1.332155537   -4.2869140  0.0000
    ## free.sulfur.dioxide     0.01189771 0.006758062    1.7605215  0.0783
    ## total.sulfur.dioxide   -0.01169463 0.002337537   -5.0029691  0.0000
    ## density              -357.09515257 0.957203148 -373.0609884  0.0000
    ## pH                      1.00301995 0.487705589    2.0566095  0.0397
    ## sulphates               3.41598115 0.356290440    9.5876307  0.0000
    ## alcohol.L               1.63454486 0.603658189    2.7077324  0.0068
    ## alcohol.Q              -0.64607822 0.447964743   -1.4422524  0.1492
    ## alcohol.C              -0.19296577 0.214934588   -0.8977883  0.3693
    ## 3|4                  -354.98560111 0.987933219 -359.3214542  0.0000
    ## 4|5                  -353.07357731 0.985767314 -358.1713171  0.0000
    ## 5|6                  -349.43792440 0.984398029 -354.9762537  0.0000
    ## 6|7                  -346.63898806 0.986371913 -351.4282834  0.0000
    ## 7|8                  -343.66812342 1.010921758 -339.9552148  0.0000

 From the p-values we see that only *citric acid* is not significant in
the model, while all 10 other wine attributes and each intercept is
statistically significant with p-values less than 0.05. However, we will
want to assess the model fit before diving into the results and
interpretation of the results.

 To assess the goodness of fit for OLR models it is recommended to run
four tests alongside one another: the Lipsitz goodness of fit test for
OLR models, the Hosmer-Lemeshow goodness of fit test for OLR models,and
the Pulkstenis-Robinson goodness of fit <i>χ<sup>2</sup></i> and
deviance tests for OLR models. We will first use the Lipsitz test using
the `lipsitz.test()` function on our fitted OLR model which bins the
data into equal groups, the number of which can be changed using the `g`
option. It is suggested that the number of bins be at least 6 and less
than the number of observations, *n*, divided by 5 times the number of
predictor variables, *c*, (*n*/5*c*) By default, the number of bins is
set to 10 which is good for our data.

``` r
## Goodness of fit
library(generalhoslem)

lipsitz.test(fit)
```

    ## 
    ##  Lipsitz goodness of fit test for ordinal response models
    ## 
    ## data:  formula:  quality ~ fixed.acidity + volatile.acidity + citric.acid + residual.sugar + formula:      chlorides + free.sulfur.dioxide + total.sulfur.dioxide + formula:      density + pH + sulphates + alcohol
    ## LR statistic = 15.14, df = 9, p-value = 0.08716

 In the results of the Lipsitz goodness of fit test we are given the
likelihood ratio statistic (`LR statistic`), the degrees of freedom for
the test (`df`), and the `p-value`, which is not statistically
significant (greater than 0.05) and therefore suggests that our OLR
model satisfies the proportional odds assumption.

 Next, we will use the `logitgof()` function to use the Hosmer-Lemeshow
goodness of fit test on the observed values (`wine$quality`) and the
expected values fitted by the model which we can get from the `fitted()`
function. Similarly, the Hosmer-Lemeshow test bins the data into groups
for which it is recommended that the number of groups be at least 6 to
avoid too much within group heterogeneity that would reduce the power of
the test, and not so large so that the contingency table be sparsely
populated and not adhere to the <i>χ<sup>2</sup></i> distribution. The
number of bins can be set by the `g` option which is also 10 by default
and good for our data. Additionally, we should set the `ord` option to
`TRUE` so that the ordinal version of the test is used, otherwise the
multinomial version of the test will be used by default.

``` r
logitgof(wine$quality, fitted(fit), ord = TRUE)
```

    ## Warning in logitgof(wine$quality, fitted(fit), ord = TRUE): At least one cell in the expected
    ## frequencies table is < 1. Chi-square approximation may be incorrect.

    ## 
    ##  Hosmer and Lemeshow test (ordinal model)
    ## 
    ## data:  wine$quality, fitted(fit)
    ## X-squared = 88.838, df = 44, p-value = 7.3e-05

 From the results of the Hosmer-Lemeshow goodness of fit test we run
into two issues with our model. First, the resulting p-value is lower
than 0.05 to be statistically significant, indicating that the OLR model
does not fit the data well. Second, we receive a warning message which
tells us `At lesat one cell in the expected frequencies table is <1`.
Because the Hosmer-Lemeshow is using the <i>χ<sup>2</sup></i>
distribution to approximate a p-value it has similar assumptions to
other <i>χ<sup>2</sup></i> distribution-based tests, in particular here
the assumption that there are no expected frequencies that are less than
1. When this assumption does not hold and the expected values are small
the approximations of the p-value may not be correct.

 Because of this, the results of the Hosmer-Lemeshow goodness of fit
test may be inconclusive, so we should try the Pulkstenis-Robinson
goodness of fit <i>χ<sup>2</sup></i> and deviance tests with the
`pulkrob.chisq()` and `pulkrob.deviance()` functions, respectively. For
each function we give the fitted OLR model as the first argument then a
vector of names for the categorical variables in the second arguement.
In this case the *alcohol* variable is the only predictor variable that
is categorical, so we only should include it in the second arguement.

``` r
pulkrob.chisq(fit, c("alcohol"))
```

    ## Warning in pulkrob.chisq(fit, c("alcohol")): At least one cell in the expected frequencies table is
    ## < 1. Chi-square approximation may be incorrect.

    ## 
    ##  Pulkstenis-Robinson chi-squared test
    ## 
    ## data:  formula:  quality ~ fixed.acidity + volatile.acidity + citric.acid + residual.sugar + formula:      chlorides + free.sulfur.dioxide + total.sulfur.dioxide + formula:      density + pH + sulphates + alcohol
    ## X-squared = 108.35, df = 33, p-value = 5.937e-10

``` r
pulkrob.deviance(fit, c("alcohol"))
```

    ## Warning in pulkrob.deviance(fit, c("alcohol")): At least one cell in the expected frequencies table
    ## is < 1. Chi-square approximation may be incorrect.

    ## 
    ##  Pulkstenis-Robinson deviance test
    ## 
    ## data:  formula:  quality ~ fixed.acidity + volatile.acidity + citric.acid + residual.sugar + formula:      chlorides + free.sulfur.dioxide + total.sulfur.dioxide + formula:      density + pH + sulphates + alcohol
    ## Deviance-squared = 75.25, df = 33, p-value = 3.829e-05

 As we see from both Pulkstenis-Robinson tests we run into the same
issue as the Hosmer-Lemeshow test, in that both are statistically
significant with p-values less than 0.05 to indicate that the OLR model
does not fit the data well accompanied by warnings to indicate that the
<i>χ<sup>2</sup></i> distribution-based p-value approximations may be
invalid due to small expected values. While these tests also are
inconclusive because this assumption is violated, the fact that three of
our four goodness of fit tests had issues should give us pause on how
well our OLR model fits the data.

 If we choose to use the OLR model we can interpret each of the
predictor variable coefficients as the log odds of the quality of the
wine to change while all other variables are constant, with negative
values decreasing and positive values increasing the log odds of that
change in quality. Typically the magnitude of the log odds change is not
as of much interest to us as the direction of the change, and in these
instance we can have a simpler interpretation that as the predictor
variable of interest increases then the wine quality is expected to
increase (positive coefficient) or decrease (negative coefficient) when
the other predictor variables remain constant.

 If we instead decide that the OLR model has significant problems with
its fit we could instead choose other options. If we want to stick with
fitting an OLR model then we could possibly use variable selection
methods to determine which variables to include and simplify the model
in hoeps for a better fit. Alternatively, if we are more interested in
classification and prediction we could employ a number of machine
learning methods such as support vector machines or regression trees.
