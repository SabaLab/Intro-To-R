------------------------------------------------------------------------

# Data Manipulation using dplyr

Bracket subsetting is handy, but it can be cumbersome and difficult to
read, especially for complicated operations. Enter `dplyr`. `dplyr` is a
package for making data manipulation easier.

Packages in R are basically sets of additional functions that let you do
more stuff. The functions we’ve been using so far, like `str()` or
`data.frame()`, come built into R; packages give you access to more of
them. Before you use a package for the first time you need to install it
on your machine, and then you should to import it in every subsequent R
session when you’ll need it.

``` r
install.packages("dplyr")
```

While we’re installing stuff, let’s also install the ggplot2 package,
which we’ll use next.

``` r
install.packages("ggplot2")
```

You might get asked to choose a CRAN mirror – this is basically asking
you to choose a site to download the package from. The choice doesn’t
matter too much; we recommend the RStudio mirror.

``` r
library(dplyr)    ## load the package
```

## What is `dplyr`?

The package `dplyr` provides easy tools for the most common data
manipulation tasks. It is built to work directly with data frames. The
thinking behind it was largely inspired by the package `plyr` which has
been in use for some time but suffered from being slow in some cases.
`dplyr` addresses this by porting much of the computation to C++. An
additional feature is the ability to work with data stored directly in
an external database. The benefits of doing this are that the data can
be managed natively in a relational database, queries can be conducted
on that database, and only the results of the query returned.

This addresses a common problem with R in that all operations are
conducted in memory and thus the amount of data you can work with is
limited by available memory. The database connections essentially remove
that limitation in that you can have a database of many 100s GB, conduct
queries on it directly, and pull back just what you need for analysis in
R.

### Selecting columns and filtering rows

We’re going to learn some of the most common `dplyr` functions:
`select()`, `filter()`, `mutate()`, `group_by()`, and `summarize()`. To
select columns of a data frame, use `select()`. The first argument to
this function is the data frame (`toxin`), and the subsequent arguments
are the columns to keep.

``` r
selected_col <- select(toxin, lot, rain, toxin)
head(selected_col)
```

To choose rows, use `filter()`:

``` r
toxin_muchRain <- filter(toxin, rain>1)
head(toxin_muchRain)
```

    ##   lot rain noon_temp sunshine wind_speed toxin       town
    ## 1   A 1.30      20.9     6.23       13.3  18.1 Rocky Ford
    ## 2   B 2.28      25.4     8.13       10.8  28.6 Rocky Ford
    ## 3   C 1.11      28.2    10.21       10.9  15.9   La Junta
    ## 4   E 1.32      26.5     9.04        9.8  19.3     Fowler
    ## 5   G 1.56      26.7     6.69       10.0  21.7     Pueblo
    ## 6   H 1.32      30.0     8.30       12.2  16.5     Pueblo

### Pipes

The *pipe* operator (`%>%`) from the magrittr package makes it easy to
chain these actions together: the output of one function becomes the
input of the next.

``` r
toxin %>%
  filter(rain > 1) %>%
  select(lot, rain, toxin)
```

    ##   lot rain toxin
    ## 1   A 1.30  18.1
    ## 2   B 2.28  28.6
    ## 3   C 1.11  15.9
    ## 4   E 1.32  19.3
    ## 5   G 1.56  21.7
    ## 6   H 1.32  16.5
    ## 7   I 2.05  23.8
    ## 8   J 1.37  19.0

Another cumbersome bit of typing. In RStudio, type <kbd>`Ctrl`</kbd> +
<kbd>`Shift`</kbd> + <kbd>`M`</kbd> and the `%>%` operator will be
inserted.

In the above we use the pipe to send the `toxin` data set first through
`filter`, to keep rows where `rain` is greater than 1 cm/week, and then
through `select` to keep the `rain` and `toxin` columns. When the data
frame is being passed to the `filter()` and `select()` functions through
a pipe, we don’t need to include it as an argument to these functions
anymore.

If we wanted to create a new object with this smaller version of the
data we could do so by assigning it a new name:

``` r
toxin_sml <- toxin %>%
  filter(rain > 1) %>%
  select(lot, rain, toxin)
```

Note that the final data frame is the leftmost part of this expression.

### Challenge

Using pipes, subset the data to include lots from `La Junta`, and retain
the columns `lot`, `town`, and `toxin`.

<!-- end challenge -->

### Mutate

