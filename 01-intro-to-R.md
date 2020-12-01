------------------------------------------------------------------------

> Learning Objectives
> -------------------
>
> -   load external data (CSV files) in memory using the toxin table
>     (`surveys.csv`) as an example
> -   explore the structure and the content of a data frame in R
> -   understand what factors are and how to manipulate them
> -   understand the concept of a `data.frame`
> -   use sequences
> -   know how to access any element of a `data.frame`

------------------------------------------------------------------------

Basics of R
-----------

R is a versatile, open source programming/scripting language that’s
useful both for statistics but also data science. Inspired by the
programming language S.

-   Free/Libre/Open Source Software under the GPL.
-   Superior (if not just comparable) to commercial alternatives. R has
    over 7,000 user contributed packages at this time. It’s widely used
    both in academia and industry.
-   Available on all platforms.
-   Not just for statistics, but also general purpose programming.
-   For people who have experience in programming: R is both an
    object-oriented and a so-called [functional
    language](http://adv-r.had.co.nz/Functional-programming.html).
-   Large and growing community of peers.

Presentation of RStudio
-----------------------

Let’s start by learning about RStudio, the Integrated Development
Environment (IDE) that we will use to write code, navigate the files
found on our computer, inspect the variables we are going to create, and
visualize the plots we will generate. RStudio can also be used for other
things (e.g., version control, developing packages) that we will not
cover during this introductory class.

RStudio is divided into 4 “Panes”: the editor for your scripts and
documents (top-left), the R console (bottom-left), your
environment/history (top-right), and your
files/plots/packages/help/viewer (bottom-right). The placement of these
panes and their content can be customized.

R as a calculator
-----------------

The lower-left pane (the R “console”) is where you can interact with R
directly. The `>` sign is the R “prompt”. It indicates that R is waiting
for you to type something.

You can type <kbd>`Ctrl`</kbd> + <kbd>`Shift`</kbd> + <kbd>`2`</kbd> to
focus just on the R console pane. Use <kbd>`Ctrl`</kbd> +
<kbd>`Shift`</kbd> + <kbd>`0`</kbd> to get back to the four panes. I use
this when teaching but not otherwise.

Let’s start by subtracting a couple of numbers.

``` r
2020 - 1979
```

    ## [1] 41

R does the calculation and prints the result, and then you get the `>`
prompt again. (The `[1]` in the results is a bit weird; you can ignore
that for now.)

You can use R as a calculator in this way.

``` r
4*6
4/6
4^6
log(4)
log10(4)
```

Need for Scripts
----------------

We can go along, typing directly into the R console. But there won’t be
an easy way to keep track of what we’ve done.

It’s best to write R “scripts” (files with R code), and work from them.
And when we start creating scripts, we need to worry about how we
organize the scripts and data for a project.

So let’s pause for a moment and talk about file organization.

Creating an R Project
---------------------

It is good practice to keep a set of related data, analyses, and text
self-contained in a single folder, called the **working directory**. All
of the scripts within this folder can then use *relative paths* to files
that indicate where inside the project a file is located (as opposed to
absolute paths, which point to where a file is on a specific computer).
Working this way makes it a lot easier to move your project around on
your computer and share it with others without worrying about whether or
not the underlying scripts will still work.

RStudio provides a helpful set of tools to do this through its
“Projects” interface, which can create a working directory for you (or
use an existing one) and also remembers its location (allowing you to
quickly navigate to it) and optionally preserves custom settings and
open files to make it easier to resume work after a break. Below, we
will go through the steps for creating an RProject for this tutorial.

I recommend that you create a `StatsClass-IntroToR` folder on your
computer that you will use for today.

-   Start RStudio
-   Under the `File` menu, click on `New project`, choose
    `Existing directory`, then click the “`Browse`” button and find your
    `StatsClass-IntroToR` folder.
-   Click on “Create project”
-   Create a new R script (File &gt; New File &gt; R script) and save it
    in a `code` subfolder (e.g. `~/Documents/StatsClass-IntroToR/code`)

Interacting with R
==================

While you can type R commands directly at the `>` prompt in the R
console, I recommend typing your commands into a script, which you’ll
save for later reference, and then executing the commands from there.

Start by typing the following into the R script in the top-left pane.

``` r
# R intro

2020 - 1979
```

NOTE: I will use <kbd>`Ctrl`</kbd> in the instructions below. Most
often, this is true for PCs and <kbd>`command`</kbd> should be used in
its place for Macs.

Save the file clicking the computer disk icon, or by typing
<kbd>`Ctrl`</kbd> + <kbd>`S`</kbd>.

Now place the cursor on the line with `2020 - 1979` and type
<kbd>`Ctrl`</kbd> + <kbd>`Enter`</kbd>. The command will be copied to
the R console and executed, and then the cursor will move to the next
line.

You can also highlight a bunch of code and execute the block all at once
with <kbd>`Ctrl`</kbd> + <kbd>`Enter`</kbd>.

### Commenting

Use `#` signs to comment. Anything to the right of a `#` is ignored by
R, meaning it won’t be executed. Comments are a great way to describe
what your code does within the code itself, so comment liberally in your
R scripts.

Plunge straight into the data about fungus toxins from Chapter 15
-----------------------------------------------------------------

A drug precursor molecule is extracted from a type of nut. The nuts are
commonly contaminated by a fungal toxin that is difficult to remove
during the purification process. We suspect that the amount of fungus
(and hence toxin) depends on multiple factors related to the growing
site.

The dataset is stored as a CSV file: each row holds information for a
single site, and the columns represent:

| Column      | Description                                    |
|-------------|------------------------------------------------|
| lot         | Unique id for the particular growing site      |
| rain        | average weekly rainfall in cm                  |
| noon\_temp  | average temperature at noon in degrees Celsius |
| sunshine    | average hours of sunlight per day              |
| wind\_speed | average wind speed in kilometers per hour      |
| toxin       | level of toxin in micrograms per 100 grams     |
| town        | nearest town                                   |

The data are available at
<a href="https://github.com/SabaLab/Intro-To-R/blob/main/data/Chapter15_toxicFungus.csv" class="uri">https://github.com/SabaLab/Intro-To-R/blob/main/data/Chapter15_toxicFungus.csv</a>.

Save the csv file in your `StatsClass-IntroToR` folder in a new
subfolder called `data`.

Now, you can read your data into R.

``` r
toxin <- read.csv("data/Chapter15_toxicFungus.csv") 
```

### Functions

`read.csv` is a “function”. It does something, and “returns” some
result.

The file name in quotes is called the function “argument”: the
function’s “input”.

`read.csv` will read in the data from that file, and then spit the data
back out.

### Assignment operator

The `<-` symbol is used to assign the data to an object. In other words,
it gives the data a name.

You can also use `=` as assignment, but that symbol can have other
meanings, and so I recommend sticking with the `<-` combination.

In RStudio, typing <kbd>Alt</kbd> + <kbd>-</kbd> will write `<-` in a
single keystroke.

### Objects in your workspace

The objects you create get added to your “workspace”. You can list the
current objects with `ls()`.

``` r
ls()
```

RStudio also shows the objects in the Environment panel.

### Object names

Objects can be given any name such as `x`, `current_temperature`, or
`subject_id`. You want your object names to be explicit and not too
long.

They cannot start with a number (`2x` is not valid, but `x2` is).

R is case sensitive (e.g., `weight_kg` is different from `Weight_kg`).
There are some names that cannot be used because they are the names of
fundamental functions in R (e.g., `if`, `else`, `for`, see
[here](https://stat.ethz.ch/R-manual/R-devel/library/base/html/Reserved.html)
for a complete list).

In general, even if it’s allowed, it’s best to not use other function
names (e.g., `c`, `T`, `mean`, `data`, `df`, `weights`). If in doubt,
type the name to see if it’s already in use.

It’s also best to avoid dots (`.`) within a variable name as in
`my.dataset`. There are many functions in R with dots in their names for
historical reasons, but because dots have a special meaning in R (for
methods) and other programming languages, it’s best to avoid them. It is
also recommended to use nouns for variable names, and verbs for function
names. It’s important to be consistent in the styling of your code
(where you put spaces, how you name variable, etc.). In R, two popular
style guides are [Hadley Wickham’s](http://adv-r.had.co.nz/Style.html),
[tidyverse’s](https://style.tidyverse.org/), and
[Google’s](https://google.github.io/styleguide/Rguide.xml).

### Getting help

If you type `read.csv` and pause for a moment, you’ll get a pop-up with
information about the function.

Alternatively, you could type

``` r
?read.csv
```

and the documentation for the function will show up in the lower-right
pane. These are often a bit *too* detailed, and so they take some
practice to read. I generally focus on Usage and Arguments, and then on
Examples at the bottom.

Data frames
-----------

The data are stored in what’s called a “data frame”. It’s a big
rectangle, with rows being observations and columns being variables. The
different columns can be different types (numeric, character, etc.), but
they’re all the same length.

Use `head()` to view the first few rows.

``` r
head(toxin)
```

Use `tail()` to view the last few rows.

``` r
tail(toxin)
```

Use `str()` to look at the structure of the data.

``` r
str(toxin)
```

This shows that there are 10 rows and 7 columns, and then for each
column, it gives information about the data type and shows the first few
values.

For these data, the columns have two types: integer, or “Factor”. Factor
columns are text with a discrete set of possible values. We’ll come back
to this in a bit.

### Challenge

Study the output of `str(toxin)`. How are the missing values being
treated?

<!-- end challenge -->
### Another summary

Another useful function in `summary()`.

``` r
summary(toxin)
```

For the numeric data, you get six statistics (min, 1st quartile (25th
percentile), median, mean, 3rd quartile (75th percentile), and max). For
the factors, you get a table with counts for the most-frequent “levels”,
and then for the rest.

Inspecting data frames
----------------------

We already saw how the functions `head()` and `str()` can be useful to
check the content and the structure of a `data.frame`. Here is a
non-exhaustive list of functions to get a sense of the content/structure
of the data.

-   Size:
    -   `dim()` - returns a vector with the number of rows in the first
        element, and the number of columns as the second element (the
        \_\_dim\_\_ensions of the object)
    -   `nrow()` - returns the number of rows
    -   `ncol()` - returns the number of columns
-   Content:
    -   `head()` - shows the first 6 rows
    -   `tail()` - shows the last 6 rows
-   Names:
    -   `names()` - returns the column names (synonym of `colnames()`
        for `data.frame` objects)
    -   `rownames()` - returns the row names
-   Summary:
    -   `str()` - structure of the object and information about the
        class, length and content of each column
    -   `summary()` - summary statistics for each column

Note: most of these functions are “generic”, they can be used on other
types of objects besides `data.frame`.

Indexing, Sequences, and Subsetting
-----------------------------------

We can pull out parts of a data frame using square brackets. We need to
provide two values: row and column, with a comma between them.

For example, to get the element in the 1st row, 1st column:

``` r
toxin[1,1]
```

To get the element in the 2nd row, 7th column:

``` r
toxin[2,7]
```

To get the entire 2nd row, leave the column part blank:

``` r
toxin[2,]
```

And to get the entire 7th column, leave the row part blank:

``` r
town <- toxin[,7]
```

You can also refer to columns by name, in multiple ways.

``` r
town <- toxin[, "town"]
town <- toxin$town
town <- toxin[["town"]]
```

When we pull out a single column, the result is a “vector”. We can again
use square brackets to pull out individual values, but providing a
single number.

``` r
town[1]
town[10]
```

### Slices

You can pull out larger slices from the vector by providing vectors of
indices. You can use the `c()` function to create a vector.

``` r
c(1,3,5)
town[c(1,3,5)]
```

To pull out larger slices, it’s helpful to have ways of creating
sequences of numbers.

First, the operator `:` gives you a sequence of consecutive values.

``` r
1:10
10:1
5:8
```

`seq` is more flexible.

``` r
seq(1, 10, by=2)
seq(5, 10, length.out=3)
seq(50, by=5, length.out=10)
seq(1, 8, by=3) # sequence stops to stay below upper limit
seq(10, 2, by=-2)  # can also go backwards
```

To get slices of our data frame, we can include a vector for the row or
column indexes (or both)

``` r
toxin[1:3, 7]   # first three elements in the 7th column
toxin[1, 1:3]   # first three columns in the first row
toxin[2:4, 6:7] # rows 2-4, columns 6-7
```

### Challenge

The function `nrow()` on a `data.frame` returns the number of rows.

Use `nrow()`, in conjunction with `seq()` to create a new `data.frame`
called `toxin_by_2` that includes every other row of the toxin data
frame starting at row 2 (2, 4, 6, …)

<!-- end challenge -->
Missing data
------------

As R was designed to analyze datasets, it includes the concept of
missing data (which is uncommon in other programming languages). Missing
data are represented in vectors as `NA`.

``` r
heights <- c(2, 4, 4, NA, 6)
```

When doing operations on numbers, most functions will return `NA` if the
data you are working with include missing values. It is a safer behavior
as otherwise you may overlook that you are dealing with missing data.
You can add the argument `na.rm=TRUE` to calculate the result while
ignoring the missing values.

``` r
mean(heights)
max(heights)
mean(heights, na.rm = TRUE)
max(heights, na.rm = TRUE)
```

If your data include missing values, you may want to become familiar
with the functions `is.na()` and `na.omit()`.

``` r
## Extract those elements which are not missing values.
heights[!is.na(heights)]
## shortcut to that
na.omit(heights)
```

Factors
-------

Factors are used to represent categorical data. Factors can be ordered
or unordered, and understanding them is necessary for statistical
analysis and for plotting.

Factors are stored as integers, and have labels associated with these
unique integers. While factors look (and often behave) like character
vectors, they are actually integers under the hood, and you need to be
careful when treating them like strings.

Once created, factors can only contain a pre-defined set of values,
known as *levels*. By default, R always sorts *levels* in alphabetical
order. For instance, if you use `factor()` to create a factor with 2
levels:

``` r
sex <- factor(c("male", "female", "female", "male"))
```

R will assign `1` to the level `"female"` and `2` to the level `"male"`
(because `f` comes before `m`, even though the first element in this
vector is `"male"`). You can check this by using the function
`levels()`, and check the number of levels using `nlevels()`:

``` r
levels(sex)
nlevels(sex)
```

Sometimes, the order of the factors does not matter, other times you
might want to specify a particular order.

``` r
food <- factor(c("low", "high", "medium", "high", "low", "medium", "high"))
levels(food)
food <- factor(food, levels=c("low", "medium", "high"))
levels(food)
```

### Converting factors

If you need to convert a factor to a character vector, you use
`as.character(x)`.

Converting factors where the levels appear as numbers (such as
concentration levels) to a numeric vector is a little trickier. One
method is to convert factors to characters and then numbers. function.
Compare:

``` r
f <- factor(c(1, 5, 10, 2))
as.numeric(f)               ## wrong! and there is no warning...
as.numeric(as.character(f)) ## works...
```

### Challenge

The function `table()` tabulates observations.

``` r
expt <- c("treat1", "treat2", "treat1", "treat3", "treat1",
          "control", "treat1", "treat2", "treat3")
expt <- factor(expt)
table(expt)
```

-   In which order are the treatments listed?
-   How can you recreate this table with “`control`” listed last instead
    of first?

<!-- end challenge -->
<!---

```r
## Answers
##
## * The treatments are listed in alphabetical order because they are factors.
## * By redefining the order of the levels
expt <- c("treat1", "treat2", "treat1", "treat3", "treat1",
          "control", "treat1", "treat2", "treat3")
expt <- factor(expt, levels=c("treat1", "treat2", "treat3", "control"))
table(expt)
```
--->
### stringsAsFactors

The default when reading in data with `read.csv()`, columns with text
get turned into factors.

You can avoid this with the argument `stringsAsFactors=FALSE`.

``` r
toxin_chr <- read.csv("data/Chapter15_toxicFungus.csv", stringsAsFactors=FALSE)
```

Then when you look at the result of `str()`, you’ll see that the
previously factor columns are now `chr`.

``` r
str(toxin_chr)
```
