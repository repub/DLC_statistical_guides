<html>

<head>

<meta charset="utf-8" />
<meta name="generator" content="pandoc" />
<meta http-equiv="X-UA-Compatible" content="IE=EDGE" />

<meta name="viewport" content="width=device-width, initial-scale=1" />



<title>MANOVA2.knit</title>

<script>// Pandoc 2.9 adds attributes on both header and div. We remove the former (to
// be compatible with the behavior of Pandoc < 2.8).
document.addEventListener('DOMContentLoaded', function(e) {
  var hs = document.querySelectorAll("div.section[class*='level'] > :first-child");
  var i, h, a;
  for (i = 0; i < hs.length; i++) {
    h = hs[i];
    if (!/^h[1-6]$/i.test(h.tagName)) continue;  // it should be a header h1-h6
    a = h.attributes;
    while (a.length > 0) h.removeAttribute(a[0].name);
  }
});
</script>

<style type="text/css">
  code{white-space: pre-wrap;}
  span.smallcaps{font-variant: small-caps;}
  span.underline{text-decoration: underline;}
  div.column{display: inline-block; vertical-align: top; width: 50%;}
  div.hanging-indent{margin-left: 1.5em; text-indent: -1.5em;}
  ul.task-list{list-style: none;}
    </style>











</head>

<body>







<!-- TABLE OF CONTENTS -->
<aside>
<div id="toc_container">
<p class="toc_title">
Contents
</p>
<ul class="toc_list">
<a href="#manova">MANOVA</a><br>
<a href="#checking-assumptions">Checking assumptions</a><br>
<a href="#sample-size"> Sample size</a><br>
<a href="#outliers"> Outliers</a><br>
<a href="#multivariate-outliers">  Multivariate outliers</a><br>
<a href="#univariate-outliers">  Univariate outliers</a><br>
<a href="#normality"> Normality</a><br>
<a href="#multivariate-normality-test">  Multivariate normality
test</a><br> <a href="#univariate-normality-test">  Univariate normality
test</a><br> <a href="#correlations"> Correlations</a><br>
<a href="#heteroskedasticity"> Heteroskedasticity</a><br>
<a href="#one-way-manova">One-way MANOVA</a><br>
<a href="#post-hoc-anova"> Post-hoc ANOVA</a><br>
<a href="#post-hoc-pairwise-comparisons"> Pos-hoc pairwise
comparisons</a><br> <a href="#two-way-manova">Two-way MANOVA</a><br>
<a href="#post-hoc-anova2"> Post-hoc ANOVA</a><br>
<a href="#post-hoc-pairwise-comparisons2"> Pos-hoc pairwise
comparisons</a><br>
</ul>
</div>
</aside>
<div id="manova" class="section level1">
<h1>MANOVA</h1>
<p> For this tutorial we will employ functions from three different
packages:</p>
<ul>
<li><code>car</code> (Companion to Applied Regression) that includes
additional statistical tests,</li>
<li><code>rstatix</code> which provides pipe-friendly statistical tests,
and</li>
<li><code>tidyverse</code> that contains multiple packages for data
manipulation, visualization, and analysis.</li>
</ul>
<pre class="r"><code>library(car)
library(rstatix)
library(tidyverse)</code></pre>
<p> After loading the packages we can bring in the data set we will be
working with using the <code>read_csv()</code> function. This dataset
includes red and white wines (<code>type</code>) that vary among three
quality levels (<code>quality</code>) with five measures of their
chemical attributes:</p>
<ul>
<li><code>residual_sugar</code> - grams per liter of sugars remaining
after fermentation</li>
<li><code>pH</code> - acidity as measured by the negative
log<sub>10</sub> of the solution’s hydronium ion concentration</li>
<li><code>alcohol</code> - percent volumn that is alcohol</li>
<li><code>chlorides</code> - grams per liter of sodium chloride</li>
<li><code>sulphates</code> - grams per liter of potassium sulphate</li>
</ul>
<pre class="r"><code>wine &lt;- read_csv(&quot;../dat/wine-quality.csv&quot;)