Frequently you’ll want to create new columns based on the values in
existing columns, for example to do unit conversions, or find the ratio
of values in two columns. For this we’ll use `mutate()`.

To create a new column of rain in meters instead of centimeters:

``` r
toxin %>%
  mutate(rain_m = rain / 100)
```

    ##    lot rain noon_temp sunshine wind_speed toxin       town rain_m
    ## 1    A 1.30      20.9     6.23       13.3  18.1 Rocky Ford 0.0130
    ## 2    B 2.28      25.4     8.13       10.8  28.6 Rocky Ford 0.0228
    ## 3    C 1.11      28.2    10.21       10.9  15.9   La Junta 0.0111
    ## 4    D 0.74      23.7     6.96         NA  19.2   La Junta 0.0074
    ## 5    E 1.32      26.5     9.04        9.8  19.3     Fowler 0.0132
    ## 6    F 0.51        NA     7.84       12.3  14.8     Fowler 0.0051
    ## 7    G 1.56      26.7     6.69       10.0  21.7     Pueblo 0.0156
    ## 8    H 1.32      30.0     8.30       12.2  16.5     Pueblo 0.0132
    ## 9    I 2.05      24.9     9.22       10.7  23.8      Swink 0.0205
    ## 10   J 1.37      22.0     8.37         NA  19.0     Ordway 0.0137

If this runs off your screen and you just want to see the first few
rows, you can use a pipe to view the `head()` of the data (pipes work
with non-dplyr functions too, as long as the `dplyr` or `magrittr`
packages are loaded).

``` r
toxin %>%
  mutate(rain_m = rain / 100) %>%
  head
```

    ##   lot rain noon_temp sunshine wind_speed toxin       town rain_m
    ## 1   A 1.30      20.9     6.23       13.3  18.1 Rocky Ford 0.0130
    ## 2   B 2.28      25.4     8.13       10.8  28.6 Rocky Ford 0.0228
    ## 3   C 1.11      28.2    10.21       10.9  15.9   La Junta 0.0111
    ## 4   D 0.74      23.7     6.96         NA  19.2   La Junta 0.0074
    ## 5   E 1.32      26.5     9.04        9.8  19.3     Fowler 0.0132
    ## 6   F 0.51        NA     7.84       12.3  14.8     Fowler 0.0051

The sixth row has an NA for noon_temp, so if we wanted to remove all
observations with a missing value for noon_temp we could insert a
`filter()` in this chain:

``` r
toxin %>%
  filter(!is.na(noon_temp)) %>%
  mutate(rain_m = rain / 100) %>%
  head
```

    ##   lot rain noon_temp sunshine wind_speed toxin       town rain_m
    ## 1   A 1.30      20.9     6.23       13.3  18.1 Rocky Ford 0.0130
    ## 2   B 2.28      25.4     8.13       10.8  28.6 Rocky Ford 0.0228
    ## 3   C 1.11      28.2    10.21       10.9  15.9   La Junta 0.0111
    ## 4   D 0.74      23.7     6.96         NA  19.2   La Junta 0.0074
    ## 5   E 1.32      26.5     9.04        9.8  19.3     Fowler 0.0132
    ## 6   G 1.56      26.7     6.69       10.0  21.7     Pueblo 0.0156

`is.na()` is a function that determines whether something is or is not
an `NA`. The `!` symbol negates it, so we’re asking for everything that
is not an `NA`.

### Challenge

Create a new dataframe from the toxin data that meets the following
criteria:

-   contains only the `lot` column and a column that contains values
    that are the square-root of `wind_speed` values (e.g. a new column
    `wind_sqrt`).
-   In this `wind_sqrt` column, there are no NA values and all values
    are \< 3.5.

Hint: think about how the commands should be ordered

<!-- end challenge -->

### Split-apply-combine data analysis and the summarize() function

Many data analysis tasks can be approached using the
“split-apply-combine” paradigm: split the data into groups, apply some
analysis to each group, and then combine the results. `dplyr` makes this
very easy through the use of the `group_by()` function. `group_by()`
splits the data into groups upon which some operations can be run. For
example, if we wanted to group by town and find the number of rows of
data for each town, we would do:

``` r
toxin %>%
  group_by(town) %>%
  tally()
```

    ## # A tibble: 6 × 2
    ##   town           n
    ##   <chr>      <int>
    ## 1 Fowler         2
    ## 2 La Junta       2
    ## 3 Ordway         1
    ## 4 Pueblo         2
    ## 5 Rocky Ford     2
    ## 6 Swink          1

