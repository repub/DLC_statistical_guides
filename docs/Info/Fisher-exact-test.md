
# Fisher’s Exact Test

 With the Fisher’s exact test we can statistically determine whether
there are nonrandom associations between two categorical variables,
typically when we have a small sample size. The test is commonly applied
to 2x2 matrices (known as contengency tables), where it is assumed that
the counts in each row are dependent on which column they belong (or
that the rows and columns are independent from one another). For
example, we may be interested in whether there are differences in the
number of males and females in math and engineering majors, or whether
Americans and Canadians prefer cats or dogs as pets. We can phrase the
statistical hypotheses for the Fisher’s exact test as follows:

 **Null hypothesis:** The proportions of the two variables are the
equal.
<center>
<i>H<sub>0</sub>: p<sub>1</sub> = p<sub>2</sub></i>
</center>

<br>

 <b>Alternative hypothesis:</b> The proportions of the two variables are
not equal. <br>
<center>
<i>H<sub>A</sub>: p<sub>1</sub> ≠ p<sub>2</sub></i>
</center>

<br>

 Before using Fisher’s exact test to evaluate these hypotheses, we
should determine whether the following assumptions are valid.

-   The total counts are **fixed and not random**
-   **Small sample size**
-   The samples and observations are **independent** of each other
-   The observations are **mutually exclusive** and can be classified
    into only one category
-   The **expected counts is &lt; 5 but &gt; 1** for at least 20% of the
    observations (cells in the contingency table)

 While larger contengency tables (2x3, 2x4, etc.) and sample sizes can
be used with the Fisher’s exact test, as either increases so does the
computation time. This is because unlike some other statistical tests,
Fisher’s exact test calculates the exact p-value. Therefore, for larger
sample sizes (&gt;1000) or when the expected counts are &gt; 5 for at
least 80% of the cells then a <i>χ</i><sup>2</sup> test may be
preferred.

 After performing the Fisher’s exact test, with an exact p-value less
than the chosen statistical threshold (typically α = 0.05) the null
hypothesis may be rejected to conclude that there is a statistically
significant difference between the two categorical variables.
