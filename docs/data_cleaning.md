
# Data Cleaning

###### Published: 6 April 2020

The first thing with any dataset, whether we collected the data
ourselves or retrieve it from another source, is to thoroughly check the
quality of and clean our data. In fact, [preparing raw data is the most
time consuming part of data
analysis](https://visit.figure-eight.com/rs/416-ZBE-142/images/CrowdFlower_DataScienceReport_2016.pdf).
It is important that we [follow certain steps to clean our
data](https://sunscrapers.com/blog/why-is-clean-data-so-important-for-analytics-and-business-intelligence/)
to reduce issues during data analysis, which include (but not always
limited to):

  - Identifying and correcting structural errors
  - Handling missing values
  - Removing duplicated observations
  - Dealing with incorrect, mislabeled, and outlying data points
  - Formatting consistent variable schemes, removing unwanted variables,
    and generating new variables

Here, we will go over some methods to identify and handle these issues
in addition to some basics on:

  - Loops and apply functions
  - Pipes
  - Pattern recognition and substitution

Before we attempt to clean our data we should first summarize and
explore our dataset to get familiar with it and spot any possible errors
or inconsistencies.

## Summarizing and Exploring Data

First, let us set up our working directory and load it. Our data is
stored as a *.csv* file in the “*data/*” folder, so we can access it
using the `read.csv()` function.

``` r
setwd("~/CDAR")
beer <- read.csv("data/beer_to_clean.csv")
```

To get familiar with our data we should check the size of our dataset.
If we use the `dim()` function we can see its dimensions.

``` r
dim(beer)
```

    ## [1] 2021   10

The first number (2021) indicates the amount of rows, our total number
of observations, and the second (10) the columns, the number of
variables measured. 2021 is a lot of rows, much more than we can
reasonably look through. Instead, we can look at a few rows to get a
general idea of what our data looks like. To do so, we can use the
`head()` and `tail()` functions to look at the first and last few rows,
respectively.

``` r
head(beer)
```

    ##   X            brewery_name review_time review_aroma review_appearance               beer_style review_palate review_taste
    ## 1 1 TrÃ¶egs Brewing Company  1275968124          3.0               4.5            Oatmeal Stout           3.5          3.0
    ## 2 2 Victory Brewing Company  1182930981          4.5               4.0             American IPA           4.0          4.5
    ## 3 3 Victory Brewing Company  1193519101          4.0               1.5                   Tripel           4.0          4.0
    ## 4 4 Victory Brewing Company  1157967909          4.5               4.0             American IPA           3.5          4.0
    ## 5 5 Victory Brewing Company  1263695148          3.5               4.0             American IPA           3.5          4.0
    ## 6 6 TrÃ¶egs Brewing Company  1204151979          5.0               4.0 American Amber / Red Ale           4.0          4.5
    ##                 beer_name beer_abv
    ## 1 TrÃ¶egs Java Head Stout      7.5
    ## 2            HopDevil Ale      6.7
    ## 3           Golden Monkey      9.5
    ## 4            HopDevil Ale      6.7
    ## 5            HopDevil Ale      6.7
    ## 6   TrÃ¶egs Nugget Nectar      7.5

``` r
tail(beer, 3)
```

    ##         X            brewery_name review_time review_aroma review_appearance                     beer_style review_palate
    ## 2019 2019 Victory Brewing Company  1277167012          3.5               5.0                    Belgian IPA             3
    ## 2020 2020 Victory Brewing Company  1283660248          3.5               3.5 American Double / Imperial IPA             4
    ## 2021 2021 Victory Brewing Company  1280612077          4.0               3.5                         Tripel             4
    ##      review_taste     beer_name beer_abv
    ## 2019          3.0    Wild Devil      6.7
    ## 2020          4.5    Hop Wallop      8.5
    ## 2021          3.0 Golden Monkey      9.5

In the code above we used `head()` with just the name of our dataframe
and we were given the first 6 rows of observations. Instead, when we
used the `tail()` function we included a 3 to tell `R` to give us the
last 3 rows rather than the last 6 as default (this can be changed as
much as you like to view more or less rows, but we will continue to use
3 for these functions just to reduce output space).

While both the `head()` and `tail()` function provide the column names,
we can use a separate function `names()` (or alternatively `colnames()`)
to return only those column
    names.

``` r
names(beer)
```

    ##  [1] "X"                 "brewery_name"      "review_time"       "review_aroma"      "review_appearance" "beer_style"       
    ##  [7] "review_palate"     "review_taste"      "beer_name"         "beer_abv"

For this dataset the column names are fairly straight-forward.
`brewery_name` is the name of the brewery, `review_appearance` is the
score given by for how the reviewer viewed the appearance of the beer,
and so on. However, there is one column that does not make any sense,
`X`. If we look back up to our `head()` and `tail()` commands we can see
that the `X` column increases incrementally from 1. In fact, each number
for this column is the name as their corresponding row number. Maybe
these are unique ID numbers, or maybe there was a formatting issue, but
either way this column does not appear to be necessary. Typically, once
we have chosen that a column can be removed then we should remove it
before continuing with other procedures so as not to negatively affect
our data exploration. So, let us go ahead and remove that column.

To do so, we can reassign our dataframe using a variety of methods. One
method is presented in the following code block.

``` r
beer <- beer[, -1]
```

Here, we are telling `R` to reassign our dataframe using our old
dataframe without the first column by using the -1.

Alternatively, we could replace the brackets `[, -1]` with `[, 2:10]` to
tell `R` to copy over each column starting from the 2nd to the 7th
columns.

``` r
beer <- beer[, 2:10]
names(beer)
```

    ## [1] "brewery_name"      "review_time"       "review_aroma"      "review_appearance" "beer_style"        "review_palate"    
    ## [7] "review_taste"      "beer_name"         "beer_abv"

However, make sure to only run either command once\! Running the first
command a second time would delete the `brewery_name` column (which
would not be the first column), while running the second would return an
error. Again using the `names()` function we can see that we have
removed `X` from our dataframe but kept the other 9 columns.

Now, it would be good to look at a summary of our dataset, which we can
do with the `summary()`
    function.

``` r
summary(beer)
```

    ##                   brewery_name   review_time         review_aroma   review_appearance                          beer_style  
    ##  TrÃ¶egs Brewing Company: 653   Min.   :1.013e+09   Min.   :1.000   Min.   :1.500     American IPA                  : 172  
    ##  Victory Brewing Company:1018   1st Qu.:1.168e+09   1st Qu.:3.500   1st Qu.:3.500     American Amber / Red Ale      : 152  
    ##  Voodoo Brewing Company :  59   Median :1.240e+09   Median :4.000   Median :4.000     German Pilsener               : 146  
    ##  Yards Brewing Co.      : 105   Mean   :1.220e+09   Mean   :3.813   Mean   :3.918     Russian Imperial Stout        : 126  
    ##  Yuengling Brewery      : 186   3rd Qu.:1.284e+09   3rd Qu.:4.000   3rd Qu.:4.000     American Double / Imperial IPA:  95  
    ##                                 Max.   :1.326e+09   Max.   :5.000   Max.   :5.000     Belgian Strong Dark Ale       :  81  
    ##                                                                                       (Other)                       :1249  
    ##  review_palate    review_taste                         beer_name       beer_abv     
    ##  Min.   :1.000   Min.   :1.000   HopDevil Ale               : 121   Min.   : 3.300  
    ##  1st Qu.:3.500   1st Qu.:3.500   Storm King Stout           : 117   1st Qu.: 5.300  
    ##  Median :4.000   Median :4.000   Prima Pils                 : 105   Median : 7.000  
    ##  Mean   :3.847   Mean   :3.907   Hop Wallop                 :  90   Mean   : 7.281  
    ##  3rd Qu.:4.000   3rd Qu.:4.500   TrÃ¶egs Nugget Nectar      :  90   3rd Qu.: 8.700  
    ##  Max.   :5.000   Max.   :5.000   Yuengling Traditional Lager:  80   Max.   :89.000  
    ##                                  (Other)                    :1418   NA's   :17

As we can see, the `summary()` function provides the cut-off values for
the 1st, 2nd (Median), and 3rd quartiles in addition to the minimum,
maximum, and average (mean) values for numerical data. We can use the
summary statistics to get an overall idea how our data is distributed as
well as discover possible problems. One issue to point out is the
maximum values for `beer_abv` of 89, a very large (and likely
impossible) ABV for a beer\!

Are we certain this is the only outlier in this variable? If we look at
the second maximum we may be able to answer that question. We can use
the `sort()` command to help us out here, setting the `decreasing`
option to `TRUE` with a value of 2 in the brackets `[]` that follow.
This tells `R` to sort the data in decreasing order and select the
second value (our second maximum).

``` r
sort(beer$beer_abv, decreasing=TRUE)[2]
```

    ## [1] 15

The 15 seems fairly normal for a high ABV beer, so it appears there is
only one outlier here.

Alternatively, if we were uncertain how many outliers we might have we
could choose a threshold to look above (or below in other cases) and
count the amount of data points that are outside of that threshold. For
a high ABV beer, we could assume that a maximum of 30 is a reasonable
threshold. Then, we can employ a little logic to add up all of the
values that are above 30 with the following code.

``` r
sum(beer$beer_abv > 30, na.rm=TRUE)
```

    ## [1] 1

Only one beer is above our threshold, so again it looks like we have
only one outlier here. Note that we needed to set the `na.rm` option to
`TRUE`. We have missing values in this variable which we will cover in a
moment.

*Note: You may sometimes see `TRUE` and `FALSE` in `R` simplified to `T`
and `F`, however [this is not
recommended](https://www.r-bloggers.com/r-tip-avoid-using-t-and-f-as-synonyms-for-true-and-false/)*

But what if we were unfamiliar with beer and did not know if 15 was a
normal ABV or not? Instead of a threshold we set we could use
statistical methods to determine if we have an outlier, such as the
[Grubb’s test](https://www.statisticshowto.com/grubbs-test/) or the
[generalized extreme studentized deviate
test](https://www.itl.nist.gov/div898/handbook/eda/section3/eda35h3.htm),
which we will cover another time. We can also identify outliers visually
by plotting our data in boxplots, scatterplots, histograms, *etc*. These
will be covered in Exploratory Data Analysis. How we will deal with this
outlier will be explored in the next section.

Before moving forward we should bring up the `attach()` function which
allows `R` to access variables directly from their names (without having
to first type `beer$`). So, we could alternatively run the code above
as:

``` r
attach(beer)
sort(beer_abv, decreasing=TRUE)[2]
```

    ## [1] 15

``` r
sum(beer_abv > 30, na.rm=TRUE)
```

    ## [1] 1

Notice, we no longer need to include `beer$` before the variable name
and we get the same output. While that makes our typing a little bit
easier, using `attach()` [is not advised for multiple
reasons](https://stackoverflow.com/questions/10067680/why-is-it-not-advisable-to-use-attach-in-r-and-what-should-i-use-instead)
so we will avoid using the function here and in future posts.

Looking through the other variables in our summary statistics we notice
the ratings for each type of review are on a scale from 1 to 5 in
increments of 0.5. Are we certain that there are not any incorrect
values here? The minimum and maximum values look fine, and so do the
quartiles. The means are not divisible by 0.5, however that is not a
concern since it is a measure of the average score. What else can we do
to make sure there are no obvious issues in this variable?

One approach is to use some simple math and logic. If we know that the
scale is in increments of 0.5, then if we divide each score by 0.5 then
they should all be whole numbers, and thus will not have remainders in
their division. Therefore, we can divide all of the values by 0.5 then
add up each time the remainder is not 0 as shown in the code below (in
`R` we use `%%` to give the remainder of division).

``` r
sum(beer$review_aroma %% 0.5 > 0)
```

    ## [1] 0

Our code returned a 0, meaning that there were no remainders to summate,
so it looks like the `review_aroma` variable is solid. We can perform
the same procedure with the other review variables to confirm that their
data at least appears correct. We could copy the same code above and
replace `review_aroma` for each of the other review variables, or we
could use `apply()` to run our function on each column of interest and
simplify our
    code.

``` r
apply(beer[,c(3,4,6,7)], 2, function(x) sum(x %% 0.5 > 0))
```

    ##      review_aroma review_appearance     review_palate      review_taste 
    ##                 0                 0                 0                 0

Here, we first identify which data we will be applying our function on,
then the 2 indicates that the function will be on the columns, then we
describe the function we want to apply. As we can see, each of our
review variables return 0 so everything seems okay here.

However, not all of our data is numerical to provide measures of
quartiles, means, *etc*. that based some of our initial observations on.
For instance, `brewery_name` gives the total counts for each brewery,
but how did `R` know what to do here? We can call another function
`str()` to look at the structure of the dataframe our data is in.

``` r
str(beer)
```

    ## 'data.frame':    2021 obs. of  9 variables:
    ##  $ brewery_name     : Factor w/ 5 levels "TrÃ¶egs Brewing Company",..: 1 2 2 2 2 1 1 1 3 2 ...
    ##  $ review_time      : int  1275968124 1182930981 1193519101 1157967909 1263695148 1204151979 1289009043 1317653511 1264743263 1300169458 ...
    ##  $ review_aroma     : num  3 4.5 4 4.5 3.5 5 4 4 3 4 ...
    ##  $ review_appearance: num  4.5 4 1.5 4 4 4 4 4.5 3.5 4.5 ...
    ##  $ beer_style       : Factor w/ 67 levels "Altbier","American Adjunct Lager",..: 54 11 63 11 11 3 50 5 8 59 ...
    ##  $ review_palate    : num  3.5 4 4 3.5 3.5 4 3.5 4.5 3.5 4 ...
    ##  $ review_taste     : num  3 4.5 4 4 4 4.5 4 4.5 3.5 4 ...
    ##  $ beer_name        : Factor w/ 173 levels "132 Cents IPA",..: 134 38 30 38 38 137 84 27 163 115 ...
    ##  $ beer_abv         : num  7.5 6.7 9.5 6.7 6.7 7.5 7 9.3 7.3 9.1 ...

From this output we have three different types of data in our dataframe:

  - Integer (int) which are whole numbers
  - Numeric (num) which is also known as Double
  - Factor

At first look it appears that Factors are Strings (or known in `R` as
Characters), however that is not the case. Instead, Factors are
categorized data which fall into a number of levels. For example, if we
observe `brewery_name` in our output above we see the variable is of the
Factor type with 5 levels. We can use the `levels()` command to look
further.

``` r
levels(beer$brewery_name)
```

    ## [1] "TrÃ¶egs Brewing Company" "Victory Brewing Company" "Voodoo Brewing Company"  "Yards Brewing Co."      
    ## [5] "Yuengling Brewery "

Now, we see that each of the 5 levels of the `brewery_name` variable is
the name of one of our 5 breweries. We can also see there are some
issues in the names, but we will take care of that in the next section.

If we go back to our summary statistics there is one more important
observation we should make. In the `beer_abv` variable we are given a
value for “NA’s” of 17 (remember we needed to use the `na.rm` option
previously). This is telling us that there are 17 values that are
missing in this variable, or are “Not Available”. Could there be other
missing values in our dataset? We can use the following command to find
that out.

``` r
sum(is.na(beer))
```

    ## [1] 17

Here, we are using the function `is.na()` which returns `TRUE` for every
value of “NA” in the dataset. We use the `sum()` command to add up each
time `is.na()` returned `TRUE` to get a total number of values in the
dataset with “NA”. As we can see, this number matches the number of
“NA’s” in the `beer_abv` column so it does not appear that there are
missing values elsewhere. We could also use the following command to see
the total number of “NA” values in each
    column.

``` r
colSums(is.na(beer))
```

    ##      brewery_name       review_time      review_aroma review_appearance        beer_style     review_palate 
    ##                 0                 0                 0                 0                 0                 0 
    ##      review_taste         beer_name          beer_abv 
    ##                 0                 0                17

The `colSums()` function works similar to the `sum()` function, however
it calculates the sum for each column rather than for the dataset
overall. Again, we can see that there are 17 missing values in
`beer_abv` but none in the other columns. If we were interested in
looking at rows we could similarly use the `rowSums()` function.

While we do not observe them in our dataset here there are [three other
types of missing data in
`R`](https://www.r-bloggers.com/r-null-values-null-na-nan-inf/): `NULL`,
`NA`, and `Inf`. While slightly different, [there are functions to
handle these missing value
types](https://www.r-bloggers.com/r-null-values-null-na-nan-inf/)
similar to those available for `NA` values.

So now that we know that we have `NA` values what should we do with
them? There are a few options, and we will cover them in the next
section as well.

For now, let’s check on one more thing, is any of our data duplicated?
Since each row represents a unique observation, we should check whether
there are any non-unique rows. To do so, we can use the `duplicated()`
function, which returns `TRUE` whenever it finds a row that matches a
previous row, along with the `sum()` function to count each time there
is a duplicated row, similar to how we counted `NA` values above.

``` r
sum(duplicated(beer))
```

    ## [1] 21

It looks like we do have duplicated rows, 21 of them to be exact.
However, our function above only tells us the total amount of duplicates
but not where they are in the dataset. One way we can find the location
of duplicates is through the `anyDuplicated()` function.

``` r
anyDuplicated(beer)
```

    ## [1] 2001

From our output above, the first row that matches one of the preceding
rows is row 2001. If we do some simple math and logic, we could conclude
that the last 21 rows of our dataset are duplicates. Or, we could use a
different function, `which()`, to instead show each duplicated
    row.

``` r
which(duplicated(beer))
```

    ##  [1] 2001 2002 2003 2004 2005 2006 2007 2008 2009 2010 2011 2012 2013 2014 2015 2016 2017 2018 2019 2020 2021

How convenient\! We could easily remove those 21 rows similar to how we
removed the `X` column above, but what if the duplicates were
interleaved in the dataset? Using our previous method we would have to
indicate every specific row we wanted to remove (an easy way to make
mistakes). While we could again employ the `which()` command, that
approach could get messy. Instead, we have a few other options which
will start us off in our next section, cleaning up our dataset.

## Data Cleaning Procedures

#### Removing duplicates

Before we remove any duplicated observations we first must consider if
our data could have duplicates by design. For instance, if we asked
customers to rank our beer on a scale from 1 to 10 but we did not keep
track of each individual’s response with some unique identifier, then we
would expect some customers to give the same rating by chance,
particularly if we survey a lot of customers. Conversely, if we asked
customers to rank 15 different attributes it is less likely that any two
reviewers would score each attribute the same.

Our dataset observes individual reviews of beers, and since we only have
four ratings on a scale of 1 to 5 in 0.5 increments, it is plausible
that two independent reviewers could give the same beer the same score.
However, we have another variable, `review_time`, which records the time
the review was made as a unique ID for every review. Therefore, the
duplicates in our dataset are true duplicates due to some error in data
processing rather than by happenstance.

There are multiple ways we can deal with duplicated rows, [such as using
the `!` operator or the `distinct()`
function](https://www.datanovia.com/en/lessons/identify-and-remove-duplicate-data-in-r/),
however for our purposes the `unique()` function will do the trick of
only keeping non-duplicated rows.

``` r
beer <- unique(beer)

sum(duplicated(beer))
```

    ## [1] 0

``` r
dim(beer)
```

    ## [1] 2000    9

We then again use the `sum()` and `duplicated()` functions to show we no
longer have duplicated rows and the `dim()` function to see that we have
2021-21 = 2000 rows and 9 columns, which we expect. So now can move
ahead to clean up specific values.

#### Outliers & misrepresented data

In our summary statistics table above we noticed that there appears to
be an outlying observation in the `beer_abv` variable. We can call the
`which.max` function to tell us which specific observation (row number)
has the outlier, then use that number to index the observation as shown
in the following code.

``` r
max.abv <- which.max(beer$beer_abv)
max.abv
```

    ## [1] 121

``` r
beer[max.abv, ]
```

    ##                brewery_name review_time review_aroma review_appearance beer_style review_palate review_taste
    ## 121 Victory Brewing Company  1325017838          3.5                 4     Tripel           3.5            3
    ##             beer_name beer_abv
    ## 121 Tettnager Trippel       89

The “*Tettnager Trippel*” in row 121 contains our outlier. To correct
it, we can first start by looking to see if there are any other
“*Tettnager Trippel*” in the
    dataset.

``` r
beer[beer$beer_name == "Tettnager Trippel", ]
```

    ##                brewery_name review_time review_aroma review_appearance beer_style review_palate review_taste
    ## 121 Victory Brewing Company  1325017838          3.5                 4     Tripel           3.5            3
    ##             beer_name beer_abv
    ## 121 Tettnager Trippel       89

It looks like there is only one observation for this beer, so we have a
few choices. We could:

  - Discard this observation (generally, this should be our last
    option).
  - Replace the ABV with the average or median ABV from the same beer
    style, “*Tripel*”.  
  - Since `beer_abv` is a descriptor variable in our dataset it is
    likely that we could look up the true value.

Whichever method we choose, since this is one observation in 2000 it
likely will not make a huge difference, however in cases with smaller
sample sizes this choice could be very significant. If we look up this
brand of beer we [can see that the actual ABV
is 8.9%](https://www.beeradvocate.com/beer/profile/345/67529/?ba=glid02),
therefore we can assume that there was a data input error and the actual
value was supposed to be 8.9 rather than 89. We can then reassign this
single value with the following code.

``` r
beer$beer_abv[max.abv] <- 8.9

summary(beer$beer_abv)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
    ##   3.300   5.300   7.000   7.236   8.700  15.000      17

And from the summary statistics for the `beer_abv` variable, the
distributions of the beer ABVs appears to be more understandable.
However, we still have 17 `NA` values to deal with.

#### Missing values

One way we can choose to deal with missing values is to replace them
with a measure of central tendency. In our summary statistics table we
saw that the median and mean values for `beer_abv` were pretty similar,
7.00 and 7.236, respectively. So if we went with this method it probably
will not make a difference between which we choose, so we will stick
with the average value.

*(NOTE: We are not evaluating the following code block because we will
be using a more efficient method ahead)*

``` r
mean.abv <- mean(beer$beer_abv, na.rm=TRUE)

beer[is.na(beer)==TRUE] <- mean.abv
```

In the first line we assign the mean for `beer_abv`. Again note that we
need to set the `na.rm` option to `TRUE` in the `mean()` function to
tell `R` to ignore missing values in the calculation, otherwise when we
print out `mean.abv` we will also receive a value of `NA`. Many
functions in `R` have methods for dealing with missing values, however
they typically do so by ignoring (or removing) those missing values
which is generally not the best choice in data analysis. Next, we assign
all of the `NA` values with the mean ABV.

While the code above works for our dataset above, what would happen if
we had missing values in other columns, such as in the `beer_style`
variable? We would have assigned `NA` values with the mean ABV which
would have been incorrect\! An alternative way to perform the code above
that only works on our column of interest is as follows:

``` r
mean.abv <- mean(beer$beer_abv, na.rm=TRUE)

beer$beer_abv[is.na(beer$beer_abv)==TRUE] <- mean.abv
```

Here, we use the `$` operator after `beer` to tell `R` we are only
working with the `beer_abv` column, so it will not change values of “NA”
in other columns.

Alternatively, we could try to make our replacement values a little bit
more accurate by using prior information. For instance, we could replace
the `NA` values with the median ABV for the respective style of beer.
That is, for our first observation with a `NA` we want to replace it
with the median ABV for “Hefewizen”, then the second observation with
the median ABV for “German Pilsner”, and so forth. This would (ideally)
be a slightly more accurate correction since different beer styles can
vary in their range of ABVs. To do this, we could use a `for()` loop.

``` r
beer.na <- which(is.na(beer$beer_abv))
style.list <- as.character(beer$beer_style[beer.na])
```

First, we get two vectors, one with the row numbers of each `NA` value
and one with the styles of beer that correspond to each of those missing
values.

``` r
median.abv <- rep(0, length(style.list))
for(i in 1:length(style.list)){
  median.abv[i] <- median(beer$beer_abv[beer$beer_style == style.list[i]], na.rm=TRUE)
}

beer$beer_abv[beer.na] <- median.abv
```

Above, we make a list of *0’s* with the length of our vector of `NA`
values. Then, we run a `for()` loop to generate a new vector of values
for the median ABVs. Then, we replace the missing values with the median
ABVs. If we wanted to further check that our missing values are replaced
correctly, we could again use the `sum()` and `is.na()` or we could
print out the rows that we replaced those values for.

While this method works, the `for()` loop above is clunky and [many
suggest
avoiding](https://www.r-bloggers.com/for-loops-and-how-to-avoid-them/)
`for()` loops in `R` as [much as
possible](https://rpubs.com/markpayne/132503). So, alternatively we can
use the `sapply()` function to calculate the median value for each item
in our list we made
above.

``` r
median.abv <- sapply(style.list, function(x) median(beer$beer_abv[beer$beer_style == x], na.rm=TRUE))

beer$beer_abv[beer.na] <- median.abv

sum(is.na(beer))
```

    ## [1] 0

First, we use the vector we made earlier that has the styles of beer we
are interested in to apply our function to collect the median ABV for
each style. Then, we write our function to replace the `NA` values in
the dataframe with the median ABVs and use the `sum()` and `is.na()`
functions to show that we no longer have missing values.

Alternatively, we could remove all of our observations that have missing
values. In our dataset, that would be 17 deletions of 2000 observations,
so only 0.85% of our data, which could be reasonable. However, in many
situations deleting whole observations would be a much more significant
loss of data so it would not be a viable option versus the other methods
listed above (or others).

#### Renaming, moving, & creating variables

If it is not yet clear, it will be if we continue much further that
keeping the variable names as they currently are is somewhat tedious to
code. Do we really need to add “*review\_*” for each review variable? Or
have “*beer\_*” for each descriptive variable when we know we are
looking at data about beer? How about we make the variable names easier
to work with while we code by changing the column
names.

``` r
colnames(beer) <- c("brewery", "time", "aroma", "appearance", "style", "palate", "taste", "name", "abv")

names(beer)
```

    ## [1] "brewery"    "time"       "aroma"      "appearance" "style"      "palate"     "taste"      "name"       "abv"

Much better\!

*Note: If we had used the `attach()` function we would need to reapply
it here. Otherwise, `R` will still pull the original columns*

Looking back at our data we can see that there is an awkward order of
the variables. Two of our review variables, `aroma` and `appearance` are
in the beginning of the dataframe while two others, `palate` and `taste`
are near the end. While this does not technically matter it will be
easier when we do our analysis later (and make more sense) if they are
consecutive. One way we can reorder our data is shown in the following
code block.

``` r
beer <- beer[, c(2, 1, 8, 5, 9, 3:4, 6:7)]

head(beer, 3)
```

    ##         time                 brewery                    name         style abv aroma appearance palate taste
    ## 1 1275968124 TrÃ¶egs Brewing Company TrÃ¶egs Java Head Stout Oatmeal Stout 7.5   3.0        4.5    3.5   3.0
    ## 2 1182930981 Victory Brewing Company            HopDevil Ale  American IPA 6.7   4.5        4.0    4.0   4.5
    ## 3 1193519101 Victory Brewing Company           Golden Monkey        Tripel 9.5   4.0        1.5    4.0   4.0

What we have done now is move our unique identifier (`time`) to the
first column, descriptive variables to the following columns, and our
measured data (the review responses) to the last columns. The data does
not have to be organized in this way, however this is a fairly typical
order of variables you will see and will tend to make coding easier in
the future.

Alternatively, we could take advantage of one of the most used and most
helpful packages (really group of packages) called `tidyverse`. Using
the `select()` function we can order our columns by their names rather
than by their index. While this sometimes takes more typing it generally
makes more reproducible and easier to read
    code.

``` r
library(tidyverse)
```

    ## -- Attaching packages ---------------------------------------------------------------------------------------------------- tidyverse 1.3.0 --

    ## √ ggplot2 3.3.0     √ purrr   0.3.3
    ## √ tibble  2.1.3     √ dplyr   0.8.4
    ## √ tidyr   1.0.2     √ stringr 1.4.0
    ## √ readr   1.3.1     √ forcats 0.5.0

    ## -- Conflicts ------------------------------------------------------------------------------------------------------- tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
beer <- select(beer, time, brewery, name, style, abv, aroma, appearance, palate, taste)
```

Now that we have the columns in the order we want, how about we put the
rows in a preferred order. The values for the `time` variable does not
mean too much for us at the moment, but we could assume that they are in
ascending order of when the review was taken. Therefore, why don’t we
order our rows using that variable (which we are using as our unique
identifier anyway\!).

``` r
beer <- beer[order(beer$time), ]

head(beer, 3)
```

    ##            time            brewery                           name               style abv aroma appearance palate taste
    ## 1558 1013459463 Yuengling Brewery           Yuengling Black & Tan         Black & Tan 4.7   4.0          5    4.0   4.5
    ## 1669 1013692542 Yuengling Brewery  Yuengling Porter (Dark Brewed)     American Porter 4.7   3.0          4    3.5   3.5
    ## 1172 1013956672 Yuengling Brewery          Yuengling Premium Beer American Pale Lager 4.4   3.5          4    3.5   4.0

Now that our rows are ordered by `time` you might notice that our
rownames are no longer in order. We can easily rename (or renumber) the
rows as well.

``` r
rownames(beer) <- seq(1:2000)

head(beer, 3)
```

    ##         time            brewery                           name               style abv aroma appearance palate taste
    ## 1 1013459463 Yuengling Brewery           Yuengling Black & Tan         Black & Tan 4.7   4.0          5    4.0   4.5
    ## 2 1013692542 Yuengling Brewery  Yuengling Porter (Dark Brewed)     American Porter 4.7   3.0          4    3.5   3.5
    ## 3 1013956672 Yuengling Brewery          Yuengling Premium Beer American Pale Lager 4.4   3.5          4    3.5   4.0

``` r
tail(beer, 3)
```

    ##            time                 brewery                  name                   style abv aroma appearance palate taste
    ## 1998 1325767873 TrÃ¶egs Brewing Company      TrÃ¶egs Pale Ale American Pale Ale (APA) 5.4   3.5          4    4.0   4.0
    ## 1999 1326068736 Victory Brewing Company         Dark Intrigue  Russian Imperial Stout 9.1   4.0          4    4.0   4.0
    ## 2000 1326221853      Yuengling Brewery  Yuengling Light Lager             Light Lager 3.4   3.0          3    2.5   2.5

The `seq()` function above generates a sequence from 1 to 2000 at
increments of 1 for our new row names. Now our row labels and `time` are
both in ascending order.

Next, we might want to consider adding other variables that could be of
interest. We have scores for 4 different attributes, but what about how
the reviewers view each beer overall? Since we do not have an overall
ranking we can make our own based on the available data. In the
following code, we create two new variables that summate the total
scores and take the average of all scores.

``` r
beer$total <- rowSums(beer[, 6:9])
beer$average <- beer$total / 4

head(beer, 3)
```

    ##         time            brewery                           name               style abv aroma appearance palate taste total
    ## 1 1013459463 Yuengling Brewery           Yuengling Black & Tan         Black & Tan 4.7   4.0          5    4.0   4.5  17.5
    ## 2 1013692542 Yuengling Brewery  Yuengling Porter (Dark Brewed)     American Porter 4.7   3.0          4    3.5   3.5  14.0
    ## 3 1013956672 Yuengling Brewery          Yuengling Premium Beer American Pale Lager 4.4   3.5          4    3.5   4.0  15.0
    ##   average
    ## 1   4.375
    ## 2   3.500
    ## 3   3.750

Notice that since we reordered our variables we could use `[, 6:9]` to
indicate the review variables rather than `[, c(3,4,7,8)]` if we had
kept the columns in their original order.

While we would have preferred to have surveyed each reviewer on their
overall liking of the beers, at least we can now get some idea of how
beers are scored overall and which attributes are most important in that
score.

#### Formatting dates and times

Looking back at our summary statistics, we can see that the `time`
variable is filled with large numbers that really do not mean anything
to us. That is because the times were recorded with “Epoch” or “UNIX”
time stamps.

Let us go ahead and convert our `time` variable to a format more
umeaningful to us. To do so, we can use the `as.POSIXct()` function and
set our origin to January 1st, 1970 (this is not always the origin but
is usually a safe bet).

``` r
beer$time <- as.POSIXct(beer$time, origin="1970-01-01")

head(beer$time, 3)
```

    ## [1] "2002-02-11 15:31:03 EST" "2002-02-14 08:15:42 EST" "2002-02-17 09:37:52 EST"

Now we have actual dates and times that we can understand. While this
appears to have been a simple fix, [dealing with dates and times can get
very complicated](http://uc-r.github.io/dates/) and could be a post in
itself (and may well be down the line). So for now I have linked more
resources for formatting and using dates and times in `R` at the end.

#### Recoding mislabeled data

Another problem we will commonly run across is mislabeled data, either
mislabeled when the data was originally recorded or possibly during some
format conversion. As we can see in our data some of our mislabeling
probably fall in the later category. In particular, the “o” with an
umlaut (ö) in the name Tröegs is replaced with other characters. We
could try to fix this by adding back the ö, however we could continue to
run into the same issue down the road and it would be much easier to
just change it to a regular “o” for data analysis.

Also in the `tidyverse` package is a function called `recode_factor()`
which will help us rename our mislabeled factors. While we do not
necessarily need this package to do so, renaming factors in `R` is not
as straightforward or easy as we would generally like (remember they are
not Characters or Strings but a special class), so we will use
`recode_factor()` to simplify our work and our code.

``` r
beer$brewery <- recode_factor(beer$brewery,
                                   "TrÃ¶egs Brewing Company"="Troegs Brewing Company",
                                   "Yuengling Brewery "="Yuengling Brewery",
                                   "Yards Brewing Co."="Yards Brewing Company")
levels(beer$brewery)
```

    ## [1] "Troegs Brewing Company"  "Yuengling Brewery"       "Yards Brewing Company"   "Victory Brewing Company"
    ## [5] "Voodoo Brewing Company"

On top of changing the special characters that represent the “ö” to an
“o”, we also removed the space after “Yuengling Brewery” and spelled
out the full name for “Yards Brewing Company”. If we left the space at
the end of our factor name it could cause issues down the line if we try
to call that level by name, and so to save the headache we should just
change it now. Why spell out the full name for company and not just
leave the abbreviation? Just to keep things consistent since the other
levels had company spelled out. Alternatively, we could have abbreviated
the other levels to “Co.” to shorten the name with the same function.

Finally, we again use the `levels()` function to print out the name for
each level and see that our changes were successfully made.

While we fixed Tröegs in the `brewery` column, we still are not done
yet\! Tröegs likes to add their company name to their beer names as
well, which we can see if we take a look at all of the beer names in the
`name` variable. So, we can also replace the special characters with an
“o” in this column as well. However, if we used our code above we
would have to go through and change multiple beer names that include
Tröegs, which would be time consuming. Instead, we can use [pattern
matching](https://bookdown.org/rdpeng/rprogdatascience/regular-expressions.html)
to make our life (and code) simpler.

``` r
beer$name <- as.character(beer$name)
beer$name <- gsub("TrÃ¶egs", "Troegs", beer$name)
beer$name <- as.factor(beer$name)
```

We first use `sum()` and `grepl()` to add up each time “TrÃ¶egs” appears
in the column of beer names. Then, we change the variable to a
Character, use `gsub()` to match and replace each instance of “TrÃ¶egs”
with “Troegs”, then reassign the variable as a factor. Finally, we again
add up each time “TrÃ¶egs” appears to see that we completely converted
the misspelling.

If we use the above code, we first change the `name` variable from a
Factor to a Character then use the `gsub()` function to replace
“*TrÃ¶egs*” with “*Troegs*”. We then turn `name` back into a Factor.
Alternatively, we could use
[pipes](https://bookdown.org/rdpeng/rprogdatascience/managing-data-frames-with-the-dplyr-package.html#section)
(`%>%`) and [regular
expressions](https://rstudio.com/wp-content/uploads/2016/09/RegExCheatsheet.pdf)
to make coding a bit easier for us.

``` r
beer$name <- beer$name %>%
  as.character() %>%
  gsub(pattern="Tr..egs", replacement="Troegs") %>%
  as.factor()

sum(grepl("Tr..egs", as.character(beer$name))==TRUE)
```

    ## [1] 0

With pipes we only need to assign `beer$name` once then evaluate
through. Instead of including the specific “*Ã¶*” characters, we use
“*..*” which tells `R` that any character can be in that place.
Finally, we use `sum()` and `grepl()` commands to add up each time
“Tr..egs” is matched. Since it returned 0, we know we replaced all of
the “TrÃ¶egs”.

Now, let us take one “final” look at our data again using the
`summary()`
    function.

``` r
summary(beer)
```

    ##       time                                        brewery                              name     
    ##  Min.   :2002-02-11 15:31:03   Troegs Brewing Company : 646   HopDevil Ale               : 118  
    ##  1st Qu.:2006-12-27 09:01:46   Yuengling Brewery      : 186   Storm King Stout           : 116  
    ##  Median :2009-04-12 08:53:40   Yards Brewing Company  : 103   Prima Pils                 : 105  
    ##  Mean   :2008-08-30 21:34:59   Victory Brewing Company:1007   Hop Wallop                 :  89  
    ##  3rd Qu.:2010-09-10 07:53:46   Voodoo Brewing Company :  58   Troegs Nugget Nectar       :  88  
    ##  Max.   :2012-01-10 13:57:33                                  Yuengling Traditional Lager:  80  
    ##                                                               (Other)                    :1404  
    ##                             style           abv             aroma         appearance        palate          taste      
    ##  American IPA                  : 169   Min.   : 3.300   Min.   :1.000   Min.   :1.500   Min.   :1.000   Min.   :1.000  
    ##  American Amber / Red Ale      : 149   1st Qu.: 5.300   1st Qu.:3.500   1st Qu.:3.500   1st Qu.:3.500   1st Qu.:3.500  
    ##  German Pilsener               : 146   Median : 7.000   Median :4.000   Median :4.000   Median :4.000   Median :4.000  
    ##  Russian Imperial Stout        : 125   Mean   : 7.226   Mean   :3.812   Mean   :3.919   Mean   :3.847   Mean   :3.908  
    ##  American Double / Imperial IPA:  94   3rd Qu.: 8.700   3rd Qu.:4.000   3rd Qu.:4.000   3rd Qu.:4.000   3rd Qu.:4.500  
    ##  Belgian Strong Dark Ale       :  81   Max.   :15.000   Max.   :5.000   Max.   :5.000   Max.   :5.000   Max.   :5.000  
    ##  (Other)                       :1236                                                                                   
    ##      total          average     
    ##  Min.   : 5.00   Min.   :1.250  
    ##  1st Qu.:14.50   1st Qu.:3.625  
    ##  Median :16.00   Median :4.000  
    ##  Mean   :15.49   Mean   :3.872  
    ##  3rd Qu.:16.50   3rd Qu.:4.125  
    ##  Max.   :20.00   Max.   :5.000  
    ## 

From the summary table, it looks like we ahve cleaned up a lot of the
issues\! We fixed the spelling of Tröegs, removed duplicated rows, and
no longer have missing values. However, one thing to always keep in mind
is that the summary table does not show you everything\! For example,
let us take a look at the `style` column using the `levels()`
    function.

``` r
levels(beer$style)
```

    ##  [1] "Altbier"                             "American Adjunct Lager"              "American Amber / Red Ale"           
    ##  [4] "American Amber / Red Lager"          "American Barleywine"                 "American Black Ale"                 
    ##  [7] "American Blonde Ale"                 "American Brown Ale"                  "American Double / Imperial IPA"     
    ## [10] "American Double / Imperial Stout"    "American IPA"                        "American Pale Ale (APA)"            
    ## [13] "American Pale Lager"                 "American Porter"                     "American Stout"                     
    ## [16] "American Strong Ale"                 "American Wild Ale"                   "Baltic Porter"                      
    ## [19] "Belgian IPA"                         "Belgian Pale Ale"                    "Belgian Strong Dark Ale"            
    ## [22] "Belgian Strong Pale Ale"             "Black & Tan"                         "Bock"                               
    ## [25] "California Common / Steam Beer"      "Czech Pilsener"                      "Doppelbock"                         
    ## [28] "Dortmunder / Export Lager"           "Dubbel"                              "Dunkelweizen"                       
    ## [31] "Eisbock"                             "English Bitter"                      "English Brown Ale"                  
    ## [34] "English Dark Mild Ale"               "English India Pale Ale (IPA)"        "English Pale Mild Ale"              
    ## [37] "English Porter"                      "English Stout"                       "English Strong Ale"                 
    ## [40] "Extra Special / Strong Bitter (ESB)" "German Pilsener"                     "Hefeweizen"                         
    ## [43] "Herbed / Spiced Beer"                "Irish Dry Stout"                     "KÃ¶lsch"                            
    ## [46] "Keller Bier / Zwickel Bier"          "Lambic - Fruit"                      "Light Lager"                        
    ## [49] "MÃ¤rzen / Oktoberfest"               "Maibock / Helles Bock"               "Milk / Sweet Stout"                 
    ## [52] "Munich Dunkel Lager"                 "Munich Helles Lager"                 "Oatmeal Stout"                      
    ## [55] "Pumpkin Ale"                         "Quadrupel (Quad)"                    "Rauchbier"                          
    ## [58] "Roggenbier"                          "Russian Imperial Stout"              "Rye Beer"                           
    ## [61] "Saison / Farmhouse Ale"              "Smoked Beer"                         "Tripel"                             
    ## [64] "Vienna Lager"                        "Weizenbock"                          "Winter Warmer"                      
    ## [67] "Witbier"

Uh oh, we have a few more instances where special characters did not
format correctly with both K\&oulm;lsch and M\&oulm;rzen. We could go
ahead and fix those as well, but would it really matter? If we are
specifically interested in those two types of beer, or if we plan on
displaying those beer styles in some figure or table then yes, we
probably should correct them as well. However, what if we are not
interested in beer styles? Or at least not observing those two styles
specifically? Then maybe it is not worth our time fixing them and we can
leave them be. But since we are here to learn about data cleaning in
`R`, maybe it is worth our time and we can use the code below, similar
to how we worked the same problem above. And this time, we will just use
pipes and regular expressions to make things (ideally) easier on us.

``` r
beer$style <- beer$style %>%
  as.character() %>%
  gsub(pattern="M..rzen", replacement="Marzen") %>%
  gsub(pattern="K..lsch", replacement="Kolsch") %>%
  as.factor()

levels(beer$style)[c(50,54)]
```

    ## [1] "Marzen / Oktoberfest" "Oatmeal Stout"

As you can see above, we used the pipe function to first change the
column to the Character class, then correct Marzen followed by Kolsch,
then returned the column back to a Factor. By using the brackets `[]`
around the `c()` function after the `levels()` function we can tell `R`
to only return our indices of interest rather than reprinting the entire
list of 72 levels (keep in mind that after changing the Kolsch and
Marzen text their level index slightly changed to 50 and 54 from their
original indices of 49 and 53).

But what about `name`? Surely some of these beers (and there are\!).
Then we could reuse the piped code above but replace `style` with `name`
after the `$`.

Now that we have a high quality dataset (or at least good enough for
now) we should save it as a new file.

``` r
write.csv(beer, "data/beer_cleaned.csv", row.names=FALSE)
```

Here, we are setting the `row.names` function to `FALSE`. Otherwise,
when we reload this data into `R` in the future we will again get a
column titled `X` that lists the number of each row.

Are we really finished with data cleaning? Probably not. We will see
when we continue with Exploratory Data Analysis (EDA) we may find
additional issues with the data that we need to correct.

### Other Information and Resources

Code was generated in R version 3.6.3 (2020-02-29) – “Holding the
Windsock”

Data adapted from: <https://www.kaggle.com/rdoume/beerreviews>

[Cleaning
data](https://www.theanalysisfactor.com/preparing-data-analysis/)

[Basics on working with data frames in
`R`](https://www.r-bloggers.com/15-easy-solutions-to-your-data-frame-problems-in-r/)

[Data frame
management](https://bookdown.org/rdpeng/rprogdatascience/managing-data-frames-with-the-dplyr-package.html)

[Data transformation](https://r4ds.had.co.nz/transform.html)

[Using lapply](https://www.r-bloggers.com/the-lapply-command-101/)

[Loop functions in
`R`](https://bookdown.org/rdpeng/rprogdatascience/loop-functions.html)

[Pipes in `R`](https://r4ds.had.co.nz/pipes.html)

[Ways to deal with missing
data](https://measuringu.com/handle-missing-data/)

[Using Dates and Times in
`R`](https://www.r-bloggers.com/using-dates-and-times-in-r/)

[Date Formats in `R`](https://www.r-bloggers.com/date-formats-in-r/)

[Dates and Times in `R`](https://www.stat.berkeley.edu/~s133/dates.html)

[Data cleaning in python with
Pandas](https://www.dataoptimal.com/data-cleaning-with-python-2018/)

[Data cleaning in `R` with
tidyverse](https://towardsdatascience.com/data-cleaning-with-r-and-the-tidyverse-detecting-missing-values-ea23c519bc62)