Here, `tally()` is the action applied to the groups created to
`group_by()` and counts the total number of records for each category.
`group_by()` is often used together with `summarize()` which collapses
each group into a single-row summary of that group. So to view mean
`toxin` by town:

``` r
toxin %>%
  group_by(town) %>%
  summarize(mean_toxin = mean(toxin, na.rm = TRUE))
```

    ## # A tibble: 6 × 2
    ##   town       mean_toxin
    ##   <chr>           <dbl>
    ## 1 Fowler           17.0
    ## 2 La Junta         17.6
    ## 3 Ordway           19  
    ## 4 Pueblo           19.1
    ## 5 Rocky Ford       23.4
    ## 6 Swink            23.8

Weird warning message: `summarise()` ungrouping output (override with
`.groups` argument)

Solution:
<https://rstats-tips.net/2020/07/31/get-rid-of-info-of-dplyr-when-grouping-summarise-regrouping-output-by-species-override-with-groups-argument/>

``` r
# Suppress summarise info
options(dplyr.summarise.inform = FALSE)
```

``` r
toxin %>%
  group_by(town) %>%
  summarize(mean_toxin = mean(toxin, na.rm = TRUE))
```

    ## # A tibble: 6 × 2
    ##   town       mean_toxin
    ##   <chr>           <dbl>
    ## 1 Fowler           17.0
    ## 2 La Junta         17.6
    ## 3 Ordway           19  
    ## 4 Pueblo           19.1
    ## 5 Rocky Ford       23.4
    ## 6 Swink            23.8

Another thing we might do here is sort rows by `mean_toxin`, using
`arrange()`.

``` r
toxin %>%
  group_by(town) %>%
  summarize(mean_toxin = mean(toxin, na.rm = TRUE)) %>%
  arrange(mean_toxin)
```

    ## # A tibble: 6 × 2
    ##   town       mean_toxin
    ##   <chr>           <dbl>
    ## 1 Fowler           17.0
    ## 2 La Junta         17.6
    ## 3 Ordway           19  
    ## 4 Pueblo           19.1
    ## 5 Rocky Ford       23.4
    ## 6 Swink            23.8

If you want them sorted from highest to lowest, use `desc()`.

``` r
toxin %>%
  group_by(town) %>%
  summarize(mean_toxin = mean(toxin, na.rm = TRUE)) %>%
  arrange(desc(mean_toxin))
```

    ## # A tibble: 6 × 2
    ##   town       mean_toxin
    ##   <chr>           <dbl>
    ## 1 Swink            23.8
    ## 2 Rocky Ford       23.4
    ## 3 Pueblo           19.1
    ## 4 Ordway           19  
    ## 5 La Junta         17.6
    ## 6 Fowler           17.0

Also note that you can include multiple summaries.

``` r
toxin %>%
  group_by(town) %>%
  summarize(mean_toxin = mean(toxin, na.rm = TRUE),
            min_toxin = min(toxin, na.rm=TRUE)) %>%
  arrange(desc(mean_toxin))
```

    ## # A tibble: 6 × 3
    ##   town       mean_toxin min_toxin
    ##   <chr>           <dbl>     <dbl>
    ## 1 Swink            23.8      23.8
    ## 2 Rocky Ford       23.4      18.1
    ## 3 Pueblo           19.1      16.5
    ## 4 Ordway           19        19  
    ## 5 La Junta         17.6      15.9
    ## 6 Fowler           17.0      14.8

### Challenge

Use `group_by()` and `summarize()` to find the mean, min, and max
noon_temp length for each town

<!-- end challenge -->

### A bit of data cleaning

In preparations for the plotting, let’s do a bit of data cleaning:
remove rows with missing `wind_speed` or `noon_temp`.

``` r
toxin_complete <- toxin %>%
    filter(!is.na(wind_speed), !is.na(noon_temp)) 
```

There are two lots with `noon_temp` less than 25 degrees Celsius. Let’s
remove lots an average noon temperature less than 25 degrees.

``` r
reduced <- toxin_complete %>%
    filter(noon_temp>25)
```

We might save this to a file:

``` r
write.csv(reduced, "data/reduced_toxin.csv")
```

[Handy dplyr
cheatsheet](http://www.rstudio.com/wp-content/uploads/2015/02/data-wrangling-cheatsheet.pdf)
