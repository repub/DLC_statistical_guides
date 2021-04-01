
# Analyzing `CO2` with linear regression

The linear regression equation model has the
equation:

\[{y_i = {\beta}_0 + {\beta}_1 x_{i,1} + {\beta}_2 x_{i,2} + ... + {\beta}_n x_{i,n} + {\epsilon}_i}\]

Where: - \(y_i\) is the response variable with \(i\) observations, -
\(x_i\) are predictor variables for an \(n\) number of predictors and
\(i\) observations, - \({\beta}\) are coefficients for \(n\) predictor
variables and the intercept, \(\beta_0\), - \({\epsilon}_i\) is the
error term for \(i\) observations.

In our case with two categorical and one continuous variable, our
equation will look
like:

\[y_{i, \textit{uptake}} = {\beta}_0 + {\beta}_\textit{Treatment} x_{i,\textit{Treatment}} + {\beta}_\textit{Type} x_{i,\textit{Type}} + {\beta}_\textit{conc} x_{i,\textit{conc}} + {\epsilon}_i\]

When we wish to include categorical data (factors) in a linear
regression model we need to [recode the
variables](https://stats.idre.ucla.edu/spss/faq/coding-systems-for-categorical-variables-in-regression-analysis/)
to numerical values so that they may be entered into the regression
equation and solved for. Conveniently, `R` automatically dummy codes
factors when they are included in a regresson model, so we can go ahead
to fit and analyze the model using the following code.

``` r
# Fit a linear model with uptake as the response variable
CO2.lm <- lm(uptake~Type+Treatment+conc, data=CO2)

# Print summary statistics for the linear model
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

Taking a quick look at the summary statistics we can see that the linear
model has a statistically significant fit overall (`p < 2.2e-16`) with a
reasonably high R<sup>2</sup> (0.6839) and [adjusted
R<sup>2</sup>](https://online.stat.psu.edu/stat501/lesson/10/10.3)
(0.6721). From the p-values for our estimated coefficients (`Pr(>|t|)`),
we can see that each of the estimated coefficients are significant
predictors of CO<sub>2</sub> uptake. Therefore, we can conclude that our
three predictors variables, `conc`, `Treatment`, and `Type`, are
significant predictors of `uptake` and we can insert the coefficients
into the linear regression equation
to:

\[{y_{i, \textit{uptake}} = 29.260 - 12.660 x_{i,\textit{Type}} - 6.860 x_{i,\textit{Treatment}}  + 0.018 x_{i,\textit{conc}}}\]

From these results we can conclude that <b> when `Type` and `Treatment`
remain unchanged, for every one unit increase in `conc`, `uptake`
increases by 0.018 umol/m<sup>2</sup></b>. While it is easy to make
conclusions for continuous variables like `conc`, the categorical
variables `Type` and `Treatment` take slightly more work. First, we need
to see how `R` coded the categorical variables which we can do by
calling `$xlevels` from the `lm()` object we just created.

``` r
CO2.lm$xlevels
```

    ## $Type
    ## [1] "Quebec"      "Mississippi"
    ## 
    ## $Treatment
    ## [1] "nonchilled" "chilled"

From the output we can see that for the Type and Treatment variables
`Quebec` and `nonchilled` are first followed by `Mississippi` and
`chilled`, respectively. Knowing that R uses dummy coding for factors in
regression we can surmise that the first levels are coded as 0 and the
second as 1 for the two variables, which we can confirm when we use
`summary()` to see the results of our regression model.

Notice that the Type and Treatment variables are concatenated with
`Mississippi` and `chilled`, respectively. When Type is `Quebec`, the
dummy code is 0 and therefor the value will be 0 in the regression
equation (\(\beta_3 * x_{i,\textit{Type}} = -12.659 * 0 = 0\)).
Conversely, when Type is `Mississippi` which is dummy coded to 1 the
value for that variable will equal the estimated coefficient
(\(\beta_3 * x_{i,\textit{Type}}=-12.659 * 1 = -12.659\)). If we
happened to have a third level in Type, the value for that factor would
be double the estimated coefficient, triple for a fourth level, and so
forth.

Therefore, we can conclude that when `Type` is `Mississippi`, `uptake`
is reduced by 12.660 umol/m<sup>2</sup> compared to when `Type` is
`Quebec` when the other variables remain constant. Similarly, `chilled`
decreases `uptake` by 6.860 umol/m<sup>2</sup> compared to `nonchilled`
while `Type` and `conc` remain constant.

However, if we take a look back at our plots we may be convinced that
there is an
[interaction](https://online.stat.psu.edu/stat501/lesson/8/8.6) in the
data, particularly between our two categorical variables `Type` and
`Treatment`. Information may be lost by not including this interaction
in our model, so we should fit a new model that includes an interaction
term and assess its significance (or lack thereof). We can include
interaction terms in `R` formulas two ways:

  - Use `*` between variables to include their stadard terms and
    interaction terms (`Type*Treatment)`).
  - Use `+` for standard variable terms and `:` for interaction terms
    (`Type+Treatment+Type:Treatment`).

The `*` is the simplest way to include interaction terms and because
very useful when we have a lot of terms to include where the `+` and `:`
method would take a lot of typing and increase our chance for errors.

Interaction terms are the product of a coefficient and the variables
invovled in the interaction, which would make our regression equation
for modeling CO<sub>2</sub>
uptake:

\[y_{i, \textit{uptake}} = {\beta}_0 + {\beta}_\textit{Type} x_{i,\textit{Type}} + {\beta}_\textit{Treatment} x_{i,\textit{Treatment}} + {\beta}_\textit{conc} x_{i,\textit{conc}} + \beta_\textit{Type*Treatment}x_{i, \textit{Type}}x_{i, \textit{Treatment}} + {\epsilon}_i\]

We code this in `R` and print a summary of the results with:

``` r
CO2.lm <- lm(uptake~Type*Treatment+conc, data=CO2)

