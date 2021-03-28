
# Analyzing `CO2` with linear regression and ANOVA

To determine which, if any, of the variables have a significant relationship with CO<sub>2</sub> uptake we can use two powerful statistical tools, linear regression and Analysis of Variance (ANOVA).

Although the conc variable has 7 distinct levels, molecular concentrations are continuous and it would be sensible to model conc as a continuous variable instead of a categorical one.

<center>
*y*<sub>*i*</sub> = *β*<sub>0</sub> + *β*<sub>1</sub>*x*<sub>*i*1</sub> + *β*<sub>2</sub>*x*<sub>*i*2</sub> + ... + *β*<sub>*n*</sub>*x*<sub>*i**n*</sub> + *ϵ*<sub>*i*</sub>
</center>
-   *y*<sub>*i*</sub> is the response variable
-   *x*<sub>*i*</sub> are our predictor variables
-   *β* are coefficients for the predictor variables and the intercept
-   *ϵ*<sub>*i*</sub> is the error term

In our case, our equation will look like:

*y*<sub>*i*, *U**p**t**a**k**e*</sub> = *β*<sub>0</sub> + *β*<sub>1</sub>*x*<sub>*i*, *C**o**n**c**e**n**t**r**a**t**i**o**n*</sub> + *β*<sub>2</sub>*x*<sub>*i*, *T**r**e**a**t**m**e**n**t*</sub> + *β*<sub>3</sub>*x*<sub>*i*, *T**y**p**e*</sub> + *ϵ*<sub>*i*</sub>

When using categorical data to fit a linear regression model we would need to [recode the variables](https://stats.idre.ucla.edu/spss/faq/coding-systems-for-categorical-variables-in-regression-analysis/) to numerical values so that they may be entered into the regression equation. Thankfully, `R` automatically dummy codes factors when they are added to regression model, so we can go ahead and generate and analyze our model using the following code.

``` r
CO2.lm <- lm(uptake~Type+Treatment+conc, data=CO2)

summary(CO2.lm)
```

    ## 
    ## Call:
    ## lm(formula = uptake ~ Type + Treatment + conc, data = CO2)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -17.344  -2.826   1.070   4.137  11.267 
    ## 
    ## Coefficients:
    ##                    Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)       29.259814   1.538928  19.013  < 2e-16 ***
    ## TypeMississippi  -12.659524   1.351439  -9.367 1.67e-14 ***
    ## Treatmentchilled  -6.859524   1.351439  -5.076 2.46e-06 ***
    ## conc               0.017731   0.002297   7.719 2.87e-11 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 6.193 on 80 degrees of freedom
    ## Multiple R-squared:  0.6839, Adjusted R-squared:  0.6721 
    ## F-statistic:  57.7 on 3 and 80 DF,  p-value: < 2.2e-16

Looking at our summary statistics we can see that the linear model has a statistically significant fit (p &lt; 2.2e-16) with a fairly high R<sup>2</sup> (0.6839) and adjusted R<sup>2</sup> (0.6721). From the p-values for our estimated coefficients (`Pr(>|t|)`), we can see that each of our variables are significant predictors of CO<sub>2</sub> uptake. Therefore, we can conclude that our three predictor variables, concentration, Treatment, and Type, are significant predictors of CO<sub>2</sub> uptake. We can then rewrite our regression equation to:

*y*<sub>*i*, *U**p**t**a**k**e*</sub> = 29.260 + 0.018*x*<sub>*i*, *C**o**n**c**e**n**t**r**a**t**i**o**n*</sub> − 6.860*x*<sub>*i*, *T**r**e**a**t**m**e**n**t*</sub> − 12.660*x*<sub>*i*, *T**y**p**e*</sub>

To see how `R` coded the categorical variables we can call `$xlevels` from the `lm()` object we just created.

``` r
CO2.lm$xlevels
```

    ## $Type
    ## [1] "Quebec"      "Mississippi"
    ## 
    ## $Treatment
    ## [1] "nonchilled" "chilled"

From the output we can see that for the Type and Treatment variables "Quebec" and "nonchilled" are first followed by "Mississippi" and "chilled", respectively. Knowing that R uses dummy coding for factors in regression we can conclude that the first levels are coded as 0 and the second as 1 for the two variables, which we can see further when we use `summary()` to see the results of our regression model.

Notice that the Type and Treatment variables are concatenated with "Mississippi" and "chilled", respectively. When Type is "Quebec", the dummy code is 0 and therefor the value will be 0 in the regression equation (*β*<sub>3</sub> \* *x*<sub>*i*, *T**y**p**e*</sub> = −12.659 \* 0 = 0). Conversely, when Type is "Mississippi" which is dummy coded to 1 the value for that variable will equal the estimated coefficient (*β*<sub>3</sub> \* *x*<sub>*i*, *T**y**p**e*</sub> = −12.659 \* 1 = −12.659). If we happened to have a third level in Type, the value for that factor would be double the estimated coefficient, triple for a fourth level, and so forth.

``` r
CO2.aov <- aov(CO2.lm)
```

``` r
anova(CO2.aov)
```

    ## Analysis of Variance Table
    ## 
    ## Response: uptake
    ##           Df Sum Sq Mean Sq F value    Pr(>F)    
    ## Type       1 3365.5  3365.5  87.749 1.668e-14 ***
    ## Treatment  1  988.1   988.1  25.763 2.459e-06 ***
    ## conc       1 2285.0  2285.0  59.576 2.872e-11 ***
    ## Residuals 80 3068.3    38.4                      
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

### Other Resources

[STHDA: One-Way ANOVA Test in R](http://www.sthda.com/english/wiki/one-way-anova-test-in-r)

[Scribbr: ANOVA in R: A step-by-step guide](https://www.scribbr.com/statistics/anova-in-r/)

[Data Analysis in R: Understanding ANOVA in R](https://bookdown.org/steve_midway/DAR/understanding-anova-in-r.html)

[Datanovia: ANOVA in R](https://www.datanovia.com/en/lessons/anova-in-r/)

[Applied Statistics with R: Analysis of Variance](https://daviddalpiaz.github.io/appliedstats/analysis-of-variance.html)

[Penn State: Regression Methods](https://online.stat.psu.edu/stat501/lesson/welcome-stat-501)
