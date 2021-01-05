# Introduction to `dplyr` in RStudio

<a href="http://creativecommons.org/licenses/by-nc/4.0/" rel="license"><img style="border-width: 0;" src="https://i.creativecommons.org/l/by-nc/4.0/88x31.png" alt="Creative Commons License" /></a>
This tutorial is licensed under a <a href="http://creativecommons.org/licenses/by-nc/4.0/" rel="license">Creative Commons Attribution-NonCommercial 4.0 International License</a>.

## Lab Goals

Data manipulation is central to data analysis and is often the most time consuming portion of an analysis. The dplyr package contains a suite of functions to make data manipulation easier. The core functions of the dplyr package can be thought of as verbs for data manipulation.

## Acknowledgements

This lab procedure is adapted from and based on Ryan Miller's ["Introduction to dplyr"](https://remiller1450.github.io/s230f19/Data_Wrangling.html) (Fall 2019, Intro to Data Science STA 230 course, Grinnell College).

# Table of Contents

- [What is `dplyr`?](#what-is-dplyr)
- [Data and Environment Setup](#data-and-environment-setup)
- [Working With Rows](#working-with-rows)
  * [Filtering Rows](#filtering-rows)
  * [Slicing Rows](#slicing-rows)
  * [Arranging Rows](#arranging-rows)
  * [Summarizing Rows](#summarizing-rows)
- [Working With Columns](#working-with-columns)
  * [Selecting Columns](#selecting-columns)
  * [Mutating Data (Adding New Columns)](#mutating-data-adding-new-columns)
- [Groupwise Manipulation](#groupwise-manipulation)
  * [Combining `group_by` With Other Commands](#combining-group_by-with-other-commands)
- [Additional Resources](#additional-resources)
- [Lab Notebook Questions](#lab-notebook-questions)

[Click here](https://raw.githubusercontent.com/kwaldenphd/dplyr-intro/main/dplyr_intro_markdown.Rmd) and select the "Save As" option to download this lab as an RMarkdown file.

# What is `dplyr`?

1. “dplyr is a grammar of data manipulation, providing a consistent set of verbs that help you solve the most common data manipulation challenges:
  * `mutate()` adds new variables that are functions of existing variables
  * `select()` picks variables based on their names.
  * `filter()` picks cases based on their values.
  * `summarise()` reduces multiple values down to a single summary.
  * `arrange()` changes the ordering of the rows.

These all combine naturally with `group_by()` which allows you to perform any operation “by group”. You can learn more about them in [`vignette("dplyr")`](https://dplyr.tidyverse.org/articles/dplyr.html). As well as these single-table verbs, dplyr also provides a variety of two-table verbs, which you can learn about in [`vignette("two-table")`](https://dplyr.tidyverse.org/articles/two-table.html).” [Source: [dplyr.tidyverse.org](https://dplyr.tidyverse.org/)]

2. More dplyr documentation: [cran.r-project.org/web/packages/dplyr/vignettes/dplyr.html](https://cran.r-project.org/web/packages/dplyr/vignettes/dplyr.html)

3. For more on the conceptual foundations for data transformation in `R`:
- [Chapter 5, "Data Transformation"](https://r4ds.had.co.nz/transform.html) in Hadley Wickham and Garrett Grolemund, [*R for Data Science: Visualize, Model, Transform, Tidy, and Import Data*](https://r4ds.had.co.nz/index.html) (O'Reilly, 2017).

# Data and Environment Setup

4. To begin, let’s make sure that our dataset and the `dplyr` package are loaded:
```R
#install.packages("dplyr")
library(dplyr)
colleges <- read.csv("https://raw.githubusercontent.com/kwaldenphd/dplyr-intro/main/colleges2015.csv")
```

5. The file `college2015.csv` contains information on predominantly bachelor’s-degree granting institutions from 2015 that might be of interest to a college applicant.

6. To get a feel for what data are available, look at the first six rows:
```R
head(colleges)
```

7. And look at the last six rows:
```R
tail(colleges)
```

8. We can also look at the structure of the data frame:
```R
str(colleges)
```

# Working With Rows

## Filtering Rows

9. To extract the rows only for colleges and universities in a specific state we use the `filter` function. 

10. For example, we can extract the colleges in Indiana from the colleges dataset.
```R
in <- filter(colleges, state =="IN")
head(in)
```

11. The first argument given to `filter` is always the data frame.

12. This is true for all the core functions in `dplyr`.

13. The data frame is followed by logical tests that the returned cases must pass. 

14. In our example, the test was whether the school was in Indiana, which is written as state == "IN".

15. We have to use `==` to indicate equality because in R syntax, `=` is equivalent to `<-`.

16. When testing character variables, be sure to use quotes to specify the value of the variable that you are testing.

17. To specify multiple tests, use a comma to separate the tests (think of the comma as the word “and”). 

18. For example, the following code returns only those rows corresponding to schools in Indiana with fewer than 2,000 undergraduate students.

```R
small_IN <- filter(colleges, state == "IN", undergrads < 2000)
```

19. To specify that at least one test must be passed, use the `|` character instead of the comma. 

20. For example, the code below test checks whether a college is in Indiana, Michigan, or Illinois, so it returns all of the colleges in Indiana, Michigan, or Illinois.
```R
In_Mi_Il <- filter(colleges, state == "IN" | state == "MI" | state == "IL")
```

21. You can use both `|` and `,` to specify multiple tests, or test for multiple conditions. 

22. For example, we can return all colleges with fewer than 2,000 undergraduate students in Indiana, Michigan, and Illinois.
```R
small_In_Mi_Il <- filter(colleges, ststate == "IN" | state == "MI" | state == "IL", undergrads < 2000)
```

23. Common comparison operators in `R` include: 

Symbol | Explanation
--- | ---
`>` | Greater than
`>=` | Greater than or equal to
`<` | Less than
`<=` | Less than or equal to
`!=` | Not equal to
`==` | Equal to

24. To remove rows with missing values, use the R command `na.omit`. 

25. For example, the following code will reduce the dataset to only rows with no missing values.
```R
colleges <- na.omit(colleges)
```

26. The following code will eliminate only rows with `NA` in the `cost` column.

```R
colleges <- filter(colleges, !is.na(cost))
```

<blockquote>Q1: Open a new R Script, and on the first line add the comment “Q1”. On the line below this comment use the filter function to write code the answers the following questions.
          <ul>
                    <li>How many Maryland colleges are in the colleges data frame? (The abbreviation for Maryland is MD.)</li>
                    <li>How many private Maryland colleges with under 5000 undergraduates are in the colleges data frame?</li>
          </ul>
</blockquote>

## Slicing Rows

27. To extract rows 10 through 16 from the colleges data frame we can use the `slice` function.
```R
slice(colleges, 10:16)
```

28. To select consecutive rows, create a vector of the row indices by separating the first and last row numbers with a `:`.

29. To select non-consecutive rows, create a vector manually by concatenating the row numbers using `c()`. 

30. For example, to select the 2nd, 18th, and 168th rows:
```R
slice(colleges, c(2, 18, 168))
```

31. NOTE: Remember `R` syntax starts index counting at 1 rather than 0.

## Arranging Rows

32. To sort the rows by total cost, from the least expensive to the most expensive, we use the `arrange` function.

```R
costDF <- arrange(colleges, cost)
head(costDF)
```

33. By default, arrange assumes that we want the data arranged in ascending order by the specified variable(s).

34. To arrange the rows in descending order, wrap the variable name in the `desc` function. 

35. For example, to arrange the data frame from most to least expensive we would use the following command: 

```R
costDF <- arrange(colleges, desc(cost))
```

36. To arrange a data frame by the values of multiple variables, list the variables in a comma separated list. 

37. The order of the variables specifies the order in which the data frame will be arranged. 

38. For example, the following code reorders colleges first by the median ACT math score (in descending order) and then by the ACT english score (in descending order).

```R
actDF <- arrange(colleges, desc(ACTmath), desc(ACTenglish))
```

<blockquote>Q2: Add a comment in your R Script indicating Q2, below the comment write R code to answer the following questions.
          <ul>
                    <li>Which school is most expensive in the entired dataset?</li>
                    <li>Which school has the least expensive tuition in Indiana?</li>
          </ul>
          </blockquote>
          
## Summarizing Rows

39. To create summary statistics for columns within the dataset we must aggregate all of the rows using the `summarize` command. 

40. NOTE: `R` will also accept the British English spelling `summarise`

41. For example, to calculate the median cost of all 1776 colleges in our dataset we run the following command:
```R
summarize(colleges, medianCost = median(cost, na.rm = TRUE))
```

42. As with all of the functions we have seen, the first argument should be the name of the data frame.

43. We add `na.rm = TRUE` here to remove any missing values in the cost column before the calculation. 

44. Many functions, including this `summarize` function, will return an error if there are missing values (blanks, NAs or NaNs) in your data. 

45. Notice that we add this argument inside of the `median()` function.

46. `summarize` returns a data frame, with one row and one column.

47. We can ask for multiple aggregations in one line of code by simply using a comma separated list. 

48. For example, we can calculate the five number summary of cost for all 1776 colleges in our dataset.
```R
summarize(colleges, 
          min = min(cost, na.rm = TRUE), 
          Q1 = quantile(cost, .25, na.rm = TRUE), 
          median = median(cost, na.rm = TRUE), 
          Q3 = quantile(cost, .75, na.rm = TRUE), 
          max = max(cost, na.rm = TRUE))
```

49. Notice that even when multiple statistics are calculated, the result is a data frame with one row and the number of columns correspond to the number of summary statistics.

# Working With Columns

## Selecting Columns

50. Suppose that you are only interested in a subset of the columns in the dataset and want to create a data frame with only these columns. 

51. To do this, we select the desired columns (`college`, `city`, `state`, `undergrads`, and `cost`):
```R
lessCols <- select(colleges, college, city, state, undergrads, cost)
head(lessCols)
```

52. After specifying the data frame, list the variable names to select from the data frame separated by commas.

53. In some cases you may want to drop a small number of variables from a data frame. 

54. In this case, putting a negative sign before a variable name tells select to select all but the negated variables. 

55. For example, if we only wished to drop the `unitid` variable, we run the following command:
```R
drop_unitid <- select(colleges, -unitid)
head(drop_unitid)
```

## Mutating Data (Adding New Columns)

56. Datasets often do not contain the exact variables we need, but contain all of the information necessary to calculate the needed variables. 

57. In this case, we can use the `mutate` function to add a new column to a data frame that is calculated from other variables. 

58. For example, we may wish to report percentages rather than proportions for the admissions rate.
```R
colleges <- mutate(colleges, admissionPct = 100 * admissionRate)
```

59. After specifying the data frame, give the name of the new variable and its definition. 

60. Notice that we need to use `=` to assign the value of the new variable.

61. To add multiple variables once, separate the list of new variables by commas. 

62. For example, we can also add percentage versions of `FYretention` and `gradRate`.

```R
colleges <- mutate(colleges, FYretentionPct = 100 * FYretention, gradPct = 100 * gradRate)
```

# Groupwise Manipulation

63. There are many situations in which we might want to manipulate data within groups. 

64. For example, we might be more interested in creating separate summaries for each state, or for private and public colleges. 

65. We can do this using the `group_by` function.

66. Most often `group_by` is paired with `summarise` or `mutate`.

67. Let’s first consider comparing the cost of private and public colleges. 

68. First, we must specify that the variable type defines the groups of interest.

```R
colleges_by_type <- group_by(colleges, type)
```

69. After specifying the data frame, list the categorical variable(s) defining the groups.

70. Multiple variables can be used to specify the groups. For example, to specify groups by state and type, we would run the following command: `colleges_state_type <- group_by(colleges, state, type)`


72. By itself, `group_by` doesn’t do anything other than prepare the data for other functions. 

73. So if we print the `colleges_state_type` data.frame we’ll notice it looks just like the original object.

## Combining `group_by` with other commands

74. Once we have a grouped data frame, we can obtain summaries by group via `summarize`. 

75. For example, if we wanted to obtain the five number summary of cost by institution type:
```R
summarize(colleges_by_type, 
          min = min(cost, na.rm = TRUE), 
          Q1 = quantile(cost, .25, na.rm = TRUE), 
          median = median(cost, na.rm = TRUE), 
          Q3 = quantile(cost, .75, na.rm = TRUE), 
          max = max(cost, na.rm = TRUE))
```

76. We can also calculate new variables within groups, such as the standardized cost of attendance within each state:
```R
colleges_by_state <- group_by(colleges, state)
colleges_by_state <- mutate(colleges_by_state, 
                            mean.cost = mean(cost, na.rm = TRUE), 
                            sd.cost = sd(cost, na.rm = TRUE),
                            std.cost = (cost - mean.cost) / sd.cost)
head(colleges_by_state)
```

77. `mutate` allows you to use variables defined earlier to calculate a new variable. 

78. This is how `std.cost` was calculated.

79. The `group_by` function returns an object of class `c("grouped_df", "tbl_df",     "tbl", "data.frame")`.

80. This might look confusing, but it essentially allows the data frame to be printed neatly. 

81. Notice that only the first 10 rows print when we print the data frame in the console by typing `colleges_by_state`, and the width of the console determines how many variables are shown.

82. To print all columns we can convert the results back to a data.frame using the `as.data.frame` function. 

83. What's the difference between tibbles and data frames?

84. "There are two main differences in the usage of a data frame vs a tibble: printing, and subsetting. Tibbles have a refined print method that shows only the first 10 rows, and all the columns that fit on screen. This makes it much easier to work with large data. In addition to its name, each column reports its type, a nice feature borrowed from `str()`" (Hadley Wickham, ["tibble 1.0.0](https://blog.rstudio.com/2016/03/24/tibble-1-0-0/), *RStudio Blog*, 24 March 2016).

85. Try running the following code.

```R
head(as.data.frame(colleges_by_state))
```

86. You can also use the viewer by running the following command.
```R
view(colleges_by_state)
```

87. Another option is to select a reduced number of columns to print.

<blockquote>Q3: Add a comment to your R Script indicating you are now answering question 3, below the comment write code that accomplishes the following tasks:
          <ul>
                    <li>Filters the college data to include only private schools</li>
                    <li>Creates a new variable `total.avg.cost4` that is the cumulative average cost of attendance (assuming that a student finishes in four years and that costs increase 3% each year)</li>
                    <li>Summarizes the cumulative average cost of attendance by region using the mean, median, and 90th percentile.</li>
          </ul>
          </blockquote>
          
# Additional Resources

88. [RStudio’s data wrangling cheat sheet](https://www.rstudio.com/wp-content/uploads/2015/02/data-wrangling-cheatsheet.pdf) provides a nice summary of the functions in the dplyr package, including those covered in this tutorial.

89. The [introductory vignette to `dplyr`](https://cran.rstudio.com/web/packages/dplyr/vignettes/dplyr.html) provides an example of wrangling a dataset consisting of 336,776 flights that departed from New York City in 2013.

90. Roger Peng, ["Introduction to the `dplyr` R package"](https://youtu.be/aywFompr1F4) *YouTube* (31 December 2014).

# Lab Notebook Questions

Q1: Open a new R Script, and on the first line add the comment “Q1”. On the line below this comment use the filter function to write code the answers the following questions.
- How many Maryland colleges are in the colleges data frame? (The abbreviation for Maryland is MD.)
- How many private Maryland colleges with under 5000 undergraduates are in the colleges data frame?

Q2: Add a comment in your R Script indicating Q2, below the comment write R code to answer the following questions.
- Which school is most expensive in the entired dataset?
- Which school has the least expensive tuition in Indiana?

Q3: Add a comment to your R Script indicating you are now answering question 3, below the comment write code that accomplishes the following tasks:
- Filters the college data to include only private schools
- Creates a new variable `total.avg.cost4` that is the cumulative average cost of attendance (assuming that a student finishes in four years and that costs increase 3% each year)
- Summarizes the cumulative average cost of attendance by region using the mean, median, and 90th percentile.
