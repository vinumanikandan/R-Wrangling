# R-Wrangling

## Overview

In this workshop, we will explore the fundamentals of data wrangling using the `tidyverse` package in R. Data wrangling involves transforming and preparing raw data into a format suitable for analysis and visualization.  


## Prerequisites

Participants should have:
- Basic knowledge of R syntax
- Familiarity with data frames and vectors
- Installed R and RStudio

If you are new to R please refer to [Basic R Guide](https://github.com/vinumanikandan/R-visualization/blob/main/README.md) for a comprehensive introduction and helpful resources.


## Agenda

1. **Introduction to tibble**
   - Why tibble ?
   - Create and transform to tibble


2. **Introduction to `dplyr`**
   - Overview of the `dplyr` package
   - Basic commands : `select`, `filter`, `arrange`, `mutate`, `summarize`

3. **Data Transformation with `dplyr`**
   - Working with factors and character strings
   - Using pipes (`%>%`) for chaining operations
   - Case study: Cleaning and transforming messy data

4. **Reshaping Data with `tidyr`**
   - Introduction to `tidyr` for data reshaping
   - Converting between wide and long formats
   - Handling missing values

5. **Combining Data Sets**
   - Joining (`left_join`, `inner_join`, etc.) data frames
   - Case study: Merging datasets from different sources

6. **Data Visualization with `ggplot2`**
   - Introduction to `ggplot2` for data visualization
   - Basic plotting functions: `ggplot`, `geom_point`, `geom_bar`, `geom_line`
   - Customizing plots with themes and aesthetics
   - Exercises

7. **Advanced `dplyr` Techniques**
   - Grouping and summarizing data (`group_by`, `summarize`)
   - Advanced filtering and conditional operations
   - Exercises

8. **Case Study: Real-world Data Wrangling**
   - Applying `tidyverse` principles to a complex dataset
   - Practical tips and strategies for efficient data wrangling
   - Exercises

9. **Conclusion and Next Steps**
   - Recap of key concepts and techniques learned
   - Resources for further learning (books, online tutorials, etc.)
   - Q&A and Feedback


# 1. Introduction to tibble


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

**Converting tibble as data frame**

```
# Convert tibble  to a normal data frame

penguins_df <- as.data.frame(penguins

# Create a tibble from an existing dataframe
penguins_df_tibble<-as_tibble(penguins_df)

```

**Excercise 1**

Using the Base R methods do the following on the dataframe `penguins_df`

1) subset columns species, island, bill_length_mm, bill_depth_mm
2) Consider only bill_length_mm is greater than 40
3) Create a new new column called `bill_ratio` (bill_length_mm / bill_depth_mm)
4) find mean bill_length_mm by species
   
<details>
  <summary>Excercise 1 Answer</summary>
  
```
# subset columns species, island, bill_length_mm, bill_depth_mm
penguins_selected_df <- penguins_df[, c("species", "island", "bill_length_mm", "bill_depth_mm")]

# Consider only bill_length_mm is greater than 40

penguins_selected_filtered_df <- penguins_selected_df[penguins_selected_df$bill_length_mm > 40, ]
dim(penguins_selected_filtered_df)

# Create a new new column called `bill_ratio` (bill_length_mm / bill_depth_mm)

penguins_selected_filtered_df <- transform(penguins_selected_filtered_df, bill_ratio = bill_length_mm / bill_depth_mm)
penguins_selected_filtered_df



#  find mean bill_length_mm by species

mean_bill_length_mm <- aggregate(penguins_selected_filtered_df$bill_length_mm ~ penguins_selected_filtered_df$species, FUN = mean)
mean_bill_length_mm
```

</details>

# 2. Introduction to `dplyr`

dplyr package is used for data manipulation and its attained using the following methods :

   - select() picks variables based on their names.
   - filter() picks cases based on their values.
   - mutate() adds new variables that are functions of existing variables.
   - slice() picks cases based on their position.
   - summarize() reduces multiple values down to a single summary.
   - arrange() changes the ordering of the rows.
   - group_by() perform any operation by group.

  For more information on [`dplyr`](https://dplyr.tidyverse.org)
  
- **i. select()**
  
The select function in dplyr is used to choose specific columns (subset) from a data relevant to analysis. By providing the names of the columns you want to keep, select allows you to quickly and intuitively subset your dataset.

There are helper functions to select columns based on specific patterns such as

   -   starts_with()
   -    ends_with()
   -    contains()
   -    matches()
     
This function enhances code readability and efficiency by streamlining the process of working with large datasets and eliminating the need to repeatedly specify unwanted columns.

Syntax : **select(`tb`, COLUMN_NAMES)**

for example subset columns species, island, bill_length_mm, bill_depth_mm from the tibble `penguins`

```
# Method 1 : here first variable is tibble and rest are the coulmns of interest
penguins_selected <- select(penguins,species, island, bill_length_mm, bill_depth_mm)

# Method 2 :c() for combining selections
penguins_selected2 <- select(penguins,c(species, island, bill_length_mm, bill_depth_mm))

# Method 3 : for selecting a range of consecutive variables.
penguins_selected3 <- select(penguins,species:bill_length_mm, sex)

# Method 4 : ! for taking the complement of a set of variables.
penguins_selected4 <- select(penguins,!(body_mass_g:year))
penguins_selected4

```
- **ii. filter()**
The filter function in dplyr is used to subset rows based on specified conditions.This function is particularly useful for cleaning and downsampling datasets to the relevant observations for analysis. With filter, you can efficiently handle missing values, outliers, and any other data points that do not fit the requirements of your analysis.

There are many functions and operators that are useful when constructing the expressions used to filter the data:
   - ==, >, >= etc
   - &, |, !, xor()
   - is.na()
   - between(), near()

```
# Filtering by single criteria 
penguins_selected_filtered <- filter(penguins_selected,bill_length_mm > 40)

# Filtering by multiple criteria within a single logical expression, could use &|,
filter(penguins_selected, bill_length_mm >= 40 & bill_depth_mm >= 20)
filter(penguins_selected, bill_length_mm >= 40 , bill_depth_mm >= 20)

# Filtering by multiple criteria within a single logical expression,either 1 condition satisfies,
filter(penguins_selected, bill_length_mm >= 40 | bill_depth_mm >= 20)

```
- **iii. mutate()**

`mutate()` creates new columns that are functions of existing variables. It can also modify value (existing column) and delete columns (by setting their value to NULL).

```
## adding new variables

penguins_selected_filtered_mutated<- mutate(penguins_selected_filtered,bill_ratio = bill_length_mm / bill_depth_mm)
penguins_selected_filtered_mutated
## adding new variables, remove existing variables

penguins_selected_filtered_mutated2<- mutate(penguins_selected_filtered,bill_ratio = bill_length_mm / bill_depth_mm,species=NULL)
penguins_selected_filtered_mutated2

## adding new variables, remove existing variables and modify existing variables.
penguins_selected_filtered_mutated3<- mutate(penguins_selected_filtered,bill_ratio = bill_length_mm / bill_depth_mm,species=NULL,bill_depth_mm=bill_depth_mm ^2)
penguins_selected_filtered_mutated3


```
- **iv. sumarize()**
- **v. group_by()**
- **vi. arrange()**
   


