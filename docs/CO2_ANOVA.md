
# Analyzing `CO2` with ANOVA

In an ANOVA model each of our predictor variables are of the categorical
type.

A three factor ANOVA with interactions follows the following
formula:

\[{y_{ijkl} = {\mu}_{...} + {\alpha}_i + {\beta}_j + {\gamma}_k + (\alpha\beta)_{ij} + (\alpha\gamma)_{ik} + (\beta\gamma)_{jk} + (\alpha\beta\gamma)_{ijk} + {\epsilon}_{ijkl}}\]

Where:

  - \(y_{ijkl}\) is the response for \(i\), \(j\), and \(k\) factor
    levels and \(l\) observations,
  - \({\mu}_{...}\) is the grand mean,
  - \({\alpha}\), \(\beta\), \(\gamma\) are factors with \(i\), \(j\),
    and \(k\) levels respectively,
  - \((\alpha\beta)\), \((\alpha\gamma)\), \((\beta\gamma)\), and
    \((\alpha\beta\gamma)\) are interactions between the factors, and
  - \(\epsilon\) is the error term for \(i\), \(j\), and \(k\) factor
    levels and \(l\) observations.

In the `CO2` dataset the `conc` variable represents molecular
concentrations of which are continuous, however because 7 distinct
concentrations were used in this study it could be sensible to model
`conc` as a categorical variable with 7 levels. We can easily do this
without making changes to the data by calling `factor()` on the variable
within the model formula. We can then use `summary()`, or alternatively
`anova()`, to print out the ANOVA table for our fitted linear model.

``` r
CO2.aov <- aov(uptake~Type+Treatment+factor(conc), data=CO2)

summary(CO2.aov)
```

    ##              Df Sum Sq Mean Sq F value Pr(>F)    
    ## Type          1   3366    3366  196.50 <2e-16 ***
    ## Treatment     1    988     988   57.69  7e-11 ***
    ## factor(conc)  6   4069     678   39.59 <2e-16 ***
    ## Residuals    75   1285      17                   
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

*Note: Because linear regression and ANOVA are only different in
language but not math, you could also use `aov()` on a previously fit
`lm()` object.*

*Note: I have seen others suggest that both `aov()` and `anova()` fit an
ANOVA model in `R`, but that is not exactly true. While `anova()` prints
an ANOVA table from a fitted model, only `aov()` actually fits an ANOVA
model in base `R`.*

