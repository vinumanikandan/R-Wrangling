# R-Wrangling

## Overview

In this workshop, we will explore the fundamentals of data wrangling using the `tidyverse` package in R. Data wrangling involves transforming and preparing raw data into a format suitable for analysis and visualization.  


## Prerequisites

Participants should have:
- Basic knowledge of R syntax
- Familiarity with data frames and vectors
- Installed R and RStudio

If you are new to R and encounter any difficulties, please refer to the [Basic R Guide](https://github.com/vinumanikandan/R-visualization/blob/main/README.md) for a comprehensive introduction and helpful resources.


## Agenda

1. **Introduction to `dplyr`**
   - Overview of the `dplyr` package
   - Basic verbs: `select`, `filter`, `arrange`, `mutate`, `summarize`
   - Exercises

2. **Data Transformation with `dplyr`**
   - Working with factors and character strings
   - Using pipes (`%>%`) for chaining operations
   - Case study: Cleaning and transforming messy data
   - Exercises

3. **Reshaping Data with `tidyr`**
   - Introduction to `tidyr` for data reshaping
   - Converting between wide and long formats
   - Handling missing values
   - Exercises

4. **Combining Data Sets**
   - Joining (`left_join`, `inner_join`, etc.) data frames
   - Case study: Merging datasets from different sources
   - Exercises

5. **Data Visualization with `ggplot2`**
   - Introduction to `ggplot2` for data visualization
   - Basic plotting functions: `ggplot`, `geom_point`, `geom_bar`, `geom_line`
   - Customizing plots with themes and aesthetics
   - Exercises

6. **Advanced `dplyr` Techniques**
   - Grouping and summarizing data (`group_by`, `summarize`)
   - Advanced filtering and conditional operations
   - Exercises

7. **Case Study: Real-world Data Wrangling**
   - Applying `tidyverse` principles to a complex dataset
   - Practical tips and strategies for efficient data wrangling
   - Exercises

8. **Conclusion and Next Steps**
   - Recap of key concepts and techniques learned
   - Resources for further learning (books, online tutorials, etc.)
   - Q&A and Feedback


**What is Tidyverse ?**

[Tidyverse](https://www.tidyverse.org) is an ecosytem of R packages designed the usability of data which includes data wrangling,data manuplation,data managment and data plotting. Tidyverse achieves this by creating independent packages in its universe 

**1. ggplot2 :** ggplot2 (grammer of graphics) is a R package for data visulization

**2. dplyr   :** data manipulation

**3. tibble  :** data managment

**4: tidyr   :** data wrangling

 .... and many more

# 1. Introduction to dplyr


A data frame in R is a two-dimensional data structure used to store tabular data. It is similar to a table in a database or a spreadsheet in Excel.

***Normal structure of Dataframe***
  - Columns: Each column represents a variable and can hold different types of data. For example, one column might contain numeric data, while another column might contain character strings.
  - Rows: Each row represents an observation or a record. All rows must have the same number of elements (i.e., the same number of columns).

**Cons of dataframes**


  1. Data frames can be slower and less memory-efficient when handling very large datasets & can affect performance during data manipulation and analysis.
  2. data frames support basic data manipulation tasks and complex manupulations will be strenuous
  3. Difficult to handle datasets that have non-standard or special character column names.


**tibble**
[tibble](https://tibble.tidyverse.org) are a modern reimagining of data frames provided by the `tibble` package in the tidyverse collection. One of the main benefits of tibbles is that they are more robust and user-friendly: 


  1. they do not change variable names or types, are more consistent in their behavior
  2. provide a cleaner print method that fit on screen, which is particularly useful for large datasets
  3. provide better support for non-standard column names , making them more compatible with the dplyr and tidyr packages for data manipulation.
  
Overall, using tibbles can lead to more readable and maintainable code, enhancing the data wrangling process.

### Set up

```
install.packages("palmerpenguins")

# install the complete tidyverse packages
install.packages("tidyverse")

# Alternatively, install just dplyr:
install.packages("dplyr")

# Load the packages
library(palmerpenguins)

## Load either 1 of the following
library(dplyr) ## load only dplyr
library(tidyverse) ## load all tidyverse packages

```

**Load the Dataset**
The palmerpenguins package provides a dataset with measurements of three penguin species
```
penguins

```

![penguins tibble ](images/tibble1.png){width :50%}