summary(CO2.lm)
```

    ## 
    ## Call:
    ## lm(formula = uptake ~ Type * Treatment + conc, data = CO2)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -16.4240  -2.3674   0.7641   3.8749   9.6278 
    ## 
    ## Coefficients:
    ##                                   Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)                      27.620528   1.627945  16.966  < 2e-16 ***
    ## TypeMississippi                  -9.380952   1.851185  -5.068 2.59e-06 ***
    ## Treatmentchilled                 -3.580952   1.851185  -1.934   0.0566 .  
    ## conc                              0.017731   0.002225   7.969 1.00e-11 ***
    ## TypeMississippi:Treatmentchilled -6.557143   2.617972  -2.505   0.0143 *  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 5.999 on 79 degrees of freedom
    ## Multiple R-squared:  0.7072, Adjusted R-squared:  0.6923 
    ## F-statistic: 47.69 on 4 and 79 DF,  p-value: < 2.2e-16

The new model that includes the interaction term is still statistically
significant (`p < 2.2e-16`), however we achieved a slightly higher
R<sup>2</sup> and adjusted R<sup>2</sup> and we should note that the
estimates for the coefficients and their test statistics have changed.
In particular, `Treatment` is no longer statistically significant
although the interaction term between `Type` and `Treatment` is. This
indicates that there is an interaction between these two factors, and
even though `Treatment` is no longer statistically significant
conventionally we should keep it in our model since it has a significant
interaction with another variable.

Our regression equation now has the the
formula:

\[y_{i, \textit{uptake}} = 27.621 - 9.381 x_{i,\textit{Type}} - 3.581 x_{i,\textit{Treatment}} + 0.018 x_{i,\textit{conc}} - 6.557 x_{i, \textit{Type}}x_{i, \textit{Treatment}} + {\epsilon}_i\]

Now that we know that there is an interaction between at least two of
our variables our interpretation for the linear regression model that
predicts `uptake` changes. Specifically, we cannot claim that when our
`Treatment` variable changes from `Quebec` to `Mississippi` the expected
change in `uptake` decreases by 9.381 umol/m<sup>2</sup></b> (the
coefficient for `Treatment` in our model with the interaction) when all
other variables are constant because when the `Treatment` variable
changes so does its respective interaction with `Type`. Instead, the
expected value for `uptake` changes by -9.381 - 6.557 \* `Treatment`
umol/m<sup>2</sup></b>.

From the exploratory plots of our data we may also be interested if
there are any interactions between our two categorical factors (`Type`
and `Treatment`) and `conc`.

*Note: The results from the linear regression model using `lm()` are the
same as those from the ANOVA model using `aov()` for the model without
interactions. However, you might notice that the model with the `Type x
Treatment` interaction term has different statistical results between
the summary for `lm()` and `aov()`. This is because of the types of [sum
of
squares](https://www.r-bloggers.com/2011/03/anova-%E2%80%93-type-iiiiii-ss-explained/)
used.*

[Penn State: Regression
Methods](https://online.stat.psu.edu/stat501/lesson/welcome-stat-501)
