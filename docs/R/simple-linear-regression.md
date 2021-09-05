
# Simple Linear Regression (SLR)

### [SLR hypotheses](https://online.stat.psu.edu/stat501/lesson/1/1.9)

    **Null hypothesis:** The correlation coefficient (*ρ*) for the two
variables is equal to 0.  
<center>
<i>H<sub>0</sub>: ρ = 0</i>
</center>

 

    **Alternative hypothesis:** The correlation between the two
variables is not equal to 0.  
<center>
<i>H<sub>A</sub>: ρ ≠ 0, </i>or<i> H<sub>A</sub>: ρ &gt; 0 or ρ &lt;
0</i>
</center>

 

### [SLR assumptions](https://online.stat.psu.edu/stat501/lesson/4/4.1)

-   The predictor and response variables have a **linear relationship**.
-   The errors are **independent**, **normally distributed**, and have
    **equal variances**.

### Loading the data in R

    In this example we will be using a sample dataset offered in
[Example 1-2 of the STAT 501 Regression Methods
course](https://online.stat.psu.edu/stat501/lesson/1/1.7) where we will
fit a linear model to assess the relationship between a building’s
height and the number of stories that building has. First, we will need
to read the file into R which has a number of functions to do so. In our
case, the dataset is in a tabular format, so we will use the
`read.table()` function to read the dataset into R. Note that our
dataset has header information, so we should include the `header = TRUE`
option.

``` r
bldgstories <- read.table("dat/bldgstories.txt", header = TRUE)
```

    Now that the dataset is loaded, you can view a brief summary of the
dataset using the `summary()` function on the assigned object.

### Fitting a SLR model

    To fit a linear regression model we can use the `lm()` function in R
where the response variable is to the left of, and predictor variable to
the right of a `~` as follows:

``` r
fit.lm <- lm(HGHT ~ STORIES, data = bldgstories)
```

### Interpreting the results

    Using the `summary()` function we can print out summary statistics
of our fitted linear model.

``` r
summary(fit.lm)
```

    ## 
    ## Call:
    ## lm(formula = HGHT ~ STORIES, data = bldgstories)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -156.759  -33.239    5.995   28.450  167.487 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  90.3096    20.9622   4.308 6.44e-05 ***
    ## STORIES      11.2924     0.4844  23.310  < 2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 58.33 on 58 degrees of freedom
    ## Multiple R-squared:  0.9036, Adjusted R-squared:  0.9019 
    ## F-statistic: 543.4 on 1 and 58 DF,  p-value: < 2.2e-16

    From the summary statistics we can fist note that the *STORIES*
variable is statistically significant with a p-value that is well below
0.05, indicating that the number of stories a building has is a
significant predictor of the building’s height. Also, the r<sup>2</sup>
correlation coefficient is very high (0.9036) and the model is
statistically significant with a p-value less than 0.05, together
indicating that there is a very strong linear relationship between these
two variables.

### Full code block

``` r
# Load the dataset
bldgstories <- read.table("dat/bldgstories.txt", header = TRUE)

# Fit a linear model
fit.lm <- lm(HGHT ~ STORIES, data = bldgstories)

# View the results of the linear model
summary(fit.lm)
```
