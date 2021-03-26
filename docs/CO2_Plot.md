CO2 plotting
================
tylerbg

# Plotting CO2

To see how CO2 uptake rates may change by the concentration of CO2 we can create a basic ggplot object then use `geom_point()` to define the layout of our ggplot layer.

``` r
library(ggplot2)

CO2.plot <- ggplot(CO2, aes(x=conc, y=uptake))

CO2.plot + geom_point()
```

<img src="/home/tyler/CDAR/docs/CO2_Plot_files/figure-markdown_github/geom_point-1.png" style="display: block; margin: auto auto auto 0;" />

![image1](./CO2_Plot_files/figure-markdown_github/geom_point-1.png)

Because the CO2 concentrations are pre-defined and not continuous, we could also display a boxplot to better see the distribution of our data. To do so, we can use the same ggplot object with `geom_boxplot()` and set `group` to the seven CO2 concentrations in our dataset.

``` r
CO2.plot + geom_boxplot(aes(group=conc))
```

<img src="/home/tyler/CDAR/docs/CO2_Plot_files/figure-markdown_github/geom_boxplot-1.png" style="display: block; margin: auto auto auto 0;" />

Overall, there appears to be some increasing trend in CO2 uptake as CO2 concentration increases. However, we have other factors that are so far not included in our plot that we should take into account, namely the origin of the plant and their treatment.

We can start by going back to our scatterplot and using different colors and symbols to represent our two factors.

``` r
CO2.plot <- CO2.plot + geom_point(aes(color=Treatment, pch=Type))

CO2.plot
```

<img src="/home/tyler/CDAR/docs/CO2_Plot_files/figure-markdown_github/geom_point_colors-1.png" style="display: block; margin: auto auto auto 0;" />

*Note: When you assign an object in R you can surround the line with parenthesis* `()` *to also print the object rather than calling it in a new line. This will be done in the following code blocks.*

Next, to better see the trends in our data it can be useful to display lines for each of our groups. However, we might prefer the lines to go through our group means rather than each individual data point. One way to do this is to use `stat_summary` to calculate the means of each group.

``` r
(CO2.plot <- CO2.plot + stat_summary(fun=mean, geom="line",
                                    aes(group=interaction(Treatment, Type),
                                        color=Treatment, lty=Type)))
```

<img src="/home/tyler/CDAR/docs/CO2_Plot_files/figure-markdown_github/geom_point_lines-1.png" style="display: block; margin: auto auto auto 0;" />

By setting `group` to the interaction of *Treatment* and *Type*, `stat_summary()` knows to calculate the mean of each of our four groups. We further identify *Treatment* to be represented by color and *Type* by the type of line (either solid or dashed by default).

It will be clearer if we give our plot proper titles.

``` r
(CO2.plot <- CO2.plot +
  labs(title="Cold Tolerance of Echinochloa crus-galli",
       x="CO2 Concentration (mL/L)",
       y="CO2 Uptake Rate (umol/m^2 sec)"))
```

<img src="/home/tyler/CDAR/docs/CO2_Plot_files/figure-markdown_github/geom_point_labs-1.png" style="display: block; margin: auto auto auto 0;" />

`labs()` is a simple way to add titles to our plot.

However, it would be better if we use some simple formatting changes.

``` r
(CO2.plot <- CO2.plot +
  labs(title=bquote("Cold Tolerance of"~italic("Echinochloa crus-galli")),
       x=bquote("CO"[2]~"Concentration (mL/L)"),
       y=bquote("CO"[2]~"Uptake Rate ("*mu*"mol/"*m^2*"s)")))
```

<img src="/home/tyler/CDAR/docs/CO2_Plot_files/figure-markdown_github/geom_point_labs2-1.png" style="display: block; margin: auto auto auto 0;" />

As you may notice, our previous labels were replaced by similar text but with updated formatting. By using `bquote()` we can add simple formatting changes such as superscripts (`^`), subscripts (`[]`), and italics (`italic()`) among many other formatting options. `expression()` is an alternative method to formatting text that is popular as well.