head(wine)</code></pre>
<pre><code>## # A tibble: 6 × 7
##   type  quality residual_sugar    pH alcohol chlorides sulphates
##   &lt;chr&gt; &lt;chr&gt;            &lt;dbl&gt; &lt;dbl&gt;   &lt;dbl&gt;     &lt;dbl&gt;     &lt;dbl&gt;
## 1 red   good               1.8  3.38    11.8     0.066      0.72
## 2 red   good               2    2.94     9.2     0.082      0.66
## 3 red   good               3    3.16    11.5     0.103      1   
## 4 red   good               2.5  3.27     9.3     0.097      0.6 
## 5 red   good               2.6  3.36     9.4     0.093      0.86
## 6 red   good               3.6  3.39    11       0.067      0.66</code></pre>
<p> Note that the <code>type</code> and <code>quality</code> variables
are enccoded as characters. This is usually fine, but for ease of use we
should go ahead and recode them to factors. For the <code>type</code>
variable we can code is as a normal factor, however the
<code>quality</code> variable is actually ordered, so we should recode
it as an ordered factor.</p>
<pre class="r"><code>wine$type &lt;- factor(wine$type)
wine$quality &lt;- factor(wine$quality,
                       levels = c(&quot;mediocre&quot;, &quot;good&quot;, &quot;very good&quot;),
                       ordered = TRUE)

str(wine)</code></pre>
<pre><code>## spec_tbl_df [60 × 7] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ type          : Factor w/ 2 levels &quot;red&quot;,&quot;white&quot;: 1 1 1 1 1 1 1 1 1 1 ...
##  $ quality       : Ord.factor w/ 3 levels &quot;mediocre&quot;&lt;&quot;good&quot;&lt;..: 2 2 2 2 2 2 2 2 2 2 ...
##  $ residual_sugar: num [1:60] 1.8 2 3 2.5 2.6 3.6 2 2 8.8 2 ...
##  $ pH            : num [1:60] 3.38 2.94 3.16 3.27 3.36 3.39 3.22 3.37 3.38 3.11 ...
##  $ alcohol       : num [1:60] 11.8 9.2 11.5 9.3 9.4 11 11.2 9.4 10.1 10.1 ...
##  $ chlorides     : num [1:60] 0.066 0.082 0.103 0.097 0.093 0.067 0.12 0.082 0.084 0.067 ...
##  $ sulphates     : num [1:60] 0.72 0.66 1 0.6 0.86 0.66 0.58 0.5 0.64 0.97 ...
##  - attr(*, &quot;spec&quot;)=
##   .. cols(
##   ..   type = col_character(),
##   ..   quality = col_character(),
##   ..   residual_sugar = col_double(),
##   ..   pH = col_double(),
##   ..   alcohol = col_double(),
##   ..   chlorides = col_double(),
##   ..   sulphates = col_double()
##   .. )
##  - attr(*, &quot;problems&quot;)=&lt;externalptr&gt;</code></pre>
<p> From the output of <code>str()</code> we can see that
<code>type</code> and <code>quality</code> are now coded as a factor and
an ordered factor where its levels are ordered as <em>mediocre</em> &lt;
<em>good</em> &lt; <em>very good</em>, respectively.</p>
<pre class="r"><code>wine %&gt;%
  pivot_longer(cols = c(residual_sugar, pH, alcohol, chlorides, sulphates)) %&gt;%
  ggplot(aes(x = name, y = value, color = factor(quality))) +
    geom_boxplot() +
    facet_wrap(~ type) +
    labs(x = NULL) +
    theme_bw() +
    theme(axis.text = element_text(angle = 45, vjust = 0.5, hjust = 1))</code></pre>
