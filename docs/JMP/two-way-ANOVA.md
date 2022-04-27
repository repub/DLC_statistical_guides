
# Two-way Analysis of Variance (ANOVA<sup>2</sup>)

 For this example we will use [this data
set](https://github.com/tylerbg/DLC_stat_resources/tree/master/docs/JMP/dat/stress_test.jmp)
originally from [STAT 414 Introduction to Mathematical
Statistics](https://online.stat.psu.edu/stat415/lesson/14/14.1) where a
physiologist wanted to determine if smoking history affected how a
subject responds to different types of stress tests. The time to maximum
oxygen uptake (VO<sub>2max</sub>) was measured from subjects belonging
to three different categories of smoking history (*Nonsmoker*,
*Moderate*, and *Heavy*) while performing three different stress tests
(*Bicycle*, *Step Test*, and *Treadmill*).

<center>
<img src="img/two-way-ANOVA/smoking_test_boxplot.png" style="display: block; margin: auto;" />
</center>

<br>

1.  Does smoking history affect the time to VO<sub>2max</sub>?
    <center>
    <i>H<sub>0</sub></i>:<i> α<sub>Nonsmoker</sub> =
    α<sub>Moderate</sub> = α<sub>Heavy</sub> = 0</i>
    </center>
    <center>
    <i>H<sub>A</sub></i>: at least one <i>α<sub>i</sub> ≠ 0</i>
    </center>
2.  Are there differences between the types of stress tests and the time
    to reach VO<sub>2max</sub>?
    <center>
    <i>H<sub>0</sub></i>:<i> β<sub>Bicycle</sub> = β<sub>Step Test</sub>
    = β<sub>Treadmill</sub> = 0</i>
    </center>
    <center>
    <i>H<sub>A</sub></i>: at least one <i>β<sub>j</sub> ≠ 0</i>
    </center>
3.  Is there an interaction between the three types of stress tests and
    smoking history?
    <center>
    <i>H<sub>0</sub></i>: <i>αβ<sub>ij</sub> = 0</i>
    </center>
    <center>
    <i>H<sub>A</sub></i>: <i>αβ<sub>ij</sub> ≠ 0</i>
    </center>

### Fit a two-way ANOVA

 After loading our data set into JMP we can fit a two-way ANOVA by
selecting *Analyze* -&gt; *Fit Model* to bring up a new window where we
will specify the parameters of our model. First, we put the **Time**
variable into the *Y* box, then we add the **Smoking History** and
**Test** Variables to the *Construct Model Effects* box. Since we also
want to include the interaction term between **Smoking History** and
**Test** we should also highlight both variables in the *Select Columns*
box and then select *Cross* in the *Construct Model Effects* box. We can
keep all of the other options at their defaults and select *Run* to fit
our model.

<center>
<img src="img/two-way-ANOVA/smoking_test_ANOVA.png" style="display: block; margin: auto;" />
</center

&emsp;From the output we can see in the *Analysis of Variance* table that the model overall has a statistically significant fit, which in the *Effect Summary* table we see that the **Test** and **Smoking History** variables are significant while their interaction term is not.  Many of the other information given to us is summary statistics and model diagnostics to help us assess whether the two-way ANOVA meets our assumptions and is actually a good fit.  For example, in the *Residual by Predicted Plot* we see the residuals (errors) for time on the y-axis against the respective predicted values of time for each observation on the x-axis.  The important thing to note from this plot is that there are no obvious outliers or patterns and that the residuals randomly spread along the horizontal blue line at 0, satisfying some of our assumptions for the two-way ANOVA.

&emsp;While the ANOVA model indicates that at least one of the group means in the **Time** and **Smoking History** variables are significantly different from at least one of the other group means, we are not told which ones.  For this, we need to follow-up with a 'post-hoc' pairwise comparisons test, for which we will use the Tukey's Honestly Significant Difference (HSD) test.  By clicking the red arrow next to *Response Time* we can then select *Estimates* ->

*Multiple Comparisons*. In the pop-out window we can select the variable
we want to run post-hoc tests for in the *Choose an Effect* box, check
the box next to *All Pairwise Comparisons - Tukey HSD*, then click *OK*
to perform the test.

<center>
<img src="img/two-way-ANOVA/smoking_test_Tukey.png" style="display: block; margin: auto;" />
</center>

 The results from the pairwise Tukey HSD test on the **Test** variable
indicate that the mean time to maximum oxygen uptake is significantly
different between each of the three stress tests, with *Bicycle* with
the shortest time, then the *Treadmill*, and then the *Step Test* with
the highest time. For **Smoking History**, the Tukey HSD results
indicate that there is a significantly lower mean time to maximum oxygen
uptake between *Heavy* and the *Moderate* and *Nonsmoker* groups while
there is not a statistically significant difference between *Moderate*
and *Nonsmoker*. Therefore, we could conclude that heavy smokers reach
their maximum oxygen uptake during high intensity exercise faster than
moderate smokers or non-smokers.