We can further refine our plot by changing the size, transparency, and outline of our labels, in addition to selecting a more appealing color scheme and theme.

``` r
(CO2.plot <- CO2.plot +
  geom_point(aes(color=Treatment, pch=Type), cex=2.5, alpha=0.75)) +
  geom_point(pch=c(1,2)[CO2$Type], color="black", cex=2.5, alpha=0.75) +
  scale_color_brewer(palette="Paired") +
  theme_bw()
```

<img src="/home/tyler/CDAR/docs/CO2_Plot_files/figure-markdown_github/geom_point_scheme-1.png" style="display: block; margin: auto auto auto 0;" />

The first `geom_point()` is the same as above except we add `cex` to increase the size of our points and `alpha` to make them slightly transparent to better see points that overlap. In the second `geom_point()` command we draw black outlines for our points by changing the shapes of our points to unfilled circles and triangles using `pch`. In `scale_color_brewer()` we use a preset color scheme to change the colors that represent *Treatment* to two shades of blue. Finally, `theme_bw()` sets the plotting theme to one of the default themes provided by `ggplot2`.

``` r
ggplot(CO2, aes(x=factor(conc), y=uptake)) +
  geom_point(aes(color=Treatment, pch=Type), cex=2.5, alpha=0.75) +
  geom_point(pch=c(1,2)[CO2$Type], color="black", cex=2.5, alpha=0.75) +
  stat_summary(fun=mean, geom="line", aes(group=interaction(Treatment, Type),
                                          color=Treatment, lty=Type)) +
  scale_color_brewer(palette="Paired") +
  labs(x=bquote("CO"[2]~"Concentration (mL/L)"),
       y=bquote("CO"[2]~"Uptake Rate ("*mu*"mol/"*m^2*"s)")) +
  ggtitle(bquote("Cold Tolerance of"~italic("Echinochloa crus-galli"))) +
  theme_bw() +
  theme(legend.position="bottom")
```

<img src="/home/tyler/CDAR/docs/CO2_Plot_files/figure-markdown_github/geom_point_full-1.png" style="display: block; margin: auto auto auto 0;" />

Alternatively, we could use boxplots to display the centrality and distribution of our data. An example of some code to do this that keeps most of the same formatting as our prevoius plots is given below.

``` r
ggplot(CO2, aes(x=factor(conc), y=uptake)) +
  geom_boxplot(aes(color=Type, fill=Treatment)) +
  scale_fill_brewer(palette="Paired") +
  scale_color_manual(values=c("goldenrod", "goldenrod4")) +
  labs(title=bquote("Cold Tolerance of"~italic("Echinochloa crus-galli")),
       x=bquote("CO"[2]~"Concentration (mL/L)"),
       y=bquote("CO"[2]~"Uptake Rate ("*mu*"mol/"*m^2*"s)")) +
  theme_bw() +
  theme(legend.position="bottom")
```

<img src="/home/tyler/CDAR/docs/CO2_Plot_files/figure-markdown_github/geom_boxplot_full-1.png" style="display: block; margin: auto auto auto 0;" />

*Note: the conc varaible is numeric and not a factor, which can cause some issues when trying to plot with* `geom_boxplot()`*. An easy way to fix this without altering our dataframe is to use* `factor()` *to read conc as a factor while plotting.*

### Other Resources

[STHDA: ggplot2 colors](http://www.sthda.com/english/wiki/ggplot2-colors-how-to-change-colors-automatically-and-manually)

[STHDA: ggplot2 box plot](http://www.sthda.com/english/wiki/ggplot2-box-plot-quick-start-guide-r-software-and-data-visualization)

[STHDA: ggplot2 scatter plot](http://www.sthda.com/english/wiki/ggplot2-scatter-plots-quick-start-guide-r-software-and-data-visualization)