<p><img src="data:image/png;base64,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" width="576" style="display: block; margin: auto;" /></p>
<div id="checking-assumptions" class="section level2">
<h2>Checking assumptions</h2>
<p> Before starting the MANOVA analysis we should first check whether
some of its assumptions are valid with this data set. These assumptions
include:</p>
<ul>
<li>Adequate <em>sample size</em>,</li>
<li>No <em>outliers</em> or <em>high leverage points</em>,</li>
<li>Approximately multivariate <em>normality</em>,</li>
<li>No major <em>multicollinearity</em>, and</li>
<li>Homogeneity of covariances, or <em>heteroskedacity</em>.</li>
</ul>
<p><a id="sample-size"></a></p>
<div id="sample-size" class="section level3">
<h3>Sample size</h3>
<p> For the MANOVA test to be valid, the number of observations (n) for
each group (g) must be greater than the number of outcome variables (p).
With this data, we have 2 levels in the <code>type</code> variable and 3
levels in the <code>quality</code> variable for 2x3 = 6 total groups, so
we will want our n for each group to be greater than 6. We can use the
<code>group_by()</code> function to create our groups then count the
number of observations within them with the <code>summarize()</code> and
<code>n()</code> functions.</p>
<pre class="r"><code>wine %&gt;%
  group_by(type, quality) %&gt;%
  summarize(n = n())</code></pre>
<pre><code>## # A tibble: 6 × 3
## # Groups:   type [2]
##   type  quality       n
##   &lt;fct&gt; &lt;ord&gt;     &lt;int&gt;
## 1 red   mediocre     10
## 2 red   good         10
## 3 red   very good    10
## 4 white mediocre     10
## 5 white good         10
## 6 white very good    10</code></pre>
<p> </p>
</div>
<div id="outliers" class="section level3 tabset tabset-fade tabset-pills">
<h3 class="tabset tabset-fade tabset-pills">Outliers</h3>
<div id="multivariate-outliers" class="section level4">
<h4>Multivariate outliers</h4>
<p> To flag multivariate outliers we can calculate the Mahalanobis
distances for each point, then compare those distances against a
χ<sup>2</sup> distribution where the degrees of freedom is equal to the
number of outcome variables (in this case 3) and α = 0.001. We first
group our independent variables of interest, <i>type</i> and
<i>quality</i>, using <code>group_by()</code> then calculate the
Mahalanobis distances with the <code>mahalanobis_distance()</code>
funciton. We will also use the <code>filter()</code> function to only
print out significant outliers.</p>
<pre class="r"><code>wine %&gt;%
  group_by(type, quality) %&gt;%
  mahalanobis_distance() %&gt;%
  filter(is.outlier == TRUE)</code></pre>
<pre><code>## # A tibble: 0 × 7
## # … with 7 variables: residual_sugar &lt;dbl&gt;, pH &lt;dbl&gt;, alcohol &lt;dbl&gt;, chlorides &lt;dbl&gt;,
## #   sulphates &lt;dbl&gt;, mahal.dist &lt;dbl&gt;, is.outlier &lt;lgl&gt;</code></pre>
<p> Since the output is empty we can conclude that there are no
significant multivariate outliers as determined by the Mahalanobis
distance.</p>
</div>
<div id="univariate-outliers" class="section level4">
<h4>Univariate outliers</h4>
<pre class="r"><code>wine %&gt;%
  group_by(type, quality) %&gt;%
  identify_outliers(pH)</code></pre>
<pre><code>## # A tibble: 1 × 9
##   type  quality  residual_sugar    pH alcohol chlorides sulphates is.outlier is.extreme
##   &lt;fct&gt; &lt;ord&gt;             &lt;dbl&gt; &lt;dbl&gt;   &lt;dbl&gt;     &lt;dbl&gt;     &lt;dbl&gt; &lt;lgl&gt;      &lt;lgl&gt;     
## 1 red   mediocre            2.5  3.53     9.5     0.076      0.55 TRUE       FALSE</code></pre>
<pre class="r"><code>wine %&gt;%
  group_by(type, quality) %&gt;%
  identify_outliers(residual_sugar)</code></pre>
