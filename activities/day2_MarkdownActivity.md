Chunk and output options in R Markdown
================
ECON 122
Day 2

**Disclaimer** This .Rmd lacks the usual global chunk options that
most/all of my .Rmd files have. This was done to allow you to explore
how these options work when compared to the default settings. Since we
are exploring R chunk options, to get the most out of this activity you
should view the .Rmd file and see the output produced. Just viewing the
html is not all that useful\!

## 1\. Chunk output options

Here we read in the German credit data and show the breakdown of
good/bad loans but don’t show the commands:

    ## 
    ##  BadLoan GoodLoan 
    ##      300      700

Here you show commands that use the `test-assignment` version of the
data, but you don’t actually evalate (run) the
commands:

``` r
loans <- read.csv("http://raw.githubusercontent.com/mgelman/data/master/day1CreditData.csv")
table(loans$Good.Loan)
```

The next command we run does evaluate the code. Notice that `loans` is
still the full 1000 case dataset, not the 700 case day 1 data.
**Why?\!**

``` r
table(loans$Good.Loan)
```

    ## 
    ##  BadLoan GoodLoan 
    ##      300      700

The default chunk options both show code and output.

Notice that the first two chunks are named. We can rerun code in a named
chunk just by creating a chunk with the same name but **no** code
included. **What happens if we reuse the second chunk, named
`day1creditdata`, but this time evaluate the
code?**

``` r
loans <- read.csv("http://raw.githubusercontent.com/mgelman/data/master/day1CreditData.csv")
table(loans$Good.Loan)
```

    ## 
    ##  BadLoan GoodLoan 
    ##      218      482

Notice that we **did not** need to repeat the code in the
`day1creditdata` chunk\!

Let’s return the dataset `loans` to contain full credit dataset. We can
reuse the first code chunk but not include any output:

**What chunk options would you set so you only show a boxplot but not
the code used to create it?**

## 2\. Chunk formatting

The default output look for R chunks highlights code in gray (without
`>` prompts), denotes output with two hashtags `##`:

``` r
summary(loans$Duration.in.month)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##     4.0    12.0    18.0    20.9    24.0    72.0

``` r
tapply(loans$Duration.in.month, loans$Good.Loan, summary)
```

    ## $BadLoan
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##    6.00   12.00   24.00   24.86   36.00   72.00 
    ## 
    ## $GoodLoan
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##    4.00   12.00   18.00   19.21   24.00   60.00

Here we can supress the output hashtags with `comment=NULL`:

``` r
summary(loans$Duration.in.month)
```

``` 
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    4.0    12.0    18.0    20.9    24.0    72.0 
```

``` r
tapply(loans$Duration.in.month, loans$Good.Loan, summary)
```

    $BadLoan
       Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
       6.00   12.00   24.00   24.86   36.00   72.00 
    
    $GoodLoan
       Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
       4.00   12.00   18.00   19.21   24.00   60.00 

**What happens if you set `comment=";)"`?**

Here we can add `>` in front of code with `prompt=TRUE`:

``` r
> summary(loans$Duration.in.month)
```

``` 
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    4.0    12.0    18.0    20.9    24.0    72.0 
```

``` r
> tapply(loans$Duration.in.month, loans$Good.Loan, summary)
```

    $BadLoan
       Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
       6.00   12.00   24.00   24.86   36.00   72.00 
    
    $GoodLoan
       Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
       4.00   12.00   18.00   19.21   24.00   60.00 

Here we can collapse all commands and output in a chunk into one gray
command/output field with `collapse=TRUE`:

``` r
> summary(loans$Duration.in.month)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    4.0    12.0    18.0    20.9    24.0    72.0 
> tapply(loans$Duration.in.month, loans$Good.Loan, summary)
$BadLoan
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
   6.00   12.00   24.00   24.86   36.00   72.00 

$GoodLoan
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
   4.00   12.00   18.00   19.21   24.00   60.00 
```

## 3\. Inline R code

You can use inline R code chunks to place R numerical outout in your
text. The full German credit dataset contains 1000 cases. The mean loan
duration of these cases is 20.903 months.

You can use the `round` command to get a sensible number of digits
(duration is integer valued). The mean loan duration of these cases is
20.9 months. **Use inline R coding to get the duration standard
deviation reported to 1 decimal spot.**

For really big numbers you can use `format` to add commas. The maximum
credit amount in the data is $18,424.

## 4\. Supressing messages

I often load libraries that have annoying messages:

``` r
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

Check this chunk in your knitted document. You may have an error message
in your knitted document (and in your console if you run it there) that
tells you to install the package `dplyr`. If so, install the package and
reknit your document\! Once `dplyr` is installed, you will then get
annoying package message information when loading this library. To
supress this message include the option `message=FALSE`.

## 5\. Debugging code

If you did not have `dplyr` installed on your computer, then running the
`library(dplyr)` command in part 4 would normally cause a “fatal”
compilation error that stops your file from knitting. You then get to
frantically search your .Rmd file for the location of your error. But by
adding the option `error=TRUE` you can get your document to knit and
show you the error in the knitted output. Here is an example with
`error=TRUE`:

``` r
x<- 1:5
(x-1)(x+1)
```

    ## Error in eval(expr, envir, enclos): attempt to apply non-function

**What is the error in this code?? What happens if you set
`error=FALSE`? (Try it and see\!)**

One option for debugging a .Rmd file that has R code errors is to set
`knitr::opts_chunk$set(error=TRUE)` as a global option at the start of
the document. I usually don’t start out my Markdown files with
`error=TRUE` because it can be too easy to overlook an error in your
code this way.

There are generally two types of errors you get with Markdown docs. If
your doc knits after setting `error=TRUE` then you have an R code issue.
If the document still does not knit after adding this option, then you
have an error in formatting that is not related to your R code. If you
have a typo in your chunk options, then you will still get a reference
to a chunk line location. Other errors can involve mistakes in your
header. **Change your output header type to `guthub_document` to see a
non-R chunk type of error message.**
