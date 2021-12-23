
# Fisher’s Exact Test

 With Fisher’s exact test we are testing whether the counts in each row
are dependent on which column they belong (or that the rows and columns
are independent from one another). We can phrase our statistical
hypotheses as follows:

 **Null hypothesis:** The proportions of the two variables are the
equal.
<center>
<i>H<sub>0</sub>: p<sub>1</sub> = p<sub>2</sub></i>
</center>
 <b>Alternative hypothesis:</b> The proportions of the two variables are
not equal.
<center>
<i>H<sub>A</sub>: p<sub>1</sub> ≠ p<sub>2</sub></i>
</center>

<br>

 Before using Fisher’s exact test to evaluate these hypotheses, we
should determine whether the following
[assumptions](https://online.stat.psu.edu/stat504/lesson/3/3.3) are
valid.

-   The total counts are **fixed and not random**
-   **Small sample size**
-   The samples and observations are **independent** of each other
-   The observations are **mutually exclusive** and can be classified
    into only one category
-   The **expected counts is &lt; 5 but &gt; 1** for at least 20% of the
    observations

### Performing Fisher’s exact test

 In this tutorial we will be using data provided by [Freeman and
Campbell from The University of
Sheffield](https://www.sheffield.ac.uk/polopoly_fs/1.43998!/file/tutorial-9-fishers.pdf)
that examined whether intra-muscular magnesium better treated chronic
fatigue syndrome compared to a placebo treatment in 32 patients. The
results of the study are provided in the table below.

<div align="center">

<table>
<tbody>
<tr>
<td>
</td>
<td>
<b>
<center>
Magnesium</b>
</center>
</td>
<td>
<b>
<center>
Placebo</b>
</center>
</td>
</tr>
<tr>
<td>
<b>Felt better</b>
</td>
<td>
12
</td>
<td>
3
</td>
</tr>
<tr>
<td>
<b>Did not feel better</b>
</td>
<td>
3
</td>
<td>
14
</td>
</tr>
</tbody>
</table>

</div>

 For this example we will manually put the data into R rather than
reading it from a file. Using the `data.frame()` function we can build a
data frame object where we input the data from top to bottom row-wise
then left to right column-wise. We can do so by first providing a name
for the column, placing an equal sign, then using `c()` to record the
data row-wise. The `row.names` option can also be used to create a list
of names for the rows in the data frame.

``` r
df <- data.frame(Magnesium = c(12, 3), Placebo = c(3, 14),
                row.names = c("Felt better", "Did not feel better"))

df
```

    ##                     Magnesium Placebo
    ## Felt better                12       3
    ## Did not feel better         3      14

 We can now run the test by wrapping the data frame in the
`fisher.test()` function. Like other functions we can change whether we
want the test to be two- or one-sided through the `alternative` option,
however because the magnesium treatment could have a better, worse, or
no effect we should keep it to the default `two.sided`.

``` r
fisher.test(df)
```

    ## 
    ##  Fisher's Exact Test for Count Data
    ## 
    ## data:  df
    ## p-value = 0.001033
    ## alternative hypothesis: true odds ratio is not equal to 1
    ## 95 percent confidence interval:
    ##    2.507119 159.407427
    ## sample estimates:
    ## odds ratio 
    ##   16.41917

 From the results of the test we can see that with a p-value less than
0.05 (`p-value = 0.001033`) we can reject <i>H<sub>0</i></sub> to
conclude that the rows and columns are not independent. In the context
of our data, we can conclude that patients given the magnesium treatment
overall felt better than those who took the placebo.

### Full code block

``` r
# Create a data frame
df <- data.frame(Magnesium = c(12, 3),Placebo = c(3, 14),
                row.names = c("Felt better", "Did not feel better"))

df

# Run a two-sided Fisher's exact test
fisher.test(df)
```