<pre><code>## # A tibble: 4 × 9
##   type  quality   residual_sugar    pH alcohol chlorides sulphates is.outlier is.extreme
##   &lt;fct&gt; &lt;ord&gt;              &lt;dbl&gt; &lt;dbl&gt;   &lt;dbl&gt;     &lt;dbl&gt;     &lt;dbl&gt; &lt;lgl&gt;      &lt;lgl&gt;     
## 1 red   mediocre             7.9  3.27     9.3     0.086      0.57 TRUE       TRUE      
## 2 red   mediocre             2    3.25     9.4     0.074      0.54 TRUE       FALSE     
## 3 red   good                 8.8  3.38    10.1     0.084      0.64 TRUE       TRUE      
## 4 red   very good            3.4  3.17    12.2     0.128      0.84 TRUE       FALSE</code></pre>
<pre class="r"><code>wine %&gt;%
  group_by(type, quality) %&gt;%
  identify_outliers(`alcohol`)</code></pre>
<pre><code>## # A tibble: 3 × 9
##   type  quality   residual_sugar    pH alcohol chlorides sulphates is.outlier is.extreme
##   &lt;fct&gt; &lt;ord&gt;              &lt;dbl&gt; &lt;dbl&gt;   &lt;dbl&gt;     &lt;dbl&gt;     &lt;dbl&gt; &lt;lgl&gt;      &lt;lgl&gt;     
## 1 red   mediocre             2.6  3.22    13       0.056      0.37 TRUE       TRUE      
## 2 red   very good            2.2  3.07     9.2     0.075      0.84 TRUE       FALSE     
## 3 white mediocre             5.6  2.96    11       0.039      0.52 TRUE       TRUE</code></pre>
</div>
</div>
<div id="normality" class="section level3">
<h3>Normality</h3>
<div id="multivariate-normality-test" class="section level4">
<h4>Multivariate normality test</h4>
<pre class="r"><code>wine %&gt;%
  select(pH, alcohol, residual_sugar, chlorides, sulphates) %&gt;%
  mshapiro_test()</code></pre>
<pre><code>## # A tibble: 1 × 2
##   statistic  p.value
##       &lt;dbl&gt;    &lt;dbl&gt;
## 1     0.899 0.000116</code></pre>
</div>
<div id="univariate-normality-test" class="section level4">
<h4>Univariate normality test</h4>
<pre class="r"><code>wine %&gt;%
  group_by(type, quality) %&gt;%
  shapiro_test(residual_sugar, pH, alcohol, chlorides, sulphates) %&gt;%
  adjust_pvalue(method = &quot;BH&quot;) %&gt;%
  add_significance() %&gt;%
  print(n = Inf)</code></pre>
<pre><code>## # A tibble: 30 × 7
##    type  quality   variable       statistic          p    p.adj p.adj.signif
##    &lt;fct&gt; &lt;ord&gt;     &lt;chr&gt;              &lt;dbl&gt;      &lt;dbl&gt;    &lt;dbl&gt; &lt;chr&gt;       
##  1 red   mediocre  alcohol            0.615 0.0000832  0.000772 ***         
##  2 red   mediocre  chlorides          0.966 0.855      0.902    ns          
##  3 red   mediocre  pH                 0.930 0.450      0.643    ns          
##  4 red   mediocre  residual_sugar     0.513 0.00000518 0.000155 ***         
##  5 red   mediocre  sulphates          0.662 0.000308   0.00185  **          
##  6 red   good      alcohol            0.879 0.129      0.297    ns          
##  7 red   good      chlorides          0.926 0.414      0.631    ns          
##  8 red   good      pH                 0.848 0.0543     0.156    ns          
##  9 red   good      residual_sugar     0.591 0.0000426  0.000640 ***         
## 10 red   good      sulphates          0.901 0.224      0.449    ns          
## 11 red   very good alcohol            0.914 0.312      0.585    ns          
## 12 red   very good chlorides          0.840 0.0445     0.148    ns          
## 13 red   very good pH                 0.965 0.845      0.902    ns          
## 14 red   very good residual_sugar     0.849 0.0570     0.156    ns          
## 15 red   very good sulphates          0.924 0.390      0.631    ns          
## 16 white mediocre  alcohol            0.623 0.000103   0.000772 ***         
## 17 white mediocre  chlorides          0.878 0.123      0.297    ns          
## 18 white mediocre  pH                 0.953 0.700      0.840    ns          
## 19 white mediocre  residual_sugar     0.927 0.421      0.631    ns          
## 20 white mediocre  sulphates          0.948 0.650      0.840    ns          
## 21 white good      alcohol            0.840 0.0441     0.148    ns          
## 22 white good      chlorides          0.968 0.871      0.902    ns          
## 23 white good      pH                 0.968 0.872      0.902    ns          
## 24 white good      residual_sugar     0.838 0.0421     0.148    ns          
## 25 white good      sulphates          0.890 0.168      0.361    ns          
## 26 white very good alcohol            0.923 0.381      0.631    ns          
## 27 white very good chlorides          0.975 0.932      0.932    ns          
## 28 white very good pH                 0.943 0.588      0.802    ns          
## 29 white very good residual_sugar     0.834 0.0377     0.148    ns          
## 30 white very good sulphates          0.951 0.680      0.840    ns</code></pre>
</div>
</div>
<div id="correlations" class="section level3">
<h3>Correlations</h3>
<pre class="r"><code>wine %&gt;%
  select(residual_sugar:sulphates) %&gt;%
  cor_mat()</code></pre>
