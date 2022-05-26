
# Multiple Linear Regression (MLR)

 For data with a continuous response variable where we want to know the
significance, magnitude, and direction of more than one predictor
variable the single linear regression (SLR) model can be extended to a
multiple linear regression (MLR) model. While the response variable
should be continuous (or continuous-like) the predictor variables can
vary among many data types. For instance, we may want to predict the
height of trees (continuous) from the amount of water (continuous) and
type of fertilizer (nominal) given. Or, we could study how education
level (ordinal), age (continuous), and gender (binomial) may predict
income (continuous). Like SLR, MLR has the same LINE assumptions:

-   a **Linear relationship** exists between the response and predictor
    variables,
-   and the residuals (errors) are **Independent**, approximately
    **Normally distributed**, and have **Equal variance
    (Homoscedasticity)**

In addition to the LINE assumptions, MLR models assume:

-   there are **no significant outliers** or **high leverage points**,
-   the observations are **randomly selected** from the population,
-   and the independent variables have **little to no
    multicolinearity**.

 The assumption that the observations are randomly selected can be
validated prior to fitting a MLR model, however the remaining
assumptions generally must be assessed after the model has been fit.
Multicolinearity, which is colinearity among the predictor variables,
can be determined by calculating variance inflation factors (VIFs) for
each variable. It is commonly considered that VIF values greater than 5
indicate moderate colinearity that should be explored, while VIFs
greater than 10 indicate a problematic amount of colinearity that may
affect the model and results.

 The LINE assumptions and the assumption of no significant outliers or
high leverage points are typically assessed visually. Unlike the SLR
model which has one predictor, it is difficult to plot multiple
predictors together to assess whether a linear relationship exists, so
is typically assumed in MLR models unless there is reason not to. The
other assumptions can be assessed through diagnostic plots of the
residuals. These types of diagnostic plots can include plotting the
residual errors against the fitted values, normal quantile-quantile
(Q-Q) plots, and Cook’s distance plots (or plotting the residuals
against their leverage).

 The MLR model formula is written as:

<center>
<img src="img/multiple-linear-regression/MLR-full.PNG" style="display: block; margin: auto;" />
</center>

<br>

where <i>y<sub>i</sub></i> is the response for observation <i>i</i>,
<i>ϵ<sub>i</sub></i> is the error term for each observation <i>i</i>,
<i>β<sub>0</sub></i> is the intercept, <i>β<sub>n</sub></i> is the
regression coefficient (slope) for the independent variable <i>n</i>,
and <i>x<sub>in</sub></i> is the value for the independent variable
<i>n</i> at observation <i>i</i>.

 Both the significance of the overall model and of each independent
variable is tested through an Analysis of Variance (ANOVA). The adjusted
Sums of Squares (SS<sub>adj</sub>) is calculated for the whole model and
for each independent variable (which sums to the SS<sub>adj</sub> for
the overall model). The F-statistics are then used to approximate
p-values from the F-distribution. Additionally, the

 **Null hypothesis:** The estimated coefficient <i>β</i> for variable
*i* is equal to 0.  
<center>
<i>H<sub>0</sub></i>:<i> β<sub>i</sub> </i>=<i> 0</i>
</center>

 

 **Alternative hypothesis:** <i>β<sub>i</sub></i> is not equal to 0.  
<center>
<i>H<sub>A</sub></i>:<i> β<sub>i</sub> </i>≠<i> 0</i>, or<i>
H<sub>A</sub></i>:<i> β<sub>i</sub> </i>&gt;<i> 0 </i>or<i>
β<sub>i</sub> </i>&lt;<i> 0</i>
</center>

 
