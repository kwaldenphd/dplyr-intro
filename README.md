# Introduction to `dplyr` in RStudio

<a href="http://creativecommons.org/licenses/by-nc/4.0/" rel="license"><img style="border-width: 0;" src="https://i.creativecommons.org/l/by-nc/4.0/88x31.png" alt="Creative Commons License" /></a>
This tutorial is licensed under a <a href="http://creativecommons.org/licenses/by-nc/4.0/" rel="license">Creative Commons Attribution-NonCommercial 4.0 International License</a>.

## Lab Goals

Data manipulation is central to data analysis and is often the most time consuming portion of an analysis. The dplyr package contains a suite of functions to make data manipulation easier. The core functions of the dplyr package can be thought of as verbs for data manipulation.

## Acknowledgements

This lab procedure is adapted from and based on Ryan Miller's ["Introduction to dplyr"](https://remiller1450.github.io/s230f19/Data_Wrangling.html) (Fall 2019, Intro to Data Science STA 230 course, Grinnell College).

# Table of Contents

# What is `dplyr`?

“dplyr is a grammar of data manipulation, providing a consistent set of verbs that help you solve the most common data manipulation challenges:
mutate() adds new variables that are functions of existing variables
select() picks variables based on their names.
filter() picks cases based on their values.
summarise() reduces multiple values down to a single summary.
arrange() changes the ordering of the rows.

These all combine naturally with group_by() which allows you to perform any operation “by group”. You can learn more about them in vignette("dplyr"). As well as these single-table verbs, dplyr also provides a variety of two-table verbs, which you can learn about in vignette("two-table").” [Source: [dplyr.tidyverse.org](https://dplyr.tidyverse.org/)]

More dplyr documentation: [cran.r-project.org/web/packages/dplyr/vignettes/dplyr.html](https://cran.r-project.org/web/packages/dplyr/vignettes/dplyr.html)

# Data and Environment Setup

To begin, let’s make sure that our data set and the dplyr package are loaded:
```R
#install.packages("dplyr")
library(dplyr)
colleges <- read.csv("https://raw.githubusercontent.com/kwaldenphd/dplyr-intro/main/colleges2015.csv")
```

The file college2015.csv contains information on predominantly bachelor’s-degree granting institutions from 2015 that might be of interest to a college applicant.

To get a feel for what data are available, look at the first six rows:
```R
head(colleges)
```

And look at the last six rows:
```R
tail(colleges)
```

We can also look at the structure of the data frame:
```R
str(colleges)
```

# Working With Rows

## Filtering Rows

## Slicing Rows

## Arranging Rows

## Summarizing Rows

# Working With Columns

## Selecting Columns

## Mutating Data (Adding New Columns)

# Groupwise Manipulation

## Combining group_by with other commands

# Additional Resources

# Lab Notebook Questions