<pre><code>## # A tibble: 5 × 6
##   rowname        residual_sugar    pH alcohol chlorides sulphates
## * &lt;chr&gt;                   &lt;dbl&gt; &lt;dbl&gt;   &lt;dbl&gt;     &lt;dbl&gt;     &lt;dbl&gt;
## 1 residual_sugar           1    -0.33   -0.36     -0.29     -0.3 
## 2 pH                      -0.33  1       0.18      0.11      0.11
## 3 alcohol                 -0.36  0.18    1        -0.22      0.14
## 4 chlorides               -0.29  0.11   -0.22      1         0.36
## 5 sulphates               -0.3   0.11    0.14      0.36      1</code></pre>
</div>
<div id="heteroskedasticity" class="section level3">
<h3>Heteroskedasticity</h3>
<pre class="r"><code>wine %&gt;%
  select(residual_sugar:sulphates) %&gt;%
  box_m(wine$quality)</code></pre>
<pre><code>## # A tibble: 1 × 4
##   statistic p.value parameter method                                             
##       &lt;dbl&gt;   &lt;dbl&gt;     &lt;dbl&gt; &lt;chr&gt;                                              
## 1      33.8   0.290        30 Box&#39;s M-test for Homogeneity of Covariance Matrices</code></pre>
<pre class="r"><code>wine %&gt;%
  pivot_longer(cols = c(pH, alcohol, residual_sugar, chlorides, sulphates)) %&gt;%
  group_by(name) %&gt;%
  levene_test(value ~ type * quality) %&gt;%
  adjust_pvalue() %&gt;%
  add_significance()</code></pre>
<pre><code>## # A tibble: 5 × 7
##   name             df1   df2 statistic         p    p.adj p.adj.signif
##   &lt;chr&gt;          &lt;int&gt; &lt;int&gt;     &lt;dbl&gt;     &lt;dbl&gt;    &lt;dbl&gt; &lt;chr&gt;       
## 1 alcohol            5    54     1.65  0.164     0.378    ns          
## 2 chlorides          5    54     2.05  0.0863    0.345    ns          
## 3 pH                 5    54     1.81  0.126     0.378    ns          
## 4 residual_sugar     5    54     6.56  0.0000773 0.000387 ***         
## 5 sulphates          5    54     0.556 0.733     0.733    ns</code></pre>
</div>
</div>
<div id="one-way-manova" class="section level2">
<h2>One-way MANOVA</h2>
<pre class="r"><code>wine_manova1 &lt;- manova(cbind(pH, alcohol, residual_sugar, chlorides, sulphates) ~ quality,
                       data = wine)