We can see that just like in the [linear regression
model](https://tylerbg.github.io/CDAR/docs/CO2_LR) each of the three
variables are statistically significant.

``` r
CO2.aov <- aov(uptake~Type*Treatment*factor(conc), data=CO2)

summary(CO2.aov)
```

    ##                             Df Sum Sq Mean Sq F value   Pr(>F)    
    ## Type                         1   3366    3366 399.758  < 2e-16 ***
    ## Treatment                    1    988     988 117.368 2.32e-15 ***
    ## factor(conc)                 6   4069     678  80.548  < 2e-16 ***
    ## Type:Treatment               1    226     226  26.812 3.15e-06 ***
    ## Type:factor(conc)            6    374      62   7.412 7.24e-06 ***
    ## Treatment:factor(conc)       6    101      17   1.999   0.0811 .  
    ## Type:Treatment:factor(conc)  6    112      19   2.216   0.0547 .  
    ## Residuals                   56    471       8                     
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

``` r
CO2.aov <- aov(uptake~Type*Treatment+factor(conc)+Type:factor(conc), data=CO2)

summary(CO2.aov)
```

    ##                   Df Sum Sq Mean Sq F value   Pr(>F)    
    ## Type               1   3366    3366  334.39  < 2e-16 ***
    ## Treatment          1    988     988   98.18 7.95e-15 ***
    ## factor(conc)       6   4069     678   67.38  < 2e-16 ***
    ## Type:Treatment     1    226     226   22.43 1.15e-05 ***
    ## Type:factor(conc)  6    374      62    6.20 3.15e-05 ***
    ## Residuals         68    684      10                     
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

``` r
TukeyHSD(CO2.aov)
```

    ##   Tukey multiple comparisons of means
    ##     95% family-wise confidence level
    ## 
    ## Fit: aov(formula = uptake ~ Type * Treatment + factor(conc) + Type:factor(conc), data = CO2)
    ## 
    ## $Type
    ##                         diff       lwr       upr p adj
    ## Mississippi-Quebec -12.65952 -14.04098 -11.27807     0
    ## 
    ## $Treatment
    ##                         diff       lwr       upr p adj
    ## chilled-nonchilled -6.859524 -8.240977 -5.478071     0
    ## 
    ## $`factor(conc)`
    ##                diff        lwr       upr     p adj
    ## 175-95   10.0250000  6.0898540 13.960146 0.0000000
    ## 250-95   16.6166667 12.6815207 20.551813 0.0000000
    ## 350-95   18.4083333 14.4731874 22.343479 0.0000000
    ## 500-95   18.6166667 14.6815207 22.551813 0.0000000
    ## 675-95   19.6916667 15.7565207 23.626813 0.0000000
    ## 1000-95  21.3250000 17.3898540 25.260146 0.0000000
    ## 250-175   6.5916667  2.6565207 10.526813 0.0000606
    ## 350-175   8.3833333  4.4481874 12.318479 0.0000003
    ## 500-175   8.5916667  4.6565207 12.526813 0.0000001
    ## 675-175   9.6666667  5.7315207 13.601813 0.0000000
    ## 1000-175 11.3000000  7.3648540 15.235146 0.0000000
    ## 350-250   1.7916667 -2.1434793  5.726813 0.8086960
    ## 500-250   2.0000000 -1.9351460  5.935146 0.7173440
    ## 675-250   3.0750000 -0.8601460  7.010146 0.2255511
    ## 1000-250  4.7083333  0.7731874  8.643479 0.0092036
    ## 500-350   0.2083333 -3.7268126  4.143479 0.9999984
    ## 675-350   1.2833333 -2.6518126  5.218479 0.9543325
    ## 1000-350  2.9166667 -1.0184793  6.851813 0.2825264
    ## 675-500   1.0750000 -2.8601460  5.010146 0.9809314
    ## 1000-500  2.7083333 -1.2268126  6.643479 0.3696323
    ## 1000-675  1.6333333 -2.3018126  5.568479 0.8669428
    ## 
    ## $`Type:Treatment`
    ##                                                  diff        lwr        upr     p adj
    ## Mississippi:nonchilled-Quebec:nonchilled    -9.380952 -11.959501  -6.802404 0.0000000
    ## Quebec:chilled-Quebec:nonchilled            -3.580952  -6.159501  -1.002404 0.0027416
    ## Mississippi:chilled-Quebec:nonchilled      -19.519048 -22.097596 -16.940499 0.0000000
    ## Quebec:chilled-Mississippi:nonchilled        5.800000   3.221451   8.378549 0.0000007
    ## Mississippi:chilled-Mississippi:nonchilled -10.138095 -12.716644  -7.559547 0.0000000
    ## Mississippi:chilled-Quebec:chilled         -15.938095 -18.516644 -13.359547 0.0000000
    ## 
    ## $`Type:factor(conc)`
    ##                                         diff         lwr        upr     p adj
    ## Mississippi:95-Quebec:95          -3.6166667  -9.9868641   2.753531 0.7772879
    ## Quebec:175-Quebec:95              13.0166667   6.6464693  19.386864 0.0000001
    ## Mississippi:175-Quebec:95          3.4166667  -2.9535307   9.786864 0.8372159
    ## Quebec:250-Quebec:95              21.8666667  15.4964693  28.236864 0.0000000
    ## Mississippi:250-Quebec:95          7.7500000   1.3798026  14.120197 0.0050910
    ## Quebec:350-Quebec:95              24.0166667  17.6464693  30.386864 0.0000000
    ## Mississippi:350-Quebec:95          9.1833333   2.8131359  15.553531 0.0003269
    ## Quebec:500-Quebec:95              24.0666667  17.6964693  30.436864 0.0000000
    ## Mississippi:500-Quebec:95          9.5500000   3.1798026  15.920197 0.0001550
    ## Quebec:675-Quebec:95              25.4333333  19.0631359  31.803531 0.0000000
    ## Mississippi:675-Quebec:95         10.3333333   3.9631359  16.703531 0.0000301
    ## Quebec:1000-Quebec:95             27.9333333  21.5631359  34.303531 0.0000000
    ## Mississippi:1000-Quebec:95        11.1000000   4.7298026  17.470197 0.0000058
    ## Quebec:175-Mississippi:95         16.6333333  10.2631359  23.003531 0.0000000
    ## Mississippi:175-Mississippi:95     7.0333333   0.6631359  13.403531 0.0176004
    ## Quebec:250-Mississippi:95         25.4833333  19.1131359  31.853531 0.0000000
    ## Mississippi:250-Mississippi:95    11.3666667   4.9964693  17.736864 0.0000032
    ## Quebec:350-Mississippi:95         27.6333333  21.2631359  34.003531 0.0000000
    ## Mississippi:350-Mississippi:95    12.8000000   6.4298026  19.170197 0.0000001
    ## Quebec:500-Mississippi:95         27.6833333  21.3131359  34.053531 0.0000000
    ## Mississippi:500-Mississippi:95    13.1666667   6.7964693  19.536864 0.0000001
    ## Quebec:675-Mississippi:95         29.0500000  22.6798026  35.420197 0.0000000
    ## Mississippi:675-Mississippi:95    13.9500000   7.5798026  20.320197 0.0000000
    ## Quebec:1000-Mississippi:95        31.5500000  25.1798026  37.920197 0.0000000
    ## Mississippi:1000-Mississippi:95   14.7166667   8.3464693  21.086864 0.0000000
    ## Mississippi:175-Quebec:175        -9.6000000 -15.9701974  -3.229803 0.0001399
    ## Quebec:250-Quebec:175              8.8500000   2.4798026  15.220197 0.0006354
    ## Mississippi:250-Quebec:175        -5.2666667 -11.6368641   1.103531 0.2141161
    ## Quebec:350-Quebec:175             11.0000000   4.6298026  17.370197 0.0000072
    ## Mississippi:350-Quebec:175        -3.8333333 -10.2035307   2.536864 0.7032540
    ## Quebec:500-Quebec:175             11.0500000   4.6798026  17.420197 0.0000064
    ## Mississippi:500-Quebec:175        -3.4666667  -9.8368641   2.903531 0.8231108
    ## Quebec:675-Quebec:175             12.4166667   6.0464693  18.786864 0.0000003
    ## Mississippi:675-Quebec:175        -2.6833333  -9.0535307   3.686864 0.9693310
    ## Quebec:1000-Quebec:175            14.9166667   8.5464693  21.286864 0.0000000
    ## Mississippi:1000-Quebec:175       -1.9166667  -8.2868641   4.453531 0.9985767
    ## Quebec:250-Mississippi:175        18.4500000  12.0798026  24.820197 0.0000000
    ## Mississippi:250-Mississippi:175    4.3333333  -2.0368641  10.703531 0.5146407
    ## Quebec:350-Mississippi:175        20.6000000  14.2298026  26.970197 0.0000000
    ## Mississippi:350-Mississippi:175    5.7666667  -0.6035307  12.136864 0.1163772
    ## Quebec:500-Mississippi:175        20.6500000  14.2798026  27.020197 0.0000000
    ## Mississippi:500-Mississippi:175    6.1333333  -0.2368641  12.503531 0.0705724
    ## Quebec:675-Mississippi:175        22.0166667  15.6464693  28.386864 0.0000000
    ## Mississippi:675-Mississippi:175    6.9166667   0.5464693  13.286864 0.0213157
    ## Quebec:1000-Mississippi:175       24.5166667  18.1464693  30.886864 0.0000000
    ## Mississippi:1000-Mississippi:175   7.6833333   1.3131359  14.053531 0.0057383
    ## Mississippi:250-Quebec:250       -14.1166667 -20.4868641  -7.746469 0.0000000
    ## Quebec:350-Quebec:250              2.1500000  -4.2201974   8.520197 0.9955994
    ## Mississippi:350-Quebec:250       -12.6833333 -19.0535307  -6.313136 0.0000002
    ## Quebec:500-Quebec:250              2.2000000  -4.1701974   8.570197 0.9945342
    ## Mississippi:500-Quebec:250       -12.3166667 -18.6868641  -5.946469 0.0000004
    ## Quebec:675-Quebec:250              3.5666667  -2.8035307   9.936864 0.7931184
    ## Mississippi:675-Quebec:250       -11.5333333 -17.9035307  -5.163136 0.0000022
    ## Quebec:1000-Quebec:250             6.0666667  -0.3035307  12.436864 0.0775311
    ## Mississippi:1000-Quebec:250      -10.7666667 -17.1368641  -4.396469 0.0000119
    ## Quebec:350-Mississippi:250        16.2666667   9.8964693  22.636864 0.0000000
    ## Mississippi:350-Mississippi:250    1.4333333  -4.9368641   7.803531 0.9999378
    ## Quebec:500-Mississippi:250        16.3166667   9.9464693  22.686864 0.0000000
    ## Mississippi:500-Mississippi:250    1.8000000  -4.5701974   8.170197 0.9992541
    ## Quebec:675-Mississippi:250        17.6833333  11.3131359  24.053531 0.0000000
    ## Mississippi:675-Mississippi:250    2.5833333  -3.7868641   8.953531 0.9774535
    ## Quebec:1000-Mississippi:250       20.1833333  13.8131359  26.553531 0.0000000
    ## Mississippi:1000-Mississippi:250   3.3500000  -3.0201974   9.720197 0.8550458
    ## Mississippi:350-Quebec:350       -14.8333333 -21.2035307  -8.463136 0.0000000
    ## Quebec:500-Quebec:350              0.0500000  -6.3201974   6.420197 1.0000000
    ## Mississippi:500-Quebec:350       -14.4666667 -20.8368641  -8.096469 0.0000000
    ## Quebec:675-Quebec:350              1.4166667  -4.9535307   7.786864 0.9999455
    ## Mississippi:675-Quebec:350       -13.6833333 -20.0535307  -7.313136 0.0000000
    ## Quebec:1000-Quebec:350             3.9166667  -2.4535307  10.286864 0.6729046
    ## Mississippi:1000-Quebec:350      -12.9166667 -19.2868641  -6.546469 0.0000001
    ## Quebec:500-Mississippi:350        14.8833333   8.5131359  21.253531 0.0000000
    ## Mississippi:500-Mississippi:350    0.3666667  -6.0035307   6.736864 1.0000000
    ## Quebec:675-Mississippi:350        16.2500000   9.8798026  22.620197 0.0000000
    ## Mississippi:675-Mississippi:350    1.1500000  -5.2201974   7.520197 0.9999952
    ## Quebec:1000-Mississippi:350       18.7500000  12.3798026  25.120197 0.0000000
    ## Mississippi:1000-Mississippi:350   1.9166667  -4.4535307   8.286864 0.9985767
    ## Mississippi:500-Quebec:500       -14.5166667 -20.8868641  -8.146469 0.0000000
    ## Quebec:675-Quebec:500              1.3666667  -5.0035307   7.736864 0.9999638
    ## Mississippi:675-Quebec:500       -13.7333333 -20.1035307  -7.363136 0.0000000
    ## Quebec:1000-Quebec:500             3.8666667  -2.5035307  10.236864 0.6912149
    ## Mississippi:1000-Quebec:500      -12.9666667 -19.3368641  -6.596469 0.0000001
    ## Quebec:675-Mississippi:500        15.8833333   9.5131359  22.253531 0.0000000
    ## Mississippi:675-Mississippi:500    0.7833333  -5.5868641   7.153531 1.0000000
    ## Quebec:1000-Mississippi:500       18.3833333  12.0131359  24.753531 0.0000000
    ## Mississippi:1000-Mississippi:500   1.5500000  -4.8201974   7.920197 0.9998507
    ## Mississippi:675-Quebec:675       -15.1000000 -21.4701974  -8.729803 0.0000000
    ## Quebec:1000-Quebec:675             2.5000000  -3.8701974   8.870197 0.9828658
    ## Mississippi:1000-Quebec:675      -14.3333333 -20.7035307  -7.963136 0.0000000
    ## Quebec:1000-Mississippi:675       17.6000000  11.2298026  23.970197 0.0000000
    ## Mississippi:1000-Mississippi:675   0.7666667  -5.6035307   7.136864 1.0000000
    ## Mississippi:1000-Quebec:1000     -16.8333333 -23.2035307 -10.463136 0.0000000

ANCOVA

``` r
CO2.ancova <- aov(uptake~Type*Treatment*conc, data=CO2)

summary(CO2.ancova)
```

    ##                     Df Sum Sq Mean Sq F value   Pr(>F)    
    ## Type                 1   3366    3366 100.416 1.52e-15 ***
    ## Treatment            1    988     988  29.482 6.51e-07 ***
    ## conc                 1   2285    2285  68.177 3.55e-12 ***
    ## Type:Treatment       1    226     226   6.735   0.0113 *  
    ## Type:conc            1    208     208   6.206   0.0149 *  
    ## Treatment:conc       1     32      32   0.951   0.3326    
    ## Type:Treatment:conc  1     56      56   1.657   0.2019    
    ## Residuals           76   2547      34                     
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

``` r
#!if(require(car))(install.packages("car"))(library(car))

library(car)
```

    ## Loading required package: carData

``` r
Anova(CO2.ancova, type="3")
```

    ## Anova Table (Type III tests)
    ## 
    ## Response: uptake
    ##                     Sum Sq Df  F value    Pr(>F)    
    ## (Intercept)         4313.5  1 128.7005 < 2.2e-16 ***
    ## Type                 167.6  1   5.0000   0.02828 *  
    ## Treatment             57.1  1   1.7045   0.19564    
    ## conc                 912.6  1  27.2275  1.52e-06 ***
    ## Type:Treatment         5.0  1   0.1501   0.69952    
    ## Type:conc             24.3  1   0.7247   0.39727    
    ## Treatment:conc         1.6  1   0.0487   0.82593    
    ## Type:Treatment:conc   55.5  1   1.6570   0.20192    
    ## Residuals           2547.2 76                       
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

``` r
CO2.ancova <- aov(uptake~Type+Treatment+conc, data=CO2)

Anova(CO2.ancova, type="2")
```

    ## Anova Table (Type II tests)
    ## 
    ## Response: uptake
    ##           Sum Sq Df F value    Pr(>F)    
    ## Type      3365.5  1  87.749 1.668e-14 ***
    ## Treatment  988.1  1  25.763 2.459e-06 ***
    ## conc      2285.0  1  59.576 2.872e-11 ***
    ## Residuals 3068.3 80                      
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

### Other Resources

[STHDA: One-Way ANOVA Test in
R](http://www.sthda.com/english/wiki/one-way-anova-test-in-r)

[Scribbr: ANOVA in R: A step-by-step
guide](https://www.scribbr.com/statistics/anova-in-r/)

[Data Analysis in R: Understanding ANOVA in
R](https://bookdown.org/steve_midway/DAR/understanding-anova-in-r.html)

[Datanovia: ANOVA in
R](https://www.datanovia.com/en/lessons/anova-in-r/)

[Applied Statistics with R: Analysis of
Variance](https://daviddalpiaz.github.io/appliedstats/analysis-of-variance.html)
