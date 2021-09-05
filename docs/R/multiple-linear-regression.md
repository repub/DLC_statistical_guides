
# Multiple Linear Regression (MLR)

### [MLR hypotheses](https://online.stat.psu.edu/stat501/lesson/5/5.3)

    **Null hypothesis:** The estimated coefficient (*β*) for variable
*i* is equal to 0.  
<center>
<i>H<sub>0</sub>: β<sub>i</sub> = 0</i>
</center>

 

    **Alternative hypothesis:** *β*<sub>*i*</sub> is not equal to 0.  
<center>
<i>H<sub>A</sub>: β<sub>i</sub> ≠ 0, </i>or<i> H<sub>A</sub>:
β<sub>i</sub> &gt; 0 or β<sub>i</sub> &lt; 0</i>
</center>

 

### [MLR assumptions](https://online.stat.psu.edu/stat501/lesson/7/7.3)

-   The predictor and response variables have a **linear relationship**.
-   The errors are **independent**, **normally distributed**, and have
    **equal variances**.

### Loading the data in R

    In this example we will be using a dataset provided in [Example 5-1
of the STAT 501 Regression Methods
course](https://online.stat.psu.edu/stat501/lesson/5/5.1) that includes
data on participant’s performance IQ score, brain size, and body size in
height & weight.

``` r
iqsize <- read.table("dat/iqsize.txt", header = TRUE)
```

    Now that the dataset is loaded, you can view a brief summary of the
dataset using the `summary()` function on the assigned object.

### Fitting a MLR model

    To fit a linear regression model we can use the `lm()` function in R
where the response variable *PIQ* is to the left of of the `~` and the
predictor variables brain size (*Brain*), *Height*, and *Weight* are to
the right and separated by `+` as follows:

``` r
fit.lm <- lm(PIQ ~ Brain + Height + Weight, data = iqsize)
```

### Interpreting the results

    Using the `summary()` function we can print out summary statistics
of our MLR model.

``` r
summary(fit.lm)
```

    ## 
    ## Call:
    ## lm(formula = PIQ ~ Brain + Height + Weight, data = iqsize)
    ## 
    ## Residuals:
    ##    Min     1Q Median     3Q    Max 
    ## -32.74 -12.09  -3.84  14.17  51.69 
    ## 
    ## Coefficients:
    ##               Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  1.114e+02  6.297e+01   1.768 0.085979 .  
    ## Brain        2.060e+00  5.634e-01   3.657 0.000856 ***
    ## Height      -2.732e+00  1.229e+00  -2.222 0.033034 *  
    ## Weight       5.599e-04  1.971e-01   0.003 0.997750    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 19.79 on 34 degrees of freedom
    ## Multiple R-squared:  0.2949, Adjusted R-squared:  0.2327 
    ## F-statistic: 4.741 on 3 and 34 DF,  p-value: 0.007215

    From the summary statistics we can fist note that *PIQ* can be
predicted by the variables included in our model as the p-value for the
MLR model is below 0.05, although the multiple r<sup>2</sup> is \~3 to
say that relationship is weak. Further, the *Brain* and *Height*
variables are statistically significant predictors of *PIQ* given by the
p-values for their coefficients (`Pr(>|t|)` column) being below 0.05.
Conversely, the *Weight* variable is no statistically significant and
even close to 1, so we could conclude that the weight of the participant
has no relationship with their performance IQ.

### Full code block

``` r
# Load the dataset
iqsize <- read.table("dat/iqsize.txt", header = TRUE)

# Fit a linear model
fit.lm <- lm(PIQ ~ Brain + Height + Weight, data = iqsize)

# View the results of the linear model
summary(fit.lm)
```