Manova(wine_manova1, test.statistic = &quot;Pillai&quot;)</code></pre>
<pre><code>## 
## Type II MANOVA Tests: Pillai test statistic
##         Df test stat approx F num Df den Df  Pr(&gt;F)  
## quality  2   0.33506   2.1734     10    108 0.02472 *
## ---
## Signif. codes:  0 &#39;***&#39; 0.001 &#39;**&#39; 0.01 &#39;*&#39; 0.05 &#39;.&#39; 0.1 &#39; &#39; 1</code></pre>
<pre class="r"><code>Manova(wine_manova1, test.statistic = &quot;Wilks&quot;)</code></pre>
<pre><code>## 
## Type II MANOVA Tests: Wilks test statistic
##         Df test stat approx F num Df den Df  Pr(&gt;F)  
## quality  2   0.67572    2.295     10    106 0.01764 *
## ---
## Signif. codes:  0 &#39;***&#39; 0.001 &#39;**&#39; 0.01 &#39;*&#39; 0.05 &#39;.&#39; 0.1 &#39; &#39; 1</code></pre>
<pre class="r"><code>Manova(wine_manova1, test.statistic = &quot;Hotelling-Lawley&quot;)</code></pre>
<pre><code>## 
## Type II MANOVA Tests: Hotelling-Lawley test statistic
##         Df test stat approx F num Df den Df  Pr(&gt;F)  
## quality  2   0.46394   2.4125     10    104 0.01272 *
## ---
## Signif. codes:  0 &#39;***&#39; 0.001 &#39;**&#39; 0.01 &#39;*&#39; 0.05 &#39;.&#39; 0.1 &#39; &#39; 1</code></pre>
<pre class="r"><code>Manova(wine_manova1, test.statistic = &quot;Roy&quot;)</code></pre>
<pre><code>## 
## Type II MANOVA Tests: Roy test statistic
##         Df test stat approx F num Df den Df   Pr(&gt;F)   
## quality  2   0.42653   4.6065      5     54 0.001426 **
## ---
## Signif. codes:  0 &#39;***&#39; 0.001 &#39;**&#39; 0.01 &#39;*&#39; 0.05 &#39;.&#39; 0.1 &#39; &#39; 1</code></pre>
<div id="post-hoc-anova" class="section level3">
<h3>Post-hoc ANOVA</h3>
<pre class="r"><code>wine %&gt;%
  pivot_longer(cols = c(pH, alcohol, residual_sugar, chlorides, sulphates)) %&gt;%
  group_by(name) %&gt;%
  anova_test(value ~ quality)</code></pre>
<pre><code>## # A tibble: 5 × 8
##   name           Effect    DFn   DFd     F        p `p&lt;.05`   ges
## * &lt;chr&gt;          &lt;chr&gt;   &lt;dbl&gt; &lt;dbl&gt; &lt;dbl&gt;    &lt;dbl&gt; &lt;chr&gt;   &lt;dbl&gt;
## 1 alcohol        quality     2    57 9.82  0.000216 &quot;*&quot;     0.256
## 2 chlorides      quality     2    57 0.601 0.552    &quot;&quot;      0.021
## 3 pH             quality     2    57 1.55  0.221    &quot;&quot;      0.052
## 4 residual_sugar quality     2    57 1.37  0.262    &quot;&quot;      0.046
## 5 sulphates      quality     2    57 2.16  0.124    &quot;&quot;      0.071</code></pre>
</div>
<div id="post-hoc-pairwise-comparisons" class="section level3">
<h3>Post-hoc pairwise comparisons</h3>
<pre class="r"><code>wine %&gt;%
  tukey_hsd(alcohol ~ quality)</code></pre>
<pre><code>## # A tibble: 3 × 9
##   term    group1   group2    null.value estimate conf.low conf.high    p.adj p.adj.signif
## * &lt;chr&gt;   &lt;chr&gt;    &lt;chr&gt;          &lt;dbl&gt;    &lt;dbl&gt;    &lt;dbl&gt;     &lt;dbl&gt;    &lt;dbl&gt; &lt;chr&gt;       
## 1 quality mediocre good               0    0.907   0.0585      1.75 0.0336   *           
## 2 quality mediocre very good          0    1.55    0.707       2.40 0.000135 ***         
## 3 quality good     very good          0    0.648  -0.200       1.50 0.166    ns</code></pre>
</div>
</div>
<div id="two-way-manova" class="section level2">
<h2>Two-way MANOVA</h2>
<pre class="r"><code>wine_manova2 &lt;- manova(cbind(pH, alcohol, residual_sugar, chlorides, sulphates) ~ quality * type,
                       data = wine)

