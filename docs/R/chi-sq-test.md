
# The Chi-square (χ<sup>2</sup>) Test

 When our data includes two categorical variables, we can determine if
there is a statistically significant relationship (or alternatively
dependence) between them with the χ<sup>2</sup> test. WIth the
χ<sup>2</sup> test, we compare the observed counts (our data) with the
expected counts if the data came from the same distributions.

 For this tutorial we will use data from an example offered in [STAT 500
Applied Statistics](https://online.stat.psu.edu/stat500/lesson/8) where
participants were asked to give their party affiliation (*Democrat* or
*Republican*) and their opinion on a tax reform bill (*Favor*,
*Indifferent*, or *Opposed*).

<div align="center">

<table>
<tbody>
<tr>
<td>
</td>
<td>
<b>Favor</b>
</td>
<td>
<b>Indifferent</b>
</td>
<td>
<b>Opposed</b>
</td>
</tr>
<tr>
<td>
<b>Democrat</b>
</td>
<td>
138
</td>
<td>
83
</td>
<td>
64
</td>
</tr>
<tr>
<td>
<b>Republican</b>
</td>
<td>
64
</td>
<td>
67
</td>
<td>
84
</td>
</tr>
</tbody>
</table>

</div>

 The researcher wants to know whether a relationship exists between
party affiliation and opinion which corresponds to the following
statistical hypotheses:

<center>
<i>H<sub>0</sub></i>: No relationship exists between party affiliation
and opinion on the tax reform bill
</center>
<center>
<i>H<sub>A</sub></i>: There is a significant relationship between party
affiliation and opinion on the tax reform bill
</center>

### [Assumptions and considerations](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3900058/)

 Before using the χ<sup>2</sup> test to test association (or
independence) we should make sure we meet the following conditions:

-   The data is in non-transformed frequencies or counts
-   Each level is mutually exclusive
-   Each subject or observation can only contribute data once
-   The expected value should be 5 or more for at least 80% of the cells
    and none should be &lt; 1
-   No cells in the observations table should be 0

 When either of the last two conditions are not satisfied then you may
consider a Fisher-exact test.

### Run the χ<sup>2</sup> test

 Using the `data.frame()` function we can easily code the data from the
above table into R going from top to bottom row-wise then left to right
column-wise. By putting the variable name before the `=` in each line we
can set the desired names for each column then set the `row.names`
option to add the desired names to the rows.

``` r
opinion <- data.frame(Favor = c(138, 64),
                      Indifferent = c(83, 67),
                      Opposed = c(64, 84),
                      row.names = c("Democrat", "Republican"))

opinion
```

    ##            Favor Indifferent Opposed
    ## Democrat     138          83      64
    ## Republican    64          67      84

 Now that we have our data in a table we can run the χ<sup>2</sup> test
by simply using the `chisq.test()` function on our data frame.

``` r
opinion.chisq <- chisq.test(opinion)
opinion.chisq
```

    ## 
    ##  Pearson's Chi-squared test
    ## 
    ## data:  opinion
    ## X-squared = 22.152, df = 2, p-value = 1.548e-05

 The output of the `chisq.test()` function gives us the test statistic
(`X-squared = 22.152`), the degrees of freedom (`df = 2`), and the
p-value associated with the test statistics (`p-value = 1.548e-05`).
Importantly for answering our original question, the p-value is much
less than 0 so that we can conclude that an association does exist
between party affiliation and a person’s opinion on the tax reform bill.

``` r
opinion.chisq$expected
```

    ##             Favor Indifferent Opposed
    ## Democrat   115.14        85.5   84.36
    ## Republican  86.86        64.5   63.64

 Comparing the observed counts in our original table with the expected
counts we can see that respondents who identified as *Republican* had
more *Opposed* responses than expected compared to those who identified
as *Democrat*, who also had more responses for *Favor*. The observed and
expected counts almost match for the *Indifferent* responses from both
parties.
