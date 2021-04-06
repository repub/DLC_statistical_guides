
# EDA with `ToothGrowth`

*y*<sub>*ijk*</sub> = *μ*<sub>..</sub> + *α*<sub>*i*</sub> + *β*<sub>*j*</sub> + (*α**β*)<sub>*ij*</sub> + *ϵ*<sub>*ijk*</sub>
 Where: - *y* is the response with *i* and *j* factor levels and *k* observations, - *μ* is the grand mean, - *α* and *β* are factors with *i* and *j* levels respectively, - (*α**β*) is the interaction term for *i* and *j* levels, and - *ϵ* is the error term with *i* and *j* factor levels and *k* observations.

*len*<sub>*ijk*</sub> = *μ*<sub>..</sub> + *supp*<sub>*i*</sub> + *dose*<sub>*j*</sub> + (*supp* \* *dose*)<sub>*ij*</sub> + *ϵ*<sub>*ijk*</sub>

``` r
tg.aov <- aov(len~supp*factor(dose), data=ToothGrowth)

summary(tg.aov)
```

    ##                   Df Sum Sq Mean Sq F value   Pr(>F)    
    ## supp               1  205.4   205.4  15.572 0.000231 ***
    ## factor(dose)       2 2426.4  1213.2  92.000  < 2e-16 ***
    ## supp:factor(dose)  2  108.3    54.2   4.107 0.021860 *  
    ## Residuals         54  712.1    13.2                     
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

### Other Resources