Manova(wine_manova2, test.statistic = &quot;Pillai&quot;, type = 3)</code></pre>
<pre><code>## 
## Type III MANOVA Tests: Pillai test statistic
##              Df test stat approx F num Df den Df    Pr(&gt;F)    
## (Intercept)   1   0.99821   5570.7      5     50 &lt; 2.2e-16 ***
## quality       2   0.36076      2.2     10    102   0.02065 *  
## type          1   0.78403     36.3      5     50 1.635e-15 ***
## quality:type  2   0.24110      1.4     10    102   0.19173    
## ---
## Signif. codes:  0 &#39;***&#39; 0.001 &#39;**&#39; 0.01 &#39;*&#39; 0.05 &#39;.&#39; 0.1 &#39; &#39; 1</code></pre>
<p><a id="post-hoc-anova2"></a></p>
<div id="post-hoc-anova-1" class="section level3">
<h3>Post-hoc ANOVA</h3>
<pre class="r"><code>wine %&gt;%
  pivot_longer(cols = c(pH, alcohol, residual_sugar, chlorides, sulphates)) %&gt;%
  group_by(name) %&gt;%
  anova_test(value ~ quality + type, type = 2)</code></pre>
<pre><code>## # A tibble: 10 × 8
##    name           Effect    DFn   DFd      F        p `p&lt;.05`     ges
##  * &lt;chr&gt;          &lt;chr&gt;   &lt;dbl&gt; &lt;dbl&gt;  &lt;dbl&gt;    &lt;dbl&gt; &lt;chr&gt;     &lt;dbl&gt;
##  1 alcohol        quality     2    56  9.66  2.49e- 4 &quot;*&quot;     0.256  
##  2 alcohol        type        1    56  0.051 8.22e- 1 &quot;&quot;      0.00091
##  3 chlorides      quality     2    56  1.54  2.24e- 1 &quot;&quot;      0.052  
##  4 chlorides      type        1    56 89.7   3.17e-13 &quot;*&quot;     0.616  
##  5 pH             quality     2    56  1.77  1.79e- 1 &quot;&quot;      0.06   
##  6 pH             type        1    56  9.20  4   e- 3 &quot;*&quot;     0.141  
##  7 residual_sugar quality     2    56  1.81  1.73e- 1 &quot;&quot;      0.061  
##  8 residual_sugar type        1    56 19.4   4.75e- 5 &quot;*&quot;     0.258  
##  9 sulphates      quality     2    56  3.03  5.6 e- 2 &quot;&quot;      0.098  
## 10 sulphates      type        1    56 23.9   8.93e- 6 &quot;*&quot;     0.299</code></pre>
<p><a id="post-hoc-pairwise-comparisons2"></a></p>
</div>
<div id="post-hoc-pairwise-comparisons-1" class="section level3">
<h3>Post-hoc pairwise comparisons</h3>
<pre class="r"><code>wine %&gt;%
  tukey_hsd(alcohol ~ quality)</code></pre>
<pre><code>## # A tibble: 3 × 9
##   term    group1   group2    null.value estimate conf.low conf.high    p.adj p.adj.signif
## * &lt;chr&gt;   &lt;chr&gt;    &lt;chr&gt;          &lt;dbl&gt;    &lt;dbl&gt;    &lt;dbl&gt;     &lt;dbl&gt;    &lt;dbl&gt; &lt;chr&gt;       
## 1 quality mediocre good               0    0.907   0.0585      1.75 0.0336   *           
## 2 quality mediocre very good          0    1.55    0.707       2.40 0.000135 ***         
## 3 quality good     very good          0    0.648  -0.200       1.50 0.166    ns</code></pre>
</div>
</div>
</div>



<!-- code folding -->


<!-- dynamically load mathjax for compatibility with self-contained -->
<script>
  (function () {
    var script = document.createElement("script");
    script.type = "text/javascript";
    script.src  = "https://mathjax.rstudio.com/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML";
    document.getElementsByTagName("head")[0].appendChild(script);
  })();
</script>

</body>
</html>
